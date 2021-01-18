# kafka

## 1.kafka

Kafka是一种高吞吐、分布式、基于发布和订阅模型的消息系统，最初住Linkedln 公司开发，使用Scala编写，目前是Apache的开源项目。kafka用于离线和在线消息的消费。kafka将消息数据按顺序保存在磁盘上，并在集群内以副本的形式存储以防止数据丢失。

Kafka依赖ZooKeeper进行集群的管理，kafka与Storm、Spark能够非常友好地集成，用于实时流式计算。

## 2.Kafka的组成

Kafka的核心概念有Producer、Consumer、Broker和Topic。其中，Producer为消息生产者；Consumer为消息消费者；Broker为Kafka的消息服务端，负责消息的存储和转发；Topic为消息类别，Kafka按照Topic来对消息分类。

为了提高集群的并发度，Kafka还设计了Partition用于Topic上数据的分区，一个Topic数据可以分为多个Partition，每个Partition都负责保存和处理其中一部分消息数据。Partition的个数对应了消费者和生产者的并发度，比如Partition的个数为3，则集群中最多同时有3个线程的消费者并发处理数据。

Consumer Group为消费者组，每个Consumer都必须属于同一个Group，同一个Group内的Consumer可以并发地消费消息。

Kafka消息队列的原理。

![](D:\workspace\Java-Interview-Offer\images\kafka001.png)

ZooKeeper为Kafka提供集群的管理。它保存着集群的Broker、Topic、Partition等元数据，还负责Broker故障发现、Leader选举、负载均衡等。

## 3.Kafka的数据存储设计

### Partition数据文件

Partition中的每条Message都包含3个属性：Offset、MessageSize、Data。其中，Offset表示Message在这个Partition中的偏移量，它在逻辑上是一个值，唯一确定了Partition中的一条Message;MessageSize表示消息内容Data的大小；Data为Message的具体内容。

### Segment数据文件

Partition在物理上由多个Segment数据文件组成，每个Segment数据文件都大小相等、按顺序读写。每个Segment数据文件都以该段中最小的Offset命名，文件扩展名为.log。这样在查找指定Offset的Message的时候，用二分查找就可以定位到该Message在哪个Segment数据文件中。

Segment数据文件首先会被存储在内存中，当Segment上的消息条数达到配置值或消息发送时间超过阈值时，其上的消息会被Flush到磁盘，只有被Flush到磁盘的消息才能被消费者消费到。

Segment达到一定的大小（可以通过配置文件设定，默认为1GB）后将不会再往该Segment中写数据，Broker会创建新的Segment。

Kafka Segment的存储结构

![](D:\workspace\Java-Interview-Offer\images\kafka002.png)

### 数据文件索引

Kafka为每个Segment数据文件都建立了索引文件以方便数据寻址，索引文件的文件名与数据文件的文件名一致，不同的是索引文件的扩展名为.index。Kafka的索引文件并不会为数据文件中的每条Message都建立索引，而是采用稀疏索引的方式，每隔一定字节建立一条索引。这样可以有效地降低索引文件的大小，方便将索引文件加载到内存中以提高集群的吞吐量。索引文件中的第一位表示索引对应的Message的编号，第二位表示索引对应的Message的数据位置。

## 4.生产者并发设计

### 多个Producer并发生产消息

Kafka将一个Topic分为多个Partition，每个Partition上的数据都均衡分布在不同的Broker上，这样一个Topic上的数据就可以被多个Broker并发地接收或发送。

在实际应用过程中，为了提高消息的吞吐量，应用程序可以将Topic的Partition设置为多个（Partition的个数也不宜太多，一般依据集群大小和Topic上的数据量来决定，Partition的个数不能超过Broker节点的个数）。Producer可以通过随机或者Hash等方式将消息平均发送到多个Partition上以实现负载均衡。Kafka Partition多个Producer并发生产消息。

### 批量发送消息

批量发送消息是提高吞吐量的重要方式，Producer端可以在内存中合并多条消息后，以一次请求的方式发送批量的消息给Broker，从而大大减少Broker存储消息的I/O操作次数。但批量发送的时间应该在业务能够接受的延迟时间范围内。

### 压缩消息

Producer端可以通过gzip或Snappy格式对消息集合进行压缩。消息在Producer端进行压缩，在Consumer端进行解压。压缩的好处就是减少网络传输的数据量，减轻对网络带宽传输的压力。在实时处理海量数据的集群环境下，系统瓶颈往往体现在网络I/O和带宽上，因为内存和CPU对数据的处理效率常常是I/O的几十倍甚至上百倍。

## 5.消费者并发设计

### 多个Consumer并发消费消息

Topic的消息以Partition的形式存在于多个Broker上，应用程序可以启动多个Consumer并行地消费Topic上的数据以提高消息的处理效率。需要注意的是，一个Partition上的消息是时间有序的，多个Partition之间的顺序无法保证。Kafka多个Consumer并发消费消息。

### Consumer Group的概念和特性

Consumer Group是一个消费者组，同一个Consumer Group中的多个Consumer 线程可以并发地消费Topic上的消息，Consumer的线程并发数一般等于Partition的个数。同一个Consumer Group中的多个Consumer不能同时消费同一个Partition上的数据。不同Consumer Group 中的Consumer在同一个Topic上的数据消费互不影响。Consumer Group和 Consumer以group.id和client.id唯一标识。每个Consumer的每条消费记录都以Offset的形式提交到Kafka集群的Broker上，用以记录消息消费的位置。

同一个Partition内的消息是有序的，多个Partition上的数据无法保证时间的有序性。Consumer通过Pull方式消费消息。Kafka不删除已消费的消息。在Partition内部，Kafka消息按顺序读写磁盘数据，以时间复杂度O(1)的方式提供消息的持久化功能.

## 6.Kafka的安装

Kafka的安装依赖Java和ZooKeeper，

( 1）下载Kafka安装包。

( 2 ）解压Kafka安装包：到Kafka下载目录执行以下命令解压安装包

```
tar -xzf kafka 2.12-2.2.0.tgz 
```

( 3 ）配置Kafka：到Kafka配置文件目录执行以下命令并配置server.properties配置

```
cd kafka 2.12-2.2.0 vim config/server properties 
```

Kafka server.properties配置文件的核心配置如下。其中，broker.id是Broker的唯一标识，host.name和port分别是Kafka服务监听的地址和端口，log.dirs是Kafka数据文件存储的目录，zookeeper.connect是zookeeper的服务地址。

( 4 ）启动ZooKeeper：到zookeeper安装目录执行start命令启动ZooKeeper。

```
bin/zkServer sh start
```

( 5 ）启动Kafka：到Kafka安装目录执行start令启动Kafka。其中“＞／dev/null2>&1 ＆”表示后台启动并将日志输出到Linux黑洞。

```
bin/kafka-server-start.sh config/server.properties>/dev/null 2＞&1 & 
```

( 6 ）建立Kafka Topic：到Kafka安装目录调用kafka-topics.sh脚本，执行以下命令建立名称为TopicA、数据副本的个数为1、Partition的个数为3的Topic。

```
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 
--replication-factor 1 --partitions 3 --topic TopicA 
```

在上述命令中，--create表示对Topic的创建动作，--bootstrap-server为Kafka服务端的地址，--replication-factor为数据副本的个数，--partitions为消息分区的个数。

( 7)查看Kafka Topic ：到Kafka安装目录调用kafka-topics.sh脚本，执行以下命令查看刚刚建立的Topic信息。其中--describe表示打印Topic的描述信息。

( 8 ）安装Kafka集群：Kafka集群的安装只需要在各个服务端的server.properties上设置不同的broker.id然后启动即可。

## 7.基于SpringBootKafka应用

下面以SpringBoot为例介绍Kafka消息生产和消费的使用方法。

( 1 ）引人pom.xml依赖：新建SpringBoot项目，并在pom.xml中添加如下Kafka依赖，其中spring-kafka是Spring对Kafka客户端操作的封装。

( 2 ）添加配置文件：在application.properties配置文件中添加如下Kafka配置，其中spring.kafka.bootstrap-servers为Kafka的服务地址。

( 3 ）配置SpringBoot异步多线程：由于SpringBoot注册的组件默认是单线程的，所以为了提高并发度，应用程序需要以多线程的方式注册Kafka Cosumer。

( 4 ）定义Kafka Producer类：由于Spring将Kafka的操作封装为KafkaTemplate，所以基于SpringBoot项目应用程序只需要引入spring-kafka的依赖包，并在application.properties中设置好Kafka配置。SpringBoot在项目初始化后自动为应用程序创建KafkaTemplate，在应用程序上只需要依赖注入并使用即可。

( 5 ）定义Kafka Consumer : Consumer监听的定义只需要在方法上配置@KafkaListener注解并在注解上配置要监听的Topic即可。这里的Topics可以是多个。同时，为了提高消息消费的并发度，需要在方法上添@Async()注解，表示该方法是一个多线程异步执行的方法。