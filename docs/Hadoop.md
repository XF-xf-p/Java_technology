## 1.Hadoop

Hadoop是一个大数据解决方案，提供了一套分布式系统基础架构，核心内容包含HDFS ( Hadoop Distributed File System，分布式文件系统）、MapReduce计算引擎和YARN (Yet Another Resource Negotiator，另一种资源协调者）统一资源管理调度。

其中，HDFS分为NameNode和DataNode，NameNode负责保存元数据的基本信息，DataNode负责具体数据的存储。

MapReduce分为JobTracker和TaskTracker,JobTracker 负责任务的分发，TaskTracker负责具体任务的执行。

Hadoop集群是Master/Slave( M/S ）架构，NameNode和JobTracker运行在Master节点上，DataNode和TaskTracker运行在Slave节点上。

## 2.HDFS 

HDFS全称是Hadoop Distributed File System，是Hadoop的分布式文件系统。它由很多机器组成，每台机器上运行一个DataNode进程，负责管理一部分数据。然后有一台机器上运行了NameNode进程，NameNode大致可以认为是负责管理整个HDFS集群的这么一个进程，他里面存储了HDFS集群的所有元数据。然后有很多台机器，每台机器存储一部分数据！

HDFS是Hadoop的存储系统，包含客户端（Client）、元数据节点（NameNode）、备份节点（Secondary NameNode）和数据存储节点（DataNode）。

### Client

Client是HDFS客户端，它提供了一个文件系统接口，用户通过Client对NameNode和DataNode进行交互访问来操作HDFS中的文件。

### NameNode

在一个Hadoop集群中只有一个NameNode,NameNode是HDFS的管理节点，负责HDFS的目录树和相关的文件元数据的存储。这些元数据以HDFS元数据镜像文件( fsimage）和HDFS文件改动日志（editlog）两种形式存放在本地磁盘上。HDFS元数据镜像文件和HDFS文件改动日志在HDFS启动时构造。此外，NameNode负责监控集群中DataNode的健康状态，一旦发现某个DataNode宕掉，则将该DataNode从HDFS集群移除并在其他DataNode上重新备份该DataNode的数据（该过程被称为数据重平衡，即rebalance），以保障数据副本的完整性和集群的高可用性。

### Secondary NameNode 

Secondary NameNode是NameNode元数据的备份，在NameNode宕机后，Secondary NameNode会接替NameNode的工作，负责整个集群的管理。同时，在HDFS设计的时候，为了减小NameNode的压力，NameNode并不会自己合并HDFS元数据镜像文件和HDFS文件改动日志，而是将该任务交由Secondary NameNode完成，Secondary NameNode在合并完成后将结果发送到NameNode,NameNode再将合并后的结果存储到本地磁盘。

### DataNode

在一个Hadoop集群中有多个DataNode，一般情况下在一个Hadoop Slave节点上部署一个DataNode,DataNode负责具体的数据存储，并将数据的元信息定期汇报给NameNode。DataNode以固定大小的块（Block）为基本单位组织和存储文件内容（Block的大小通过dfs.blocksize设置，默认为64MB）。

#### HDFS文件的写入流程

在应用程序调用客户端API上传一个文件到HDFS时，该请求首先会到达NameNode, NameNode检查文件的状态和权限。如果通过检查，则写本地文件改动日志并返回可写的DataNode列表；客户端在收到列表后将文件切分为固定大小的多个块发送到最近的DataNode上；等待每个DataNode上的数据块都写完成并返回确认信息后，文件写操作完成。在写文件的过程中，文件会被切分成若干小块分别存储到不同的DataNode上。

#### HDFS文件的读取流程

在应用程序读取文件时，首先会将请求发送给NameNode,NameNode返回文件块所在的DataNode列表（DataNode列表按照客户端距离DataNode网络拓扑的远近排序），应用程序从每个DataNode上都获取数据并将数据读取到本地。

#### HDFS数据副本

为了保证数据可靠性，HDFS会将同一个数据块对应的数据副本（数据副本个数默认为3）存储在多个不同的DataNode上。在某个DataNode宕机后，HDFS会将该节点上的数据存储到其他可用的DataNode上，以保障数据的安全。

## 3.MapReduce

Hadoop MapReduce是一个分布式计算框架，其最大的特点是基于分布式文件系统的海量数据（至少TB级数据）的分析特征，在不同的节点上快速地完成计算并将计算结果合并返回。它为在廉价的服务器上并行处理TB级的数据集提供了可能’性，其计算的核心思想是计算跟着数据走，尽可能减少网络上数据的传输以提高计算的效率。

Hadoop MapReduce采用了Master/Slave架构。Master/Slave架构由一个Master节点的JobTracker和多个Slave节点的TaskTracker共同组成。Hadoop MapReduce中的一个作业由多个MapReduce任务组成，这些任务分布在不同的Slave节点上（Hadoop会尽可能地将任务分配在其对应的数据节点上）。Master负责任务的调度并监控其执行状态，Slave负责执行由Master指派的任务，同时定期向Master发送心跳以汇报其状态并最终将运行结果返回给Master。在Slave的某个任务执行失败后，Master会尝试将该任务发送到其他运行正常的节点上重新执行，以提高系统的可靠性。

MapReduce的作业会把输入的数据集按照Partition分为若干独立的数据集，再针对不同的数据集以Map方式完全并行地进行处理。在Map阶段执行完成后将Map的计算结果进行排序，然后把Map阶段的计算结果输入Reduce任务，Reduce任务执行完成后将最终的结果返回给客户端。Map阶段和Reduce阶段的执行结果通常被存储在HDFS上，整个框架的计算由YARN负责任务的调度和监控，以及重新执行已经失败的任务。

通常MapReduce框架会将一组计算任务分配在其对应的数据节点上，即计算跟着数据走。这种任务分配机制保障了计算任务在距离其最近的数据节点上高效地运行，尽可能减少网络上数据的传输，从而提高整体系统的吞吐量。

### Job Client 

用户编写的MapReduce程序通过JobClient提交到JobTracker并运行，同时，用户通过Client提供的接口查看程序运行状态。在Hadoop内部使用作业（Job ）表示MapReduce程序。一个MapReduce程序可以包含若干作业，而每个作业又会被分解成若干MapReduce任务（Task）在不同的节点上执行。

### JobTracker 

Job Tracker主要负责作业的调度和资源的监控。JobTracker在将作业调度起来后会实时监控所有作业中任务的运行状态，一旦发现某个任务执行失败，则会将该任务转移到其他健康的节点上运行，以保障任务的正常运行；同时，JobTracker会实时跟踪任务的执行状态、执行进度、系统资源使用情况等信息，并将这些信息同步到任务调度器，调度器会根据每个节点上系统资源的使用情况为每个节点分配不同的任务。

### TaskTracker 

Task Tracker接收JobTracker发送过来的命令并执行相应的操作（如启动新任务、杀死任务等），同时会周期性地通过Heartbeat将本节点上任务的运行情况和资源的使用情况汇报给JobTracker 。TaskTracker使用Slot等量划分本节点上的资源量。Slot代表计算资源（如CPU、内存等）。一个Task在获取一个Slot后才有机会运行，而Hadoop调度器的作用就是将各个TaskTracker上的空闲Slot分配给Task。Slot分为Map Slot和Reduce Slot两种，分别供Map Task和Reduce Task使用。TaskTracker通过Slot数量（可配置参数）来限定Task的并发度。

### Task 

Task是具体执行计算的任务，分为Map Task和Reduce Task两种，均由TaskTracker 启动。MapReduce处理的基本单位是Data Split, Data Split包含了任务元数据信息（例如，数据起始位置、数据长度、数据所在节点等），Data Split的划分方法完全由客户端决定，Data Split的多少决定Map Task的数量，因为每个Data Split都被交由一个MapTask 处理。

在任务读取到数据后会将数据载入HDFS, Map Task首先将HDFS按照分区（Partition ）分为多个DataSplit，每一个DataSplit都被迭代解析成一个个Key-Value键值对，然后依次调用用户自定义的Map（）函数对数据进行处理，并将临时处理的结果存放到HDFS供Reduce阶段使用，HDFS的临时数据也会被分成若干分区（Partition ），每个分区的数据都将被一个Reduce Task任务执行和处理。

### Reduce Task的执行过程

Reduce Task的执行过程分为3个阶段.

( 1 ) Shuffle阶段：从远程节点上读取Map Task的中间结果。

( 2) Map阶段：按照Key调用Map（）函数处理Key-Value链值对的数据，将处理结果写入HDFS供Reduce阶段使用。

( 3) Reduce阶段：依次读取＜Key,Value List＞，调用Reduce（）函数处理数据，并将最终结果存入HDFS。

### Hadoop MapReduce的作业生命周期

MapReduce的作业生命周期包括作业提交与初始化、任务调度与监控、任务运行环境准备、任务执行和作业完成5个阶段。

( 1 ）作业提交与初始化：用户在执行Submit命令提交作业后，会通过Job Client将作业的相关信息（例如，程序JAR包、依赖JAR包、配置文件、分片元数据信息文件等）上传到HDFS。其中，分片元数据信息文件记录了每个输入分片的逻辑位置信息。然后Job Client通过RPC接口通知JobTracker有新作业提交。JobTracker在收到新作业提交的请求后，通过作业调度模块对作业进行初始化。在初始化完成后，调度模块会为作业创建一个JoblnProgress对象以跟踪作业的运行状况，同时JoblnProgress会为每个Task都创建一个TasklnProgress对象以跟踪每个任务的运行状况，一个TasklnProgress可能需要管理多个Task Attempt。

( 2） 任务调度与监控：通过JobTracker来完成。TaskTracker定时通过心跳向Job Tracker汇报当前节点的服务器资源使用情况。当服务器有空闲资源时，JobTracker会按照一定的策略选择一个合适的任务在该服务器上执行，该过程为任务的调度过程。任务调度过程的核心是优先考虑数据本地性，即Hadoop会尽量将任务在其所对应的数据节点上执行。除了作业的调度，JobTracker还负责跟踪作业的整个运行过程，当发现TaskTracker或Task运行失败时，JobTracker会将计算转移到其他健康的节点上；当发现某个Task的执行进度远落后于同一作业的其他Task时，JobTracker会重新启动一个相同的Task来执行，并以率先执行完成的Task计算结果作为最终结果。

( 3 ）任务运行环境准备：在运行环境准备期间，TaskTracker会为每个Task都申请一定的独立资源并启动一个JVM来运行，以避免不同的Task在运行过程中相互影响；同时，TaskTracker使用操作系统进程来实现资源隔离，以防止Task之间相互争抢系统资源（主要指内存、CPU等资源）。

( 4 ）任务执行：TaskTracker为Task准备好运行环境后会启动Task并开始运行Task。在Task运行过程中，每个Task运行的最新进度都首先由Task通过RPC接口汇报给TaskTracker，再由TaskTracker汇报给JobTracker。

( 5 ）作业完成：在等待所有Task执行完成后，整个作业才执行成功，并将执行结果输出到HDFS。

## 4.YARN 

YARN是一个分布式资源管理和任务调度框架，其核心由ResourceManager、NodeManager、ApplicationMaster3个模块组成。其中，ResourceManager负责集群资源的管理、监控和分配，NodeManager负责节点的维护，ApplicationMaster负责具体应用程序的调度和协调。对于所有的应用，ResourceManager拥有绝对的控制权和对资源的分配权。ApplicationMaster都会与ResourceManager协商资源，同时与NodeManager通信来执行和监控Task。

### ResourceManager

ResourceManage为YARN的资源管理系统，负责整个集群的资源管理和分配。NodeManage以心跳的方式向ResourceManager汇报资源使用情况（主要是CPU和内存的使用情况）。ResourceManage只接收NodeManager的资源汇报信息，对于具体的资源处理则交给NodeManager自己处理。

### NodeManage

NodeManage负责具体节点的资源管理和任务分配，它是ResourceManager管理该节点的代理，负责该节点程序的运行和资源的管理与监控。YARN集群的每个节点都运行一个NodeManage。

NodeManage定时向ResourceManager汇报本节点资源（CPU和内存等）的使用情况和Container的运行状态。当主ResourceManager宕机时，NodeManager会自动连接ResourceManager的备用节点。NodeManager接收并处理来自ApplicationMaster的Container启动、停止等各种请求。

### ApplicationMaster 

用户提交的每个应用程序均包含一个ApplicationMaster，它可以运行在ResourceManager以外的机器上。ApplicationMaster的主要职责如下。

( 1）申请资源：与ResourceManager协商来获取系统资源，ResourceManager以Container的形式将资源分配给ApplicationMaster。

( 2 ）启停任务：与NodeManager通信来启动或停止任务。

( 3 ）监控任务：监控任务的运行状态。

( 4 ）错误重试：在任务运行失败后，ApplicationMaster会重新为任务申请资源并重启该任务。ResourceManager只负责监控AppIication Master，并在ApplicationMaster运行失败后启动ApplicationMaster。ResourceManager不负责ApplicationMaster内部任务的容错，任务的容错由ApplicationMaster自身完成。

### Container 

Contain是YARN所管理的集群中资源的抽象，它封装了每个节点上的资源信息（例如，内存、CPU、磁盘、网络等）。在ApplicationMaster向ResourceManager申请资源时，ResourceManager为AppIicationMaster返回的资源便是以Container表示的。YARN会为每个任务都分配一个Container，且该任务只能使用该Container中描述的资源，以达到资源隔离的目的。

### YARN的任务提交和运行流程

YARN的任务提交由Client向ResourceManager发起，然后由ResourceManager启动ApplicationMaster并为其分配用于运行作业的Container资源，ApplicationMaster在收到Container资源列表后初始化Container再交由NodeManager来启动Container容器并运行具体的任务（MapReduce任务或其他Spark、Flink任务）。在任务运行完成后，ApplicationMaster向ResourceManager注销自己并释放资源。

![](D:\workspace\Java-Interview-Offer\images\Hadoop001.png)

( 1 ) Client向ResourceManager提交应用程序，其中包括启动该应用程序ApplicationMaster的必需信息，例如，ApplicationMaster程序、启动ApplicationMaster命令、用户程序等。

( 2) ResourceManager启动一个Container，在容器中启动和运行ApplicationMaster。

( 3 ）启动中的ApplicationMaster向ResourceManager注册自己，启动成功后与ResourceManager保持心跳。

( 4) ApplicationMaster向ResourceManager发送请求，申请相应数量的Container。

( 5) ResourceManager返回ApplicationMaster申请的Container信息。

( 6）申请成功的Container由ApplicationMaster进行初始化。

( 7）在Container的启动信息初始化后，ApplicationMaster与对应的NodeManager通信，要求NodeManager启动Container。

( 8 ) ApplicationMaster与NodeManager保持定时心跳，以便实时对在NodeManager上运行的任务进行监控管理。

( 9 ) Container在运行期间通过RPC协议向对应的ApplicationMaster汇报自己的进度和状态等信息，ApplicationMaster对Container进行监控。

( 10）在应用程序运行期间，Client通过RPC协议与ApplicationMaster通信获取应用的状态、进度更新等信息。

( 11 ）在应用程序运行结束后，ApplicationMaster向ResourceManager注销自己，并允许属于它的Container被收回。

## 5.HDFS的NameNode架构原理**

NameNode有一个很核心的功能：**管理整个HDFS集群的元数据**，比如说文件目录树、权限的设置、副本数的设置，等等。

每次内存里改完了，写一条edits log，元数据修改的操作日志到磁盘文件里，不修改磁盘文件内容，就是顺序追加，这个性能就高多了。

每次Active NameNode（主节点）修改一次元数据都会生成一条edits log，**除了写入本地磁盘文件，还会写入JournalNodes集群。**

然后Standby NameNode就可以从JournalNodes集群拉取edits log，应用到自己内存的文件目录树里，跟Active NameNode保持一致。

然后每隔一段时间，Standby NameNode都把自己内存里的文件目录树写一份到磁盘上的fsimage，这可不是日志，这是完整的一份元数据。**这个操作就是所谓的checkpoint检查点操作。**

然后把这个fsimage上传到到Active NameNode，接着清空掉Active NameNode的旧的edits log文件，这里可能都有100万行修改日志了！

然后Active NameNode继续接收修改元数据的请求，再写入edits log，写了一小会儿，这里可能就几十行修改日志而已！

如果说此时，Active NameNode重启了，bingo！没关系，只要把Standby NameNode传过来的fsimage直接读到内存里，**这个fsimage直接就是元数据**，不需要做任何额外操作，纯读取，效率很高！

然后把新的edits log里少量的几十行的修改日志回放到内存里就ok了！

这个过程的启动速度就快的多了！因为不需要回放大量上百万行的edits log来恢复元数据了！

## 6.Hadoop的应用

### MapReduce应用程序概述

MapReduce是Hadoop的分布式计算框架，MapReduce应用程序首先需要声明数据的输入和输出路径（一般为HDFS路径），然后实现接口或抽象类的Map和Reduce函数，最后加上作业配置（Job Configuration ），就完成了MapReduce应用程序的构建。

应用程序由Hadoop的Job Client将作业（JAR包或可执行程序）和配置信息提交给集群Master上的JobTracker, Job Tracker负责分发这些作业和配置信息给集群中的Slave,调度任务并监控它们的执行，同时提供状态和诊断信息给Job Client。

### MapReduce应用程序的输入与输出模型

MapReduce框架运行的基础数据模型是＜Key,Value＞键值对，MapReduce框架要求作业的输入为一组＜Key,Value＞链’值对，经过Map（）、Reduce（）计算，输出的结果也是一组＜Key,Value＞键值对，这两组键值对的类型可能不同。

MapReduce框架需要对Key和Value对应的类进行序列化操作，因此，这些类需要实现Hadoop提供的Writable接口。另外，为了方便MapReduce框架执行排序操作，Key类必须实现WritableComparable接口。

## 7.HDFS TB级文件上传

### **Chunk缓冲机制**

首先，数据会被写入一个chunk缓冲数组，这个**chunk是一个512字节大小的数据片段**，然后这个缓冲数组可以容纳多个chunk大小的数据在里面缓冲。

这个缓冲，首先就可以让客户端快速的写入数据了，不至于说几百字节就要进行一次网络传输.

### **Packet数据包机制**

接着，当chunk缓冲数组都写满了之后，就会把这个chunk缓冲数组进行一下**chunk切割**，切割为一个一个的chunk，一个chunk是一个数据片段。然后多个chunk会直接一次性写入另外一个内存缓冲数据结构，就是**Packet数据包**。

一个Packet数据包，设计为可以容纳127个chunk，大小大致为64mb。所以说大量的chunk会不断的写入Packet数据包的内存缓冲中。

通过这个Packet数据包机制的设计，又可以在内存中容纳大量的数据，**进一步避免了频繁的网络传输影响性能。**

### **内存队列异步发送机制**

**当一个Packet被塞满了chunk之后，就会将这个Packet放入一个内存队列来进行排队。**

然后有一个DataStreamer线程会不断的获取队列中的Packet数据包，通过网络传输直接写一个Packet数据包给DataNode。

如果一个Block默认是128mb的话，那么一个Block默认会对应两个Packet数据包，每个Packet数据包是64MB。

也就是说，**传送两个Packet数据包给DataNode之后，就会发一个通知说，一个Block的数据都传输完毕。**

这样DataNode就知道自己收到一个Block了，里面包含了人家发送过来的两个Packet数据包。