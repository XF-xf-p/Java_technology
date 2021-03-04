# ZooKeeper

## 1.Zookeeper概念

ZooKeeper是一个分布式协调服务，其设计的初衷是为分布式软件提供一致性服务。Zoo Keeper提供了一个类似Linux文件系统的树形结构，ZooKeeper的每个节点既可以是目录也可以是数据，同时，ZooKeeper提供了对每个节点的监控与通知机制基于ZooKeeper一致性服务，可以方便地实现分布式锁、分布式选举、服务发现和监控、配置中心等功能。

## 2.ZooKeeper的角色

ZooKeeper是一个基于主从复制的高可用集群，ZooKeeper的角色包括Leader、Follower、Observer。Leader是集群主节点，主要负责管理集群状态和接收用户的写请求；Follower是从节点，主要负责集群选举投票和接收用户的读请求；Observer的功能与Follower类似，只是没有投票权，主要用于分担Follower的读请求，降低集群的负载。

### Leader

一个运行中的ZooKeeper集群只有一个Leader服务，Leader服务主要有两个职责：一是负责集群数据的写操作；二是发起并维护各个Follower及Observer之间的心跳以监控集群的运行状态。在ZooKeeper集群中，所有的写操作都必须经过Leader，只有Leader写操作完成后，才将写操作广播到其他Follower。只有超过半数节点（不包括Observer节点）写入成功时，该写请求才算写成功。

### Follower

一个ZooKeeper集群可以有多个Follower，Follower通过心跳和Leader保持连接。Follower服务主要有两个职责：一是负责集群数据的读操作，二是参与集群的Leader选举。Follower在接收到一个客户端请求后会先判断该请求是读请求是写请求，如果是读请求，则Follower从本地节点上读取数据并返回给客户端；如果是写请求，则Follower会将写请求转发给Leader来处理；同时，在Leader失效后，Follower需要在集群选举时进行投票。

### Observer

一个ZooKeeper集群可以有多个Observer，Observer主要职责是负责集群数据的读操作。在ZooKeeper的设计上，Observer的功能与Follower类似，主要的差别是Observer无投票权。ZooKeeper集群在运行过程中要支持更多的客户端并发的操作，就需要增加更多的服务实例，而过多的服务实例会使集群的投票阶段变得复杂，集群选主时间过长，不利于集群故障的快速恢复。因此，ZooKeeper引入了Observer角色，Observer不参与投票，只用来接收客户端的连接并响应客户端的读请求，将写请求转发给Leader节点。加入更多的Observer节点，不仅提高了ZooKeeper集群的吞吐量，也保障了系统的稳定性。

## 3.ZAB协议

ZAB ( ZooKeeper Atomic Broadcast）即ZooKeeper原子消息广播协议。该协议主要通过唯一的事务编号Zxid( ZooKeeper Transaction id）保障集群状态的唯一性。Zxid与RDBMS中的事务id类似，用于标识一次提议（Proposal）的id；为了保证顺序性，Zxid必须单调递增。

( 1 ) Epoch: Epoch指当前集群的周期号（年代号），集群的每次Leader变更都会产生一个新的周期号，周期号的产生规则是在上一个周期号的基础上加1，这样当之前的Leader崩溃恢复后会发现自己的周期号比当前的周期号小，说明此时集群已经产生了新的Leader，旧的Leader会再次以Follower的角色加入集群。

( 2) Zxid: Zxid指ZAB协议的事务编号，它是一个64位的数字，其中低32位存储的是一个简单的单调递增的计数器，针对客户端的每一个事务请求，计数器都加1。高32位存储的是Leader的周期号Epoch。每次选举产生一个新的Leader时，该Leader都会从当前服务器的日志中取出最大事务的Zxid，获取其中高32位的Epoch值并加1，以作为新的Epoch，并将低32位从0开始重新计数.

ZA协议有两种模式，分别是恢复模式（集群选主）和广播模式（数据同步）。

( 1 ）恢复模式：当集群启动、集群重启或者Leader崩溃后，集群将开始选主，该过程为恢复模式。

( 2 ）广播模式：当Leader被选举出来后，Leader将最新的集群状态广播给其他Follower，该过程为广播模式。在半数以上的Follower完成与Leader的状态同步后，广播模式结束。

### ZAB协议的4个阶段

( 1 ）选举阶段（Leader Election ）：在集群选举开始时，所有节点都处于选举阶段。当某一个节点的票数超过半数节点后，该节点将被推选为准Leader。选举阶段的目的就是产生一个准Leader。只有到达广播阶段（Broadcast）后，准Leader才会成为真正的Leader。

( 2 ）发现阶段（Discovery）： 在发现阶段，各个Follower开始和准Leader进行通信，同步Follower最近接收的事务提议。这时，准Leader会产生一个新的Epoch，并尝试让其他Follower接收该Epoch后再更新到本地。发现阶段的一个Follower只会连接一个Leader，如果节点1认为节点2是Leader，则当节点1尝试连接节点2时，如果连接被拒绝，则集群会进入重新选举阶段。发现阶段的主要目的是发现当前大多数节点接收的最新提议。

( 3 ）同步阶段（Synchronizatation）：同步阶段主要是将Leader在前一阶段获得的最新提议信息同步到集群中所有的副本，只有当半数以上的节点都同步完成时，准Leader才会成为真正的Leader。Follower只会接收Zxid比自己的lastZxid大的提议。同步阶段完成后集群选主的操作才完成，新的Leader将产生。

( 4 ）广播阶段（Broadcast）：在广播阶段，ZooKeeper集群开始正式对外提供事务服务，这时Leader进行消息广播，将其上的状态通知到其他Follower，如果后续有新的节点加入，则Leader会对新节点进行状态的同步。

### ZAB协议的Java实现

ZAB协议的Java实现与其定义略有不同，在实际实现时，选段采用Fast Leader Election模式。在该模式下，节点首先向所有Server提议自己要成为Leader，当其他Server收到提议以后，判断Epoch信息并接收对方的提议，然后向对方发送接收提议完成的消息；同时，在Java的实现过程中将发现阶段和同步阶段合并为恢复阶段（Reovery）。因此，ZAB协议的Java实现只有3个阶段Fast Leader Election、Recovery和Broadcast。

## 3.ZooKeeper的选举机制和流程

ZooKeeper的选举机制被定义为：每个Server首先都提议自己是Leader，并为自己投票，然后将投票结果与其他Server的选票进行对比，权重大的胜出，使用权重较大的选票更新自身的选票箱，具体选举过程如下。

( 1 ）每个Server启动以后都询问其他Server给谁投票，其他Server根据自己的状态回复自己推荐的Leader并返回对应的Leader id和Zxid。在集群初次启动时，每个Server都会推荐自己为Leader。

( 2 ）当Server收到所有其他Server的回复后，计算出Zxid最大的Server，并将该Server设置成下一次要投票推荐的Server。

( 3 ）计算过程中票数最多的Server将成为获胜者，如果获胜者的票数超过集群个数的一半，则该Server将被推选为Leader，否则，继续投票，直到Leader被选举出来。

( 4 ) Leader等待其他Server连接。

( 5 ) Follower连接Leader，将最大的Zxid发送给Leader。

( 6) Leader根据Follower的Zxid确定同步点，至此，选举阶段完成。

( 7) 在选举阶段完成后，Leader通知其他Follower集群已经成为Uptodate状态，

( 8) Follower收到Uptodate消息后，接收Client的请求并开始对外提供服务。

下面以5台服务器Server1、Server2、Server3、Server4、Servers5的选主为例，假设它们的编号分别是1、2、3、4、5，按编号依次启动，它们的选举过程。

( 1 ) Server1启动：Server1提议自己为Leader并为自己投票，然后将投票结果发送给其他服务器，由于其他服务器还未启动，因此收不到任何反馈信息.此时Server1处于Looking状态（Looking状态表示当前集群正处于选举状态）。

( 2) Server2启动：Server2提议自己为Leader为自己投票，然后与Server1交换投票结果，由于Server2的编号大于Server1，因此Server2胜出。同时，由于投票未过半，两个服务器均处于Looking状态。

( 3 ) Server3启动：Server3提议自己为Leader并为自己投票，然后与Server1、Server2交换投票结果，由于Server3编号最大，因此Server3胜出。此时Server3票数大于半数集群，因此Server3成为Leader，Server1、Server2成为Follower。

( 4) Server4启动：Server4提议自己为Leader并为自己投票，然后与Server1、Server2、Server3交换投票结果，发现Server3已经成为Leader，因此Server4成为Follower。

( 5 ) Servers5启动：首先给自己投票，然后与其他服务器交换信息，发现Server3已经成为Leader，因此Server5成为Follower。

## 4.Zookeeper工作原理（原子广播）

1.Zookeeper的核心是原子广播，这个机制保证了各个server之间的同步。实现这个机制的协议叫做Zab协议。Zab协议有两种模式，它们分别是恢复模式和广播模式。

2.当服务启动或者在领导者崩溃后，Zab就进入了恢复模式，当领导者被选举出来，且大多数server的完成了和leader的状态同步以后，恢复模式就结束了。

3.状态同步保证了leader和server具有相同的系统状态.

4.一旦leader已经和多数的follower进行了状态同步后，他就可以开始广播消息了，即进入广播状态。这时候当一个server加入zookeeper服务中，它会在恢复模式下启动，发现leader，并和leader进行状态同步。待到同步结束，它也参与消息广播。Zookeeper服务一直维持在Broadcast状态，直到leader崩溃了或者leader失去了大部分的followers支持。

5.广播模式需要保证proposal被按顺序处理，因此zk采用了递增的事务id号(zxid)来保证。所有的提议(proposal)都在被提出的时候加上了zxid。

6.实现中zxid是一个64为的数字，它高32位是epoch用来标识leader关系是否改变，每次一个leader被选出来，它都会有一个新的epoch。低32位是个递增计数。

7.当leader崩溃或者leader失去大多数的follower，这时候zk进入恢复模式，恢复模式需要重新选举出一个新的leader，让所有的server都恢复到一个正确的状态。

## 5.Znode有四种形式的目录节点

1.PERSISTENT：持久的节点。

2.EPHEMERAL：暂时的节点。

3.PERSISTENT_SEQUENTIAL：持久化顺序编号目录节点。

4.EPHEMERAL_SEQUENTIAL：暂时化顺序编号目录节点。

## 6.ZooKeeper的数据模型

ZooKeeper使用一个树形结构的命名空间来表示其数据结构，在结构上与标准文件系统非常相似，ZooKeeper树中的每个节点都被称为一个Znode。ZooKeeper的数据结构类似文件系统的目录树，ZooKeeper树中的每个节点都可以拥有子节点；与文件系统不同是，ZooKeeper的每个节点都存储了数据信息，同时提供对节点信息的监控（Watch）等操作。

### Znode的数据模型

ZooKeeper命名空间中的Znode兼具文件和目录两种特点。它既能像文件一样保存和维护数据，又能像目录一样作为路径标识的一部分。每个Znode都由3部分组成.

( 1 ) Stat：状态信息，用于存储该Znode的版本、权限、时间戳等信息。

( 2 ) Data: Znode上具体存储的数据。

( 3 ) Children: Znode子节点的信息描述。

Znode节点虽然可以存储数据，但它并不能像数据库那样存储大量的数据。设计Znode的初衷是存储分布式应用中的配置文件、集群状态等元数据信息。

### Znode的控制访问

(1) ACL：每一个Znode节点都拥有一个访问控制列表（Access Control List, ACL ) , 该列表规定了用户对节点的访问权限，应用程序可以根据需求将用户分为只读、只写和读写用户。

(2 ）原子操作：每一个Znode节点上的数据都具有原子操作的特性，读操作将获取与节点相关的数据，写操作将替换节点上的数据。

### Znode的节点类型

ZooKepe中的节点有两种，分别为临时节点和永久节点。节点的类型在创建时被确定并且不能改变。

( 1 ）临时节点：临时节点的生命周期取决于过期时间，当临时节点过期后系统会自除该节点，此外，ZooKeeper的临时节点不允许拥有子节点。临时节点常用于心跳监控，例如，可以设置过期时间为30s，要求各个子节点对应的服务端每5s发送一次心跳到ZooKeeer集群，当有服务端连续30s没有向ZooKeeper汇报心跳信息（连续6次未收到心跳信息，6次×5s=30s），就可以认为该节点宕机，将其从服务列表中移除。

( 2 ）永久节点：永久节点的数据会一直存储，直到用户调用接口将其数据删除。该节点一般用于存储一些永久性的配置信息。

### Znode的节点Watch

ZooKeeper的每个节点上都有Watch用于监控节点数据的变化，当节点状态发生改变时（Znode新增、删除、修改）将会触发Watch所对应的操作。在Watch被触发时，Zoo Keeper会向监控该节点的客户端发送一条通知说明节点的变化情况

## 7.Zookeeper的安装

( 1）下载ZooKeeper安装包：到ZooKeeper官网下载最新的安装包。

( 2）解压ZooKeeper安装包：到ZooKeeper下载目录执行以下命令解压安装包

```
tar -xzvf apache-zookeeper-3.5.5-bin 
```

( 3 ）配置ZooKeeper：到ZooKeeper配置文件目录执行以下命令并复制一份配置文件。

```
cd apache-zookeeper-3.5.5-bin／conf/
cp zoo_sample.cfg zoo.cfg 
```

ZooKeeper配置文件的核心配置如下，其中，dataDir是ZooKeeper数据文件存储的目录，ClientPort为ZooKeeper监听的端口号。

```
#ZooKeeper数据存储的目录
dataDir=/tmp/zookeeper
#ZooKeeper监听的端口
clientPort=2181
```

( 4）启动ZooKeeper：到ZooKeeper安装目录执行start命令启动集群。

```
bin/zkServer.sh start 
```

( 5）安装ZooKeeper集群：ZooKeeper集群的安装需要在前4步的基础上执行。

1.创建myid文件：在每个ZooKeeper的conf目录下都执行以下命令创建myid文件，并设服务器编号，该编号为集群选主时服务的唯一标识。

```
＃创建myid文件
touch myid 
＃设置服务编号，在第一个节点上设置为0，第二个节点设置为1，依次类推。
0＞myid
```

2.配置zoo.cfg：在zoo.cfg中加入集群的配置信息。其中，server后面的数字（0、1、2）为myid中的集群编号.

```
server.0=192.168.1.2:2888:3888 
server.1=192.168.1.2:2888:3888 
server.2=192.168.1.2:2888:3888
```

## 8.Zookeeper的应用场景

ZooKeeper可用于统一命名服务、配置管理、集群管理、分布式通知协调、分布式锁等应用场景。

### 统一命名服务

在分布式环境下，应用程序经常需要对服务进行统一命名，以便识别不同的服务和快速获取服务列表，应用程序可以将服务名称和服务地址信息维护在ZooKeeper上，客户端通过ZooKeeper获取可用服务列表。

### 配置管理

在分布式环境下，应用程序可以将配置文件统一在ZooKeeper上管理。配置信息可以按照系统配置、告警配置、业务开关配置、业务阈值配置等分类存储在不同的Znode上。 各个服务在启动的时候从ZooKeeper上读取配置，同时监听各个节点的Znode数据，一且Znode中的配置被修改，ZooKeeper就将通知各个服务然后在线更新配置。

使用ZooKeeper做统一配置管理，不但避免了维护散落在各个服务器上的配置文件的复杂性，同时在配置信息变化的时候能及时通知各个服务在线更新配置，而不用重启服务。

### 集群管理

在分布式环境下，实时管理每个服务的状态是ZooKeeper使用最广的场景，常见的HBase、Kafka、Storm、HDFS等集群都依赖ZooKeeper做统一的状态管理。

### 分布式通知协调

基于Znode的临时节点和Watch特性，应用程序可以很容易地实现一个分布式通知协调系统。比如在集群中为每个服务都创建一个周期为30s的临时节点作为服务状态监控，要求各个服务每10s定时向ZooKeeper汇报监控状态。当ZooKeeper连续30s未收到服务的状态反馈时，则可以认为该服务异常，将其从服务列表移除，同时将该结果通知到他监控节点状态的服务。

### 分布式锁

由于ZooKeeper是强一致性的，多个客户端同时在ZooKeeper上创建相同的Znode时，只有一个能创建成功。基于该机制，应用程序可以实现锁的独占性，当多个客户端同时在ZooKeeper上创建相同的Znode时，创建成功的那个客户端将得到锁，其他客户端则等待。

同时将锁节点设置为EPHEMERAL_SEQUENTIAL，这样该Znode可掌握全局锁的访问时序。

## 9.ZooKeeper节点的操作

下面以SpringBoot为例介绍ZooKeeper节点的操作。

( 1 ）引入pom.xml依赖

( 2 ）添加配置文件：在appIication. properties配置文件中添加如下ZooKeeper配置。其中，zookeeper.server为服务地址，zookeeper.namespace为命令空间。

( 3 ）连接Zookeeper客户端

通过CuratorFrameworkFactory.builder（）构建一个ZooKeeper客户端并启动，客户端启动后便可以基于该客户端来操作ZooKeeper的Znode节点。

( 4 ）创建ZooKeeper节点：其中比较重要的是对节点类型CreateMode的定义。ZooKeeper支持以下4种节点类型.

1.PERSISTENT：持久化目录节点。

2.PERSISTENT_SEQUENTIAL ：按顺序自动编号的持久化目录节点，自动编号的规则为上个节点编号加1。

3.EPHEMERAL: 临时目录节点。在创建该节点的客户端和服务端连接Session超时后，该节点及其数据将会被自动删除。

4.EPHEMERAL_SEQUENTIAL: 临时自动编号节点，当创建该节点的客户端和服务端连接Session超时后，该节点及其数据将会被自动删除，自动编号的规则为上个节点编号加1。

( 5 ）查询ZooKeeper节点：其中ZooKeeper查询到的节点数据信息是字节码数组，需要应用程序按照具体的需求转换为对应类型的数据。

( 6 ）更新ZooKeeper节点的数据。

client.setData().forPath(path, datas); 

( 7 ) ZooKeeper节点的删除操作

deleteNode(path, true); 

( 8 ）监听ZooKeeper节点的数据：在监听节点数据的时候，可以设置线程池来提高系统的并发度。设置了节点监听后，当节点数据变化时会触发该操作。

```
ExecutorService pool= Executors.newFxedThreadPool(2);
TreeCache cache= new TreeCache(clwatchPath); 
cache.getListenable().addListener(listener,pool); 
```



