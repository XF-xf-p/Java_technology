## 1.HBase原理及应用

HBase是一个开源的分布式Key-Value数据库，其主要作用是面向数十亿级数据的实时入库和快速随机访问。HBase底层存储基于HDFS实现，集群的管理基于ZooKeeper实现。HBase良好的分布式架构设计为海量数据的快速存储、随机访问提供了可能，基于数据副本机制和分区机制可以轻松实现在线扩容、缩容和数据容灾，是大数据领域中Key-Value数据结构存储最常用的数据库方案。

## 2.HBase的概念

HBase是一个面向列式存储的分布式数据库。HBase的数据模型与BigTable十分相似。在HBase表中，一条数据拥有一个全局唯一的键（RowKey）和任意数量的（Column），一列或多列组成一个列族（Column Family），同一个列族中列的数据在物理上都存储在同一个HFile中，这样基于列存储的数据结构有利于数据缓存和查询。HBase中的表是疏松地存储的，因此用户可以动态地为数据定义各种不同的列。HBase中的数据按主键排序，同时，HBase会将表按主键划分为多个Region存储在不同Region Server上，以完成数据的分布式存储和读取。

HBase可以将数据存储在本地文件系统，也可以存储在HDFS文件系统。在生产环境中，HBase一般运行在HDFS上，以HDFS作为基础的存储设施。HBase通过HBase Client提供的Java API来访问HBase数据库，以完成数据的写入和读取。HBase集群主要由HMaster、Region Server和ZooKeeper组成。HMaster负责集群的管理，Region Server负责具体数据的存取，ZooKeeper负责集群的状态管理和元数据的存储。

## 3.行式存储和列式存储

列式存储是相对于行式存储而言的，传统的关系型数据库Oracle、MySQL、SQL Server都是以行来存储数据的，而与大数据相关的数据库HBase、Cassandra等以列的方式存储数据。列式存储最大的好处是，由于查询数据时查询条件是通过列来定义的，因此整个数据库是自动索引的。

### 行式存储的原理和特点

行式存储以行的形式在磁盘上存储数据，因为大部分查询都是基于某个字段查询和输出结果的，所以在行式存储中会存在大量的磁盘转动寻址的操作，磁盘转动次数多，磁头移动幅度大，性能相对较慢。对于User表要查询性别为男的用户的名称，由于gender属性值跳跃地存储在磁盘上，因此，在查询的时候，磁盘需要转动多次，才能完成数据查找并将结果返回。

### 列式存储的原理和特点

列式存储以列的形式在磁盘上存储数据，因为大部分应用程序的查询都是基于某个字段查询和输出结果的，所以列式存储能很方便地找到需要的数据，磁盘转动次数少，磁头移动幅度小，性能相对快。对于User表要查询性别为男的用户的名称，由于gender属性值按顺序存储在磁盘上，因此，在查询的时候，磁盘只需要转动少量次数，就能完成数据查找并将结果返回。

## 4.HBase列式存储的数据模型

HBase根据列族来存储数据，一个列族对应物理存储上的一个HFile，列族包含多列，列族在创建表的时候被指定。

### Column Family

Column Family即列族，HBase基于列族划分数据的物理存储，一个列族可以包含任意多列。HBase在创建表的时候就必须指定列族。HBase的列族不是越多越好，官方推荐一个表的列族数量最好小于或者等于3，过多的列族不利于HBase数据的管理和索引.

HBase的列族在表结构上与关系型数据库中的列类似，但是两者是完全不同的概念。HBase基于列族来完成列式数据的存储，而关系型数据库基于行来完成数据的存储，列只是一种数据结构上的表示。

### RowKey 

Row Key的概念与关系型数据库中的主键相似，HBase使用RowKey来唯一标识某一行的数据。HBase只支持3种查询方式：基于RowKey的单行查询、基于RowKey的范围查询和全表查询。

### Region

Region的概念与关系型数据库表的横向分区相似（比如MySQL根据id的一致性Hash值将数据存储在不同数据库中）。HBase将表中的数据基于RowKey的不同范围划分到不同Region上，每个Region都负责一定范围的数据存储和访问。

HBase数据分区的过程与其数据库的Shard类似，这样即使有一个包括上百亿条数据的表，由于数据被划分到不同的Region上，每个Region都可以独立地进行写入和查询，HBase在写入或查询的时候可以基于多个Region分布式并发操作，因此访问速度也不会有太大的降低。

### TimeStamp

TimeStamp是实现HBase多版本的关键。在HBase中，使用不同TimeStamp来标识相同RowKey对应的不同版本的数据。在写入数据的时候，如果用户没有指定对应的TimeStamp, HBase会自动添加一个TimeStamp,TimeStamp与服务器时间保持一致。在HBase中，相同RowKey的数据按照TimeStamp倒序排列。默认查询的是最新的版本，用户可以指定TimeStamp的值来读取指定版本的数据。

## 5.HBase的架构组成

HBase的核心架构由HBase Client、ZooKeeper、HMaster、Region Server、HDFS组成。其中，HBase Client为用户提供API操作，ZooKeeper负责HBase集群的管理，HMaster是集群的主节点，用于管理Region Server上数据的分配和节点的运行，同时接收客户端的请求并将请求分发到Region Server上，Region Server是数据具体的存储和计算节点，HDFS是HBase的底层数据存储引擎。

### HBase Client

HBase Client包含了访问HBase的接口，还维护了对应的Cache来加速HBase的访问，比如元数据信息的Cache。

### ZooKeeper

HBase通过ZooKeeper来完成选举HMaster、监控Region Server、维护元数据集群配置等工作，主要工作职责如下。

( 1 ）选举HMaster：通过ZooKeeper来保证集群中只有1个HMaster在运行，如果HMaster异常，则会通过选举机制产生新的HMaster来提供服务。

( 2 ）监控Region Server：通过ZooKeeper来监控Region Server的状态，当Region Server有异常的时候，通过回调的形式通知HMaster有关Region Server上下线的信息。

( 3 ）维护元数据和集群配置：通过ZooKeeper存储HBase集群和数据的元数据信息，并对外提供访问接口。

### HMaster

HMaster是HBase集群的主节点，负责整个集群的管理工作，主要工作职责如下。

( 1 ）分配Region：HMaster根据用户数据的分布规则为Region Server分配Region,每个Region都对应一部分数据。

( 2 ）负载均衡：HMaster维护整个集群的负载均衡，包含数据的负载均衡（将用户的数据均衡地分布在各个Region Server上，防止Region Server数据倾斜过载）和请求的负载均衡（将用户的请求均衡地分布在各个Region Server上，防止Region Serve请求过热）。

( 3 ）维护数据：维护集群的元数据信息，发现失效的Region，并将失效的Region分配到正常的Region Server上。在Region Server失效的时候，协调对应的HLog进行任务的拆分。

### HregionServer

HregionServer是数据具体的存储和请求节点，它直接对接用户的读写请求，主要职责如下。

( 1 ）管理HMaster为其分配的Region。

( 2 ）处理来自客户端的读写请求.

( 3 ）负责与底层的HDFS交互，存储数据到HDFS.

( 4 ）负责Region变大以后的拆分。

( 5 ）负责StoreFile的合并工作。

( 6 ) Region Server和HMaster定时通信，并以租约的形式从HMaster上更新集群的信息到本地，这样在客户端查询数据的时候，Region Server就可以直接处理，而不用每次都去HMaster请求集群信息，从而避免HMaster请求过热、单点故障。

一个Region Server包含多个Region，每个Region又都有多个Store，每个Store都对应一个Column Family, Store又包含MemStore和StoreFile，这便组成了Region Server 数据存储的基本结构，它们的主要职责如下。

1.Region：每个Region都保存表中某段连续的数据。一开始每个表都只有一个Region，随着数据量不断增加，当Region大小达到一个阈值时，Region就会被Region Server水平切分成两个新的Region。当Region很多时，HMaster会将Region保存到其他Region Server上。

2.Store：一个Region由多个Store组成，每个Store都对应一个Column Family, Store包含MemStore和StoreFile。

3.MemStore：MemStore是HBase的内存数据存储，数据的写操作会先写到MemStore中，当MemStore中的数据增长到一个阈值后，Region Server会启动flashcatch 进程将MemStore中的数据写入StoreFile持久化存储，每次写入后都形成一个单独的StoreFile。当客户端检索数据时，先在MemStore中查找，如果MemStore中不存在，则会在StoreFile中继续查找。

4.StoreFile：StoreFile存储着具体的HBase数据，当StoreFile数量增长到一个阈值时，系统会自动进行合并（Minor Compaction和Major Compaction）。 在合并过程中会进行版本的合并和删除工作，形成更大的StoreFile。当一个Region中所有StoreFile的大小和数量都增长到超过一个阈值时，HMaster会把当前Region分割为两个，并分配到其他Region Server上，实现负载均衡。

5.HFile：HFile和StoreFile是同一个文件，只不过站在HDFS的角度称这个文件为HFile，站在HBase的角度就称这个文件为StoreFile。

6.HLog：HLog是一个普通的Hadoop SequenceFile ，记录着数据的操作日志，主要用来在HBase出现故障时进行日志重放、故障恢复。例如，磁盘掉电导致MemStore中的数据没有持久化存储到StoreFile，这时就可以通过HLog日志重放来恢复数据。

7.HDFS：HDFS为HBase提供底层数据存储服务，同时为HBase提供高可用的支持，HBase将HLog存储在HDFS上，当服务器发生异常宕机时，可以重放HLog来恢复数据。

### Region寻址方式（通过zookeeper .META）

第1步：Client请求ZK获取.META.所在的RegionServer的地址。

第2步：Client请求.META.所在的RegionServer获取访问数据所在的RegionServer地址，client会将.META.的相关信息cache下来，以便下一次快速访问。

第3步：Client请求数据所在的RegionServer，获取所需要的数据。

## 6.HBase的数据读写流程

### HBase的数据写入流程

![](D:\workspace\Java-Interview-Offer\images\hbase002.png)

#### 获取RegionServe

( 1 ) Region Server寻址：HBase Client从ZooKeeper上获取数据并写入Region所在的Region Server。

#### 请求写Hlog

( 2 ）写HLog：HBase Client将向Region Server发送写HLog请求，Region Server将HLog存储在HDFS上，并通过按顺序写磁盘提高效率。当Region Server出现异常时，需要使用HLog来恢复数据。

#### 请求写MemStore

( 3 ）写MemStore并返回结果：HBase Client向Region Server发送写MemStore请求。只有当写HLog和写MemStore请求都成功后才算写入请求完成，并将写请求的结果反馈给HBase Client，这时对于客户端来说，整个写流程已经完成。

#### MemStore刷盘

( 4 ) MemStore刷盘：HBase根据MemStore配置的刷盘策略定时将数据刷新到StoreFile中，完成数据持久化存储。

### HBase MemStore的刷盘逻辑

为了提高HBase的写入性能，在写请求进入MemStore后，HBase不会立即刷盘，而是要等到MemStore满足一定条件后才进行刷盘操作。触发刷盘操作的场景有如下几个。

( 1 ）全局内存限制：当所有MemStore占用的内存超过内存使用配置的最大比例时，HBase会触发刷盘操作。该配置参数为hbase.regionserver.global.memstore.upperLimit，默认为整个JVM堆内存的40%。当全局内存超限触发刷盘操作时，HBase并不会将所有MemStore都进行刷盘操作，而是通过另外一个参数hbase.regionserver.global.memstore. lowerLimit 来控制，默认是整个JVM堆内存的35%。当刷盘操作执行到所有的MemStore占整个JVM 堆内存的比率小于35%的时候将停止刷盘。由于刷盘是一个耗费资源的过程，该策略主要是为了减少大量刷盘对HBase性能带来的影响，实现在时间维度上均衡系统负载的目的。

( 2 ) MemStore达到上限：当MemStore的大小达到hbase.hregion. memstore. flush.size 的时候会触发刷盘，默认为128MB。

( 3 ) Region Server的HLog数量达到上限：HLog是为保证HBase数据故障恢复而设计的，在某个Region Server宕机恢复后会重放HLog来恢复数据，但是如果HLog太多，则会导致故障恢复的时间过长，因此，HBase会对HLog文件的最大个数做限制。在HLog文件的个数达到限制后，会强制刷盘。该参数是hase.regionserver.max.logs，默认为32个HLog文件。

( 4 ）手工触发：可以通过HBase Shell或者JavaAPI手工触发刷盘操作。

( 5 ）关闭Region Server触发：在正常关闭Region Server后会触发刷盘操作。在全部数据都刷盘完成后将不再需要使用HLog恢复数据。

( 6 ) Region Server故障恢复完成后触发：当某个Region Server出现故障时，该Region    Server的Region会迁移到其他正常运行的Region Server上，并在该Region Server上执行HLog重放以恢复数据。在数据迁移和HLog日志重放完成后会触发刷盘操作，只有刷盘完成后，该Region Server才会再次对外提供服务。

### HBase的数据读取流程

( 1 ) Region Server寻址：HBase Client请求ZooKeeper获取元数据表所在的Region Server的地址。

( 2) Region寻址：HBase Client请求Region Server获取需要访问的数据所在Region的地址，同时，HBase Client会将元数据表的相关信息缓存下来，以便下一次快速访问。

( 3 ）数据读取HBaseli请求数据所在的RegionSeve取所需要的数据Reg首先在MeStore查找若命中返回；如果MemStore中找不到，则通过oomFiter判断数据是否存在；如果存在，贝！StoreFil中扫描并将结果返回客户端

### HBase的数据删除

HBase的数据删除操作并不会立即将数据从磁盘上删除，因为HBase的数据通常被保存在HDFS中，而HDFS只允许新增或者追加数据文件，所以删除操作主要对要被删除的数据进行标记。当执行删除操作时，HBase新插入一条相同的Key-Value数据，但是keyType=Delete，这便意味着数据被删除了，直到发生Major_compaction操作，数据才会真正地被从磁盘上删除。

HBase这种基于标记删除的方式是按顺序写磁盘的，因此很容易实现海量数据的快速删除，有效避免了在海量数据中查找数据、执行删除及重建索引等复杂的流程。

### HBase的数据更新

HBase的数据更新指在原有的数据基础上新加一条数据并更新该数据的版本号，用户查询的时候默认查询最新的一条数据，也可以指定版本号查询数据。更新操作也是按顺序写磁盘的，避免了海量数据的查询、更新和重建索引的流程，因此，HBase的更新操作也很快。

## 7.HBase架构上的特点

（1）强一致读写

他不是zk那种最终一致性，是强一致的，你写成功了立马就可以读。这个功能是极为实用的，他是依靠的分布式存储才做到的，zk那种是属于主从同步，你读follower机器是可能读到不一致数据的.

（2）高可用 

每台机器上部署一个RegionServer，管理一大堆的region数据分片，RegionServer都是支持高可用的，一个RegionServer挂掉不会导致数据丢失，他自动可以由别的机器接管他的工作运行下去.

（3）支持mapreduce/spark这种分布式计算引擎

对hbase里的数据进行分布式计算，可以从hbase里分布式抽数据去计算，也可以把计算后的结果写入hbase分布式存储.

（4）Java API/thrift API/REST API的支持 

当然支持Java API了，咱们的Java业务系统经常会有海量数据NoSQL存储的需求，此时就可以基于Java API来操作hbase里的数据了. 

（5）支持协处理器，块缓存和布隆过滤器，可以用于优化查询性能 

（6）hbase现在最新版本都是支持web界面的方式来对hbase集群进行运维管理的

## 8.概念

（1）分布式架构 

hbase定位是分布式nosql数据库，把自己的nosql数据库的功能是通过多台机器来实现的，有多个RegionServer，分布式管理数据，分布式执行你的各种nosql数据库的操作.

（2）分布式数据存储和自动数据分片 

这个功能是极为强大的，比如你搞一个hbase里的表，然后在表里搞很多很多的数据，这个表会分为很多的region，每个region里是一个数据分片，然后这些region数据分片就会分散在多台机器上

假设你的表里的数据太多了，此时region会自动进行分裂，分裂成更多的region，自动分散在更多的机器上，对我们使用是极为方便的.

（3）集成hdfs作为分布式文件存储系统

## 9.列式存储

列方式所带来的重要好处之一就是，由于查询中的选择规则是通过列来定义的，因此整个数据库是自动索引化的。

这里的列式存储其实说的是列族存储，Hbase是根据列族来存储数据的。列族下面可以有非常多的列，列族在创建表的时候就必须指定。为了加深对Hbase列族的理解，下面是一个简单的关系型数据库的表和Hbase数据库的表：

![](D:\workspace\Java-Interview-Offer\images\hbase001.png)