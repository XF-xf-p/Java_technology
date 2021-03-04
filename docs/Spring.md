# Spring

## 1.Spring的特性

Spring特性包括轻量、控制反转（Inversion of Control, IoC ）、面向容器、面向切面（Aspect Oriented Programming, AOP）和框架灵活。

![](D:\workspace\Java-Interview-Offer\images\spring002.png)

### 控制反转

Spring的控制反转指一个对象依赖的其他对象将会在容器的初始化完成后主动将其依赖的对象传递给它，而不需要这个对象自己创建或者查找其依赖的对象。Spring基于控制反转技术实现系统对象间依赖的解耦。

### 面向容器

Spring实现了对象的配置化生成和对象的生命周期管理，因此，可以理解为其是面向容器的，通过Spring的XML文件或者注解方式，应用程序可以配置每个Bean对象被创建和销毁的时间，以及Bean对象创建的先后顺序和依赖关系。Spring中的实例对象可以是全局唯一的单例模式，也可以在每次需要时都重新生成一个新的实例，具体以哪种方式创建Bean对象由Bean的生命周期决定，通过prototype属性来定义。

### 面向切面

Spring提供了面向切面的编程支持，面向切面技术通过分离系统逻辑和业务逻辑来提高系统的内聚性。在具体的使用过程中，业务层只需要关注并实现和业务相关的代码逻辑，而不需要关注系统功能（例如系统日志、事务支持）和业务功能的复杂关系，Spring通过面向切面技术将系统功能自动织入业务逻辑的关键点。

## 2.Spring的模块

Spring提供的常用模块有核心容器层（Core Container）、 数据访问层（DataAccess ）、Web应用层（Web Access）。除此 之外，Spring还包括AOP、Aspects、Instrumentation、Messaging和Test。

![](D:\workspace\Java-Interview-Offer\images\spring001.png)

## 3.Spring 常用模块

![](D:\workspace\Java-Interview-Offer\images\spring003.png)



## 4.Spring 主要包

![](D:\workspace\Java-Interview-Offer\images\spring006.png)

## 5.Spring 常用注解

bean注入与装配的的方式有很多种，可以通过xml，getset方式，构造函数或者注解等。简单易用的方式就是使用Spring的注解了，Spring提供了大量的注解方式。

![](D:\workspace\Java-Interview-Offer\images\spring007.png)

## 6.Spring 第三方结合

![](D:\workspace\Java-Interview-Offer\images\spring008.png)

## 13.Spring loC简介

Spring通过一个配置文件描述Bean和Bean之间的依赖关系，利用Java的反射功能实例化Bean并建立Bean之间的依赖关系。Spring的IoC容器在完成这些底层工作的基础上，还提供了Bean实例缓存管理、Bean生命周期管理、Bean实例代理、事件发布和资源装载等高级服务。

## 14.Spring Bean的装配流程

Spring在启动时会从XML配置文件或注解中读取应用程序提供的Bean配置信息，并在Spring容器中生成一份相应的Bean配置注册表；然后根据这张注册表实例化Bean，装配好Bean之间的依赖关系，为上层业务提供基础的运行环境。其中Bean缓存池为HashMap实现。

![](D:\workspace\Java-Interview-Offer\images\spring004.png)

## 15.IOC容器实现

### BeanFactory-框架基础设施

BeanFactory 是Spring 框架的基础设施，面向Spring 本身；ApplicationContext 面向使用Spring 框架的开发者，几乎所有的应用场合我们都直接使用ApplicationContext 而非底层的BeanFactory。

![](D:\workspace\Java-Interview-Offer\images\spring009.png)

#### BeanDefinitionRegistry注册表

Spring 配置文件中每一个节点元素在Spring 容器里都通过一个BeanDefinition 对象表示，它描述了Bean 的配置信息。而BeanDefinitionRegistry 接口提供了向容器手工注册BeanDefinition 对象的方法。

#### BeanFactory 顶层接口

位于类结构树的顶端，它最主要的方法就是getBean(String beanName)，该方法从容器中返回特定名称的Bean，BeanFactory 的功能通过其他的接口得到不断扩展：

#### ListableBeanFactory

该接口定义了访问容器中Bean 基本信息的若干方法，如查看Bean 的个数、获取某一类型Bean 的配置名、查看容器中是否包括某一Bean 等方法；

#### HierarchicalBeanFactory父子级联

父子级联IoC 容器的接口，子容器可以通过接口方法访问父容器；通过HierarchicalBeanFactory 接口，Spring 的IoC 容器可以建立父子层级关联的容器体系，子容器可以访问父容器中的Bean，但父容器不能访问子容器的Bean。Spring 使用父子容器实现了很多功能，比如在Spring MVC 中，展现层Bean 位于一个子容器中，而业务层和持久层的Bean 位于父容器中。这样，展现层Bean 就可以引用业务层和持久层的Bean，而业务层和持久层的Bean 则看不到展现层的Bean。

#### ConfigurableBeanFactory

是一个重要的接口，增强了IoC 容器的可定制性，它定义了设置类装载器、属性编辑器、容器初始化后置处理器等方法；

#### AutowireCapableBeanFactory自动装配

定义了将容器中的Bean 按某种规则（如按名字匹配、按类型匹配等）进行自动装配的方法；

#### SingletonBeanRegistry运行期间注册单例Bean

定义了允许在运行期间向容器注册单实例Bean 的方法；对于单实例（singleton）的Bean 来说，BeanFactory会缓存Bean 实例，所以第二次使用getBean() 获取Bean 时将直接从IoC 容器的缓存中获取Bean 实例。Spring 在DefaultSingletonBeanRegistry 类中提供了一个用于缓存单实例Bean 的缓存器，它是一个用HashMap 实现的缓存器，单实例的Bean 以beanName 为键保存在这个HashMap 中。

#### 依赖日志框框

在初始化BeanFactory 时，必须为其提供一种日志框架，比如使用Log4J，即在类路径下提供Log4J 配置文件，这样启动Spring 容器才不会报错。

### ApplicationContext面向开发应用

ApplicationContext 由BeanFactory 派生而来，提供了更多面向实际应用的功能。ApplicationContext 继承了HierarchicalBeanFactory 和ListableBeanFactory 接口，在此基础上，还通过多个其他的接口扩展了BeanFactory 的功能：

![](D:\workspace\Java-Interview-Offer\images\spring010.png)

1.ClassPathXmlApplicationContext：默认从类路径加载配置文件。

2.FileSystemXmlApplicationContext：默认从文件系统中装载配置文件。

3.ApplicationEventPublisher：让容器拥有发布应用上下文事件的功能，包括容器启动事件、关闭事件等。

4.MessageSource：为应用提供i18n 国际化消息访问的功能；

5.ResourcePatternResolver ：所有ApplicationContext 实现类都实现了类似于PathMatchingResourcePatternResolver 的功能，可以通过带前缀的Ant 风格的资源文件路径装载Spring 的配置文件。

6.LifeCycle：该接口是Spring 2.0 加入的，该接口提供了start()和stop()两个方法，主要用于控制异步处理过程。在具体使用时，该接口同时被ApplicationContext 实现及具体Bean 实现，ApplicationContext 会将start/stop 的信息传递给容器中所有实现了该接口的Bean，以达到管理和控制JMX、任务调度等目的。

7.ConfigurableApplicationContext 扩展于ApplicationContext，它新增加了两个主要的方法：refresh()和close()，让ApplicationContext 具有启动、刷新和关闭应用上下文的能力。在应用上下文关闭的情况下调用refresh()即可启动应用上下文，在已经启动的状态下，调用refresh()则清除缓存并重新装载配置信息，而调用close()则可关闭应用上下文。

### WebApplication体系架构

WebApplicationContext 是专门为Web 应用准备的，它允许从相对于Web 根目录的路径中装载配置文件完成初始化工作。从WebApplicationContext 中可以获得ServletContext 的引用，整个Web 应用上下文对象将作为属性放置到ServletContext 中，以便Web 应用环境可以访问Spring 应用上下文。

## 3.核心容器层

核心容器层由Spring-Beans、Spring-Core、Spring-Context和SpEL( Spring Expression Language, Spring表达式语言)等模块组成。

### Spring-Beans 

Spring-Beans 模块基于工厂模式实现对象的创建。Spring-Beans通过XML配置文件实现了声明式的对象管理，将对象之间复杂的依赖关系从实际编码逻辑中解耦出来。

### Spring-Core

Spring-Core模块是Spring的核心功能实现，具体包括控制反转和依赖注入，所谓依赖注入，是指在一个Bean实例中引用另外一个Bean实例时，Spring容器会自动创建其所依赖的Bean实例，并将该Bean实例注入（传递）至对应的Bean中。

### Spring-Context 

Spring-Context模块是在Spring-Beans和Spring-Core模块的基础上构建起来的。Spring-Context模块继承自Spring-Beans模块，并且添加了国际化、事件传播、资源加载和透明地创建上下文等功能。

同时，Spring-Context模块提供了一些J2EE的功能，比如EJB、JMX和远程调用等。ApplicationContext接口是Spring-Context模块操作Bean的入口。

Spring-Context-Support提供了将第三方库集成到Spring-Context的支持，比如缓存( EhCache、Guava、JCache）、邮件(JavaMail）、调度（CommonJ、Quartz）和模板引擎( FreeMarker、JasperReports、Velocity）等。

### SpEL 

SpEL模块提供了丰富的表达式语言支持，用于在运行过程中查询和操作对象实例。SpEL在JSP2.1表达式语言规范的基础上进行了扩展，支持set方法、get方法、属性赋值、方法调用、访问数组集合、索引内容、逻辑算术运算、命名变量、基于名称在Spring IoC容器检索对象，同时支持列表的投影、选择和聚合等功能。

## 7.数据访问层

数据访问层包括JDBC、ORM、OXM、JMS和事务处理模块。

它们的术语描述。

- JDBC：Java Date Base Connectivity
- ORM：Object Relational Mapping
- OXM：Object XML Mapping
- JMS：Java Message Service

### JDBC

JDBC模块提供了JDBC抽象层。Spring持久化层基于JDBC抽象层实现了在不同数据库之间灵活切换，而不用担心不同数据库之间SQL语法的不兼容。

### ORM 

ORM模块提供了对象关系映射API的集成，包括JPA( Java Persistence API）、JDO( Java Data Object）和 Hibernate等。基于该模块，ORM框架能很容易地和Spring的其它功能（例如事务管理）整合。

### OXM

OXM模块提供了对OXM实现的支持，比如JAXB、Castor、XMLBeans、JiBX、XStream等

### JMS 

JMS模块包含消息的生产（Produc）和消费（onsume）功能。从Spring4.1开始，Spring集成了Spring-Messaging模块，用于实对消息队列的支持。

### 事务处理

事务处理（Transactions ）模块基于接口方式实现了声明式事务管式。编程式事务的实现需要应用程序调用相应的beanTransaction（）、commit（）、rollback（）等方法来实现事务的管理，Spring声明式事务只需要通过注解或配置即可实现事务的管理，具体的事务管理工作由Spring自动处理，应用程序不需要关心事务的提交（Commit）和回滚（Rollback）。

## 8.Web应用层

Web应用层主要包含Web交互和数据传输等相关功能，由WebWeb-MVWebSocketWeb-Portlet组成。

### Web 

Web模块不但提供了面向Web应用的基本功能，还提供了HTTP( Hyper Text Transfer Protocol，超文本传输协议）客户端及Spring远程调用中与Web相关的部分。Web模块基于Servlet监听器初始化IoC容器

### Web-MVC 

Web-MVC模块为Web应用提供了模型视图控制（ModeView Controller, MVC）和REST API服务的实现。Spring的MVC框架使数据模型和视图分离，数据模型负责数据的业务逻辑，视图负责数据的展示。同时，Web-MVC可与Spring框架的其他模块方便地集成

### Web-Socket 

Web-Socket模块提供了对WebSocket-Base的支持，用于实现在Web应用程序中服务端和客户端实时双向通信，尤其在实时消息推送中应用广泛

### Web-Portlet

Web-Portlet模块提供了基于Portlet环境的MVC实现，井提供了与Spring web-MVC模块相关的功能。

### 其他重要模块

除了上述介绍的模块，Spring还有其他一些重要的模块，例如，AOP、Aspects、Instrumentation、Messaging和Test等。

#### AOP 

AOP模块提供了面向切面的编程实现，允许应用程序通过定义方法拦截器和切人点来实现系统功能和业务功能之间的解耦。

#### Aspects 

Aspects模块提供了Spring与AspectJ的集成，是一个面向切面编程的模块

#### Instrumentation 

Instrumentation模块在应用中提供了对Instrumentation的支持和类加载器的实现。

#### Messaging 

Messaging模块为STOMP( Simple Text Orientated Messaging Protocol ，简单文本定向消息协议）提供了支持，主要用于应用程序中WebSocket子协议的实现。同时，Messaging模块可通过注解的方式来选择和处理来自WebSocket客户端的STOMP消息。

#### Test 

Test （测试）模块用于对JUnit或TestNG等测试框架提供支持，以实现Spring代码的自动化测试。

## 9.Spring的核心JAR包

Spring基于模块化实现，每个模块都对应不同的JAR包。

- Spring Core：Spring的核心工具包
- Spring Beans：spring IOC的实现，通过XML配置文件或注解的方式实现Spring Bean的管理
- Spring  Context：Spring上下文环境，用于Bean关系的管理和维护等
- Spring Aspects：Spring对对AspectJ框架的整合和支持
- Spring Context Support：Spring Context的扩展支持，用于MVC方面功能的支持
- Spring Expression Language：Spring表达式语言
- Spring Framework Bom：处理不同的项目依赖了不同版本的Spring引起的版本冲突问题
- Spring Instrument：Spring针对服务器的代理接口
- Spring Instrument Tomcat：Spring针对Tomcat的集成
- Spring JDBC：Spring针对JDBC的封装
- Spring JMS：Spring为简化JMS API的使用面做的封装
- Spring Messaging：Spring集成Messaging API和消息协议
- Spring ORM：Spring整合第三方的ORM的实现，如Hibernate、MyBatis、JDO及Spring的JPA
- Spring OXM：Spring对Object/XML映射的支持，可以让Spring在Java与XML之间方便的切换
- Spring Test：Spring对Junit等测试框架的支持
- Spring TX：Spring为JDBC、Hibernate、JDO、JPA等提供的一致的声明式事务管理和编程式事务管理
- Spring Web：基于Spring构建Web应用开发所需的核心类，包括自动载入WebApplicationContext、Struts与JSF集成、文件上传、Filter类和其他辅助工具类
- Spring WebMVC：包含Spring MVC框架相关的所有类，例如国际化、标签、Theme、视图展现的FreeMarker、JasperResports、Tiles、Vclocity、XSLT相关类。当然，如果你的应用使用了独立的MVC框架，则不需要使用这个JAR文件里的
- Spring WebMVC Portlet：基于Portlet环境的Spring MVC的增强

## 10.Spring的注解

Spring注解将应用程序中Bean定义和Bean之间复杂的依赖关系的配置从XML配置中解放出来，应用程序只需在需要某些服务或者功能时，使用注解依赖注入即可，具体Bean定义和依赖关系由Spring的自动装配来完成。这使得Spring的使用更加方便。

## 11.Spring 注解的使用

Spring注解的使用很简单，首先导人命名规范，然后指定IoC的扫描包，最后在Java类中直接使用注解依赖注入需要的资源即可。

### 导入命名空间及规范

在Spring的applicationContext.xm配置文件中导入命名空间及规范。

### 配置扫描包

在applicationContext.xml配置文件中配置需要扫描的包。如下代码开启了自动扫描com.alex.spring包下的所有类，也就是说，只有在com.alex.spring包中的注解才能够生效。

```
<!--指定Spring IOC容器扫描的包>
<context:component-scan base-package="com.alex.spring">
</context:component-scan>
```

### 使用注解

使用注解比较简单，直接在Java类中用＠按需使用即可。

```
@Controller／／使用＠Contrller注解声明一个控制器
public class RESTController { 
	@Autowired／／使用＠Autowired注解依赖注入RestTemplate
	RestTemplate rest; 
	／／使用＠RequestMapping注解声明一个服务，服务地址为rest
	@RequestMapping(value="/rest") 
	public String rest() { 
		return "rest ”; 
	}
}
```

## 12.Spring的常用注解

Spring的注解在开发中无处不在。

### Bean声明

@Component：定义基础层的通用组件，没有明确的角色

@Service：定义业务逻辑层的服务组件

@Repository：在数据访问层定义数据资源服务

@Controller：在展现层使用，用于定义控制器

### Bean注入

@Autowired ：服务依赖注入，一般用于注入＠Component、@Sevice定义的组件

@Resource：服务依赖注入，一般用于注入＠Repository定义的组件

### 配置类注解

@Configuration ：声明该类为配置类，其中＠Value属性可以直接和配置文件属性映射

@Bean：注解在方法上，声明该方法的返回值为一个Bean实例

@ComponentScan ：用于对Component进行扫描配置

### AOP注解

@EnableAspectJAutoProxy ：开启Spring对AspectJ代理的支持

@Aspect：声明一个切面，使用＠After、＠Before、＠Around定义通知（Advice），可直接将拦截规则（切点）作为参数

@After：在方法执行之后执行

@Before：在方法执行之前执行

@Around ：在方法执行之前和之后都执行

@PointCut ：声明一个切点

### @Bean属性支持注解

@Scope：设置Spring容器Bean实例的生命周期，取值有singleton、prototype、request、sessio和global session 

@PostConstruct：声明方法在构造函数执行完之后开始执行

@PreDestroy：声明方法在Bean销毁之前执行

@Value：为属性注入值

@Property Source ：声明和加载配置文件

### 异步操作注解

@EnableAsync ：声明在类上，开启对异步任务的支持

@Async ：声明方法是一个异步任务，Spring后台基于线程池异步执行该方法

### 定时任务相关

@EnableScheduling：声明在调度类上，开启对任务调度的支持

@Scheduled：声明一个定时任务，包柄cron、fixDelay、fixRate 等参数

### 开启功能支持

@EnableAspectJ AutoProxy：开启对AspectJ自动代理的支持

@EnableAsync ：开启对异步方法的支持

@EnableScheduling ：开启对计划任务的支持

@EnableWebMVC ：开启对WebMVC的配置支持

@EnableConfigurationProperties ：开启对＠ConfigurationProperties注解配置Bean的支持

@EnableJpaRepositories：开启对SpringData JPA Repository的支持

@EnableTransactionManagement ：开启对事务的支持

@EnableCaching ：开启对缓存的支持

### 测试相关注解

@Run With ：运行器，Spring中通常用于对JUnit的支持

@ContextConfiguration：用来加载配置ApplicationContext，其中classes属性用来加载配置类

## 13.Spring loC

Spring通过一个配置文件描述Bean和Bean之间的依赖关系，利用Java的反射功能实例化Bean并建立Bean之间的依赖关系。Spring的IoC容器在完成这些底层工作的基础上，还提供了Bean实例缓存管理、Bean生命周期管理、Bean实例代理、事件发布和资源装载等高级服务。

## 14.Spring Bean的装配流程

Sprin在启动时会从XML配置文件或注解中读取应用程序提供的Bean配置信息，并在Spring容器中生成一份相应的Bean配置注册表；然后根据这张注册表实例化Bean，装配好Bean之间的依赖关系，为上层业务提供基础的运行环境，其中Bean缓存池为HashMap实现。

![](D:\workspace\java\images\spring001.png)

## 15.Spring Bean的作用域

Spring为Bean定义了5种作用域，分别为Singleton（单例）、Prototype（原型）、Request （请求级别）、Session（会话级别）和Global Session （全局会话）。

### Singleton ：单例模式（多线程下不安全）

Singleton是单例模式，当实例类型为单例模式时，Spring IoC容器中只会存在一个共享的Bean实例，无论有多少个Bean引用它，都始终指向同一个Bean对象。该模式在多线程下是不安全的。Singleton作用域是Spring中的默认作用域，也可以通过配置将Bean定义为Singleton模式，具体配置如下

```
<bean id="userDao" class="com.alex.UserDaoImpl" scope="singleton">
```

### Prototype：原型模式每次使用时创建

Prototype是原型模式，每次通过Spring容器获取Prototype定义的Bean时，容器都将创建一个新的Bean实例，每个Bean实例都有自己的属性和状态，而Singleton全局只有一个对象。因此，对有状态的Bean经常使用Prototype作用域，而对无状态的Bean则使用Singleton作用域。具体配置如下

```
<bean id="userSerice" class="com.alex.UserSerice" scope="prototype">
```

### Request：一次request一个实例

Request指在一次HTTP请求中容器会返回该Bean的同一个实例，而对不同的HTTP请求则会创建新的Bean实例，并且该Bean实例仅在当前HTTP请求内有效，当前HTTP请求结束后，该Bean实例也将会随之被销毁。具体配置如下

```
<bean id="loginAction" class="com.alex.Login" scope="request">
```

### Session

Session指在一次HTTP Session中容器会返回该Bean的同一个实例，而对不同的Session请求则会创建新的Bean实例，该Bean实例仅在当前Session内有效。和HTTP请求相同，每一次Session都会创建新的Bean实例，而不同的Bean实例之间不共享数据，且Bean实例仅在自己的Session内有效，请求结束，则Bean实例将随之被销毁。具体配置如下

```
<bean id="userSession" class="com.alex.UserSession" scope="session">
```

### Global Session

Global Session指在一个全局的HTTP Session中容器会返回该Bean的同一个实例，且仅在使用Portlet Cotext时有效。

## 16.Spring Bean的生命周期

![](D:\workspace\Java-Interview-Offer\images\spring005.png)

#### 实例化

1.实例化一个Bean，也就是我们常说的new。

#### IOC依赖注入

2.按照Spring上下文对实例化的Bean进行配置，也就是IOC注入。

#### setBeanName实现

3.如果这个Bean已经实现了BeanNameAware接口，会调用它实现的setBeanName(String)方法，此处传递的就是Spring配置文件中Bean的id值

#### BeanFactoryAware实现

4.如果这个Bean已经实现了BeanFactoryAware接口，会调用它实现的setBeanFactory，setBeanFactory(BeanFactory)传递的是Spring工厂自身（可以用这个方式来获取其它Bean，只需在Spring配置文件中配置一个普通的Bean就可以）。

#### ApplicationContextAware实现

5.如果这个Bean已经实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，传入Spring上下文（同样这个方式也可以实现步骤4的内容，但比4更好，因为ApplicationContext是BeanFactory的子接口，有更多的实现方法）

#### postProcessBeforeInitialization接口实现-初始化预处理

6.如果这个Bean关联了BeanPostProcessor接口，将会调用postProcessBeforeInitialization(Object obj, String s)方法，该方法在Bean初始化前调用，常用于定义初始化Bean的前置工作，BeanPostProcessor经常被用作是Bean内容的更改，并且由于这个是在Bean初始化结束时调用那个的方法，也可以被应用于内存或缓存技术。

#### init-method

7.如果Bean在Spring配置文件中配置了init-method属性会自动调用其配置的初始化方法。

#### postProcessAfterInitialization

8.如果这个Bean关联了BeanPostProcessor接口，将会调用postProcessAfterInitialization(Object obj, String s)方法。注：以上工作完成以后就可以应用这个Bean了，那这个Bean是一个Singleton的，所以一般情况下我们调用同一个id的Bean会是在内容地址相同的实例，当然在Spring配置文件中也可以配置非Singleton。

#### Destroy过期自动清理阶段

9.当Bean不再被需要时，会在清理阶段被清理掉。如果Bean实现了DisposableBean接口，则Spring会在退出前调用实现类的destroy（）方法

#### destroy-method自配置清理

10.如果某个Bean的Spring配置文件中配置了destroy-method属性，在Bean被销毁前会自动调用其配置的销毁方法。

![](D:\workspace\Java-Interview-Offer\images\spring011.png)

11.bean 标签有两个重要的属性（init-method和destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（@PostConstruct和@PreDestroy）。

<bean id="" class="" init-method="初始化方法" destroy-method="销毁方法">。

## 17.Spring的4种依赖注入

### 构造器注入

构造器注入指通过在类的构造函数中注入属性或对象来实现依赖注入。如下代码通过＜bean></bean＞标签配置了一个id为persionDaolmpl的Bean，并通过<constructor-arg></constructor-arg>标签在其构造函数中注入了一个message属性，注入完成后在类中可以直接通过this.message获取注入的属性值。

```
／／在构造函数中注入message属性
public PersonDaoImpl(String message) { 
	this.message= message; 
}	
<!--定义Bean实例并在构造函数(constructor-arg)中注入message属性-->
<bean id=”persionDaoImpl" class=”com.PersionDaoImpl”>		<constructor-arg value=”message”></constructor-arg> </bean> 
```

###  set 方法注入

set方法注入是通过在类中实现get、set方法来实现属性或对象的依赖注入的。如下代码通过<bean></bean>标签配置了一个id为persionDaolmpl的Bean，并通过<property></property>标签在Bean中注入了一个id为123的属性值，注入完成后在类中可以直接使用getld（）获取注入的属性。

```
public class PersionDaoImpl { 
	private int id; ／／定义属性和其对应的get、set方法
	public int getId () { 
		return id; 
	} 
	public void setId（int id) { 
		this.id= id; 
	}
	<！--定义Bean实例并通过property注入id为123的属性值-->
<bean id="persionDaoImpl" class="com.PersionDaoImpl">		<property name=”id" value=”123”></property> 
</bean> 
```

### 静态工厂注入

静态工厂注入是通过调用工厂类中定义的静态方法来获取需要的对象的，为了让Spring管理所有对象，应用程序不能直接通过“工厂类．静态方法（）”的方式获取对象，而需要通过Spring注入的方式获取。

```
public class DaoFactory { //1 ：定义静态工厂
	public static final FactoryDao getStaticFactoryDaoImpl() { 
		return new StaticFactoryDaoImpl(); 
	}
}
public class SpringAction { 
	private FactoryDao staticFactoryDao; //2.定义工厂对象
    //3：注入工厂对象
    public void setStaticFactoryDao(FactoryDao staticFactoryDao){ 
    	this.staticFactoryDao = staticFactoryDao; 
    }
}
```

上述代码定义了一个DaoFactory工厂类和getStaticFactoryDaolmpl（）静态工厂方法，该方法实例化并返回一个StaticFactoryDaolmpl实例；同时定义了一个SpringAction类，并通过setStaticFactory Dao获取注入的FactoryDao。具体的XML注入语法如下

```
<!--：定义获取工厂对象的静态方法一〉<bean name="staticFactoryDao" class="DaoFactory" factory-method="getStaticFactoryDaoImpl"></bean>
<!-- factory-method＝”getStaticFactoryDaoImpl用于指定调用哪个工厂方法-->
<bean name="springActon" class="SpringAction">
	<!--2：注入静态工厂实例-->
	<property name="staticFactoryDao ref＝"staticFactoryDao"></property>
</bean> 
```

上述代码定义了一个name为staticFactoryDao的工厂类，并通过factory-method定义了实例化对象的方法，这里实例化对象的方法是一个名为getStaticFactoryDaoImpl的静态方法。该静态方法返回一个工厂类实例，在springAction中通过＜property></property＞标签注入静态工厂实例。

### 实例工厂注入

实例工厂注入指的是获取象实例的方法是非静态的，因此首先需要实例化，然后调用对例化方法来实例化对象

```
public class DaoFactory { //1： 实例工厂
	public FactoryDao getFactoryDaoImpl() { 
		return new FactoryDaoImpl(); 
	}
}
public class SpringAction { 
	private FactoryDao factoryDao; //2:注入对象				public void setFactoryDao(FactoryDao factoryDao) { 			this.factoryDao = factoryDao; 
	}
}
<bean name=”springAction”class=”SpringAction”>〈
	<！--1：使用实例工厂的方式注入对象-->
	<property name=”factoryDao" ref="factoryDao"></property>
</bean> 
<!--2:获取对象的方式是从工厂类中获取实例-->
<bean name=”daoFactory”class=”com.DaoFactory”></bean> <bean name="factoryDao" factory-bean=”daoFactory”factory-method=”getFactoryDaoImpl”>
</bean>
```

上述代码定义了一个name为factoryDao的工厂类，并通过factory-method定义了实例化对象的方法，这里实例化对象的方法是一个名为getFactoyDaolmpl的方法。该方法返回一个工厂类，在springAction中通过<property></property＞标签注入工厂实例。

## 18.自动装配的5种方式

Spring的装配方式包括手动装配和自动装配。手动装配包括基于XML装配（构造方法、set方法等）和基于注解装配2种方式。自动装配包括5种装配方式，这5种方式均可以用来引导Spring容器自动完成依赖注入，具体如下。

( I ) no：关闭自动装配，通过显式设置ref属性来进行对象装配。

( 2) byName：通过参数名自动装配，Bean的autowire被设置为byName后，Spring容器试图匹配并装配与该属性具有相同名字的Bean。

( 3) byType：通过参数类型自动装配，Bean的autowire被设置为byTyp后，Spring容器试图匹配并装配与该Bean的属性具有相同类型的Bean。

( 4 ) constructor：通过设置构造器参数的方式来装配对象，如果没有匹配到带参数的构造器参数类型，则Spring会抛出异常。

( 5 ) autodetect：首先尝试使用constructor来自动装配，如果无法完成自动装配，则使用byType方式进行装配。

## 19.Spring AOP简介

"横切"的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为"Aspect"，即切面。所谓"切面"，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性。

使用"横切"技术，AOP把软件系统分为两个部分：核心关注点和横切关注点。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处基本相似，比如权限认证、日志、事物。AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来。

AOP主要应用场景有：

- Authentication ：权限统一管理和授权
- Caching ：缓存统一维护
- Context Passing ：内容传递
- Error Handling ：系统统一错误处理
- Lazy Loading ：数据懒加载
- Debugging ：系统调试
- logging, tracing, profiling and monitoring：记录跟踪优化校准
- Performance Optimization ：性能优化
- Persistence：持久化
- Resource Pooling：资源池统一管理和申请
- Synchronization ：操作同步
- Transactions：统一事务管理

## 20.AOP的核心概念

- 横切关注点：定义对哪些方法进行拦截，拦截后执行哪些操作，这些关注点称之为横切关注点。
- 切面（Aspect）：横切关注点的抽象。
- 连接点（Joinpoint）：在Spring中，连接点指被拦截到的方法，但是从广义上来说，连接点还可以是字段或者构造器。
- 切入点（Pointcut）：对连接点进行拦截的定义。
- 通知（Advice)：拦截到连接点之后要执行的具体操作，通知分为前置通知、后置通知、成功通知、异常通知和环绕通知5类。
- 目标对象：代理的目标对象。
- 织入（Weave）：将切面应用到目标对象并执行代理对象创建的过程。
- 引入（Introduction）： 在运行期为类动态地添加一些方法或字段而不用修改类的代码。

![](D:\workspace\Java-Interview-Offer\images\spring012.png)

## 21.AOP的2种代理方式

Spring提供了JDK和CGLib2种方式来生成代理对象，具体生成代理对象的方式由AopProxyFactory根据AdvisedSupport对象的配置来决定。Spring默认的代理对象生成策略为：如果是目标类接口，则使用JDK动态代理技术，否则使用CGLib动态代理技术。

- JDK动态代理：JDK动态代理主要通过java.lang.reflect包中Proxy类和InvocationHandler接口来实现。InvocationHandler是一个接口，不同的实现定义不同的横切逻辑，并通过反射机制调用目标类的代码，动态地将横切逻辑和业务逻辑编制在一起。Proxy类利用InvocationHandler动态创建一个符合某一接口的实例，生成目标类的代理对象。JDK1.8中Proxy类的定义如下。

```
public class Proxy implement java.io.Serializable{ 			
	private static final long serialVersionUID= -2222568056686623797L; 
	//1 ：在构造方法参数中定义不同的InvocationHandler实现类
	private static final Class<?>[] constructorParams = { InvocationHandler.class }; 
	//2: Proxy类缓存列表
	private static final WeakCache<ClassLoader, Class<?>[], Class<?>>proxyClassCache =new WeakCache<>(new KeyFactory(), new ProxyClassFactory()); 
	//3 ：当前代理需要调用的Handler实例对象（该对象需要经过序列化〉
	protected InvocationHandler h; 
	／／．．．．此处忽略部分实现
	//4: Proxy类构造函数，参数InvocationHandler为当前代理的对象
	protected Proxy(InvocatonHandler h) { 				
		Objects.requireNonNull(h); 
		this.h = h; 
	}
	.......
}
```

- CGLib动态代理：CGLib即 Code Generation Library ，它是一个高性能的代码生成类库，可以在运行期间扩展Java类和实现Java接口。CGLib包的底层通过字节码处理框架ASM来实现，通过转换字节码生成新的类。
- CGLib动态代理和JDK动态代理的区别：JDK只能为接口创建代理实例，而对于没有通过接口定义业务方法的类，则只能通过CGLib创建动态代理来实现。

## 22.AOP的5种通知类型

- 前置通知：在一个方法执行之前执行通知
- 后置通知：在一个方法执行之后执行通知（无论方法执行成功还是失败通知都会执行）
- 成功通知：在一个方法执行成功之后执行通知（只有在方法执行成功时才执行通知）
- 异常通知：当一个方法执行抛出异常退出时，才执行该通知
- 环绕通知：在拦截方法调用之前和之后，分别执行通知

## 23.AOP的代码实现

在Spring中，AOP的使用比较简单，如下代码通过＠Aspect注解声明一个切面，通过＠Pointcut定义需要拦截的方法，然后用＠Before、＠AfterReturning、＠Around分别实现前置通知、后置通知和环绕通知要执行的方法

```
@Aspect//step 1 ：定义切面
public class TransactionDemo { 
	//step 2：定义要拦截的方法
	@Pointcut(value=”execution(* com.alex.core.service.*.*.*(..))")
	public void point() { 
	}
	@Before(value=”point()”) //step 3：定义前置通知
	public void before() { 		
		Systemout.printlntransactbegin”); 
	}
	@AfterReturning(value =”point()”)//step 4：定义后置通知
	public void after () { 
		System.out.println(”transaction commit ”); 
	}
	@Around(”point ()”) //step 5：定义环绕通知
	public void around(ProceedngJoinPoint joinPoint) throws Throwable{ 
		System.out.println（"transaction begin”); 			joinPoint.proceed(); 
		System.out.println（"transaction commit”); 	
	}
}
```

