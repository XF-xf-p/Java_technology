# Spring Cloud

## 1.Spring Cloud

Spring Cloud为企业级分布式Web系统构建提供了一站式的解决方案。为了简化分布式系统的开发流程和降低开发难度，Spring Cloud以组件化的形式提供了配置管理、服务发现、断路器、智能路由、负载均衡和消息总线等模块，应用程序只需要根据需求引入模块，便可方便地实现对应的功能.。

- Spring Cloud Config: Spring Cloud的配置中心，用于将配置存储到服务器中进行中集中化管理，支持本地存储、Git和Subversion 3种存储方式。配置中心除了Spring Cloud Config，还有Apollo配置中心和基于ZooKeeper等方式实现的配置中心。
- Spring Cloud Bus: Spring Cloud的事件消息总线，用于监听和传播集群中事件的状态的变化，例如集群中配置的变化检测和广播等。
- Eureka:Netflix提供的服务注册和发现组件，集群中各个服务以REST的方式将服务注册到注册中心，并与注册中心保持心跳连接，主要用于服务发现和自动故障转移。
- Hystrix:Netflix提供的服务熔断器，主要提供了服务负载过高或服务故障时的容错处理机制，以便集群在出现故障时依然能够对外提供服务，防止服务雪崩。
- Zuul:Netflix Zuul为集群提供通用网关的功能，前端服务访问后端服务均需要通过Zuul的动态路由来实现。同时可以在Zuul上实现服务的弹性扩展、安全监测、统一权限认证等功能。
- Archaius:Netflix提供的配置管理库，用于实现动态化属性配置和验证、线程安全配置操作、轮询框架、回调机制等功能。
- Consul: Consul是基于Golang开发的一个服务注册、发现和配置工具，其功能与Eureka类似。
- SpringCloud for Cloud Foundry: Spring Cloud for Cloud Foundry Pivotal通过OAuth 2.0协议绑定服务到Cloud Foundry, Cloud Foundry是VMware推出的开源PaaS云平台。
- SpringCloud Sleuth: Spring的日志收集工具包，封装了Dapper和Log-based追踪及Zipkin和HTrace操作，为Spring Cloud的应用实现了一种分布式链路追踪解决方案。
- Spring Cloud Data Flow: Spring Cloud Data Flow是一个混合计算模型，结合了流式数据与批量数据的处理方式，为Spring Cloud处理大数据提供了可能。
- Spring Cloud Security：基于Spring Security工具包实现的安全管理组件，主要用于应用程序的安全访问和控制。
- Spring Cloud ZooKeeper: Spring Cloud ZooKeeper封装了操作ZooKeeper的API,用于方便地操作ZooKeeper并实现服务发现和配置管理功能。
- Spring Cloud Stream: Sping的流式数据处理工具包，封装了Redis、RabbitMQ、Kafka等消息的接收和发送功能，用于快速实现流式数据分析功能。
- Spring Cloud CLI：基于Spring Boot CLI, Spring可以让用户使用命令行方式快速建立云组件。
- Ribbon: Netflix Ribbon用于分布式系统API调用的负载均衡，提供随机负载、轮询负载等多种负载均衡策略，常配合服务发现和断路器使用。
- Turbine:Netflix Turbine是实时消息或事件流的聚合工具，常用来监控集群下Hystrix的健康指标数据。
- Feign:Feign是一种声明式、模板化的HTTP访问客户端。
- Spring Cloud Task: Spring Cloud Task是SpringCloud提供的分布式环境下集群任务的统一管理和调度工具。
- SpringCloud Connectors: Spring Cloud Connectors为在云平台上运行的基于JVM的应用程序提供了一个简单的抽象，可以在JVM运行时发现绑定的服务和部署信息，并且支持将发现的服务注册为Spring Bean。
- Spring Cloud Cluster：提供LeaderShip（选举）功能，如ZooKeeper、Redis、Hazeleast、Consul等常见状态模式的抽象和实现。
- Spring Cloud Starters: Spring Boot式的启动项目，为SpringCloud提供开箱即用的依赖管理。