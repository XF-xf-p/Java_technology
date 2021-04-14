# Kafka消息中间件内核源码深度剖析

### 001_Kafka源码分析环境搭建：JDK、Scala以及Gradle的安装

kafka核心原理、生产部署、开发使用、线上规划，在上一个课程都已经讲完了，一般的场景使用kafka来开发都能搞定了，kafka异常报错 & 各种技术难题对应的解决方案

先阅读kafka的源码

kafka线上生产高阶故障处理、性能优化以及解决方案

我在自己本地很早都已经搭建好了kafka源码分析的环境了，但是我会把完整的步骤给出来，大家参照着就可以一步一步的把kafka源码的环境搭建起来，读源码就很方便了

在win上安装JDK 1.8，这个不用多说了

在win上安装Scala 2.10.6，上官网找到2.10.6版本对应的下载地址，kafka的服务器端的源码是scala写的，但是新版本的客户端的源码是java写的

https://www.scala-lang.org/download/2.10.6.html

然后就可以下载win上的安装包，scala.msi，下载好之后傻瓜式安装就可以了，接着必须配置SCALA_HOME和PATH两个环境变量，首先必须得有Java和Scala两种编程语言的支持才可以

接着需要安装Gradle，现在国外很多知名的开源项目，项目构建（依赖管理、打包、远程部署、运行单元测试、静态代码检测，写好了java源代码，接着要把这个写好的代码打包存储起来，或者准备部署，项目构建），不是用maven了

都是用Gradle来进行项目的构建了，Kafka也是如此，所以需要安装Gradle来完成Kafka源码的构建，使用gradle 3.1，从官网下载gradle-3.1-bin.zip，解压缩即可，然后配置GRADLE_HOME和PATH

就比如说你的kafka的源码写好了之后，现在要打包，打成一个压缩包提供给我们来下载，下载了之后就可以在本地安装和部署kafka了

https://gradle.org/releases/

验证三个基础的依赖都正确安装了

java -version

scala -version

gradle -version

### 002_Kafka源码分析环境搭建：在Windows上部署和启动ZooKeeper

要搭建分析kafka源码的环境，那么zookeeper是必须使用的，因为需要在win本地以源码的方式来启动kafka，那么kafka依赖zookeeper，所以直接用之前hadoop课程里的zk 3.4.9即可，在win上解压缩，修改zoo.cfg里的dataDir指向本地win目录

然后执行bin目录下的zkServer.cmd命令启动即可

### 003_Kafka源码分析环境搭建：使用Gradle来构建Kafka源码 

从kafka官网下载kafka-0.10.0.1版本的源码，现在还是0.10.x版本用的人比较多，所以先分析这个版本的源码即可，其实核心的基本都是类似的，也算是比较新的版本了，下载的是kafka-0.10.0.1-src.tgz源码压缩包，解压缩 

http://kafka.apache.org/downloads 

通过win命令行进入kafka-0.10.0.1-src目录下，然后执行“gradle idea”为源码导入idea进行构建，如果要导入eclipse，那么就执行“gradle eclipse”，但是我们还是用idea来查看源码就可以了 

这个过程会下载大量的依赖jar包，建议是可以连通国外的网，比如一些vpn，这样速度快，过程看起来如下图：

![](D:\workspace\Java-Interview-Offer\images\003-01.jpg)  

需要耐心等待 

这个过程中可能会报错，比如下面的错误：

![](D:\workspace\Java-Interview-Offer\images\003-02.jpg)  

解决方案如下： 

用编辑器打开build.gradle文件，加入以下内容： 

ScalaCompileOptions.metaClass.daemonServer = true

ScalaCompileOptions.metaClass.fork = true

ScalaCompileOptions.metaClass.useAnt = false

ScalaCompileOptions.metaClass.useCompileDaemon = false 

如下图：

![](D:\workspace\Java-Interview-Offer\images\003-03.jpg) 

最后成功了如下图：

![](D:\workspace\Java-Interview-Offer\images\003-04.jpg) 

### 004_Kafka源码分析环境搭建：将构建好的Kafka源码导入IntelliJ IDEA中 

构建好了kafka源码之后 

首先你需要进入IntelliJ IDEA的这个界面：

![](D:\workspace\Java-Interview-Offer\images\004-01.jpg)  

在右下方有一个“Configure”，里面有个“Settings”，进入那个界面

![](D:\workspace\Java-Interview-Offer\images\004-02.jpg) 

左侧有一个“Plugins”，搜索scala相关的插件，此时一开始是找不到的，然后点击“search in repositories”，找到一个“Scala”插件，他的类别是“Language”，在线装即可，他会下载之后安装

![](D:\workspace\Java-Interview-Offer\images\004-03.jpg)  

安装完了在plugins里面就可以找到scala插件了，然后点击“ok”就会提示你重启intellij idea来激活安装好的插件 

然后点击里面的那个Import Project按钮即可，选择你的kafka源码所在的目录，选择你构建项目的方式是“gradle”，导入的过程也需要不少的时间，需要耐心等待，会显示的是如下的图：

![](D:\workspace\Java-Interview-Offer\images\004-04.jpg)  

最后导入成功了，应该是如下图：

![](D:\workspace\Java-Interview-Offer\images\004-05.jpg) 

### 005_Kafka源码分析环境搭建：对IntelliJ IDEA中的Kafka进行正确配置 

我们肯定是要看到log4j输出的日志的，所以必须把config目录下的log4j.properties给放到src/main/scala目录下去，这样才能看到服务端运行起来的程序打印出来的日志，另外 需要修改 config目录下的server.properties 

这个文件中只有一个配置项是必须修改的，那就是log.dirs，这个是配置kafka的日志存储目录的，可以配置成win下的一个目录即可，别的不用修改，zk配置默认连接本机的2181端口的 

如果我们要在IntelliJ IDEA里启动kafka，通过源码的方式来启动 

此外，Kafka的启动类是“kafka.Kafka”，他是要读取“server.properties”文件的，必须给他指定这个文件的所在位置才可以，在上方的菜单栏里，有一个“Run”菜单，点击后，里面有一个“Edit Configuration”菜单，点击这个

![](D:\workspace\Java-Interview-Offer\images\005-01.jpg)  

出现上图之后，选择“+”号，然后选择“Application”，“Name”输入为“Kafka”，“Main Class”输入为“kafka.Kafka”，“Program arguments”输入为“config/server.properties”，“use classpath of module”输入为“core_main”

![](D:\workspace\Java-Interview-Offer\images\005-02.jpg) 

### 006_Kafka源码分析环境搭建：直接在IntelliJ IDEA中启动Kafka

上一讲配置完毕了kafka之后，包括他的主类，配置文件，使用的是core module，等等，接着右上方会出现下图：

![](D:\workspace\Java-Interview-Offer\images\006-01.png)  

如果你要启动kafka，就点击这里的绿色箭头按钮即可，他就会启动kafka.Kafka这个主类，第一次启动，会重新编译整个项目，燃火才会正常启动

![](D:\workspace\Java-Interview-Offer\images\006-02.jpg)  

这里有很多的warning，不过是不要紧的，编译完成之后，kafka就会成功启动，我们来分析一下他的启动日志： 

[2019-04-16 11:44:20,079] INFO KafkaConfig values: 

​     advertised.host.name = null

​     metric.reporters = []

​     quota.producer.default = 9223372036854775807

​     offsets.topic.num.partitions = 50

​     log.flush.interval.messages = 9223372036854775807

​     auto.create.topics.enable = true

​     controller.socket.timeout.ms = 30000

​     log.flush.interval.ms = null

​     principal.builder.class = class org.apache.kafka.common.security.auth.DefaultPrincipalBuilder

​     replica.socket.receive.buffer.bytes = 65536

​     min.insync.replicas = 1

​     replica.fetch.wait.max.ms = 500

​     num.recovery.threads.per.data.dir = 1

​     ssl.keystore.type = JKS

​     sasl.mechanism.inter.broker.protocol = GSSAPI

​     default.replication.factor = 1

​     ssl.truststore.password = null

​     log.preallocate = false

​     sasl.kerberos.principal.to.local.rules = [DEFAULT]

​     fetch.purgatory.purge.interval.requests = 1000

​     ssl.endpoint.identification.algorithm = null

​     replica.socket.timeout.ms = 30000

​     message.max.bytes = 1000012

​     num.io.threads = 8

​     offsets.commit.required.acks = -1

​     log.flush.offset.checkpoint.interval.ms = 60000

​     delete.topic.enable = false

​     quota.window.size.seconds = 1

​     ssl.truststore.type = JKS

​     offsets.commit.timeout.ms = 5000

​     quota.window.num = 11

​     zookeeper.connect = localhost:2181

​     authorizer.class.name = 

​     num.replica.fetchers = 1

​     log.retention.ms = null

​     log.roll.jitter.hours = 0

​     log.cleaner.enable = true

​     offsets.load.buffer.size = 5242880

​     log.cleaner.delete.retention.ms = 86400000

​     ssl.client.auth = none

​     controlled.shutdown.max.retries = 3

​     queued.max.requests = 500

​     offsets.topic.replication.factor = 3

​     log.cleaner.threads = 1

​     sasl.kerberos.service.name = null

​     sasl.kerberos.ticket.renew.jitter = 0.05

​     socket.request.max.bytes = 104857600

​     ssl.trustmanager.algorithm = PKIX

​     zookeeper.session.timeout.ms = 6000

​     log.retention.bytes = -1

​     log.message.timestamp.type = CreateTime

​     sasl.kerberos.min.time.before.relogin = 60000

​     zookeeper.set.acl = false

​     connections.max.idle.ms = 600000

​     offsets.retention.minutes = 1440

​     replica.fetch.backoff.ms = 1000

​     inter.broker.protocol.version = 0.10.0-IV1

​     log.retention.hours = 168

​     num.partitions = 1

​     broker.id.generation.enable = true

​     listeners = null

​     ssl.provider = null

​     ssl.enabled.protocols = [TLSv1.2, TLSv1.1, TLSv1]

​     log.roll.ms = null

​     log.flush.scheduler.interval.ms = 9223372036854775807

​     ssl.cipher.suites = null

​     log.index.size.max.bytes = 10485760

​     ssl.keymanager.algorithm = SunX509

​     security.inter.broker.protocol = PLAINTEXT

​     replica.fetch.max.bytes = 1048576

​     advertised.port = null

​     log.cleaner.dedupe.buffer.size = 134217728

​     replica.high.watermark.checkpoint.interval.ms = 5000

​     log.cleaner.io.buffer.size = 524288

​     sasl.kerberos.ticket.renew.window.factor = 0.8

​     zookeeper.connection.timeout.ms = 6000

​     controlled.shutdown.retry.backoff.ms = 5000

​     log.roll.hours = 168

​     log.cleanup.policy = delete

​     host.name = 

​     log.roll.jitter.ms = null

​     max.connections.per.ip = 2147483647

​     offsets.topic.segment.bytes = 104857600

​     background.threads = 10

​     quota.consumer.default = 9223372036854775807

​     request.timeout.ms = 30000

​     log.message.format.version = 0.10.0-IV1

​     log.index.interval.bytes = 4096

​     log.dir = /tmp/kafka-logs

​     log.segment.bytes = 1073741824

​     log.cleaner.backoff.ms = 15000

​     offset.metadata.max.bytes = 4096

​     ssl.truststore.location = null

​     group.max.session.timeout.ms = 300000

​     ssl.keystore.password = null

​     zookeeper.sync.time.ms = 2000

​     port = 9092

​     log.retention.minutes = null

​     log.segment.delete.delay.ms = 60000

​     log.dirs = F:\development\kafka\logs

​     controlled.shutdown.enable = true

​     compression.type = producer

​     max.connections.per.ip.overrides = 

​     log.message.timestamp.difference.max.ms = 9223372036854775807

​     sasl.kerberos.kinit.cmd = /usr/bin/kinit

​     log.cleaner.io.max.bytes.per.second = 1.7976931348623157E308

​     auto.leader.rebalance.enable = true

​     leader.imbalance.check.interval.seconds = 300

​     log.cleaner.min.cleanable.ratio = 0.5

​     replica.lag.time.max.ms = 10000

​     num.network.threads = 3

​     ssl.key.password = null

​     reserved.broker.max.id = 1000

​     metrics.num.samples = 2

​     socket.send.buffer.bytes = 102400

​     ssl.protocol = TLS

​     socket.receive.buffer.bytes = 102400

​     ssl.keystore.location = null

​     replica.fetch.min.bytes = 1

​     broker.rack = null

​     unclean.leader.election.enable = true

​     sasl.enabled.mechanisms = [GSSAPI]

​     group.min.session.timeout.ms = 6000

​     log.cleaner.io.buffer.load.factor = 0.9

​     offsets.retention.check.interval.ms = 600000

​     producer.purgatory.purge.interval.requests = 1000

​     metrics.sample.window.ms = 30000

​     broker.id = 0

​     offsets.topic.compression.codec = 0

​     log.retention.check.interval.ms = 300000

​     advertised.listeners = null

​     leader.imbalance.per.broker.percentage = 10

 (kafka.server.KafkaConfig)

[2019-04-16 11:44:20,251] INFO starting (kafka.server.KafkaServer)

[2019-04-16 11:44:20,283] INFO Connecting to zookeeper on localhost:2181 (kafka.server.KafkaServer)

[2019-04-16 11:44:20,423] INFO Starting ZkClient event thread. (org.I0Itec.zkclient.ZkEventThread)

[2019-04-16 11:44:29,485] INFO Client environment:zookeeper.version=3.4.6-1569965, built on 02/20/2014 09:09 GMT (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,485] INFO Client environment:host.name=DESKTOP-FV82PGK (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,485] INFO Client environment:java.version=1.8.0_151 (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,485] INFO Client environment:java.vendor=Oracle Corporation (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,485] INFO Client environment:java.home=F:\development\Java\jdk1.8.0_151\jre (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,485] INFO Client environment:java.class.path=F:\development\Java\jdk1.8.0_151\jre\lib\charsets.jar;F:\development\Java\jdk1.8.0_151\jre\lib\deploy.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\access-bridge-64.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\cldrdata.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\dnsns.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\jaccess.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\jfxrt.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\localedata.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\nashorn.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\sunec.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\sunjce_provider.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\sunmscapi.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\sunpkcs11.jar;F:\development\Java\jdk1.8.0_151\jre\lib\ext\zipfs.jar;F:\development\Java\jdk1.8.0_151\jre\lib\javaws.jar;F:\development\Java\jdk1.8.0_151\jre\lib\jce.jar;F:\development\Java\jdk1.8.0_151\jre\lib\jfr.jar;F:\development\Java\jdk1.8.0_151\jre\lib\jfxswt.jar;F:\development\Java\jdk1.8.0_151\jre\lib\jsse.jar;F:\development\Java\jdk1.8.0_151\jre\lib\management-agent.jar;F:\development\Java\jdk1.8.0_151\jre\lib\plugin.jar;F:\development\Java\jdk1.8.0_151\jre\lib\resources.jar;F:\development\Java\jdk1.8.0_151\jre\lib\rt.jar;F:\development\kafka\kafka-0.10.0.1-src\core\out\production\classes;F:\development\kafka\kafka-0.10.0.1-src\clients\out\production\classes;C:\Users\lixue\.gradle\caches\modules-2\files-2.1\net.sf.jopt-simple\jopt-simple\4.9\ee9e9eaa0a35360dcfeac129ff4923215fd65904\jopt-simple-4.9.jar;C:\Users\lixue\.gradle\caches\modules-2\files-2.1\com.yammer.metrics\metrics-core\2.2.0\f82c035cfa786d3cbec362c38c22a5f5b1bc8724\metrics-core-2.2.0.jar;C:\Users\lixue\.gradle\caches\modules-2\files-2.1\org.scala-lang\scala-library\2.10.6\421989aa8f95a05a4f894630aad96b8c7b828732\scala-library-2.10.6.jar;C:\Users\lixue\.gradle\caches\modules-2\files-2.1\org.slf4j\slf4j-log4j12\1.7.21\7238b064d1aba20da2ac03217d700d91e02460fa\slf4j-log4j12-1.7.21.jar;C:\Users\lixue\.gradle\caches\modules-2\files-2.1\com.101tec\zkclient\0.8\c0f700a4a3b386279d7d8dd164edecbe836cbfdb\zkclient-0.8.jar;C:\Users\lixue\.gradle\caches\modules-2\files-2.1\org.apache.zookeeper\zookeeper\3.4.6\1b2502e29da1ebaade2357cd1de35a855fa3755\zookeeper-3.4.6.jar;C:\Users\lixue\.gradle\caches\modules-2\files-2.1\net.jpountz.lz4\lz4\1.3.0\c708bb2590c0652a642236ef45d9f99ff842a2ce\lz4-1.3.0.jar;C:\Users\lixue\.gradle\caches\modules-2\files-2.1\org.xerial.snappy\snappy-java\1.1.2.6\48d92871ca286a47f230feb375f0bbffa83b85f6\snappy-java-1.1.2.6.jar;C:\Users\lixue\.gradle\caches\modules-2\files-2.1\org.slf4j\slf4j-api\1.7.21\139535a69a4239db087de9bab0bee568bf8e0b70\slf4j-api-1.7.21.jar;C:\Users\lixue\.gradle\caches\modules-2\files-2.1\log4j\log4j\1.2.17\5af35056b4d257e4b64b9e8069c0746e8b08629f\log4j-1.2.17.jar;F:\development\JetBrains\IntelliJ IDEA Community Edition 2018.1.4\lib\idea_rt.jar (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,485] INFO Client environment:java.library.path=F:\development\Java\jdk1.8.0_151\bin;C:\WINDOWS\Sun\Java\bin;C:\WINDOWS\system32;C:\WINDOWS;C:\Program Files (x86)\Intel\iCLS Client\;C:\ProgramData\Oracle\Java\javapath;C:\Program Files\Intel\iCLS Client\;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Program Files (x86)\NVIDIA Corporation\PhysX\Common;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;C:\Program Files\MySQL\MySQL Utilities 1.6\;C:\WINDOWS\System32\OpenSSH\;C:\Program Files\Intel\WiFi\bin\;C:\Program Files\Common Files\Intel\WirelessCommon\;F:\development\Git\cmd;C:\Program Files (x86)\Intel\Intel(R) Management Engine Components\DAL;C:\Program Files\Intel\Intel(R) Management Engine Components\DAL;C:\Program Files (x86)\Intel\Intel(R) Management Engine Components\IPT;C:\Program Files\Intel\Intel(R) Management Engine Components\IPT;C:\Users\lixue\AppData\Local\Microsoft\WindowsApps;F:\development\Java\jdk1.8.0_151\bin;F:\development\Maven\apache-maven-3.5.2\bin;F:\development\gradle\gradle-2.10\bin;F:\development\hadoop-source-code;;. (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,490] INFO Client environment:java.io.tmpdir=C:\Users\lixue\AppData\Local\Temp\ (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,490] INFO Client environment:java.compiler=<NA> (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,490] INFO Client environment:os.name=Windows 10 (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,490] INFO Client environment:os.arch=amd64 (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,490] INFO Client environment:os.version=10.0 (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,490] INFO Client environment:user.name=zhonghuashishan (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,490] INFO Client environment:user.home=C:\Users\lixue (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,490] INFO Client environment:user.dir=F:\development\kafka\kafka-0.10.0.1-src (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:29,492] INFO Initiating client connection, connectString=localhost:2181 sessionTimeout=6000 watcher=org.I0Itec.zkclient.ZkClient@25359ed8 (org.apache.zookeeper.ZooKeeper)

[2019-04-16 11:44:30,546] INFO Waiting for keeper state SyncConnected (org.I0Itec.zkclient.ZkClient)

[2019-04-16 11:44:30,551] INFO Opening socket connection to server 0:0:0:0:0:0:0:1/0:0:0:0:0:0:0:1:2181. Will not attempt to authenticate using SASL (unknown error) (org.apache.zookeeper.ClientCnxn)

[2019-04-16 11:44:30,553] INFO Socket connection established to 0:0:0:0:0:0:0:1/0:0:0:0:0:0:0:1:2181, initiating session (org.apache.zookeeper.ClientCnxn)

[2019-04-16 11:44:30,722] INFO Session establishment complete on server 0:0:0:0:0:0:0:1/0:0:0:0:0:0:0:1:2181, sessionid = 0x16a23e965e60000, negotiated timeout = 6000 (org.apache.zookeeper.ClientCnxn)

[2019-04-16 11:44:30,724] INFO zookeeper state changed (SyncConnected) (org.I0Itec.zkclient.ZkClient)

[2019-04-16 11:44:31,319] INFO Log directory 'F:\development\kafka\logs' not found, creating it. (kafka.log.LogManager)

[2019-04-16 11:44:31,338] INFO Loading logs. (kafka.log.LogManager)

[2019-04-16 11:44:31,357] INFO Logs loading complete. (kafka.log.LogManager)

[2019-04-16 11:44:31,482] INFO Starting log cleanup with a period of 300000 ms. (kafka.log.LogManager)

[2019-04-16 11:44:31,487] INFO Starting log flusher with a default period of 9223372036854775807 ms. (kafka.log.LogManager)

[2019-04-16 11:44:31,499] WARN No meta.properties file under dir F:\development\kafka\logs\meta.properties (kafka.server.BrokerMetadataCheckpoint)

[2019-04-16 11:44:31,637] INFO Awaiting socket connections on 0.0.0.0:9092. (kafka.network.Acceptor)

[2019-04-16 11:44:31,647] INFO [Socket Server on Broker 0], Started 1 acceptor threads (kafka.network.SocketServer)

[2019-04-16 11:44:31,696] INFO [ExpirationReaper-0], Starting (kafka.server.DelayedOperationPurgatory$ExpiredOperationReaper)

[2019-04-16 11:44:31,701] INFO [ExpirationReaper-0], Starting (kafka.server.DelayedOperationPurgatory$ExpiredOperationReaper)

[2019-04-16 11:44:31,803] INFO Creating /controller (is it secure? false) (kafka.utils.ZKCheckedEphemeral)

[2019-04-16 11:44:31,940] INFO Result of znode creation is: OK (kafka.utils.ZKCheckedEphemeral)

[2019-04-16 11:44:31,942] INFO 0 successfully elected as leader (kafka.server.ZookeeperLeaderElector)

[2019-04-16 11:44:32,158] INFO [ExpirationReaper-0], Starting (kafka.server.DelayedOperationPurgatory$ExpiredOperationReaper)

[2019-04-16 11:44:32,163] INFO [ExpirationReaper-0], Starting (kafka.server.DelayedOperationPurgatory$ExpiredOperationReaper)

[2019-04-16 11:44:32,205] INFO [GroupCoordinator 0]: Starting up. (kafka.coordinator.GroupCoordinator)

[2019-04-16 11:44:32,206] INFO [GroupCoordinator 0]: Startup complete. (kafka.coordinator.GroupCoordinator)

[2019-04-16 11:44:32,221] INFO [Group Metadata Manager on Broker 0]: Removed 0 expired offsets in 18 milliseconds. (kafka.coordinator.GroupMetadataManager)

[2019-04-16 11:44:32,254] INFO [ThrottledRequestReaper-Produce], Starting (kafka.server.ClientQuotaManager$ThrottledRequestReaper)

[2019-04-16 11:44:32,258] INFO [ThrottledRequestReaper-Fetch], Starting (kafka.server.ClientQuotaManager$ThrottledRequestReaper)

[2019-04-16 11:44:32,272] INFO Will not load MX4J, mx4j-tools.jar is not in the classpath (kafka.utils.Mx4jLoader$)

[2019-04-16 11:44:32,361] INFO New leader is 0 (kafka.server.ZookeeperLeaderElector$LeaderChangeListener)

[2019-04-16 11:44:50,339] INFO Creating /brokers/ids/0 (is it secure? false) (kafka.utils.ZKCheckedEphemeral)

[2019-04-16 11:44:50,484] INFO Result of znode creation is: OK (kafka.utils.ZKCheckedEphemeral)

[2019-04-16 11:44:59,494] INFO Registered broker 0 at path /brokers/ids/0 with addresses: PLAINTEXT -> EndPoint(DESKTOP-FV82PGK,9092,PLAINTEXT) (kafka.utils.ZkUtils)

[2019-04-16 11:44:59,496] WARN No meta.properties file under dir F:\development\kafka\logs\meta.properties (kafka.server.BrokerMetadataCheckpoint)

[2019-04-16 11:44:59,561] WARN Error while loading kafka-version.properties :null (org.apache.kafka.common.utils.AppInfoParser)

[2019-04-16 11:44:59,563] INFO Kafka version : unknown (org.apache.kafka.common.utils.AppInfoParser)

[2019-04-16 11:44:59,563] INFO Kafka commitId : unknown (org.apache.kafka.common.utils.AppInfoParser)

[2019-04-16 11:44:59,565] INFO [Kafka Server 0], started (kafka.server.KafkaServer) 

### 007_Kafka源码分析环境搭建：验证IntelliJ IDEA中启动的Kafka能否使用 

接着就要验证一下启动的kafka能否正常使用，也就是能否正常的生产消息，接收消息，使用之前eclipse中的kafka-demo来验证即可，我们分析客户端源码的时候，其实直接在eclipse就可以，只要粘上去源码，打断点就可以开始调试了 

如果是分析broker层面的一些源码，比如说请求的处理架构，磁盘读写，或者是consumer group coordinator的一些工作流程，我们在intellij idea里打断点来进行调试就可以了 

### 008_从一条消息的生产发送开始如何逐步探索Kafka运行的全流程  

并不是说从broker入手来探索了，我们会从一条消息开始生产出去，首先是生产端的源码运行的流程，最核心的是两块，第一块要学习一下Kafka客户端是如何去设计一个非常优秀的生产级的保证高吞吐的一个缓冲机制 

要深入的分析一下，生产端的Sender线程他的网络通信的模块，这个是我们绝对重点中的重点，必须要搞清楚kafka客户端是如何通过网络通信把一批消息发送到broker上去的，这个里面对于网络通信的很多细节，我们需要去深入的扣一下 

网络通信的设置一些对应的参数，应对网络故障，人家是怎么来做的 

kafka broker的集群架构的设计和实现，各个broker启动的时候，如何组成一个集群，集群的主控节点是如何选举出来的，后续如何监控集群里的各个broker是否正常运行的，或者是故障宕机，如何对故障的broker找备用的来进行替代，集群的元数据（topic、partition、leader/follower、isr） 

消息会到kafka broker那块去，我们要学习很多重点的地方，首先就是网络通信里的server端的处理架构应该如何来设计，以及server端的网络通信的细节，包括里面的底层网络通信对应的一些参数的设置 

深入的学习他的磁盘读写这块是如何来实现的，他的消息是如何写入磁盘的，磁盘的存储结构，怎么去使用os page cahe，怎么实现磁盘文件的顺序写，非常的优秀的，我们要来学习里面的这些东西 

多副本冗余以及高可用的架构设计，leader和follower是如何同步的，副本是如何传输的，另外就是这个过程中，他的各种offset是如何变更的，如果leader所在的broker故障了，是如何进行leader和follower的切换的，高可用的架构 

负载均衡以及伸缩架构，他是如何保证数据均匀的分布在集群的各个broker机器上的，负载均衡，如何进行topic的partition的扩容，让一个topic可以通过partition扩容来使用集群里更多的broker的机器资源，另外一个就是说broker扩容，如何通过加更多的broker机器来扩容集群的存储资源以及网络资源 

消费端的原理，每个消费组的主控节点是如何来选择的，group coordinator如何选择，consumer group leader如何选择，分区分配的方法，分布式消费的实现机制，拉取消息的原理，offset提交的原理 

kafka线上故障的处理，生产性能优化，以及各种生产问题的解决方案 

kafka是一个非常难的技术，而且在使用的过程中，是经常遇到问题的，遇到问题的概率比hdfs之类的技术高很多，数据采集，flume、canal、分布式爬虫，kafka和hdfs都是比较难的技术 

离线数仓，yarn、mapreduce、hive、spark，底层对应的是一些性能优化，故障处理，数据仓库的模型的设计，复杂ETL的开发和优化，数据治理 

实时平台，自研一套复杂的平台系统

### 009_回顾一下Kafka生产端是如何进行开发的以及涉及哪些东西

既然要从kafka生产端开始研究他的源码，就得先看一下他的生产端涉及到哪些东西 

KafkaProducer，里面包含了核心的资源，包括线程资源以及网络资源，他主要是通过一些线程实现了消息的异步发送，批处理机制，维护了跟 各个broker的网络连接，然后可以通过网络连接发送消息到broker去 

可以整个系统全局就通过唯一的一个KafkaProducer来发送消息也是可以的，多线程并发安全的，发送的消息是通过ProducerRecord来进行封装的，代表了你要生产发送的一条消息，交给KafkaProducer来进行发送即可

### 010_生产端的核心：KafkaProducer初始化时会涉及到哪些组件？ 

### 011_集群元数据拉取组件的分析以及多个拉取触发时机的分析 

### 012_在源码中分析核心参数的含义：请求超时、缓冲大小、请求大小

### 013_内存缓冲区的构建以及消息batch打包发送request的原理

### 014_底层的网络通信组件初探以及核心网络参数的分析

### 015_数据发送线程是如何初始化以及acks参数在源码中的含义分析  

我们先来看一看KafkaProducer初始化的时候会涉及到哪些内部的核心组件，默认情况下，一个jvm内部，如果你要是搞多个KafkaProducer的话，每个都默认会生成一个client.id，producer-自增长的数字，producer-1 

（1）核心组件：Partitioner，后面用来决定，你发送的每条消息是路由到Topic的哪个分区里去的 

（2）核心组件：Metadata，这个是对于生产端来说非常核心的一个组件，他是用来从broker集群去拉取元数据的Topics（Topic -> Partitions（Leader+Followers，ISR）），后面如果写消息到Topic，才知道这个Topic有哪些Partitions，Partition Leader所在的Broker 

后面肯定会每隔一小段时间就再次发送请求刷新元数据，metadata.max.age.ms，默认是5分钟，默认每隔5分钟一定会强制刷新一下 

还有就是我们猜测，在发送消息的时候，如果发现你要写入的某个Topic对应的元数据不在本地，那么他是不是肯定会通过这个组件，发送请求到broker尝试拉取这个topic对应的元数据，如果你在集群里增加了一台broker，也会涉及到元数据的变化 

（3）核心参数：每个请求的最大大小（1mb），缓冲区的内存大小（32mb），重试时间间隔（100ms），缓冲区填满之后的阻塞时间（60s），请求超时时间（30s） 

（4）核心组件：RecordAccumulator，缓冲区，负责消息的复杂的缓冲机制，发送到每个分区的消息会被打包成batch，一个broker上的多个分区对应的多个batch会被打包成一个request，batch size（16kb） 

默认情况下，如果光光是考虑batch的机制的话，那么必须要等到足够多的消息打包成一个batch，才能通过request发送到broker上去；但是有一个问题，如果你发送了一条消息，但是等了很久都没有达到一个batch大小 

所以说要设置一个linger.ms，如果在指定时间范围内，都没凑出来一个batch把这条消息发送出去，那么到了这个linger.ms指定的时间，比如说5ms，如果5ms还没凑出来一个batch，那么就必须立即把这个消息发送出去 

（5）核心行为：初始化的时候，直接调用Metadata组件的方法，去broker上拉取了一次集群的元数据过来，后面每隔5分钟会默认刷新一次集群元数据，但是在发送消息的时候，如果没找到某个Topic的元数据，一定也会主动去拉取一次的 

（6）核心组件：网络通信的组件，NetworkClient，一个网络连接最多空闲多长时间（9分钟），每个连接最多有几个request没收到响应（5个），重试连接的时间间隔（50ms），Socket发送缓冲区大小（128kb），Socket接收缓冲区大小（32kb） 

（7）核心组件：Sender线程，负责从缓冲区里获取消息发送到broker上去，request最大大小（1mb），acks（1，只要leader写入成功就认为成功），重试次数（0，无重试），请求超时的时间（30s），线程类叫做“KafkaThread”，线程名字叫做“kafka-producer-network-thread”，此处线程直接被启动 

（8）核心组件：序列化组件，拦截器组件