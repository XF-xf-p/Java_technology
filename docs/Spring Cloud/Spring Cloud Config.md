# Spring Cloud Config

## 1.Spring Cloud Config

随着项目复杂度的增加和微服务开发组件的细化，散落在服务器各个角落的微服务组件需要一套在线的配置服务；一方面为整个服务提供统一的配置，避免在每个微服务中修改配置带来的不便和易出错的问题；另一方面保证了微服务配置能自动化更新到各个组件中，避免在修改配置后重启时出现服务不稳定的情况。

Spring Cloud Config为分布式系统提供统一的配置管理工具，应用程序在使用过程中可以像使用本地配置一样方便地添加、访问、修改配心的配置。SpringCloud Config将Environment的PropertySource抽象和配置中心的配置进行映射，以便应用程序可以在任何场景下获取和修改配置。

## 2.Spring Cloud Config的原理

Spring Cloud Config支持将配置存储在配置中心的本地服务器、Git仓库或Subversion。在SpringCloud Config的线上环境中，通常将配置文件集中放置在一个Git仓库里，然后通过配置中心服务端（Config Server）来管理所有的配置文件；当某个服务实例需要添加或更新配置时，只要在该服务实例的本地将配置文件进行修改，然后推送到Git仓库，其他服务实例通过配置中心从Git服务端获取最新的配置信息。对于配置中心来说，每个服务实例都相当于客户端（Config Client）。

为了保证系统的稳定，配置中心服务端可以进行多副本集群部署，前端使用负载均衡实现服务之间的请求转发。

## 3.Config Server的定义和使用

Spring Cloud Config服务为外部配置（键值对或YAML）提供了基于HTTP的远程资源访问接口。服务端可以使用@EnableConfigServer注释开启配置中心的功能，声明该应用程序是一个配置中心服务。

创建一个Config Server分为4步，首先在pom.xml中引人spring-cloud-config-server和spring-boot-starter-actuator依赖，然后通过＠EnableConfigServer注解开启配置服务，接着配置appIication. properties配置文件，最后一步是访问和使用。

( 1) pom.xml添加依赖。

( 2 ) @EnableConfigServer添加

@EnableConfigServer开启SpringBoot项目对分布式配置中心的支持功能

```
@SpringBootApplication
@EnableConfigServer 
public class BootApplcation{ 
	public static void main(String[] args) { 		
		SpringApplication.run(BootApplication.class, args); 
	}
}
```

( 3 ) application.properties配置

Spring Cloud Config将分布式配置文件的数据存放在Git仓库中，因此需要配置Git仓库的基本信息，具体配置如下

```
＃配置中心端口号
server.port=9000 
＃配置中心名称
spring.cloud.config.server.default-application-name=config-server 
＃配置Git仓库地址
spring.cloud.config.server.git.uri＝https://github.com/LOVEGISER/SpringCloud
＃配置仓库的路径
spring.cloud.config.server.git.search-paths=SpringCloudConfig ＃配置仓库的分支
spring.cloud.config.label=master 
＃访问Git仓库的用户名
spring.cloud.config.server.git.username=username
＃访问Git仓库的用户密码如果Git为公开仓库，可以不填写用户名和密码spring.cloud.config.server.git.password=password
```

( 4）访问服务地址

启动应用程序，在浏览器地址栏中输入http://localhost: 9000/*/dev，返回配置信息。

从返回信息可以看到，Git仓库的地址为https://github.com/LOVEGISER/SpringCJoud/SpringCloudConfig，仓库中有一个配置文件application.properties，具体的配置信息在Source属性中存储。

## 4.Config Client 

配置中心的使用分为3步：首先在pom.xml中引人spring-boot-starter-actuator和spring-cloud-starter-config依赖，然后在bootstrap.properties中设置配置中心的地址，最后是配置中心的使用。

( 1) pom.xml添加依赖。

( 2) bootstrap.properties配置。

bootstrap. properties中的spring.cloud.config.uri参数用于设置应用程序从哪个服务地上获取配置信息，并且可以通过spring.cloud.config.profile指定运行环境，例如，开发环境（dev）、测试环境（test ）、正式运行环境（pro）等。

```
＃项目名称，一般与Git仓库中的文件名对应spring.application.name=config-client 
＃远程仓库的分支
spring.cloud.config.label=master 
＃运行环境：dev开发环境，test测试环境，pro正式运行环境
spring.cloud.config.profile=dev
＃配置服务中心的地址
spring.cloud.config.uri= http://localhost:9000/ 
＃服务端口号
server.port=9001 
```

( 3 ）配置信息的使用

配置信息的使用简单方便，SpringCloud Config将配置文件和PropetySource做了映射，对于应用程序来说，就像使用本地配置文件样使用ConfigServer上的配置文件，体使用如下

```
／／将Key为spring.datasource.url对应的值映射为springDatasourceURL／／其中spring.datasource.url为Git配置文件中的Key
@Value(”${spring.datasource.url}”) 
String springDatasourceURL;
```

