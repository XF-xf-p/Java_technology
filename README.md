# java工程师修炼之道

## 阶段一：java基础

- [java基础语法](/docs/java基础/java基础语法.md)
- [面向对象](/docs/java基础/面向对象.md)
- 高级类特性
  - 接口
  - 内部类
  - [枚举](/docs/java基础/枚举.md)
  - [注解](/docs/java基础/注解.md)
- JavaAPI
  - [包装类](/docs/java基础/包装类.md)
  - [String](/docs/java基础/String.md)
  - [时间处理](/docs/java基础/时间处理.md)
- [异常](/docs/java基础/异常.md)
- [集合](/docs/java基础/集合.md)
  - [List](/docs/java基础/List.md)
    - [CopyOnWriteArrayList](/docs/java基础/CopyOnWriteArrayList.md)
    - [SynchronizedList](/docs/java基础/SynchronizedList.md)
  - [Set](/docs/java基础/Set.md)
  - [Map](/docs/java基础/Map.md)
    - [HashMap](/docs/java基础/HashMap.md)
    - [ConcurrentHashMap](/docs/java基础/ConcurrentHashMap.md)
  - Iterator
  - Collections
  - [Queue](/docs/java基础/Queue.md)
- [泛型](/docs/java基础/泛型.md)
- [IO流](/docs/java基础/IO流.md)
- 多线程
  - 线程的创建
  - 线程的生命周期
  - 线程的同步
  - 死锁
- [反射](/docs/java基础/反射.md)
- 新特性
- [序列化](/docs/java基础/序列化.md)
- 网络编程
  - 网络通信协议
  - TCP协议与UDP协议
- 提升
  - [java的编译原理](/docs/java基础/java的编译原理.md)
  - [JIT编译器](/docs/java基础/JIT编译器.md)
  - [常量池](/docs/java基础/常量池.md)
  - [编程规范](/docs/java基础/编程规范.md)
  - [常用注解](/docs/java基础/常用注解.md)
  - [关键字](/docs/java基础/关键字.md)
  - [语法糖](/docs/java基础/语法糖.md)

## 阶段二：JavaWEB

- XML
- WEB服务器Tomcat
- Servlet
- Cookie&Session
- Filter&Listener
- 国际化
- 文件上传下载
- 数据库
  - 关系型数据库
    - [MySQL](/docs/并发编程/MySQL.md)
      - SQL语句
      - DML、DCL、DDL
      - 事物
      - 索引
  - 非关系型数据库
- 并发编程
  - [AQS](/docs/并发编程/AQS.md)
  - [CAS](/docs/并发编程/CAS.md)
  - [Java并发](/docs/并发编程/Java并发.md)
  - [synchronized](/docs/并发编程/synchronized.md)
  - [volatile](/docs/并发编程/volatile.md)
  - [ThreadLocal](/docs/并发编程/ThreadLocal.md)
  - [锁](/docs/并发编程/锁.md)
  - [线程](/docs/并发编程/线程.md)
  - [线程池](/docs/并发编程/线程池.md)
  - [阻塞队列](/docs/并发编程/阻塞队列.md)

## 阶段三：JavaEE主流框架

- [Spring](/docs/Spring.md)
- [Spring MVC](/docs/Spring MVC.md)
- [MyBatis](/docs/MyBatis.md)
- JPA
  - 注解
  - 映射
- SpringData
- Maven
  - 本地仓库&中央仓库
  - 继承&聚合
- Git
  - 常用操作命令
  - 工作流
- Linux
  - 常用命令
- Shell编程
  - Shell基础
- 缓存
  - [Redis](/docs/Redis.md)
  - memcache
- Shiro
  - 工作流程
  - 认证流程
  - 缓存
- Activiti5
  - 工作流&工作流引擎
  - 流程定义
- WebService
- JVM
  - [java网路编程模型](/docs/JVM/java网路编程模型.md)
  - [JVM的类加载](/docs/JVM/JVM的类加载.md)
  - [JVM的内存区域](/docs/JVM/JVM的内存区域.md)
  - [JVM的运行机制](/docs/JVM/JVM的运行机制.md)
  - [JVM的运行时内存](/docs/JVM/JVM的运行时内存.md)
  - [垃圾回收与算法](/docs/JVM/垃圾回收与算法.md)
- Quartz
  - 作业调度
- Nginx
  - 反向代理
- [NIO](/docs/NIO.md)

## 阶段四：微服务

- [Dubbo](/docs/Dubbo.md)
- NIO
  - 缓冲区与通道
  - 阻塞与非阻塞
- 分库分表
  - MyCat
    - 分片
    - 读写分离
  - Sharding-JDBC
- FastDFS
  - 文件上传
- [Spring Boot](/docs/JVM/Spring Boot.md)
- [Spring Cloud](/docs/JVM/Spring Cloud.md)
  - [Spring Cloud Config](/docs/JVM/Spring Cloud Config.md)
  - [Spring Cloud Consul](/docs/JVM/Spring Cloud Consul.md)
  - [Spring Cloud Eureka](/docs/JVM/Spring Cloud Eureka.md)
  - [Spring Cloud Feign](/docs/JVM/Spring Cloud Feign.md)
  - [Spring Cloud Gateway](/docs/JVM/Spring Cloud Gateway.md)
  - [Spring Cloud Hystrix](/docs/JVM/Spring Cloud Hystrix.md)
  - [Spring Cloud Sleuth](/docs/JVM/Spring Cloud Sleuth.md)
  - [Spring Cloud Zuul](/docs/JVM/Spring Cloud Zuul.md)
  - [Spring Cloud的链路监控](/docs/JVM/Spring Cloud的链路监控.md)
- Docker
- 单点登录
- 搜索引擎
  - [Elasticsearch](/docs/Elasticsearch.md)
- [Zookeeper](/docs/Zookeeper.md)
- 消息中间件
  - ActiveMQ
  - [RabbitMQ](/docs/RabbitMQ.md)
  - [RocketMQ](/docs/RocketMQ.md)
  - [kafka](/docs/kafka.md)
- [Netty](/docs/Netty.md)

## 阶段五：分布式

- 分布式架构
- [分布式事务](/docs/分布式/分布式事务.md)
- 分布式session
- [分布式缓存](/docs/分布式/分布式缓存.md)
- 分布式全局id
- 分布式锁
- 分布式定时任务调度
- 高可用
- 高并发
- 高流量

## 阶段六：微服务高级

- Consul
- SpringCloudAlibaba
  - Nacos：服务发现、配置和管理
  - Seata：分布式事物中间件
  - Sentinel：流量控制、熔断降级及系统负载保护
- Reddsion
- OSS
- JMeter
- JVisualVm
- Vagrant
- kibana

## 阶段七：自动化部署

- kubernetes
- Jenkins
- SonarQube：项目工程代码质量检测

## 阶段八：大数据

- [Hadoop](/docs/Hadoop.md)
- [HBase](/docs/HBase.md)
- [HDFS](/docs/HDFS.md)
- Hive
- Impala
- [Spark](/docs/Spark.md)
- [Flink](/docs/Flink.md)

## 阶段九：提升

- 算法
  - [常用算法](/docs/常用算法.md)
- 设计模式](/docs/设计模式/设计模式.md)
  - 创建型模型
    - [工厂模式（factory Pattern）](/docs/设计模式/工厂模式.md)
    - [抽象工程模式（Abstract Factory Pattern）](/docs/设计模式/抽象工程模式.md)
    - [单例模式（Singleton Pattern）](/docs/设计模式/单例模式.md)
    - [建造者模式（Builder Pattern）](/docs/设计模式/建造者模式.md)
    - [原型模式（Prototype Pattern）](/docs/设计模式/原型模式.md)
  - 结构型模式
    - [适配器模式（Adapter Pattern）](/docs/设计模式/适配器模式.md)
    - [桥接模式（BridgePattern）](/docs/设计模式/桥接模式.md)
    - 过滤器模式（Filter Pattern）
    - [组合模式（Composite Pattern）](/docs/设计模式/组合模式.md)
    - [装饰者模式（Decorator Pattern）](/docs/设计模式/装饰者模式.md)
    - [外观模式（Facade Pattern）](/docs/设计模式/外观模式.md)
    - [享元模式（Flyweight Pattern）](/docs/设计模式/享元模式.md)
    - [代理模式（Proxy Pattern）](/docs/设计模式/代理模式.md)
  - 行为型模式
    - [责任链模式（Chain of Responsibility Pattern）](/docs/设计模式/责任链模式.md)
    - [命令模式（Command Pattern）](/docs/设计模式/命令模式.md)
    - [解释器模式（Interpreter Pattern）](/docs/设计模式/解释器模式.md)
    - [迭代器模式（Iterator Pattern）](/docs/设计模式/迭代器模式.md)
    - [中介者模式（Meditor Pattern）](/docs/设计模式/中介者模式.md)
    - [备忘录模式（Memento Pattern）](/docs/设计模式/备忘录模式.md)
    - [观察者模式（Observer Pattern）](/docs/设计模式/观察者模式.md)
    - [状态模式（State Pattern）](/docs/设计模式/状态模式.md)
    - [策略模式（Straregy Pattern）](/docs/设计模式/策略模式.md)
    - [模板模式（Template Pattern）](/docs/设计模式/模板模式.md)
    - [访问者模式（Vistor Pattern）](/docs/设计模式/访问者模式.md)
- [数据结构](/docs/数据结构.md)
- 性能调优
  - 代码调优
  - [Mysql调优](/docs/Mysql调优.md)
    - 慢sql
  - jvm调优
    - 常见oom及原因
    - co
    - cpu占用过高
  - tomcat调优
- 安全技术
  - 加密
  - HTTPS
  - 安全协议
  - web安全
- 计算机基础
  - 计算机操作系统
  - [计算机网络](/docs/计算机网络.md)
  - 计算机组成原理
- UML建模
- 中台架构

## 阶段十：源码分析

## 阶段十一：手写框架

## 阶段十二：系统设计

- 秒杀系统