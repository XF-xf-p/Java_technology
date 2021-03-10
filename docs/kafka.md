# kafka

## 1.kafka

Kafka是一种高吞吐、分布式、基于发布和订阅模型的消息系统，最初住Linkedln 公司开发，使用Scala编写，目前是Apache的开源项目。kafka用于离线和在线消息的消费。kafka将消息数据按顺序保存在磁盘上，并在集群内以副本的形式存储以防止数据丢失。

Kafka依赖ZooKeeper进行集群的管理，kafka与Storm、Spark能够非常友好地集成，用于实时流式计算。

1.broker：Kafka服务器，负责消息存储和转发。

2.topic：消息类别，Kafka按照topic来分类消息。

3.partition：topic的分区，一个topic可以包含多个partition，topic消息保存在各个partition上。

4.offset：消息在日志中的位置，可以理解是消息在partition上的偏移量，也是代表该消息的唯一序号。

5.Producer：消息生产者。

6.Consumer：消息消费者。

7.Consumer Group：消费者分组，每个Consumer必须属于一个group。

8.Zookeeper：保存着集群broker、topic、partition等meta数据；另外，还负责broker故障发现，partition leader选举，负载均衡等功能。

![](D:\workspace\Java-Interview-Offer\images\kafka003.png)

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

### 数据文件分段segment（顺序读写、分段命令、二分查找）

Partition在物理上由多个Segment数据文件组成，每个Segment数据文件都大小相等、按顺序读写。每个Segment数据文件都以该段中最小的Offset命名，文件扩展名为.log。这样在查找指定Offset的Message的时候，用二分查找就可以定位到该Message在哪个Segment数据文件中。

Segment数据文件首先会被存储在内存中，当Segment上的消息条数达到配置值或消息发送时间超过阈值时，其上的消息会被Flush到磁盘，只有被Flush到磁盘的消息才能被消费者消费到。

Segment达到一定的大小（可以通过配置文件设定，默认为1GB）后将不会再往该Segment中写数据，Broker会创建新的Segment。

Kafka Segment的存储结构

![](D:\workspace\Java-Interview-Offer\images\kafka002.png)

### 数据文件索引（分段索引、稀疏存储）

Kafka为每个Segment数据文件都建立了索引文件以方便数据寻址，索引文件的文件名与数据文件的文件名一致，不同的是索引文件的扩展名为.index。Kafka的索引文件并不会为数据文件中的每条Message都建立索引，而是采用稀疏索引的方式，每隔一定字节建立一条索引。这样可以有效地降低索引文件的大小，方便将索引文件加载到内存中以提高集群的吞吐量。索引文件中的第一位表示索引对应的Message的编号，第二位表示索引对应的Message的数据位置。

![](D:\workspace\Java-Interview-Offer\images\kafka004.png)

## 4.生产者并发设计

### 负载均衡（partition会均衡分布到不同broker上）

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

## 6.Kafka 的高可用性

Kafka 一个最基本的架构认识：由多个 broker 组成，每个 broker 是一个节点；你创建一个 topic，这个 topic 可以划分为多个 partition，每个 partition 可以存在于不同的 broker 上，每个 partition 就放一部分数据。

这就是**天然的分布式消息队列**，就是说一个 topic 的数据，是**分散放在多个机器上的，每个机器就放一部分数据**。

实际上 RabbitMQ 之类的，并不是分布式消息队列，它就是传统的消息队列，只不过提供了一些集群、HA(High Availability, 高可用性) 的机制而已，因为无论怎么玩儿，RabbitMQ 一个 queue 的数据都是放在一个节点里的，镜像集群下，也是每个节点都放这个 queue 的完整数据。

Kafka 0.8 以后，提供了 HA 机制，就是 replica（复制品） 副本机制。每个 partition 的数据都会同步到其它机器上，形成自己的多个 replica 副本。所有 replica 会选举一个 leader 出来，那么生产和消费都跟这个 leader 打交道，然后其他 replica 就是 follower。写的时候，leader 会负责把数据同步到所有 follower 上去，读的时候就直接读 leader 上的数据即可。只能读写 leader？很简单，**要是你可以随意读写每个 follower，那么就要 care 数据一致性的问题**，系统复杂度太高，很容易出问题。Kafka 会均匀地将一个 partition 的所有 replica 分布在不同的机器上，这样才可以提高容错性。

如果某个 broker 宕机了，没事儿，那个 broker 上面的 partition 在其他机器上都有副本的。如果这个宕机的 broker 上面有某个 partition 的 leader，那么此时会从 follower 中**重新选举**一个新的 leader 出来，大家继续读写那个新的 leader 即可。这就有所谓的高可用性了。

**写数据**的时候，生产者就写 leader，然后 leader 将数据落地写本地磁盘，接着其他 follower 自己主动从 leader 来 pull 数据。一旦所有 follower 同步好数据了，就会发送 ack 给 leader，leader 收到所有 follower 的 ack 之后，就会返回写成功的消息给生产者。（当然，这只是其中一种模式，还可以适当调整这个行为）

**消费**的时候，只会从 leader 去读，但是只有当一个消息已经被所有 follower 都同步成功返回 ack 的时候，这个消息才会被消费者读到。

## 7.如何保证消息消费的幂等性？

Kafka 实际上有个 offset 的概念，就是每个消息写进去，都有一个 offset，代表消息的序号，然后 consumer 消费了数据之后，**每隔一段时间**（定时定期），会把自己消费过的消息的 offset 提交一下，表示“我已经消费过了，下次我要是重启啥的，你就让我继续从上次消费到的 offset 来继续消费吧”。

但是凡事总有意外，比如我们之前生产经常遇到的，就是你有时候重启系统，看你怎么重启了，如果碰到点着急的，直接 kill 进程了，再重启。这会导致 consumer 有些消息处理了，但是没来得及提交 offset，尴尬了。重启之后，少数消息会再次消费一次。

注意：新版的 Kafka 已经将 offset 的存储从 Zookeeper 转移至 Kafka brokers，并使用内部位移主题 `__consumer_offsets` 进行存储。

一条数据重复出现两次，数据库里只有一条数据，这就保证了系统的幂等性。

幂等性，通俗点说，就一个数据，或者一个请求，给你重复来多次，你得确保对应的数据是不会改变的，**不能出错**。

怎么保证消息队列消费的幂等性？其实还是得结合业务来思考，我这里给几个思路：

- 比如你拿个数据要写库，你先根据主键查一下，如果这数据都有了，你就别插入了，update 一下好吧。
- 比如你是写 Redis，那没问题了，反正每次都是 set，天然幂等性。
- 比如你不是上面两个场景，那做的稍微复杂一点，你需要让生产者发送每条数据的时候，里面加一个全局唯一的 id，类似订单 id 之类的东西，然后你这里消费到了之后，先根据这个 id 去比如 Redis 里查一下，之前消费过吗？如果没有消费过，你就处理，然后这个 id 写 Redis。如果消费过了，那你就别处理了，保证别重复处理相同的消息即可。
- 比如基于数据库的唯一键来保证重复数据不会重复插入多条。因为有唯一键约束了，重复数据插入只会报错，不会导致数据库中出现脏数据。

## 8.如何保证消息的可靠性传输

### 消费端弄丢了数据

唯一可能导致消费者弄丢数据的情况，就是说，你消费到了这个消息，然后消费者那边**自动提交了 offset**，让 Kafka 以为你已经消费好了这个消息，但其实你才刚准备处理这个消息，你还没处理，你自己就挂了，此时这条消息就丢咯。

大家都知道 Kafka 会自动提交 offset，那么只要**关闭自动提交** offset，在处理完之后自己手动提交 offset，就可以保证数据不会丢。但是此时确实还是**可能会有重复消费**，比如你刚处理完，还没提交 offset，结果自己挂了，此时肯定会重复消费一次，自己保证幂等性就好了。

生产环境碰到的一个问题，就是说我们的 Kafka 消费者消费到了数据之后是写到一个内存的 queue 里先缓冲一下，结果有的时候，你刚把消息写入内存 queue，然后消费者会自动提交 offset。然后此时我们重启了系统，就会导致内存 queue 里还没来得及处理的数据就丢失了。

### Kafka 弄丢了数据

这块比较常见的一个场景，就是 Kafka 某个 broker 宕机，然后重新选举 partition 的 leader。大家想想，要是此时其他的 follower 刚好还有些数据没有同步，结果此时 leader 挂了，然后选举某个 follower 成 leader 之后，不就少了一些数据？这就丢了一些数据啊。

生产环境也遇到过，我们也是，之前 Kafka 的 leader 机器宕机了，将 follower 切换为 leader 之后，就会发现说这个数据就丢了。

所以此时一般是要求起码设置如下 4 个参数：

- 给 topic 设置 `replication.factor` 参数：这个值必须大于 1，要求每个 partition 必须有至少 2 个副本。
- 在 Kafka 服务端设置 `min.insync.replicas` 参数：这个值必须大于 1，这个是要求一个 leader 至少感知到有至少一个 follower 还跟自己保持联系，没掉队，这样才能确保 leader 挂了还有一个 follower 吧。
- 在 producer 端设置 `acks=all` ：这个是要求每条数据，必须是**写入所有 replica 之后，才能认为是写成功了**。
- 在 producer 端设置 `retries=MAX` （很大很大很大的一个值，无限次重试的意思）：这个是**要求一旦写入失败，就无限重试**，卡在这里了。

我们生产环境就是按照上述要求配置的，这样配置之后，至少在 Kafka broker 端就可以保证在 leader 所在 broker 发生故障，进行 leader 切换时，数据不会丢失。

### 生产者会不会弄丢数据？

如果按照上述的思路设置了 `acks=all` ，一定不会丢，要求是，你的 leader 接收到消息，所有的 follower 都同步到了消息之后，才认为本次写成功了。如果没满足这个条件，生产者会自动不断的重试，重试无限次。

## 9.如何保证消息的顺序性？

比如说我们建了一个 topic，有三个 partition。生产者在写的时候，其实可以指定一个 key，比如说我们指定了某个订单 id 作为 key，那么这个订单相关的数据，一定会被分发到同一个 partition 中去，而且这个 partition 中的数据一定是有顺序的。 消费者从 partition 中取出来数据的时候，也一定是有顺序的。到这里，顺序还是 ok 的，没有错乱。接着，我们在消费者里可能会搞**多个线程来并发处理消息**。因为如果消费者是单线程消费处理，而处理比较耗时的话，比如处理一条消息耗时几十 ms，那么 1 秒钟只能处理几十条消息，这吞吐量太低了。而多个线程并发跑的话，顺序可能就乱掉了。

### 解决方案

- 一个 topic，一个 partition，一个 consumer，内部单线程消费，单线程吞吐量太低，一般不会用这个。
- 写 N 个内存队列queue，具有相同 key 的数据都到同一个内存 queue；然后对于 N 个线程，每个线程分别消费一个内存 queue 即可，这样就能保证顺序性。