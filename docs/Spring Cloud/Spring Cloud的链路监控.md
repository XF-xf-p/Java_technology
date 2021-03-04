## 1.Spring Cloud的链路监控

微服务架构是一个按业务划分服务实例的分布式架构，一个分布式系统往往有几十个甚至上百个服务实例。由于服务实例数量众多，随着业务复杂性增加，一个请求可能需要调用很多个服务才能完成业务操作，最终形成很长的调用链。而服务的调用又分内部服务间调用、跨部门服务调用和外部服务调用，一旦线上出了问题，很难快速定位，在微服务架构中，一般通过分布式链路追踪来记录一个请求有哪些服务参与、参与的顺序，以及每个服务的耗时情况，从而达到每个请求的步骤都清晰可见、出现问题都能够及时定位的目的。

目前，比较流行的链路追踪组件有Google的Dapper、Apache的Sleuth、Twitter的Zipkin、NAVER的Pinpoint和阿里巴巴的EagleEye等，它们都是非常优秀的链路追踪开源组件。

## 2.Sleuth+Zipkin 

Spring Cloud Sleuth是Spring Cloud的组成部分之一，为Spring Cloud应用提供一种分布式追踪解决方案，同时结合Zipkin 、HTrace和Log采集服务实现分布式链路追踪和展示。

### Sleuth的介绍

Sleuth主要依赖Span、Trace和Annotation实现分布式链路追踪。

( 1 ) Span: Span是基本的工作单元，包括一个64位的唯一id、一个64位的Trace码、服务调用的描述信息、时间戳事件、Key-Value注解（Tags）和Span处理者的id（通常为IP）等信息。最初的Span被称为根Span，该Span种的Spanld与TraceId值相同。

( 2 ) Trance: Trance由一系列Span组成，这些Span组成一个树型结构。

( 3 ) Annotation ：用于及时记录存在的事件。常用的Annotation如下。

- cs-Client Sent：客户端发送一个请求到服务端，作为请求的开始，也即Span的开始。
- sr-Server Receive ：服务端接收到客户端的请求并开始处理该请求。sr和cs之间的时间间隔为网络的延迟时间。
- ss-Server Sent：服务端处理完客户端的请求，开始向客户端返回处理结果。ss和sr之间的时间间隔表示服务端处理请求的执行时间。
- er-Client Received ：客户端接收到服务端返回的结果，至此请求结束。er和sr之间的时间间隔表示客户端接收服务端的数据所使用的时间。

### Sleuth+Zipkin实现分布式链路追踪

Sleuth为分布式链路追踪提供了数据结构基础，但是在使用过程中往往需要一个可视化的集中式的链路数据的存储和展示系统，方便快速定位问题，Zipkin为应用程序提供了该功能。

I ）构建Zipkin Server服务，用于日志的收集

( I) pom.xml添加依赖

在pom.xml中加入Zipkin依赖，其中，spring-cloud-starter-netflix -eureka-cIient为服务发现所需的依赖，zipkin-server为Zipkin服务中心所需的依赖，zipkin-autoconfigure-ui为监控中心WebUI所需的依赖

(2 ）通过@EnableZipkinServer注解开启对Zipkin服务端的支持。注意，Zipkin一般和服务发现配合使用，这里使用@EnableEurekaClient开启对服务发现客户端的支持。

( 3 ) application. properties配置

配置服务名称、端口，以及需要连接的服务注册中心的地址，这里注册中心使用Eureka服务。

2）构建Sleuth+Zipkin客户端服务，用于日志的上报

( 1) pom.xml添加依赖。

在pom.xml中加入Zipkin依赖，其中，spring-cIoud-starter-netflix -eureka-cIient为服务发现所需的依赖，spring-cloud-starter-zipkin为Zipkin客户端所需的依赖，spring-cloud-starter-sleuth为Sleuth所需的依赖

( 2) Zipkin客户端和Sleuth的实现只需要添加依赖的JAR包即可，在入口服务上不需要注解，但若要实现基于整个分布式微服务的监控，则需要实现服务注册，这里使用@EnableEurekaClient开启对服务发现客户端的支持。

( 3 ) application.properties配置。

配置服务名称、端口，以及要连接的服务注册中心的地址，这里注册中心使用Eureka服务。另外一个核心的配置就是spring.zipkin.base-url，它用于配置将日志汇聚到哪个Zipkin Server上存储和展示，spring.sleuth.sampler. percentage表示监控抽样比例，1表示全部请求都监控，0.1表示只对其中的10%做抽样监控，该参数在线上应用时一般会设置得较小，以尽量减小链路监控对服务器资源的压力。

(4 ）定义服务接口

( 5 ）基于Zipkin Dashboard进行查询

Zipkin Dashboard的实现只需要在Zipkin Server的pom.xml种加入zipkin-autoconfigure-ui依赖便可。

3) Zipkin的数据持久化

在默认情况下，Zipkin存储记录到内存，如果服务重启，则所有记录都丢失。为了保证持久性，Zipkin支持MySQL、ElasticSearch、Cassandra存储。

( 1 ) pom.xm添加依赖

在zipkin_dashboard项目中加入zipkin-autoconfigure-storage-mysql 依赖，使Zipkin支持MySQL的持久化，同时需要加入MySQL的驱动。

( 2) application.properties配置

加入MySQL的数据库配置，并通过zipkin.storage. type=mysq配置Zipkin的持久化类型为mysql。

( 3 ）数据库验证

重启服务，查看数据库，可以看到Zipkin自动执行数据库建库脚本zipkin.sql，并生成了数据库，数据库建库脚本通过spring.datasource.schema配置。这样就完成了数据库配置，所有Spans信息都存储在数据库中，即使重启Zipkin Server服务，也不会丢失记录。

## 3.Pinpoint

### Pinpoint的介绍

Pinpoint是一款无侵入式的全链路监控分析工具，基于字节码增强技术实现了调用链监控、方法执行详情查看和应用状态信息监控等功能。Pinpoint基于Google Dapper实现，与Zipkin（一款开源的全链路分析工具）功能类似，与Zipkin最大的不同是，Pinpoint具有无侵入式的、代码维度的监控特性。Pinpoint功能丰富，以下为常用的几种功能

( 1 ）服务拓扑图：将系统中服务组件的依赖和调用关系进行可视化展示，单击服务节点，可以查看该节点的详细信息（例如当前节点状态、请求数量等）。

( 2 ）实时活跃线程图：监控服务组件中正在运行的线程的活跃情况，常用于线程性能分析。

( 3 ）响应散点图：将单位时间内的请求数和响应时间以时间轴的方式可视化展示，拖动图表可以查看更详细的请求执行情况。

( 4 ）调用栈查看：为每个请求都提供了代码维度的调用栈查看，可以在页面中查看每个请求对应的代码维度的执行详情信息，用于快速定位问题。

( 5 ）服务状态、机器状态检查：该功能用于实时统计和显示服务组件对应的系统资源及进程执行等信息，比如CPU使用率、内存使用率、系统负载、垃圾收集状态、TPS和JVM信息等。

### Pinpoint的组件

Pinpoint主要由3个组件和HBase数据库组成，3个组件分别为Agent、Collector和Web UI，它们的功能分别为数据的收集、数据的存储和数据的展示，HBase为数据的持久化数据库。

( 1 ) Agent：收集应用程序的监控数据，Agent无侵入式，只需要在应用程序的启动命令中加入部分参数即可。

( 2 ) Collector：数据收集模块，接收Agent上报的监控数据，并将监控数据存储到HBase。

( 3) Web UI：链路监控展示模块，用于查看系统的拓扑图、实时活跃线程图、调用栈、服务状态和机器状态等，同时支持报警功能。

### Pinpoint的数据结构

在Pinpoint种，核心数据结构由Span、Trace、Traceld组成。

( 1) Span: RPC跟踪的基本单元，当一个RPC调用到达时，表明RPC工作已经处理完成，并在返回值中包含了跟踪数据。为了确保代码级别的可见性，Span用带SpanEvent标签的子结构作为数据结构。每个Span都包含一个Traceld。

( 2) Trace：多个 Span的集合，由关联RPC的一系列Span组成，同一个Trace中的Span共享相同的TransactionId。Trace通过SpanId和ParentSpanld将调用栈整理为树结构

( 3 ) Traceld：由Transactionld、SpanId、ParentSpanld组成的Key的集合，Transactionld指明消息的id、SpanId和ParentSpanld表明RPC的子父调用关系。

- Transactionld ( Txld）： 在分布式系统间实现事务唯一性的标识
- SpanId ：当收到RPC消息时处理请求的工作id，在RPC请求到达节点后生成。
- ParentSpanld ( pSpanld）： 发起RPC调用的父Span的Spanld，如果节点是事务的起点，则将没有父Span。对于这种情况，使用-1来表示这个Span是事务的根Span。

### Pinpoint的字节码增强技术

Pinpoint基于字节码增强技术（又叫动态探针技术）实现无侵入式的调用链数据采集。

其核心实现基于JVM的JavaAgent机制来实现。应用在启动时通过设置JavaAgent来指定PinpointAgent的加载路径。

在启动后，Pinpoint Agent采用字节码增强技术在加载应用Class文件之前拦截并修改字节码，在Class的方法调用的前后加上链路数据采集逻辑，从而实现链路采集功能。

在Pinpoint中，API拦截部分和数据记录部分是分开的，拦截器被注入应用程序想要追踪的方法中，并调用before（）和after（）方法来处理数据记录。通过字节码增强，Pinpoint Agent能够只从必要的方法中记录数据，这使得分析数据的大小变得紧凑。同时，应用程序不需要修改任何代码就能做到全链路监控，对应用十分友好。

### Pinpoint的使用

Pinpoint的使用包括pinpoint-collector安装、pinpoint-web安装、pinpoint-agent的配置和应用。

当应用程序需要通过Pinpoint实现全链路监控时，不需要修改应用程序的任何代码，只需要在启动的时候加上以上三个参数即可。其中，“-javaagent:$AGENT_PATH/pinpoint-bootstrap-$VERSION .jar”表示要通过Pinpoint动态字节码增强技术实现动态探针和全链路监控，agentld表示唯一标识该应用的id，applicationName表示服务名称。