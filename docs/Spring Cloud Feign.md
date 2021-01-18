## 1.Spring Cloud Feign

Feign是一种声明式、模板化的HTTP Client。它的目标是编写Java HTTP Client变得更简单。Feign通过使用Jersey和CXF等工具实现一个HTTP Client，用于构建REST或SOAP的服务。Feign还支持用户基于常用的HTTP工具包（OkHTTP、HTTPComponents ）实现自定义的HTTP Client

Feign基于注解的方式将HTTP请求模板化。Feign将HTTP请求参数写入Template,极大地简化了HTTP请求。尽管Feign目前只支持基于文本的HTTP请求，不适合文件的上传和下载，但它为HTTP请求提供了更多可能性，比如请求回放等功能。同时，Feign使HTTP单元测试变得更加方便。

## 2.Feign的应用

Feign提供了声明式接口编程的方式，其应用一般依赖服务发现组件来实现远程接口调用，在并发要求不高的情况下可以作为RPC方案使用，实现服务之间的解耦。Feign应用分为服务提供者和服务消费者。

要实现一个服务提供者分为5步：首先在pomxm中加入spring-cloud-starter-feign 依赖，然后通过＠EnableFeignClients注解开启对Feign的支持，接下来配置application. properties配置文件，再创建接口和定义需要远程调用的方法，最后一步是服务的访问和使用

( 1 ) pom.xml添加依赖

在pom.xml中加入spring-cIoud-starter-feign依赖。

( 2)通过@EnableFeignClients注解开启对feign的支持。注意，Feign一般和服务发现配合使用，以下代码使用EnableEurekaClient开启了对服务发现客户端的支持。

```
@SprngBootApplication
@EnableEurekaClient 
@EnableFeignClients 
public class FeignClientApplication { 
	public static void main(String[] args) { 									SpringApplication.run(FeignClientApplicaton.class,args); 
```

( 3 ) application.properties配置

配置服务名称、端口和需要连接的服务注册中心的地址，这里注册中使用Eureka服务

```
#Feign服务实例的名称
spring.application.name=feignclient
#Feign服务实例的端口
server.port=9004 
＃注册中心的地址eureka.client.serviceUrl.defaultZone=http://localhost:9001/eureka/ 
eureka.client.registry-fetch-interval-seconds=30
```

( 4 ) 创建接口和定义需要远程调用的方法

```
@FeignClient(”EUREKA-CLIENT") //step1 ：定义服务实例
public interface FeignClientInterface { 
	//step2 ：定义服务接口
	@RequestMapping(value="/serviceProducer”，method= RequestMethod.GET) 	Map serviceProducer(); 
}
```

上述代码首先定义了一个名为FeignClientInterface的接口，并通过FeignClient（"EUREKA-CLIENT）来实现注解的配置。这里的参数“EUREKA-CLIENT"为远程服务实例的名称；然后定义了serviceProduct（）方法，该方法通过调用服务实例名为“EUREKA-CLIENT”的“／serviceProducer”方法来实现远程调用。

( 5） 调用服务

```
@RestController 
public class FeignController { 
	@Autowired
	FeignClientInterface feignClentInterface
	@RequestMapping(value =”/consume/feign”,method = RequestMethod.GET) 	public Map serviceFeign() { 
    	return feignClientInterface.serviceProducer(); 
    }
}
```

上述代码通过注入FeignClientInterface依赖，并调用已经定义好的serviceProducer()方法来实现服务调用。

## 3.Feign的常用注解

Feign通过SpringMVC的注解来实现对HTTP参数的封调用，常用注解。

- @RequestLine：Request定义一个HTTP Method和UriTemplate，使用｛｝进行包装，并对＠Param注释参数进行解析

- @Param：定义一个HTTP请求模饭，模板中的参数将根据名称来解析

- @Headers：定义一个HeaderTemplate，在UriTemplate中使用

- @QueryMap：定义一个基于Name-Value的参数列表，也可以是Java实体类，最终以查询字符串的方式传输

- @HeaderMap：定义一个基于Name-Value的参数列表，用于扩展HTTP Headers 

- @Body：定义一个类似UriTemplate或者HeaderTemplate的模板，该模板使用＠Param注释参数解析相应的表达式

  