### 1、Hadoop

> Hadoop是一个由Apache基金会所开发的[分布式系统](https://baike.baidu.com/item/%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F/4905336?fromModule=lemma_inlink "分布式系统")基础架构。 
> 
> Hadoop 以一种可靠、高效、可伸缩的方式进行数据处理。
> 
> Hadoop实现了一个[分布式文件系统](https://baike.baidu.com/item/%E5%88%86%E5%B8%83%E5%BC%8F%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F/1250388?fromModule=lemma_inlink "分布式文件系统")（ Distributed File System），其中一个组件是[HDFS](https://baike.baidu.com/item/HDFS/4836121?fromModule=lemma_inlink "HDFS")（Hadoop Distributed File System）。

### 2、Flask

> Flask是一个用Python编写的Web应用程序框架。
> 
> 基于Werkzeug WSGI工具和Jinja2模板引擎。
> 
> Flask通常被称为微框架, 它旨在保持应用程序的核心简单且可扩展。Flask没有用于数据库处理的内置抽象层，也没有形成验证支持。相反，Flask支持扩展以向应用程序添加此类功能。

### 3、Flink

> Flink是一个框架和分布式的处理引擎，用于对无界和有界数据流进行状态计算。
> 
> - 有界的数据流就是有限量的静态数据，比如数据库里现在存好的数据，它就是有界的。
> - 无界数据流就是有一个数据源给你不断的发送数据，比如一个传感器不断的向服务器发送状态信息，比如服务器的实时监控程序。
> 
> Flink框架可以说是实现真正意义上的实时流处理，大大降低了流计算的延迟，更能满足当下的大数据处理需求。

### 4、Hive

> hive是基于[Hadoop](https://baike.baidu.com/item/Hadoop/3526507?fromModule=lemma_inlink "Hadoop")的一个[数据仓库](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E4%BB%93%E5%BA%93/381916?fromModule=lemma_inlink "数据仓库")工具，用来进行数据提取、转化、加载，这是一种可以存储、查询和分析存储在Hadoop中的大规模数据的机制。
> 
> hive数据仓库工具能将结构化的数据文件映射为一张数据库表，并提供[SQL](https://baike.baidu.com/item/SQL/86007?fromModule=lemma_inlink "SQL")查询功能，能将[SQL语句](https://baike.baidu.com/item/SQL%E8%AF%AD%E5%8F%A5/5714895?fromModule=lemma_inlink "SQL语句")转变成[MapReduce](https://baike.baidu.com/item/MapReduce/133425?fromModule=lemma_inlink "MapReduce")任务来执行。

### 5、Flume

> Flume 日志收集系统
> 
> Flume是Cloudera提供的一个高可用的，高可靠的，分布式的海量日志采集、聚合和传输的系统，Flume支持在日志系统中定制各类数据发送方，用于收集数据；同时，Flume提供对数据进行简单处理，并写到各种数据接受方（可定制）的能力。

### 6、kafka

> Kafka是一种高吞吐量的[分布式](https://baike.baidu.com/item/%E5%88%86%E5%B8%83%E5%BC%8F/19276232?fromModule=lemma_inlink "分布式")发布订阅消息系统，它可以处理消费者在网站中的所有动作流数据。
> 
> kafka是一个分布式的基于发布/订阅模式的消息队列，主要应用于大数据实时处理领域。
> 
> Kafka是一个分布式消息队列。Kafka对消息保存时根据Topic进行归类，发送消息者称为Producer，消息接受者称为Consumer，此外kafka集群有多个kafka实例组成，每个实例(server)称为broker。