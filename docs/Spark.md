## 1.Spark

Apache Spark是通用的分布式大数据计算引擎。Spark是UC Berkeley AMPLab （美国加州大学伯克利分校的AMP实验室）开源的通用并行框架。Spark拥有Hadoop MapReduce所具有的优点，但不同于Hadoop MapReduce的是，Hadoop每次经过Job执行的中间结果都存储到HDFS等磁盘上，而Spark的Job中间输出结果可以保存在内存中，而不再需要读写HDFS。因为内存的读写速度与磁盘的读写速度不在一个数量级上，所以Spark利用内存中的数据能更快速地完成数据的处理。

Spark启用了弹性分布式数据集（Resilient Distributed Dataset, RDD ），除了能够提高交互式查询效率，还可以优化迭代器的工作负载。由于弹性分布式数据集的存在，使得数据挖掘与机器学习等需要迭代的MapReduce的算法更容易实现。

## 2.Spark的原理

Spark是一个用来快速实现大规模数据计算的通用分布式计算引擎。Spark基于Scala实现，扩展了Hadoop MapReduce的计算模型，在交互式查询和分布式数据处理上更加灵活；同时，由于Spark在内部使用了弹性分布式数据集（弹性分布式数据集存储在内存中），所有Spark的计算大部分都在内存中完成，因此其运行效率很高。此外，由于Spark经过了数据模型的优化，即使在磁盘上进行分布式计算，Spark的性能仍然比Hadoop MapReduce高。

## 3.Spark的特点

### 计算速度快

Spark将每个任务都构造成一个DAG(Directed Acyclic Graph，有向无环图）来执行其内部计算过程基于弹性分布式数据集在内存中对数据进行迭代计算，因此其运行效率很高。官方数据表明，如过计算的数据从磁盘上读取，则Spark的速度是Hadoop MapReduce的10倍以上；如果计算的数据从内存中读取，则Spark的计算速度是Hadoop MapReduce的100倍以上。

### 易于使用

Spark提供了80多个高级运算操作，支持丰富的算子，开发人员只需要按照其封装好的API实现即可，不需要关心Spark的底层架构。同时，Spark支持多种语言开发，包括Java、Scala、Python。

### 通用大数据框架

Spark提供了多种类型的开发库，包括Spark Core、Spark SQL（即时查询）、Spark Streaming （实时流处理）、Spark MLlib、GraphX（图计算），使得开发人员可以在同一个应用程序中无缝组合使用这些库，而不用像传统的大数据方案那样将离线任务放在Hadoop MapReduce上运行，将实时流计算任务放在Storm上运行，并维护多个平台。Spark提供了从实时流计算、MapReduce离线计算、SQL计算、机器学习到图计算的一站式整体解决方案。

### 支持多种资源管理器

Spark支持单机、Standalone、Hadoop YARN、Apache Mesos等多种资源管理器，用户可以根据现有的大数据平台灵活地选择运行模式。

### Spark生态圈丰富

Spark生态圈以Spark Core为核心，支持从HDFS、S3、HBase等多种持久化层读取数据。同时，Spark支持以Hadoop YARN、Apache Mesos和Standalone为资源管理器调度Job，完成Spark应用程序的计算。Spark应用程序可以基于不同的组件实现，如Spark Shell、Spark Submit、Spark Streaming、Spark SQL、BlinkDB（权衡查询）、MLlib/MLbase（机器学习）、GraphX和SparkR（数学计算）等。Spark生态圈已经从大数据计算和数据挖掘扩展到机器学习、自然语言处理和语音识别等领域。

## 4.Spark的模块

Spark基于Spark Core建立了Spark SQL、Spark Streaming、MLlib、GraphX、SparkR核心组件，基于不同组件可以实现不同的计算任务，这些计算任务的运行模式有：本地模式、独立模式、Mesos模式、YARN模式。Spark任务的计算可以从HDFS、S3、Hypertable、HBase或Cassandra等多种数据源中存取数据。

![](D:\workspace\Java-Interview-Offer\images\spark001.png)

### Spark Core

Spark的核心功能实现包括基础设施、存储系统、调度系统和计算引擎。

( 1 ）基础设施：Spark中有很多基础设施，这些基础设施被Spark中的各种组件广泛使用，包括SparkConf（配置信息）、SparkContext( Spark上下文）、Spark RPC （远程过程调用）、ListenerBus（事件总线）、MetricsSystem（度量系统）、SparkEnv（环境变量）等。

1.SparkConf: SparkConf用于定义Spark应用程序的配置信息。

2.SparkContext: SparkContext是Spark应用程序的入口，Spark应用程序的提交与执行离不开SparkContxt。SparkContex隐藏了网络通信、分布式部署、消息通信、存储体系、计算引擎、度量系统、文件服务等内容，开发人员只需要使用SparkContext提供的API完成功能开发即可。

3.Spark RPC: Spark组件之间的网络通信依赖Spark RPC框架。Spark RPC基于Netty实现，使用中分同步和异步两种方式。

4.ListenerBus: ListenerBus即事件总线，主要用于SparkContext内部各组件之间的事件交互。ListenerBus属于监听者模式，采用异步调用的方式实现。

5.MetricsSystem: MetricsSystem为度量系统，用于整个Spark集群中各个组件运行状态的监控。度量系统由多种度量源和多种度量输出组成。

6.SparkEnv: SparkEnv为Spark的执行环境，SparkEnv内部封装了RpcEnv( RPC环境）、序列化管理器、BroadcastManager（广播管理器）、MapOutputTracker( Map任务输出跟踪器）、存储系统、MetricsSystem（度量系统）、OutputCommitCoordinator （输出提交协调器）等Spark程序运行所需要的基础环境组件。

( 2 ）存储系统：Spark存储系统用于管理Spark运行过程中依赖的数据的存储方式和存储位置。Spark存储系统首先考虑在各节点的内存中存储数据，当内存不足时会将数据存储到磁盘上，这种内存优先的存储策略使得Spark的计算性能无论在实时流计算还是在批量计算的场景下都表现很好。Spark的内存存储空间和执行存储空间之间的边界可以灵活控制。

( 3）调度系统：Spark调度系统主要由DAGScheduler和TaskScheduler组成。DAG Scheduler负责创建Job、将DAG中的RDD划分到不同Stage中、为Stage创建对应的Task、批量提交Task等。TaskScheduler负责按照FIFO( First Input First Output，先进先出）或者FAIR（公平调度）等调度算法对Task进行批量调度。

( 4 ）计算引擎：计算引擎由内存管理器、任务管理器、Task、Shuffle管理器等组成。

### Spark SQL

Spark SQL提供基于SQL的数据处理方式，使得分布式数据的处理变得更加简单。此外，Spark提供了对Hive SQL的支持。

### Spark Streaming

Spark Streaming提供流计算能力，支持Kafka、Flume、Kinesis和TCP等多种流式数据源。此外，Spark Streaming提供了基于时间窗口的批量流操作，用于对一定时间周期内的流数据执行批量处理。

### GraphX 

GraphX用于分布式图计算。通过Pregel提供的API可以快速解决图计算中的常见问题。

### Spark MLlib

Spark MLlib为Spark的机器学习库。Spark MLlib提供了统计、分类、回归等多种机器学习算法的实现。其简单易用的API接口降低了机器学习的门槛。

### SparkR 

SparkR是一个R语言包，提供了轻量级的基于R语言使用Spark的方式。SparkR实现了分布式的数据框，支持类似查询、过滤及聚合的操作（类似R语言中的数据框包dplyr），使得基于R语言能够更方便地处理大规模的数据集。同时，SparkR支持基于Spark MLlib进行机器学习。

## 5.Spark的运行模式

Spark的运行模式主要包括Local模式、Standalone模式、On YARN、On Mesos和运行在AWS等公有云平台上。

| 运行环境   | 运行模式 |                                                              |
| ---------- | -------- | ------------------------------------------------------------ |
| Local      | 本地模式 | 常用于本地模式，又分为Local单线程和Local-Cluster多线程两种方式 |
| Standalone | 集群模式 | 独立模式，在Spark自己的资源调度管理框架上运行，该框架采用Master/Slave架构设计 |
| On YARN    | 集群模式 | 在YARN资源管理器框架上运行，由YARN负责资源管理，Spark负责任务调度和计算 |
| On Mesos   | 集群模式 | 在Mesos资源管理器框架上运行，由Mesos负责资源管理，Spark负责任务调度和计算 |
| On Cloud   | 集群模式 | 运行在AWS、阿里云、华为云等环境                              |

### 6.Spark的集群架构

![](D:\workspace\Java-Interview-Offer\images\spark002.png)

Spark的集群架构主要由Cluster Manager （管理器）、Worker（工作节点）、Executor（执行器）、Driver（驱动器）、Applicaton（应用程序）5部分组成。

( 1) Cluster Manager: Spark集群管理器，主要用于整个集群资源的管理和分配。跟据部署模式的不同，可以分为Local、Standalone、YARN、Mesos和AWS。

( 2 ) Worker: Spark的工作节点，用于执行提交的任务。Worker的工作职责如下。

1.通过注册机制向Cluster Manager汇报自身的CPU和内存等资源使用信息。

2.在Master的指示下创建并启动Executor,Executor是真正的计算单元。

3.将资源和任务进一步分配给Executor并运行。

4.同步资源信息和Executor状态信息给Cluster Manager。

( 3 ) Executor：真正执行计算任务的组件，是某个Application运行在Worker上的一个进程。该进程负责Task的运行并且将运行的结果数据保存到内存或磁盘上。Task是运行在Executor上的任务单元，Spark应用程序最终被划分为经过优化的多个Task的集合。

( 4 ) Driver: Application的驱动程序，可以理解为驱动程序运行中的main（）函数，Driver在运行过程中会创建SparkContext。Application通过Driver与Cluster Manager和Executor进行通信。Driver可以运行在Application上，也可以由Application提交给Cluster Manager，再由Cluster Manager安排Worker运行。Driver的主要职责如下。

1.运行应用程序的main（）函数

2.创建SparkContext。

3.划分RDD并生成DAG。

4.构建Job并将每个Job都拆分为多个Task，这些Task的集合被称为Stage。各个Stage相互独立，由于Stage由多个Task构成，因此也被称为Task Set。Job是由多个Task构建的并行计算任务，具体为Spark中的Action操作（例如collect、save等）。

5.与Spark中的其他组件进行资源协调。

6.生成并发送Task到Executor。

( 5 ) Application：基于Spark API编写的应用程序，其中包括实现Driver功能的代码和在集群中多个节点上运行的Executor代码。Application通过Spark API创建RDD、对RDD进行转换、创建DAG、通过Driver将Application注册到Cluster Manager。

## 7.SPARK运行流程

![](D:\workspace\Java-Interview-Offer\images\spark003.png)

1.构建Spark Application的运行环境，启动SparkContext

2.SparkContext向资源管理器（可以是Standalone，Mesos，Yarn）申请运行Executor资源，并启动StandaloneExecutorbackend，3.Executor向SparkContext申请Task

4.SparkContext将应用程序分发给Executor

5.SparkContext构建成DAG图，将DAG图分解成Stage、将Taskset发送给Task Scheduler，最后由Task Scheduler将Task发送给Executor运行

6.Task在Executor上运行，运行完释放所有资源

### 7.Spark的运行流程

Spark的数据计算主要通过RDD的迭代完成，RDD是弹性分布式数据集，可以看作是对各种数据计算模型的统一抽象。在RDD的迭代计算过程中，其数据被分为多个分区并行计算，分区数量取决于应用程序设定的Partition数量，每个分区的数据都只会在一个Task上计算。所有分区可以在多个机器节点的Executor上并行执行。

( 1 ）创建RDD对象，计算RDD之间的依赖关系，并将RDD生成一个DAG。

( 2 ) DAGScheduler将DAG划分为多个Stage，并将Stage对应的Task Set提交到集群管理中心。划分Stage的一个主要依据是当前计算因子的输入是否确定。如果确定，则将其分到同一个Stage中，避免多个Stage之间传递消息产生的系统资源开销。

( 3 ) TaskScheduler通过集群管理中心为每个Task都申请系统资源，并将Task提交到Worker。

( 4) Worker的Executor执行具体的Task。

### 8.RDD的介绍

RDD是Spark中最基本的数据抽象，代表一个不可变、可分区、元素可并行计算的集合。RDD具有自动容错、位置感知性调度和可伸缩等特点。RDD允许用户在执行多个查询时显式地将数据集缓存在内存中，后续查询能够重用该数据集，这极大地提升了查询效率。

### 10.RDD的核心结构及概念

( 1 ) Partition: RDD内部的数据集在逻辑上和物理上都被划分为多个分区（Partition ) 以提高运行的效率，分区数量决定了计算的并行度，每一个分区内的数据都在一个单独的任务中被执行，如果在计算过程中没有指定分区数，那么Spark会采用默认分区数量。默认分区数量为程序运行分配到的CPU核数。

( 2) Partitioner: Partitioner是RDD的分区函数。分区函数不但决定了RDD本身的分区数量，也决定了其父RDD Shuffle输出时的分区数量。Spark实现了基于Hash( HashPartitioner ）和基于范围（RangePartitioner）的两种分区函数。注意：只有对于Key-Value的RDD才会有Partitioner，而非Key-Value的RDD的Partitioner值是None。

( 3) RDD的依赖关系：RDD的每次转换都会生成一个新的RDD，因此RDD之间会有前后依赖关系。当在计算过程中出现异常情况导致部分分区数据丢失时，Spark可以通过依赖关系从父RDD中重新计算丢失的分区数据，而不需要对RDD上的所有分区全部重新计算。RDD的依赖分为窄依赖和宽依赖。

窄依赖：如果父RDD的每个分区最多只能被子RDD的一个分区使用，则称之为窄依赖。

宽依赖：如果父RDD的每个分区都可以被子RDD的多个分区使用，则称之为宽依赖。

窄依赖的每个子RDD的Partitioner的生成操作都是可以并行的，而宽依赖则需要所有父Partitioner Shuffle结果完成后再被执行。

 4 ) Stage: Stage是由一组RDD组成的可进行优化的执行计划。如果RDD的依赖关系为窄依赖，则可放在同一个Stage中运行；若RDD的依赖关系为宽依赖，则要划分到不同Stage中。这样，当Spark执行作业时，会按照Stage划分不同的RDD，生成一个完整的最优的执行计划，使每个Stage内的RDD都尽可能在各个节点上并行地被执行。

( 5 ) PreferredLocation: PreferredLocation是一个用于存储每个Partitioner的优先位置的列表。对于每个HDFS文件来说，这个列表保存的是每个Partitioner所在的块的位置，也就是该HDFS文件的“划分点”。

( 6) CheckPoint: CheckPoint是Spark提供的一种基于快照的缓存机制。当需要计算的RDD过多时，为了避免任务执行失败后重新计算之前的RDD，可以对RDD做快照( CheckPoint）处理，检查RDD是否被计算，并将结果持久化到磁盘或HDFS上。此外，Spark提供另一种缓存机制Cache,Cache缓存数据由Executor管理，当Executor消失时，Cache缓存的数据将被清除，而CheckPoint将数据保存到永久性磁盘或HDFS，当计算出现运行错误时，Job可以从CheckPoint点继续计算。

## 11.利用外部数据集生成ROD

Spark可以从Hadoop或者其他外部存储系统创建RDD，包括本地文件系统、HDFS、Cassandra、 HBase、S3等。Spark RDD支持多种文件格式，包括文本文件，SequenceFiles、JSON文件和任何其他Hadoop InputFormat。

Spark加载数据的注意事项包括以下几个方面。

1.如果访问的是本地文件路径，则必须可以在工作节点上以相同路径访问该文件。一般做法是将数据文件远程复制到所有工作节点对应的路径下或使用共享文件系统实现。

2.除了支持基于文件名的方式加载文件，Spark还支持基于目录、压缩文件和通配符的方式加载文件。例如，可以使用textFile（”/my/directory”）表示加载“/my/directory"路径下的所有文件；使用textFile("/my/directory/ *.txt＂）表示加载“/my/directory”路径下所有以.txt为后缀名的文件；使用textFile（”/my/directory/ *.gz”）表示加载并解压”/my/directory”目录下所有以.gz为后缀名的文件。

3.Spark加载文件时可以设置分区数。Spark在默认情况下为每个文件块都创建一个分区（HDFS中默认文件块大小为128MB），也可以通过传递更大值来设置更多分区。需要注意的是，分区数不能小于文件块的数量。

4.除了加载文本文件，Spark的Java API还支持其他多种数据格式。

1）wholeTextFile: JavaSparkContext.wholeTextFiles允许客户端程序读取包含多个小文本文件的目录，并将每个文件都以＜文件名，内容>的键值对返回。该方法与textFile不同，textFile是将每个文件中的每行都作为一条记录返回的。

2）SequenceFiles：加载SequenceFiles需要使用SparkContext的SequenceFiles[Key，Value]方法实现，其中，Key和Value是文件中键和值的类型。这些键值对对应的是Hadoop的Writable接口的子类，比如IntWritable和Text。

3）Hadoop InputFormat：对于Hadoop InputFormat类型的数据，可以使用JavaSparkContext.hadoopRDD方法或JavaSparkContext.newAPIHadoopRDD加载并生成RDD。

4）文件保存和序列化：JavaRDD.saveAsObjectFile方法和JavaSparkContext.objectFile 方法采用Java默认序列化的方式将数据序列化并保存到RDD。同时，用户可以在保存数据的时候，使用其他更高效的序列化方法（例如Avro、Kyro等）。

## 12.RDD的转换和操作

RDD支持两种类型的操作：转换（Transformation）和操作（Action）。转换指从现有RDD创建新RDD，操作指在RDD上运行计算并将计算结果返回驱动程序。例如，map是一个转换：它将RDD的所有元素都调用map函数进行转换处理，返回一个表示转换结果的新RDD;reduce是一个操作：它将RDD的所有元素都调用reduce函数进行聚合操作，将最终计算结果返回驱动程序。表示聚合处理的函数还有reduceByKey、reduceBy等。

Spark中的所有转换（Transformation ）都是懒加载的，即不会立即执行转换操作，而是先记录RDD之间的转换关系，仅当触发操作（Action）时才会执行RDD的转换操作，并将计算结果返回驱动程序。这种懒加载的设计使Spark能够更加高效地运行。

## 13.RDD持久化的概念、级别和原则

Spark可以跨节点在内存中持久化RDD。当持久化RDD时，每个节点都会在内存中缓存计算后的分区数据，当其他操作需要使用该RDD时，可以直接重用该缓存数据，这使得之后的RDD计算速度更快（通常超过10倍）。缓存是迭代计算和交互式计算的关键。

应用程序可以使用persist（）或cache（）标记要缓存的RDD，当调用操作（Action）执行计算时，计算结果将被缓存在节点的内存中。Spark缓存具有容错性，如果RDD的某个分区丢失，则该RDD将被自动重新计算。

每个持久化RDD都可以使用不同存储级别进行存储，Spark允许将数据集存储在磁盘上或内存中。Spark将需要缓存的数据序列化为Java对象（序列化可以节省磁盘或内存空间），然后跨节点复制到其他节点上，以便其他节点重用该数据。Spark中缓存持久化级别是通过StorageLevel来设置的。具体代码如下。

lineLengths. persist ( StorageLevel. MEMORY_ONLY() ) ; 

( 1 ) Spark持久化的级别说明.

1.MEMORY_ONLY：使用未经过序列化的Java对象在内存中存储RDD。当内存不够时，将不会进行持久化；当下次需要该RDD时，再从源头处重新计算。该策略是默认的持久化策略，当使用cache（）时，使用的是该持久化策略。

2.MEMORY_AND_DISK：使用未经过序列化的Java对象存储RDD，优先尝试将RDD保存在内存中。如果内存不够，则会将RDD写入磁盘文件；当下次需要该RDD时，从持久化的磁盘文件中读取该RDD即可。

3.MEMORY_ONLY_SER: MEMORY_ONLY_SER的含义与MEMORY_ONLY类似，唯一区别是MEMORY_ONLY_SER会将RDD中的数据进行序列化。在序列化过程中，RDD的每个Partition都将会被序列化成一个字节数组，这种方式更加节省内存，从而避免持久化的RDD占用过多内存导致JVM频繁GC。

4.MEMORY_AND_DISK_SER: MEMORY_AND_DISK_SER的含义与MEMORY_AND_DISK类似.唯一区别是MEMORY_AND_DISK_SER会将RDD中的数据进行序列化。在序列化过程中，RDD的每个Partition都会被序列化成一个字节数组。这种方式更加节省内存，从而避免持久化的RDD占用过多内存导致频繁GC。

5.DISK_ONLY：使用未序列化的Java对象将RDD全部写入磁盘文件。

6.MEMORY_ONLY_2和MEMORY_AND_DISK_2 ：对于上述任意一种持久化策略，如果加上后缀_2，代表的是将每个持久化的数据都复制一份副本，并将副本保存到其他节点上。这种基于副本的持久化机制主要用于容错。假如某个节点挂掉，节点的内存或磁盘中的持久化数据丢失了，那么后续对RDD计算时还可以使用该数据在其他节点上的副本。如果没有副本，则只能将这些数据从头重新计算一遍。

7.OFF_HEAP: OFF_HEAP与MEMORY_ONLY_SER类似，但OFF_HEAP将数据存储在堆外内存中。该参数需要Spark启用堆外内存。

( 2 ）持久化的原则.

Spark提供了丰富的存储级别，旨在通过不同存储级别的设置实现内存和CPU的最佳使用，具体开发中该如何选择持久化方案呢？

1.如果RDD在默认存储级别（MEMORY_ONLY ）下运行良好，则建议使用MEMORY_ONLY 。该级别是CPU效率最高的类型，基于CPU快速计算可以使RDD上的操作可能快地运行。

2.如果系统显示内存使用过高，则尝试使用MEMORY_ONLY_SER，并选择更快速的序列化库，以加快序列化时间和节省对象的存储空间。

3.如果要快速恢复故障，则建议使用副本存储级别。其他存储级别需要通过重新计算丢失的数据来保障缓存的完整性，而副本存储级别可以在其缓存对应的副本节点上直接执行任务，不用等待重新计算丢失的分区数据。

( 3 ）删除持久化缓存。

Spark会自动监视每个节点上的缓存使用情况，并以LRU方式删除旧的数据分区。如果想手动删除RDD，则可通过RDD.unpersist（）方法完成。

## 14.Spark函数的定义

Spark API依赖driver程序中的传递函数完成在集群上执行RDD转换并完成数据计算。在JavaAPI中，函数所在的类需要实现org.apache. spark.api .java.function包中的接口。Spark提供了下面两种创建函数的方式。

1.lambda实现：使用lambda表达式简明地定义Function实现。如下代码在map中使用lambda表达式简单地求数据的长度。

JavaRDD<Integer> lineLengths = lines.map(s-> s.length()); 

2.自定义Function类：用户可以自定义类并实现Function接口，可以是匿名内部类，也可以是命名接口；然后将类的实例传递给Spark。

## 16.RDD的常用转换

RDD的转换（Transformation ）主要用于RDD的转化和数据处理，常用的转换有map（）、filter（）、flatMap（）等，具体如下。

( 1 ) map(func）：返回每个元素经过func方法处理后生成的新元素所组成的数据集合.

( 2 ) filter(func）：返回一个通过func方法筛选的元素所组成的数据集合，返回true,即代表通过筛选。

( 3) flatMap(func）：与map操作类似，但是每一个输入项都能被映射为0个或多个输出项。

( 4 ) mapPartitions(func）：与map操作类似，但是mapPartitions单独运行在RDD的每一个分区上。

( 5) mapPartitionsWithIndex(func）：与mapPartitions操作相似，但是该操作提供一个整数值代表分区的下标。

( 6) union(otherDataset）：对两个数据集执行合并操作。

( 7 ) intersection( otherDataset）：对两个数据集执行求交集运算。

( 8) distinct([numTasks］）：对数据集执行去重操作。

( 9) groupByKey([numTasks］）：返回一个根据Key分组的数据集。

( 10 ) reduceByKey(func,[numTasks］）：返回一个在不同Key上进行了聚合Value的新的＜Key，Value＞数据集，聚合方式由func方法指定。

( 11 ) sortByKey([ascending],[numTasks］）：返回排序后的键值对.

( 12 ) join(otherDataset，[numTasks］）：按照Key将源数据集合与另一数据集合进行join操作。<Key,Value1 ＞和＜Key, Value2 ＞的join结果就是＜Key,<Value1,Value2>>。

( 13) repatition(numPartitions）：通过修改Partition的个数对RDD中的数据重新进行分区平衡。

## 17.RDD的常用操作

RDD的操作（Action）主要用于对分布式环境中RDD的转化操作结果的统一处理，比如结果收集、数据保存等。常用的操作有reduce、collect、saveAsTextFile等，具体如下。

( 1 ) reduce(func）：使用一个func方法来聚合一个数据集。

( 2) collect（）：以数组的形式返回在驱动器上的数据集的所有元素。

( 3 ) countByKey（）：接照数据集中的Key进行分组，计算各个Key对应的个数。

( 4) foreach(func）：在数据集的每个元素上都运行func方法。

( 5 ) first（）：返回数据集行的第一个元素。

( 6) take(n）：以数组的形式返回数据集上的前n个元素。

( 7) takeOrdered（n，[ordering］）：返回RDD排序后的前n个元素。排序方式要么使用原生的排序方式，要么使用自定义的比较器排序。

( 8 ) saveAsTextFile(path）：将数据集中的元素写成一个或多个文本文件。参数就是文件路径，可以写在本地文件系统、HDFS，或者其他Hadoop支持的文件系统。

( 9 ) saveAsSequenceFile(path）：将RDD中的元素写成Hadoop SequenceFile 保存到本地文件系统、HDFS，或者其他Hadoop支持的文件系统，并且RDD中可用的键值对必须实现Hadoop的Writable接口。

( 10) saveAsObjectFile(path）：使用Java序列化方式将RDD中的元素进行序列化并存储到文件系统中，可以使用SparkContext.objectFile（）方法来加载该数据。

## 18.Spark广播变量的概念和使用

广播变量允许应用程序在每台服务器上都保留一个只读变副本，以便每个节点都能快速获取副本数据集进行计算。Spark使用有效的广播算法来分发广播变量，以降低通信成本。

Spark广播变量自动广播每个阶段中任务所需要的公共数据，以这种方式广播的数据以序列化形式缓存在系统中，当运行任务期间需要这些公用数据时将其进行反序列化操作即可。

在使用过程中，Spark应用程序可以通过调用SparkContext.broadcast(v）创建广播变量，并通过调用value方法访问其值。

Broadcast<int[]> broadcastVar = sc.broadcast(new int[] { 1, 2, 3}); 

broadcastVar.value(); 

## 19.Spark Streaming的介绍

Spark Streaming基于Spark API的流式计算扩展，它实现了一个高吞吐量、高容错的流式计算引擎。Spark Streaming从多种数据源获取数据（如Kafka、Flume、Kinesis、TCP等），在获取数据后使用高级函数（如map、reduce、join、window）组成的计算逻辑单元进行数据处理，最终将处理后的数据实时推送到消息服务、文件系统、数据库等。除了基本的流式计算，应用程序还可以在数据流上应用Spark的机器学习和图形处理算法。

Spark Streaming接收实时数据流并将数据分成很小的Batch，然后将这些Batch交给Spark Engine以微批量的形式进行实时处理，生成最终结果流。

在API层面，Spark Streaming将实时数据流封装为DStream( Discretized Stream ）的高级抽象以表示连续的数据流。DStream可以从Kafka、Flime和Kinesis等实时数据流建建，也可以通过在其他DStream上应用高级操作来创建。在内部，DStream表示为一系列RDD。Spark Streaming内部流计算的过程即Spark RDD的转化过程。

## 20.DStream和RDD的关系

DStream是Spark Streaming提供的基本抽象，它表示连续的数据流，可以是从其他数据源接收的输入数据流，也可以是通过转换输入流生成的。在Spark内部，DStream由一系列连续的RDD表示，这是Spark对不可变分布式数据集的抽象。应用于DStream的任何操作都转换为底层RDD上的操作。这些底层RDD转换由Spark计算引擎完成。DStream操作隐藏了大部分细节，并为开发人员提供了更高级别的API以方便使用。

## 21.Spark Streaming的数据源

Spark Streaming提供如下两种内置流式数据的数据源。

基本来源：StreamingContext API中直接提供的源，如文件系统和Socket连接等。

高级资源：Kafka、Flume、Kinesis等数据源，可通过扩展实现其他依赖。

如果要在应用程序中并行接收多个数据流，则可以通过定义多个Receiver来实现，这些接收器将同时接收多个数据源上的数据。需要注意的是，分配给Spark Streaming应用程序的核心数必须大于接收器的数量，因为系统在接收数据的同时，需要有足够的线程资源来处理数据。

( 1 ）常用的基本数据源。

1.Socket: Spark启动一个常驻内存的线程来实时监听Socket上的数据变化。具体通过如下方法创建DStream。

StreamingContext.socketTextStream（"ip”，port)；

2.HDFS文件流：从与HDFS API兼容的任何文件系统（HDFS、S3、NFS）上读取数据。具体通过如下方法。

StreamingContext.fileStream [KeyClass, ValueClass. InputFormatClass] 

3.简单文件流：从简单的文件目录上读取数据。

streamingContext.textFileStream(dataDirectory)；

( 2 ）常用的高级数据源。

Flume：利用Flume Spark Streaming从Flume中获取数据。

Kafka：利用Kafka Spark Streaming从Kafka获取数据。

Kinesis：利用Kinesis Spark Streaming从Kinesis获取数据。

## 24.Spark Streaming的性能优化

Spark Streaming的性能优化主要包括优化运行时间和优化内存两方面。

1 ）优化运行时间.

( 1 ）增加并行度：确保使用整个集群的资源，而不是把任务集中在几个特定的节点上。对于包含Shuffle的操作，增加其并行度以确保更充分地使用集群资源。

( 2 ）减少数据序列化、反序列化的负担：Spark Streaming默认将接收的数据序列化后存储，以减少内存的使用。但是序列化和反序列化需要更多CPU时间，因此更加高效的序列化方式（Kryo）和自定义的序列化接口可以更高效地使用CPU。

( 3 ）设置合理的批处理间隔：在Spark Streaming中，Job之间存在依赖关系，后面的Job必须确保前面的Job执行结束后才能提交。若前面的Job执行的时间超出了批处理间隔，那么后面的Job将无法按时提交，这样就会进一步延迟接下来的Job执行，造成后续Job的阻塞。因此需要设置一个合理的批处理间隔以确保Job能够在这个批处理间隔内结束处理计算。

( 4 )减少因任务提交和分发带来的负担：通常情况下，Akka框架能够高效地确保任务及时分发，但是当批处理问隔非常小（500ms）时，提交和分发任务的延迟就变得不可接受。使用Standalone和Coarse-grained Mesos模式通常会比使用Fine-grained Mesos模式有更小的延迟。

2 ）优化内存使用。

( 1 ）控制批处理间隔内的数据量：Spark Streaming会把批处理间隔内接收的所有数据存放在Spark内部的可用内存区域中，因此必须确保当前节点的Spark的可用内存至少能容纳批处理间隔内的所有数据，否则必须增加新的内存资源以提高集群的处理能力。

( 2 ）及时清理不再使用的数据：Spark Streaming会将接收的数据全部存储到内存区域中，因此对于处理过后就不再需要的数据应及时清理，以确保Spark Streaming有更多的可用内存空间。通过设置合理的spark.cleaner.ttl时长来及时清理超时的无用内存数据，这个参数需要小心设置以免后续操作中所需要的数据被当作超时数据清理掉。

( 3 ）调整GC策略：GC会影响Job的正常运行，延长Job的执行时间，引起一系列不可预料的问题。观察GC的运行情况，采用不同GC策略以进一步减小内存回收对Job运行的影响。

## 25.Spark SQL简介

Spark SQL是用于结构化数据处理的Spark模块。它提供了两个编程抽象，分别叫作DataFrame和DataSet，它们均用于分布式SQL查询引擎。在外部，Spark SQL会将无论是DataFrame还是DataSet定义的数据都抽象为结构化数据，这样使得开发人员可以像使用简单的SQL语句一样在Spark上轻松完成复杂的大数据交互操作。在内部，Spark SQL将SQL执行操作转换成RDD，然后提交到集群执行。

## 26.Spark SQL查询语句

Spark SQL最简单的用法是直接执行SQL查询语句，可以使用最基本的SQL语法，也可以选择HiveSQL语法。Spark SQL可以从已有的Hive中读取数据，如果用其他编程语言运行SQL，则Spark SQL将以DataFrame的形式返回结果。

## 27.DataFrame

DataFrame是一种分布式数据集合，每一条数据都由多个字段组成。从概念上来说，它与关系型数据库的表或者R语言和Python中的DataFrame等价，只不过在底层，DataFrame采取了更多优化。

DataFrame可以从很多数据源加载数据，如结构化数据文件、Hive表、关系型数据库、NoSQL数据库或者已有的RDD。

## 28.Dataset 

DataSet的目的是把RDD的优势（强类型，可以使用lambda表达式函数处理数据）和Spark SQL的优化执行引擎的优势结合到一起。DataSet由Java对象构建，在DataSet上可以使用各种Transformation算子（如map、flatMap、filter等）来完成数据的交互式计算。

## 29.Spark SOL视圈操作

SparkSession上的SQL函数可以通过编程的方式运行SQL语句，并将结果作为DataSet<Row＞返回。

应用程序可以使用如下方式将DataFrame转化为一个视图，并基于视图进行查询操作。从使用层面上，Spark SQL的视图可以理解为与数据库视图类似，只不过数据库视图是对数据库表的抽象，而Spark SQL的视图是对DataFrame的抽象。而Spark SQL的视图是对DataFrame的抽象。Spark SQL的视图可分为临时视图和全局临时视图。

( 1 ）临时视图：Spark SQL的临时视图是会话范围的，如果创建它的SparkSession终止，则它将消失。

( 2 ）全局临时视图：如果希望拥有一个在所有会话之间共享的临时视图并保持活动状态，直到Spark应用程序终止，则可以通过创建全局临时视图的方式来实现。全局临时视图与系统保留的数据库global_temp绑定，应用程序必须使用限定名称来引用它。

## 30.Spark Structured Streaming简介

Spark Structured Streaming ( Spark结构化流）是基于Spark SQL引擎扩展的流处理引擎，使得用户可以基于SQL像处理静态数据一样处理流式计算。Spark SQL引擎将负责不断地运行Structured Streaming，并在流数据持到达时更新最终结果。同时，Spark Structured Streaming通过检查点和预写日志确保端到端的一次性容错保证。简而言之，Spark Structured Streaming提供快速、可扩展、容错、端到端的精确一次流处理，而不需要用户关心具体复杂流处理实现。

在默认情况下，Spark Structured Streaming使用微批处理引擎进行处理，该引擎将数据流作为一系列小批量作业处理，从而实现低至100ms的端到端延迟和完全一次的容错保证。

从Spark 2.3后引入了一种被称为Continuous Processing的新的低延迟处理模式，可以实现低至1ms的端到端延迟，并且提供至少一次的容错保证。