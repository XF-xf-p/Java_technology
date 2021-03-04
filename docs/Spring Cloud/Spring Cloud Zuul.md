# Spring Cloud Zuul

## 1.Zuul的概念和特点

Zuul是Netflix开源的微服务网关，和Eureka、Ribbon、Hystrix等组件配合使用完成服务网关的动态路由、负载均衡等功能。其核心特点如下。

( 1 ）资源审查：对每个请求都进行资源验证审查，拒绝非法请求。

( 2 ）身份认证：到每个请求的用户都进行身份认证，拒绝非法用户，身份认证一般基于HTTP消息头完成。

( 3 ）资源监控：通过对有意义的数据进行追踪和请求统计，为分析生产环境中接口的调用状态和用户的行为提供依据。

( 4 ）动态路由：对外提供统一的网关服务，动态地将不同类型的请求路由到不同的后端集群，实现对外提供统一的网关服务和对内进行有效的服务拆分。

( 5 ）压力测试：通过配置设置不同集群的负载流量，预估集群的性能。

( 6 ）负载均衡：为每一种负载类型都分配对应的容量，针对不同的请求做更细粒度的负载均衡，并弃用超出限定值的请求，进行服务保护。

( 7 ）多区域弹性：跨越区域进行请求路由，旨在实现ELB( Elastic Load Balance，弹性负载均衡）使用的多样化，并保证边缘位置与使用者尽可能接近。

## 2.Zuul的原理

Zuul的核心是通过系列Filter将整个HTTP请求过程连成一系列操作来实现对HTTP请求的控制。Zuul提供了一个对Filter进行动态地加载、编译和运行的框架。Zuul的各个Filter之间不进行直接通信，而是通过一个RequestContext静态类来进行数据的传递，每个Web请求所需要传递的参数都通过ThreadLocal变量来记录。

Zuul Filter有filterType（）、filterOrder（）、shouldFilter（）、run()4种核心方法。

- filterType（）：用以表示路由过程中的阶段（内置包含PRE、ROUTING、POST和ERROR
- filterOrder（）：表示相同Type的Filter的执行顺序
- shouldFilter（）：表示Filter的执行条件
- run() ：表示Filter具体要执行的业务逻辑

Zuul定义了4种Filter Type，这些Filter Type分别对应请求的不同生命周期。

( 1 ) PRE Filter: PRE Filter在请求被路由之前调用。一般用于实现身份验证、资源审查、记录调试信息等。

( 2) ROUTING Filter: ROUTING Filter将请求路由到微服务实例，该Filter用于构建发送给微服务实例的请求，并使用Apache HTTPClient或Netflix Ribbon请求微服务实例。

( 3) POST Filter: POST Filter一般用来为响应添加标准的HTTP Header、收集统计信息和指标，以及将响应从微服务发送给到客户端等，该Filter在将请求路由到微服务实例以后被执行。

( 4) ERROR Filter：在其他阶段发生错误时执行ERROR Filter。

## 3.Zuul的使用

Zuul主要用来构建微服务网关，核心的使用包括定义路由和定义Filter。具体过程如下：

( 1) pom.xml添加依赖

在pom.xml中加入Zuul依赖，其中，spring-cloud-starter-netflix -eureka-client为微服务发现所需的依赖，spring-cloud-starter-netflix-zuul为Zuul网关服务所需的依赖。

( 2）通过＠EnableZuulProxy注解开启对服务网关的支持，Zuul一般和服务发现配合使用，这里使用@EnableEurekaClient开启对服务发现客户端的支持。

( 3) application.properties配置。

配置服务名称、端口，以及需要连接的服务注册中心的地址，这里注册中心使用Eureka服务。路由配置的核心zuul.routes.routel .path=/proxy／**和zuul.routes.routel.serviceld=EUREKA-CLIENT表示定义了一个名为routel的网关路由，所有以proxy开始的请求都转发到EUREKA-CLIENT服务实例上。

( 4）路由转发验证。

分别启动eurekaserver（注册中心）、eurekaclient（服务生产者）、zuul（网关服务），在浏览器地址栏中输入localhost:9006/proxy/serviceProducer/，可以看到端口9006上的网关服务路由到端口9002的EUREKA-CLIENT服务上了。

( 5 ）忽略指定微服务配置

```
zuul.ignored-services:/apil/**,xx-service
```

上述配置表示以api开头和xx-service的请求不参与路由转发

（6）指定Path和URL配置

```
zuul. routes. route-name. url= http://localhost:9002/ 
zuul. routes. route-name. path= /api/＊＊
```

上述配置将http://ZUULHOST:ZUULPORT/api/开头的请求映射到http ://localhost: 9002/，由于不是用service-id定位服务的，因此无法使用负载均衡功能。

## 4.PreRequestFilter的定义和注入

Zuul的Filter使用包括Filter的定义和注入两步，具体如下：

(1）定义PreRequestFilter。

PreRequestFilter在请求被路由之前调用，一般用于实现身份验证和资源审查等。

通过继承ZuulFilter实现了一个Zuul Filter，该Filter的类型为PRE_Type,表示在路由之前被执行，在run（）方法体中对用户名进行验证，如果用户名合法则通过，如果不合法则抛出异常，程序逻辑转到Error流程。

( 2 ）注入PreRequetFilter。

要想PreRequestAuthFilter生效还需要将Filter注入Zuul，具体实现通过在ZuulApplication中定义Bean实例即可。

```
@Bean 
public PreRequestAuthFilter preRequestAuthFilter () { 
	return new PreRequestAuthFilter(); 
}
```

## 5.Fallback Provider的服务容错

应用程序在调用第三方的服务时难免会出现错误，但是，应用程序通常不会把错误直接抛给用户，而是根据业务定义不同的错误消息，以便用户分析和排查问题，同时可将其作为全局异常处理方案，具体如下。

（ 1 ）定义DefaultFallbackProvider。

```
public class DefaultFallbackProvider implements FallbackProvider
```

（2 ）注入DefaultFallbackProvider

```
@Bean 
public Default FallbackProvider defaultFallbackProvider () { 	return new DefaultFallbackProvider();
}
```

