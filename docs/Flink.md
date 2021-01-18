## 1.Flink的原理

Flink是一个分布式计算引擎，主要用于有界数据流和无界数据流的有状态的数据分析和处理。Flink擅长处理有界数据流和无界数据流，其精确的时间控制和状态化使Flink能够安全并快速地处理海量数据。

Flink将数据抽象为有界数据流和无界数据流。

( 1 ）无界数据流：无界数据流只定义了数据流的开始，没有定义数据流的结束，即随着时间的推移数据会源源不断地产生，如滴滴打车中车辆的实时位置信息，只要车辆在运行，就有数据源源不断地上报。无界数据流必须被实时地处理，因为数据会源源不断地产生，如果处理不及时，就会产生数据积压。在数据的摄取过程中，无界数据流需要指定数据的时间顺序，例如，Event Time （事件时间）、Process Time （处理时间）、Ingestion Time （摄取时间），以便能够保障数据处理的顺序性和完整性。

( 2 ）有界数据流：有界数据流既定义了数据流的开始时间，也定义了数据流的结束时间，有界数据流在一批数据获取完成后再执行计算，其数据在内部可以被排序，因此有界数据流不需要指定数据的顺序。

## 2.Flink的特点

Flink是有状态的计算框架，也就是说运行中的Flink应用程序即使因为某些原因执行出错，也可以通过快照快速还原到故障点继续执行计算。Flink被广泛应用于事件驱动、流式分析、批量分析、数据清洗（Extract-Transform-Lood, ETL）等领域，可用于处理事件流数据、用户行为数据、物联网设备数据、系统日志数据等各种类型的数据。

Flink应用程序以事件流或数据库（文件系统、关系型数据库、非关系型数据库、Key-Value数据库等）的形式获取数据，在内存中对数据执行分布式计算，并最终将结果写入应用程序、事件日志或其他数据库系统。

Flink可运行于Kubernetes、YARN、Mesos等环境中，其支持的存储有HDFS、S3、NFS等数据库。

Flink的主要特点为：支持丰富的流式计算应用场景，保障良好的数据正确性，基于分层API的设计理念，运维方便，支持超大规模计算和延迟低、吞吐量高。

( 1 ）支持丰富的流式计算应用场景：Flink支持事件驱动模型、流式计算、批量计算、数据管道和数据清洗等多种应用场景。Flink被广泛应用于阿里巴巴、AWS、滴滴出行等互联网公司的大规模计算中。

( 2 ）保障良好的数据正确性：Flink支持Exactly-One（精确一次）数据一致性保障，用户也可以自定义事件时间以保证数据处理的顺序性，避免因网络延迟或者其他原因导致数据乱序.

( 3 ）基于分层API的设计理念：根据抽象程度的不同，Flink将API分为High-Level Analytics  API, Stream And Batch Data Processing API, Stateful Event-Driven Applications 三种不同的API。每一种API在简洁性和表达力上都有着不同侧重。

( 4 ）运维方便：Flink部署灵活，支持单机和集群模式部署，可运行于Kubernetes、YARN、Mesos等多种资源管理框架上，同时支持定时创建CheckPoint以方便程序故障恢复，支持手动创建SavePoint以方便应用程序安全升级。

( 5 ）支持超大规模计算：Flink基于Task Manager水平扩展，当资源不足时添加计算资源即可，添加集群的过程不会影响其他程序的运行。

( 6 ）延迟低、吞吐量高：Flink基于内存计算，其延迟低，吞吐量高。

## 3.Flink的核心概念

### Flink Cluster 

Flink Cluster （集群）是用于运行Flink应用程序的分布式系统，一个Flink集群由ZooKeeper、Job Manager和Task Manager 3个角色组成。在高可用模式下，一般ZooKeeper为至少3个节点的集群；Job Manager为至少2个节点的集群，Job Manager高可用模式为一主多备。在正常情况下，主节点提供服务，当主节点宕机时，一个备节点会升级为主节点对外提供服务。Task  Manager为具体的计算节点，一个集群中有一个或多个Task Manager。

### Flink Master

Flink Master指集群的管理节点，一个Flink Master由Flink Resource Manager （资源管理）、Flink Dispatcher （分发）和Flink Job Manager 3个角色组成。

### Flink Job Manage

Flink Job Manager是Flink的任务管理节点，用于任务的提交、分发和运行状态的监控。一个集群可以有一个或多个（高可用模式下）Job Manager。

### Flink Task Manager 

Flink Task Manager是Flink集群的计算节点，Flink的任务被Job Manager调度在多个Task Manager上执行。多个Task Manager上的Task相互交换计算结果以完成数据流的计算。

### Job

Job（任务）指一个运行中的Flink应用程序，Job可以通过Job Manager以命令行方式提交到集群，也可以通过Flink监控页面提交到集群。

### Flink Graph

Flink Graph（图）指Flink流式计算程序所组成的数据流程图。Flink Graph分为Logical Graph和Physical Graph。Logical Graph描述的是应用程序（通常指基于Java或者Scala实现的Flink程序）定义的数据流之间的逻辑关系，与之对应的是逻辑上的Operator （算子）、Input、Output、DataStream和DataSet。Physical Graph是Logical Graph基于分布式运行环境转换后物理上的计算逻辑图，与之对应的是物理计算上的Task、Input、Output、DataStream和DataSet。

### Flink Operator和Operator Chain

Flink Operator即Flink算子，是Operato Logical Graph中的一个节点，用于执行一个Flink  Function。一个完整的数据流通常包含一个Source Operator （数据摄取）、Process Function （数据计算）和Sink Operator （数据输出）。

多个相邻Operator相互连接组成一个Operator Chain，在一个Operator Chain内部，Operator的数据可以相互直接访问，不需要经过Flink集群的序列化和网络传输。

### Flink Task和SubTask 

Flink Task是Physical Graph的一个节点，对应物理上的一个计算单元，一个Task由多个SubTask组成，每个SubTask都对应数据流上的一个处理函数。

### Event 

Event （事件）指Flink运行时数据模型的状态变化，Event在流式计算和批量计算接口中被输入、输出以完成状态的记录和传递。

### Function 

Function （函数）是由应用程序实现的逻辑计算单元，Function一般通过实现Flink的Function接口或继承Function类来定义。常用的Function有MapFunction、Reduce Function、ProcessFunction、RichFunction等。

### Flink Record

Flink Record （记录）指数据流中的元素。

### Flink State Backend

Flink State Backend定义了Task Manager上运行的Job的状态存储方式（例如，JVM 堆内存存储、RocksDB、FileSystem），同时定义了SavePoint和CheckPoint的存储规则和存储方式。

## 4.Flink架构介绍

Flink由Job Manager、Task Manager和客户端组成。Job Manager是管理节点，负责集群任务的提交、分配和资源管理；Task Manager是具体执行任务的计算节点；客户端用于作业的提交。

### Job Manager的职责

Job Manager负责协调分布式计算节点，也被称为Master节点。它负责调度任务、协调CheckPoint、故障恢复等。Job Manager将一个作业分为多个Task，并通过Actor系统与Task Manager进行相互通信，用于Task的部署、停止、取消。

在高可用部署下会有多个Job Manager，其中有一个Leader、多个Flower。Leader总是处于Active状态，为集群提供服务；Flower处于Standby状态，在Leader宕机后会从Flower中选出一个作为Leader继续为集群提供服务。Job Manager选举通过ZooKeeper 实现。

### Task Manager的职责

Task Manager也被称为Worker节点，用于执行Job Manager分配的Task（准确来说是SubTask）。Task Manager将系统资源（CPU、网络、内存）分为多个Task Slot （计算槽），Task运行在具体的Task Slot上，Task Manager通过Actor系统与Job Manager进行相互通信，定期将Task的运行状态和Task Manager的运行状态提交给Job Manager。多个Task Manager上的Task通过DataStream进行状态计算和结果交互。

### 客户端

客户端不是运行时（Runtime）环境的一部分，它主要用于提交作业给Job Manager。在作业提交完成后，客户端可以断开连接，也可以保持连接来接收作业的运行状态。

## 5.Flink应用程序的运行流程

Flink应用程序的运行流程如下.

( 1 ）编写应用程序数据流作业，可以基于Java或者Scala。

( 2 ）构建DAG和优化流程.

( 3 ）通过客户端命令提交作业到集群的Job Manager Leader节点。

( 4) Job Manager将任务提交的结果和运行状态反馈给客户端。

( 5 ) Job Manager根据每个Task Manager上资源的使用情况，将作业拆分为多个Task，并通过Actor系统将Task部署到具体的Task Manager节点上。

( 6 ) Task Manager在Task Slot上运行Task，并定时将Task Manager的运行状态和Task的运行状态发送给Job Manager, Job Manager根据Task Manager上资源的使用情况和Task的运行状态对集群进行调度。

( 7 ) Job Manager和ZooKeeper进行交互，以完成Job Manager的选举和故障恢复。

## 6.Flink Task Slot资源分配

Job Manager将一个Task拆分为多个SubTask分配到不同Task Manage上运行。Task Manager是一个JVM进程，内部以多线程的方式运行不同SubTask。

在Task Manager内部将操作系统计算资源分为多个Slot，每个Slot都代表一份固定的计算资源，Task Manager为每个SubTask都分配一定的Slot进行运算，多个SubTask 之间不会竞争资源，很好地实现了资源隔离。强调的是，Slot的资源并不是CPU的资源划分，而是内存资源的划分。

可以通过设置Task Manager中Slot的数量来配置多个SubTask的资源隔离方式。如果Task Manager中Slot的数量为1，则意味着Task Manager上的所有SubTask共享所有资源（例如将Task Manager运行在一个独立的Docker中）。如果Task Manager中Slot的数量为多个，则多个SubTask在同一个Task Manager的JVM中运行，Task Manager将JVM内存分为多份分配给每个Slot，每个Slot上都运行不同SubTask，以实现SubTask 之间的内存资源隔离，SubTask在同一个JVM中共享TCP连接（通过多路复用技术）和心跳信息，同时各个SubTask之间可以共享数据集和数据结构，从而减少每个Task的开销。

官方建议将Slot的数量设置为与CPU核的数量相同，这样Flink能够更好地使用操作系统资源。当使用超线程时，每个Slot都将会占用2个或更多的硬件线程上下文.

## 7.Flink任务和算子

Flink将多个SubTask连接在一起组成一个Operator Chain ，每个Operator Chain都对应一个Task，如果Operator Chain只包含一个SubTask，则该Task内只有一个SubTask。

在Flink中，每个Task都运行在一个线程上，Flink将连续相邻的多个SubTask组成一个Operator Chain，能有效减少多个SubTask之间的线程数据交换、数据共享、线程上下文切换所带来的时间和性能的消耗，从而提高系统整体的吞吐量。

## 8.Flink状态存储

Flink在状态存储中以Key-Value的形式对数据的状态进行存储，存储的方式有内存、RocksDB、本地磁盘、HDFS、S3、OSS等多种方式。状态存储还可以按照应用程序的配置定时将计算状态存储到CheckPoint中，以便基于状态进行故障恢复。

Flink还支持以命令行或API的方式手动将当前运行中的集群状态存储为SavePoint。Flink应用程序在启动时可以从指定的SavePoint启动（即基于历史状态启动）。SavePoint常用于在程序升级过程中保障数据状态的一致性和完整性。

SavePoint和CheckPoint的不同在于，CheckPoint是程序按照配置定期自动生成的，并且在新的CheckPoint生成后，旧的CheckPoint将不再被需要；而SavePoint需要用户手动触发，用户可以从任何一个SavePoint状态中恢复计算。

( 1 ) Job Manager根据用户配置定期触发CheckPoint存储，并将执行CheckPoint的任务下发给Task Manager。

( 2 ) Task Manager在收到保存CheckPoint任务后进行快照备份（Save CheckPoint）。

 ( 3 ) Task Manager在执行完快照备份后将备份结果及状态上报给Job Manager。

## 9.Flink运行模式

Flink有多种运行模式。

| 运行环境           | 运行模式 |                                                        |
| ------------------ | -------- | ------------------------------------------------------ |
| Local              | 本地模式 | Flink在JVM中构建一个运行环境并运行，常用于本地开发测试 |
| Standalone Cluster | 集群模式 | 独立模式，在Flink提供的资源调度框架上运行              |
| On YARN            | 集群模式 | 在YARN资源管理器框架上运行，向YARN负责资源管理         |
| On Mesos           | 集群模式 | 在Mesos资源管理器框架上运行，由Mesos负责资源管理       |

## 10.什么是事件驱动模型

事件驱动模型是基于事件流的有状态的计算模型。它接收源源不断的事件，并根据事件的不同类型更新不同状态来触发不同计算。

事件驱动模型和一般的计算存储分离模型的最大差别在于，计算存储分离模型需要将数据存储在远程对象存储系统（如S3、OSS、OBS）、事务型数据库（MySQL、Postgres ）、分布式内存系统（Redis、Memcached、LeverDB）中。即计算存储分离模型的所有数据计算都基于本地内存和磁盘，而数据均被存储在远程存储系统中，这样做的好处是计算和存储分离，以便计算和存储可以各向扩展而相互不影响。

事件驱动模型是基于状态化流处理完成的，它并未将计算和存储分离，而是在计算过程中通过访问本地存储（操作系统内存或磁盘）获取数据以便尽可能快地完成计算。事件驱动模型系统通过定期向远程持久化存储写入CheckPoint和SavePoint来实现状态回滚、故障恢复和程序升级。

## 11.事件驱动模型的特点

事情驱动模型的特点是高效。事件驱动模型由于不需要频繁访问远程数据，大部分数据操作在本地内存中完成，少部分数据操作在磁盘上完成，因此具有更高的吞吐量和更低的延迟。

同时，事件驱动模型会定期增量地将数据处理的状态以CheckPoint的形式存储在远程持久化存储中，以便程序状态回滚和故障恢复。

## 12.Flink的事件驱动模型的特点

Flink底层基于有状态的事件数据处理而设计，提供了丰富的状态操作，并提供了Exactly-One数据一致性保障和海量规模（至少TB级）的状态数据计算。此外，Flink提供了多种窗口计算，灵活性极高。

## 13.Flink的数据分析应用

数据分析的目的是从数据中获取有价值的信息，一般将数据分析分为批量数据分析和流式数据分析。

### 批量数据分析

批量数据分析指以批量查询的方式将文件系统（HDFS ）、对象存储系统( S3/0SS/OBS ）或数据库（MySQL/Postgres ）中的数据加载到计算模型中进行计算，然后将计算结果写入外部数据库存储或以报表的形式输出。Hadoop的MapReduce和Spark的Batch Process都属于批量数据分析。由于批量计算一次只能计算一部分数据（一段时间内的数据或者符合某些查询条件的数据），因此，为了获取最新的数据计算结果，必须将数据重新加载到内存中进行计算并输出。

### 流式数据分析

( 1）什么是流式数据分析.

流式数据分析主要用于接入实时事件流，不断地处理到达系统的数据，并实时更新计算状态和结果，其特点为数据是“无穷无尽”的，即随着时间的流逝，新的数据会不断地流入并被分析。流式数据分析的中间结果会以状态的形式存储在内存中或者定期存储到本地磁盘上，计算结果往往最终被发送到高效的消息系统、Key-Value数据库或实时报表。

我们知道，从CPU到内存、本地磁盘，再到网络，计算性能是呈指数级下降的，因此流式数据分析设计的“大忌”是在计算过程中依赖远程的某个运行效率并不高的接口或数据库，从而造成系统的瓶颈，进而影响整个系统的运行效率。

( 2）流式数据分析的特点.

①高效：与批量数据分析相比，流式数据分析不用频繁地执行数据的批量查询和导人，因此性能更高，延迟更低。

②易于维护：批量数据分析通常根据业务的不同将任务分为多组件周期性的调度执行，各个任务之间根据时间上的先后顺序和依赖关系组成一个数据处理流，当其中一个任务执行出错时，将影向后续所有其他任务的执行。而流式数据分析整体运行在一个流式数据分析框架上，也就是说，一个流式数据分析可以独立地完成从数据接入到连续的数据计算和最终计算结果的输出。因此流式数据分析可以独立地完成一个工作流任务的各个节点而不依赖其他任务的计算结果，每个节点的运行状态都被记录在流式数据分析框架中（比如Flink的CheckPoint），在其中某个节点执行出错后可以快速地从故障节点重新计算，更加易于维护。

## 14.Flink中的流式数据分析

Flink为流式数据分析和批量数据分析都提供了良好的支持。Flink内置了一个符合ANSI标准的SQL接口，将批量、流式查询的语义抽象在一起。无论批量查询还是实时事件流查询，相同的SQL查询都会得到一致的处理结果。

同时，Flink支持丰富的用户自定义函数，允许在SQL中执行定制化代码。如果需要进一步定制逻辑，则可以通过Flink DataStream API和Dataset API进行底层的控制。此外，Flink的Gelly库为基于批量数据集的大规模高性能图分析提供了算法和构建模块的支持。

## 15.Flink的数据清洗和数据管道

系统之间数据的转换和迁移常用的两种方式是数据清洗和数据管道。

### 数据清洗：定时处理

数据清洗通常调用周期性的任务对数据进行清洗处理，通常从事务型的数据库中获取数据，通过提取、转换、加载将最终结果输出到分析型数据库或其他数据仓库中。

### 数据管道：实时处理

数据管道和数据清洗的功能类似，均用于数据的转换和迁移，不同的是数据管道中的数据以实时流的模式运行，因此其运行不会停止。数据管道主要用于从一个源源不断的数据流上接收事件数据流并通过实时转化将数据以低延迟的方式传递到目标节点，目标节点可以是数据库、文件系统或者日志系统。数据管道常用于日志监控、实时数据格式转换等。

### 数据管道优势：实时性高

数据管道主要用于端到端的数据转换和迁移，主要特点是吞吐量高和延迟低，另外，与数据清洗不同的是，数据管道用于持续性的消息处理，因此其处理数据的实时性比较高。

### Flink数据管道

应用程序可以通过Flink的DataStream API、SQL接口或者自定义函数实现数据管道应用。同时，Flink提供了多种连接器（如Kafka、Kinesis、ElasticSearch、JDBC数据库系统等）用于数据管道应用的开发。

## 16.Flink数据流处理基本概念

Flink是一个针对无界数据流和有界数据流进行有状态计算的框架，以数据流、状态( State ）、时间（Time ) 3个核心组件为基础构建整个框架。

### 数据流

流是流式数据处理的基本要素。

( 1 ）有界流和无界流：Flink数据流可以分为有界流和元界流，有界流定义了数据的结束时间，无界流没有定义数据的结束时间。

( 2 ）实时数据流和历史数据流：Flink中的所有数据都是以流的方式产生的，但是用户处理数据的方式可以分为实时数据流处理和历史数据流处理。实时数据流处理指将接收的数据进行“立即”处理，而历史数据流处理指先将数据流持久化到存储系统中（例如，内存、文件系统、数据库），然后对一段时间内的数据执行批量处理。

### 状态

每一个具有一定复杂度的流处理应用都应该是有状态的。任何运行基本业务逻辑的流处理应用都需要在一定时间内存储所接收的事件或中间结果，以供在后续某个时间点（例如收到下一个事件或者经过一段特定时间）进行访问并进行后续处理。

状态是Flink的核心概念。基于状态，Flink提供了多种特性。

( 1 ）多种状态基础类型：Flink为多种不同数据结构提供了对应的状态基础类型，如原子值、列表及映射。

( 2 ）插件化的状态管理：Flink通过State Backend实现状态的管理，并按照应用程序的配置进行CheckPoint。Flink支持内存、RocksDB( RocksDB是一种高效的嵌入式、持久化Key-Vaue存储引擎）、本地磁盘或S3，这些状态管理组件以插件化的形式存在Flink中，应用程序也可以通过自定义State Backend完成状态存储。

( 3 ）精确一次语义：Flink的CheckPoint和故障恢复算法保证了故障发生后应用状态的一致性。此外，Flink能够在应用程序发生故障时快速恢复，而不会造成数据不一致。

( 4 ）大规模数据状态管理：Flink基于异步增量式的Checkpoint算法，可以进行TB级应用数据存储状态的管理。

( 5 ）可弹性伸缩：Flink能够通过在节点扩容或缩容对状态进行重新分布，支持应用状态的分布式横向扩缩。

### 时间

时间是流处理应用的一个重要组成部分，因为数据总在某个时间点产生或事件总在某个时间点发生。Flink提供了丰富的时间语义支持，事件语义分为：Event Time （事件时间）Ingestion Time （摄取时间）、ProcessTime （处理事件）。

( 1）事件时间：使用事件时间语义的流处理应用根据事件本身自带的时间戳进行结果计算。因此，无论处理的是历史记录事件还是实时事件，事件时间模式的处理总能保证结果的准确性和一致性。

( 2 ）摄取时间：数据进入Flink数据源的时间。

( 3） 处理时间：除了事件时间和摄取时间，Flink还支持处理时间语义。处理时间模式根据处理引擎的时钟触发计算，一般应用于对数据处理延时性要求较高，并且能够容忍因网络延迟带来数据乱序问题的场景。

由于网络延迟或者其他原因，在数据计算完成后仍可能会有数据到达，这样的数据被称为迟到数据。Flink提供了WaterMark机制以衡量数据处理的进展。基于该机制，当应用程序收到迟到数据时，可以将这些数据重新定向到旁路输出（Side Output）或者更新之前完成计算的结果。

## 17.API分类

根据抽象程度的不同，Flink将API封装成Process Function 、DataStream API和SQL/Table API，其中，Process Function是Flink底层接口，接口内部通过Event、State、Time来实现有状态的事件计算；DataStream API是Flink基于Process Function的进一步封装，其内部通过定义不同的Stream和Window来实现流式计算和批量计算；最上层的是SQL/Table API，SQL/Table API将数据封装成一个Dynamic Table以便更加简单、方便地实现业务数据的分析。

### Process Function

Process Function是Flink所有应用底层的API。它将实时收到的事件数据流分配到不同窗口中进性计算。对于每条数据，Flink都提供了时间和状态细粒度的控制，应用程序可以获取每条计算中事件数据的状态并更新该状态值，同时，基于时间窗口，应用程序可以定义触发器在未来的某个时刻执行数据计算。

### DataStream API 

DataStream API为流处理提供了许多通用的操作。这些操作包括窗口计算、数据的转换操作、执行外部数据库查询等。DataStream API预先定义了map（）、reduce（）、aggregate（）等函数。应用程序可以通过扩展实现自定义接口或使用Java、Scala的lambda表达式实现自定义函数。

### SQL和Table API 

Flink支持SQL和Table API两种关系型API。在无边界的实时数据流和有边界的历史记录数据流上，关系型API会以相同的语义执行查询，并产生相同的结果。SQL和Table API借助Apache Calcite进行查询的解析、校验及优化。

SQL和Table API可以与DataStream API和DataSet API无缝集成，并支持用户自定义的标量函数、聚合函数及表值函数。

Flink的关系型API旨在简化数据分析、数据流处理和ETL应用。

## 18.Flink基于状态的内存计算

Flink是有状态的分布式计算框架，在运行中，任务的状态始终保留在内存中，如果状态大小超过可用内存，则会被保存在能高效访问的磁盘中，同时Flink可以将状态信息存储在远程存储中。运行中的任务通过访问保存在内存或高效磁盘上的状态来完成下一步的状态计算，因此大部分计算都在内存中完成，其吞吐量高，计算延迟低。

Flink通过定期异步地对本地状态进行持久化存储来保证发生故障时数据状态的精确一致性。

## 19.Flink的编程模型

### 数据流

Flink的编程模型由流（Stream ）和转换（Transformation ）组成。流由一系列记录(Record ）组成；转换是针对流的操作，通常以一个或者多个流作为数据源输入Flink集群，经过Map或Reduce等计算，再将计算结果输出另外一个流。

当运行Flink应用程序时，Flink会根据流的依赖关系生成一个数据流。每个数据流从一个或者多个数据源开始，以一个或者多个数据输出结束。

### 并行度

一个Flink程序由多个Task组成。一个Task包括多个并行执行的实例，且每一个实例都处理Task输入数据的一个子集。一个Task的并行实例数被称为该Task的并行度( Parallelism）。

在开发过程中，可以通过改变特定算子或者整个程序的并行度来提系统的运行效率和吞吐量。Flink默认算子的并行度为1.

并行度可以在单个算子、执行环境、客户端和Flink集群上设置。

在单个算子上设置的并行度作用于单个算子，在执行环境上设置的并行度作用于所有算子。

同时，可以在将作业提交到Flink集群后在客户端通过命令行设置并行度。

还可以在conf/flink-conf.yaml中设置parallelism.default参数来设置Flink集群的并行度。

Flink Job运行过程中并行度的优先级为：算子并行度>执行环境并行度>客户端并行度>Flink集群默认并行度。

## 20.Flink窗口计算

Flink中的窗口将源源不断的流据切分为有限大小的分段，每个分段中都包含一部分数据，Flink基于该分段上的数据执行计算。

Flink中的窗口可以分为：翻滚窗口（Tumbling Window）- 无重叠，滚动窗口( Sliding Window）-有重叠，会话窗口（Session Window）- 活动间隙，以及全局窗口(Global Window）。

### 翻滚窗口

翻滚窗口的尺寸是固定的，不能重叠。例如，如果指定一个大小为5s的翻滚窗口，Flink会每5s启动一个新窗口并针对窗口内的数据执行计算。

### 滑动窗口

滑动窗口分配器将每个元素都分配到固定大小的窗口中。滑动窗口由窗口大小和滑动长度两个参数控制。因此，如果滑动长度小于窗口大小，则滑动窗口内的数据将会重叠。在这种情况下，元素将被分配到多个窗口。例如，将窗口大小设置为10s ，将滑动长度设置为5s。这样，每10s都会生成一个新的窗口，该窗口将包含最近10s内到达的数据。

### 会话窗口

会话窗口分配器通过活动会话分配元素。与翻滚窗口和滑动窗口相比，会话窗口不会重叠，也没有固定的开始时间和结束时间。当会话窗口在一段时间内没有接收元素时，窗口会关闭。会话窗口通过设置会话间隙（不活动的时间长度）来确定窗口大小，当该时间段到期时，当前会话关闭，后续元素将被分配到新的会话窗口。

### 全局窗口

全局窗口将具有相同Key的所有元素都分配到同一个全局窗口。全局窗口只有在定义触发器后才能触发计算，否则，只会将数据分配给不同的窗口，但不会执行任何计算。

## 21.Flink故障恢复

当Task发生故障时，Flink需要重启出错的Task及其他受到影响的Task，以便作业恢复到正常执行状态。

Flink通过重启策略（Restart Strategies）和故障恢复策略（Failover Strategies）来控制Task重启。重启策略决定是否可以重启及重启的问隔，故障恢复策略决定那些Task需要重启。

### 重启策略

Flink作业如果没有定义重启策略，则会遵循集群启动时加载的默认重启策略。如果提交作业时设置了重启策略，则该策略将覆盖集群的默认重启策略。

我们可以通过Flink的配置文件flink-conf.yaml来设置集群的默认重启策略，通过配置参数restart-strategy定义重启策略。如果没有启用CheckPoint，则采用“不重启”策略。如果启用了CheckPoint且没有配置重启策略，则采用固定延迟重启策略。

除了定义默认重启策略，我们还可以为每个Flink作业都单独定义重启策略。这个重启策略通过在程序中的Execution Environment对象上调用setRestartStrategy方法来设置。当然，对于StreamExecutionEnvironment也同样适用。

Flink重启策略有3种：没有重启策略（Disable）、固定延迟重启（Fixeddelay ）、故障率重启（Failurerate）。每个重启策略都有自己的一组配置参数来控制其行为。

( 1 ）固定延迟重启.

固定延迟重启策略按照给定的次数尝试重启作业。如果尝试超过了给定的最大次数，则作业将最终失败。在连续两次重启尝试之间，重启策略等待一段固定长度的时间。

( 2 ）故障率重启策略.

故障率重启策略在故障发生之后重启作业，但是当故障率（每个时间间隔发生故障的次数）超过设定的限制时，作业会最终失败。在连续两次重启尝试之间，重启策略会等待一段固定长度的时间。

### 故障恢复策略

Flink同前支持两种不同的故障恢复策略：全局重启故障恢复（Full）、按分区重启(Region）。该策略需要在Flink配置文件flink-conf.yaml的jobmanager.execution.failover-strategy配置项进行配置。

 ( 1 ）全局重启故障恢复：在全局重启故障恢复策略下，当Task发生故障时会重启作业中的所有Task来进行故障恢复。

( 2 ）按分区重启：该策略会将作业中的所有Task划分为多个Region。当有Task发生故障时，它会尝试找出进行故障恢复需要重启的最小Region集合。

相比于全局重启故障恢复策略，按分区重启策略在一些场景下需要重启的Task会更少。此处的Region指以Pipeline的形式进行数据交换的Task集合，Batch形式的数据交换会构成Region的边界。Region的重启判断逻辑：

( 1 ) 出错Task所在的Region需要重启。

( 2 ）如果要重启Region需要消费的数据有部分无法访问（丢失或损坏），则产生该部分数据的Region也需要重启。

( 3 ）需要重启的Region的下游Region也需要重启。这是出于保障数据一致性的考虑，因为一些非确定性的计算或者分发会导致同一个Result Partition每次产生时包含的数据都不相同。

## 22.Flink的高可用

在Flink集群中，Job Manager是任务的管理节点，Task Manager是任务的计算节点，因此Task Manager可以按照计算需求进行水平扩展。Flink的高可用（High Availability, HA）主要指Job Manager的高可用。

Job Manager的高可用依赖ZooKeeper进行节点状态的管理和选举工作。Job Manager 高可用模式为主（Leader）从（Flower）模式，在正常情况下由Job Manager主节点为整个集群提供服务，Job Manager从节点处于Standby状态。在主节点宕机后集群会在现有从节点中选举一个节点作为主节点对外提供服务，在之前主节点启动后将以从节点的角色加入集群。

## 23.Flink Table API和SOL

Flink有两关系型API:Table API和SQL，Flink通过Table API和SQL来统一流处理和批处理。Table API是Scala和Java的集成查询API，允许以非常直观的方式组合执行关系运算（例如，Selection、Filter和Join）的查询。Flink的SQL基于遵循SQL标准的ApacheCalcite实现。无论批处理（DataSet ）还是流处理（DataStream），在两个接口中指定的查询都具有相同语义且具有相同结果。