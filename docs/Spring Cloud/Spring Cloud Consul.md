## 1.Spring Cloud Consul 

Consul是HashiCorp公司推的用于实现分布式系统服务注册、发现和配置的开源工具。和SpringCloud Eureka一样，Consul也是一个一站式的服务注册与发现框架，内置了服务注册与发现、分布式一致性协议实现、健康检查、Key-Vlaue存储和多数据中心方案，因此不需要依赖第三方工具（例如Zookeeper）便可完成服务注册与发现，简单易用。

Consul采用Go编写，支持Linux、Windows和macOS系统，可移植性强。安装包仅是一个可执行文件，方便部署，且与Docker配合方便。

## 2.Spring Cloud Consul的原理

Consul是一个支持多数据中心的高可用的分布式服务注册、发现和配置服务，采用Raft一致性协议算法来保证服务的高可用；使用Gossip协议管理成员状态和广播消息，并且支持ACL访问控制。相对于SpringCloud Eureka或其他服务注册与发现框架，Consul有以下特点。

### Consul的特性

- 高效的Raft一致性算法：Consul使用Raft一致性算法来保证集群状态的一致性，在实现上比Paxos一致性算法更简单（ZooKeeper采用Paxos一致性算法实现，Etcd采用Raft一致性算法实现）。
- 支持多数据中心：Consul支持多数据中心。多数据中心可以使集群避免单数据中心的单点故障问题，但在部署的过程中需要考虑网络延迟、数据分片等情况。ZooKeeper 和Etcd均不支持多数据中心。
- 健康检查：Consul支持健康检查，默认每10s做一次健康检查，保证注册中心的服务均可用，Etcd不提供此功能。
- HTTP和DNS支持：Consul支持HTTP和DNS协议接口。ZooKeeper集成了DNS协议，实现复杂，Etcd只支持HTTP。

### Consul的角色

Consul按照功能可分为服务端和客户端。

- 服务端：Server，用于保存服务配置信息的高可用集群，在局域网内与本地客户端通信，在广域网内与其他数据中心通信。每个数据中心的Server数量都推荐为3个以上以保证服务高可用。在集群中，Server又分为Server Leader和Server。Server Leader负责同步注册信息和进行各个节点的健康检查，同时负责整个集群的写请求。Server负责把配置信息持久化并接收读请求。Server在一个数据中心( Data Center, DC ）内使用LAN Gossip协议的一致性算法，在跨数据中心内使用WAN Gossip协议的一致性算法。
- 客户端：Client，是无状态的服务，将HTTP和 DNS协议的接口请求转发给局域网内的服务端集群。

Consul的服务端和客户端还支持跨数据中心的访问，提供了跨区域的高可用功能。

### Consul的服务注册与发现流程

(1 ）服务注册：Producer在启动时会向Consul服务端发送一个POST注册请求，注册请求中包含服务地址和端口等信息。

(2 ）健康检查：Consul服务端在接收Producer的注册信息后，每10s （默认）会向Producer发送一个健康检查的请求，检验Producer是否健康。

( 3 ）服务发现：当Consumer发起请求（请求格式为“/api/address”）时，首先会从Consul服务端获取一个包含Producer的服务地址和端口的临时表。该表只包含通过了健康检查的Producer的可用服务列表。

( 4 ）服务请求：客户端从临时表中获取一个可用的服务地址，向Producer发送请求。Producer在收到请求后返回请求响应。

## 3.Spring Cloud Consul的使用

### Consul的服务启动

Consul是应用服务的注册中心，服务注册与发现均在Consul上被完成。Consu的使用分3步：下载安装包、启动服务端、启动客户端。

( 1 ）下载安装包.

( 2）启动服务端

在安装包目录下，执行以下命令启动服务端。

```
./consul agent -server -bind=ip -client=0.0.0.0 -bootstrap-expect=l -data-dir=/data/consul_data/ -node=serverl -ui
```

在上述命令中，-server表示启动服务端,-data-dir表示数据的存储地址，-node表示节点名称。

- -advertise ：用来设置服务实例对外暴露的服务地址，在一般情况下，－bind地址就是服务地址。
- -bootstrap：控制一个Server为Bootstrap模式。一个DataCenter中只能有一个Server处于Bootstrap模式.当一个Server处于Bootstrap模式时，可以将自己选举为RaftLeader。
- -bootstrap expect：一个Data Center中期望的Server的节点数量，当配置后Consul一直等到达到指定的Server数量时才会引导整个集群，该参数不能和Bootstrap一起使用
- -bind：指定绑定的地址，用来设置集群内部的通信地址，默认设置为0.0.0.0
- -client ：指定Consul绑定在哪个Client地址上，这个地址提供HTTP、DNS、RPC等服务，默认是127.0.0.1
- -config－file：指定要加载哪个配置文件
- -config-dir ：指定配置文件所在的目录，目录中所有以.json结尾的配置文件都会被加载
- -data dir ：指定存放Agent的状态的数据目录
- -dc：指定Agent允许的Data Center的名称，默认是dcl
- -encrypt：指定Secret Key用于加密通信，Key可以通过Consul Keygen生成，同一个集群中的节点必须使用相同的Secret Key 
- -join：加入一个已经启动的Agent服务，一个服务可以多次指定多个Agent。如果Consul不能加入任何指定的服务中，则Agent会启动失败
- -retry-join：和Join类似，但是允许在第一次失败后进行重试
- -retry-interval ：两次Join之间的时间间隔，默认是30s
- -retry-max ：尝试重复Join的次数，默认是0，表示无限次重试
- -log-level Consul Agent：启动后显示的日志信息级别，默认是info，可选trace、debug、info、warn、error
- -node ：节点名称，在一个集群中的节点名称必须是唯一的，默认是该节点的主机名
- -protocol ：Consul 使用的协议版本
- -rejoin：使Consul忽略之前的Leave，在再次启动后仍旧尝试加入集群中
- -server：定义Agent在Server模式下运行，每个集群至少都有一个Server
- -syslog：开启系统日志功能，只在Linux和macOS上有效
- -ui-dir：指定提供存放Web UI资源的路径
- -pid－file：指定一个路径来存放pid文件，可以使用该文件进行服务的关闭和更新( SIGINT/SIGHUP）操作
- agent：在Consul方案中每个提供服务的节点上都要部署和运行Consul Agent，所有运行Consul Agent的节点的集合构成Consul Cluster 

( 3 ）启动客户端

启动客户端只需要将Server参数去掉即可，启动命令如下

```
./consul agent -bind＝ip -client=0.0.0.0 -bootstrap-expect=l -data-dir=/data/consul_data/ -node=server2
```

2. Consul服务提供者的定义

服务提供者在启动的时候会将自身的服务地址状态上报Consul Server，服务消费者在每次发起请求时都要先从Consul Server获取可用的临时服务列表（该列表维护了可用的服务提供者的地址），再选择一个可用的服务提供者的地址进行服务调用。实现一个服务提供者分为5步：首先在pom.xml中加入spring-cloud-starter-consul-discovery依赖，然后通过@EnableDiscoveryClient注解开启服务发现的功能，接着配置application. properties配置文件、定义服务接口，最后一步是访问和使用。

( I ) pom.xml 添加依赖

在pom.xml中加入spring-cloud-starter-consul-discovery依赖

( 2 通过@EnableDiscoveryClient注解开启对服务发现的支持

```
@SpringBootApplication 
@EnableDiscoveryClient 
public class ConsulProducerApplication { 
	public static void main (String [] args) { 		
		SpringApplication.run(ConsulProducerApplication.class,args); 
	}
}
```

( 3 ) application.properties配置

配置服务名称、端口、要连接的Consul Server的地址和端口

( 4）定义服务

( 5）调用服务

### Consul服务消费者的定义

服务消费者是具体的服务调用方，它每次发起请求都先从Consul Server获取可用的临时服务列表（该列表维护了可用的服务提供者的地址），然后选择一个可用的服务提供者的地址实现服务调用。要实现一个服务消费者分为4步：首先需要在pom.xml中加spring-cloud-spring-cloud-starter-consul-discovery依赖，然后配置apIication. properties配置文件，接着获取临时服务列表，最后一步是访问和使用。

( 1 ) pom.xml添加依赖

在pom.xml中加入spring-cloud-spring-cloud-starer-consul-discovery依赖。

( 2) application.properties配置

配置服务名称、端口、要连接的Consul Server的地址和端口

( 3 ）获取临时服务列表

```
@Autowired／／注入负载均衡器
private LoadBalancerClient loadBalancer; 
／／从负载均衡器获取临时服务列表
ServiceInstance serviceList = loadBalancer.choose (”service-producer”); 
```

上述代码通过依赖注入LoadBalancerClient来获取临时服务列表，具体是通过choose（）方法来实现的，choose（）方法的参数是要获取的服务提供者的服务实例名称。

( 4）调用服务

```
@RestController 
public class ConsumerController { 
	@Autowired //step1: 注入负载均衡器
	private LoadBalancerClient loadBalancer; 
	@RequestMapping(”/call ”) 
	public String call () { 
		//step2 ：从负载均衡器获取临时服务列表
		ServiceInstance serviceList= loadBalancer.choose (”service-producer"); 
		System.out.println（”服务地址：”+serviceList.getUri()); 	
		System.out.println（”服务名称:"+serviceList.getServiceId());
		//step3 ：通过临时服务列表获取远程服务地址，实现远程过程调用	
		String callServiceResult =new RestTemplate ().getForObject ( serviceList.getUri().toString() + ”/hello”, String.class);
		return callServiceResult;
	}
}	
```

上述代码通过loadBalancer.choose(”serviceName＂）获取一个服务实例，然后使用RestTemplate.getForObject（）实现远程服务调用，其中，第一个参数是服务实例的地址，第二个参数是服务请求的返回值类型。

（ 5 ）验证服务

在浏览器地址栏中输入http://127.0.0.1:8503/call访问客户端服务。

