# Spring Boot

## 1.Spring Boot

Spring Boot是由Pivotal团队开发的全新的Spring开发框架，其设计的初衷是简化Spring应用复杂的搭建及开发过程。该框架提供了一套简单的Spring模块依赖和管理工具，从而避免了开发人员处理复杂的模块依赖和版本冲突问题，同时提供打包即可用的Web服务，成为快速应用开发领域（Rapid Application Development ）的领导者

Spring Boot的特点如下。

( l ）快速创建独立的Spring应用程序

( 2 ）嵌入Tomcat和Undertow等Web容器，实现快速部署。

( 3 ）自动配置JAR包依赖和版本控制，简化Maven配置

( 4 ）自动装配Spring实例，不需要XML配置

( 5 ）提供诸如性能指标、健康检查、外部配置等线上监控和配置功能

## 2.Spring Boot的使用

Spring Boot把传统的Spring项目从繁杂的XML配置中解放出来，应用只需要用注解自动扫描即可，同时SprinBoot为应用提供了统一的JAR管理和维护，不需要应用程序管理复杂的JAR依赖和处理多版本冲突问题，只需要在pom.xml文件中加入对应模块的Starter即可。对内部的JAR依赖的管理，SpringBoot会自动维护。具体使用过程如下。

( 1) Spring Boot的引入。

Spring Boot项目定义简单，使用方便，第一步需要在pom.xml文件中引入org.springframework.boot及相关依赖。pom.xml文件如下。

( 2）配置文件设置.

Spring Boot的配置分为application.properties和application.yml两种，两种配置有语法差别，但其实现的功能相同。下面的配置文件通过server.port=9090设置了服务端口为9090，如果不设置，则默认端口为Tomcat的8080，通过server.name=hello设置了服务名称为hello

```
server.port=9090 ＃服务端口号
server.name=hello ＃服务名称
server.tomcat.uri-encoding＝UTF-8＃以Tomcat为Web容器时的字符编码为UTF-8
#spring.data.mongodb.url＝mongodb://localhost:27017/mydb #MongoDB连接地址定义
#spring.http.encoding.charset=UTF-8 #HTTP请求的字符编码为UTF-8
#spring.http.multipart.max-file-size=lOMB ＃设置文件上传时单个文件的大小限制#spring.http.multipart.max-request-size=lOOMB ＃设置文件上传时总文件的大小限制#spring.mvc.static-path-pattern＝/** #设置静态资源的请求路径#spring.resources.static-locations=classpath:/static/,classpath:/public/＃设置静态资源的路径，多个用逗号隔开
# MySQL数据库配置
#hibernate.dialect=org.hibernate.dialect.MySQL5Dialect ＃设置数据库方言为MySQL
#hibernate.show_sql=true ＃设置是否显示SQL语句
#hibernate.hbm2dll.auto=update ＃设置使用Hibernate的自动建表#entitymanager.packagesToScan=com.zslin ＃设置自动扫描的包路径#spring.datasource.url=jdbc:mysql://localhost:3306/customer?\
#useUnicode=true&characterEncoding=utf-8&useSSL=true&autoReconnect=true 
＃设置MySQL数据库连接
#spring.datasource.username=root ＃设置数据库用户名#spring.datasource.password=l23 ＃设置数据库root用户对应的密码#spring.datasource.driver-class-name＝com.mysql.jdbc.Driver＃设置数据库驱动名称
```

( 3 ）定义启动类。

启动类是SpringBoot项目的入口，应用程序通过在类上设置一个@SpringBootApplication注解，声明该类是一个SpringBoot启动类，SpringBoot会扫描启动类所在的包及其子包中的所有类的注解，并将其加载到SpringBoot的容器中进行管理。只需要在main（）函数中执行SpringApplication.run(SpringbootApplication.class, args), 便完成了启动的定义。代码如下

```
@SprigBootApplication
public class SpringbootApplication{ 
	public static void main ( String[] args) { 		
		SpringApplication.run(SpringbootApplication.class, args); 
	}
}
```

在定义启动类后，单击右键执行run，便可启动该SpringBoot项目。

( 4）定义控制器.

在SpringbootApplication的根目录定义一个控制器，用于Web接口的访问。控制器的定义方式和Spring项目中控制器的常规定义方式一样，具体代码如下。

```
@RestController 
public class BaseController { 
	@RequestMapping(”/hello ”) 
	public String home() { 
		return "Hello World !”
    }
}
```

( 5 ）项目启动和访问。

在SpringbootApplication上单击右键执行run，便可启动该SpringBoot服务，在浏览器地址栏中输入127.0.0.1:9090/hello，便能访问定义好的REST服务。

## 3.Spring Boot Application Starters 

Starters是一组资源依赖描述，用于为不同的SpringBoot应用提供一站式服务，而不必像传统的Spring项目那样，需要开发人员处理服务和服务之间的复杂依赖关系。例如，如果要使用Spring的JPA功能进行数据库访问，只需要应用程序在项目中加入Spring-boot-starter-data-j pa依赖即可，具体的依赖细节由Starters统一处理，不需要应用程序分别处理各个JAR包的依赖关系。

- spring-boot-starter ：Spring Boot的核心Starter，包括Auto-Configuration Support 、Logging和YAML
- spring-boot-starter-activemq ：构建ActiveMQ的JMS Messaging Starter 
- spring-boot-starter-amqp ：构建RabbitMQ的JMS Messaging Starter 
- spring-boot-starter-aop ：集成了Spring AOP和AspectJ
- spring-boot-starter-artemis：构建Apache Artemis的JMS Messaging Starter 
- spring-boot-starter-batch ：构建Spring Batch的Starter
- spring-boot-starter-cache：构建Spring Framework Caching的Starter
- spring-boot-starter-cloud-connectors：基于Spring Cloud Connectors的连接器，简化了与云平台（例如Cloud Foundry和Heroku）中服务的连接
- spring-boot-starter-data-cassandra：构建Cassandra分布式数据库和Spring Data Cassandra的Starter
- spring-boot-starter-data-cassandra-reactive：构建Cassandra分布式数据库和SpringData Cassandra Reactive的Starter
- spring-boot-starter-data-couchbase：构建Couchbase数据库和SpringData Couchbase的Starter
- spring-boot-starter-data-elasticsearch：构建ElasticSearch查询分析引擎和Spring Data ElasticSearch的Starter
- spring-boot-starter-data-jpa ：构建SpringData JPA的Starter
- spring-boot-starter-data-ldap：构建SpringData LDAP的Starter
- spring-boot-starter-data-mongodb：构建MongoDB文档数据库和SpringData MongoDB的Starter
- spring-boot-starter-data-neo4j：构建Neo4j图数据库和SpringData Neo4j的Starter
- spring-boot-starter-data-redis ：构建SpringData Redis和Lettuce Client的Redis Key-Value数据库的Starter
- spring-boot-starter-data-rest ：构建Spring Data REST的Starter
- spring-boot-starter-data-soIr：构建Apache Solr的Starter
- spring-boot-starter-freemarker：FreeMarker Views 集成的Starter
- spring-boot-starter-groovy-templates：构建Groovy Templates Views的Starter
- spring-boot-starter-hateoas ：基于Spring MVC和Spring HATEOAS构建RESTful应用的Starter
- spring-boot-starter-jdbc：构建HikariCP Connection Pool实现的JDBC Starter 
- spring-boot-starter-jersey：构建JAX-RS和Jersey的Starter
- spring-boot-starter-json：支持JSON读写操作的Starter
- spring-boot-starter-jta-atomikos：构建Atomikos实现的JTA Transactions 
- spring-boot-starter-jta-bitronix：构建Bitronix实现的JTA Transactions 
- spring-boot-starter-jta-narayana：构建Narayana实现的JTA Transactions
- spring-boot-starter-maiI ：支持邮件发送的Starter
- spring-boot-starter-mustache：构建Mustache Views实现Web应用的Starter
- spring-boot-starter-quartz ：构建Quartz Scheduler的任务调度Starter
- spring-boot-starter-security ：构建Spring Security的安全框架Starter
- spring-boot-starter-test ：集成了JUnit、Hamcrest和Mockito的Starter
- spring-boot-starter-thymeleaf ：构建Thymeleaf Views实现Web应用的Starter
- spring-boot-starter-validation ：构建Hibernate Validator统一验证的Starter
- spring-boot-starter-web ：提供标准的Web应用，包含RESTful的支持和嵌入Tomcat等其他Web容器的Starter
- spring-boot-starter-web-services ：基于Spring Web Services的Starter
- spring-boot-starter-webflux ：构建WebFlux应用的Starter
- spring-boot-starter-websocket ：支持WebSocket的Starter

## 4.Spring Boot的常用组件及其使用

Spring Boot的核心特点是通过Starter能快速将各个组件集成到应用中，并提供良好的操作接口。

### Spring Boot （使用MySQL）

Spring Boot基于Starter能够快速将不同的服务组件集成到应用程序中。SpringBoot 服务组件的集成过程分为引入Starter、设置application.properties和使用服务组件（组件会根据配置文件自动装配）3步。MySQL具体使用如下。

( 1 ）引入Starter

```
<dependency> 
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-jdbc</artifactId> 
</dependency> 
```

( ）设置application.properties

```
spring.datasource.url=jdbc:mysql//localhost/test＃数据库地址spring.datasource.username=dbuser＃数据库用户名spring.datasource.password=dbpass＃数据库密码
spring.datasource.driver-class-name=com.mysql.jdbc.Driver＃数据库驱动
```

( 3 ）使用服务组件

```
@Component 
public class MyBean { 
	private final JdbcTemplate jdbcTemplate; 
	@Autowired 
	public MyBean(JdbcTemplate jdbcTemplate){ 
		this.jdbcTemplate =jdbcTemplate;
	}
}
```

### Spring Boot 使用Redis 

( 1 ）引入Starter。

```
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId>spring-boot-starter-data-redis</artifactId> 
</dependency> 
```

( 2）设置appication.properties

```
#Redis数据库名称（默认为0)
spring.redis.database=O
#Redis数据库地址
spring.redis.host=l72.31.19.222 
#Redis数据库端口
spring.redis.port=6379
#Redis数据库密码（默认为空〉
sping.redis.password=
#Redis连接池的最大连接数（使用负值表示没有限制）
spring.redis.pool.max-active=8
#Redis连接池的最大阻塞等待时间（使用负值表示没有限制）
spring.redis.pool.max-wait=-1
#Redis连接池中的最大空闲连接
spring.redis.pool.max-idle=8
#Redis连接池中的最小空闲连接
spring.redis.pool.min-idle=O
```

( 3 ）使用服务组件

```
@Component 
public class MyBean { 
	private StringRedisTemplate template; 
	@Autowired 
	public MyBean(StringRedisTemplate template) { 
		this.template= template; 
	}
}
```

### Spring Boot 使用MongoDB

( 1 ）引入Starter

```
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId> spring-boot-starter-data-mongodb</artifactId> </dependency> 
```

( 2）设置application.properties

```
spring.data.mongodb.uri＝mongodb//user:secret@mongol.example.com:l2345,mongo2.example.com:23456/test 
＃数据库的连接地址
```

( 3 ）使用服务组件

```
@Component 
public class MyBean { 
	private MongoTemplate template; 
	@Autowired 
	public MyBean(MongoTemplate template) { 
		this.template = template; 
	}
}
```

### SpingBoot 使用Neo4j

( 1 ）引入Starter

```
<dependency> 
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-neo4j</artifactId> 
</dependency> 
```

( 2）设置application.properties  

```
spring.data.neo4j.uri＝bolt://my-server:7687#Neo4j图数据库地址spring.data.neo4j.username=neo4j #Neo4j图数据库用户名spring.data.neo4j.password=secret #Neo4j图数据库密码
```

( 3 ）使用服务组件

```
@Component
public class MyBean { 
	private final Session session; 
	@Autowired 
	public MyBean(Session session) { 
		this.session = session;
	}
}
```

### Spring Boot 使用Solr

( 1 ）引入Starter

```
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId>spring-boot-starter-data-solr</artifactId> 
</dependency> 
```

( 2）设置application.properties  

```
#Solr数据库地址
spring.data.solr.host: http://127.0.0.1:8080/solr/criri_core 
```

( 3 ）使用服务组件

```
@Component 
public class MyBean { 
	private SolrClient solr; 
	@Autowired 
	public MyBean(SolrClient solr) { 
		this.solr= solr; 
	}
}
```

###  Spring Boot 使用ElasticSearch

( 1 )引入Starter

```
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId> spring-boot-starter-data-elasticsearch</artifactId> </dependency> 
```

( 2）设置application.properties

```
#ElasticSearch数据库地址
spring.data.elasticsearch.cluster-nodes=localhost:9300 
```

( 3 ）使用服务组件

```
@Component 
public class MyBean { 
	private final ElasticsearchTemplate template; 
	public MyBean(ElasticsearchTemplate template) { 
		this.template = template; 
	}
}
```

### Spring Boot 使用Cassandra

( 1 ）引入starter

```
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId> spring-boot-starter-data-cassandra</artifactId> </dependency> 
```

( 2）设置application.properties

```
#Cassandra的命名空间
spring.data.cassandra.keyspace-name=mykeyspace 
#Cassandra数据库地址
spring.data.cassandra.contact-points=cassandrahostl,cassandrahost2 
```

( 3 ）使用服务组件

```
@Component
public class MyBean { 
	private CassandraTemplate template; 
	@Autowired 
	public MyBean(CassandraTemplate template) { 
		this.template = template;
	}
}
```

### SpingBoot 使用RabbitMQ

( 1 ) 引入Starter

```
<dependency> 
	<groupId>org.springframework.boot</groupId>
	<artifactId> spring-boot-starter-rabbitmq</artifactId> 
</dependency>
```

( 2）设置application.properties

```
spring.rabbitmq.host=localhost #RabbitMQ服务地址
spring.rabbitmq.port=5672 #RabbitMQ端口号
spring.rabbitmq.username=admin #RabbitMQ用户名
spring.rabbitmq.password=secret #RabbitMQ密码
```

 ( 3 ）定义服务组件

```
@Component 
public class MyBean { 
	private final AmqpAdmin amqpAdmin; 
	private final AmqpTemplate amqpTemplate; 
	@Autowired 
	public MyBean(AmqpAdmin amqpAdmin, AmqpTemplate amqpTemplate) { 	
		this.amqpAdmin = amqpAdmin;
		this.amqpTemplate = amqpTemplate;
	}
}
```

（4 ）定义队列

```
@Configuration
public class QueueConf { 
	//1：定义Queue实例对象，队列名称为someQueue
	@Bean(name=”message”) 
	public Queue queueMessage() { 
		return new Queue (”someQueue”);
	}
}
```

( 5 ）发送消息

```
@Component 
public class MyBeanSender { 
	@Autowired 
	private AmqpTemplate template; 
	public void send() { 
		//向队列someQueue发送一条消息hello,rabbit 
		template.convertAndSend(”someQueue”,”hello, rabbit”); 
	}
}
```

( 5）接收消息

```
@Component 
public class MyBean { 
	//监听和接收队列someQueue上的消息
	@RabbitListener(queues=”someQueue”) 
	public void processMessage(String cotent){ 
	}
}
```

###  Spring Boot 使用Kafka

( 1 ）引入Starter

```
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId>spring-boot-starter-kafka </artifactId> 
</dependency>
```

( 2 ）设置application.propeties

```
#Kafka服务地址
spring.kafka.bootstrap-servers=localhost:9092 
#Kafka消费组
spring.kafka.consumer.group-id=myGroup 
```

( 3 ）发送消息

```
@Component 
public class MyBean｛
	private final KafkaTemplate kafkaTemplate; 
	@Autowired
	public MyBean(KafkaTemplate kafkaTemplate) { 
		this.kafkaTemplate = kafkaTemplate; 
	}
	public Response sendKafka() { 
		//向Kafka的someTopic发送一条消息
		kafkaTemplate.send (”someTopic”,”key”,message) ; 
	}
}
```

( 4）接收消息

```
@Component 
public class MyBean { 
	//监听并接收someTopic上的消息
	@KafkaListener(topics =”someTopic”) 
	public void processMessage(String content) { 
		System.out.println(”message:”+ content) ;
    }
}
```

