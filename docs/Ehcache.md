# Ehcache

Ehcache是基于Java实现的一套简单、高效、线程安全的缓存管理类库。Ehcache提供了内存、磁盘文件及分布式存储方式等多种灵活的Cache管理方案，特点是快速、轻量、可伸缩、操作灵活、支持持久化等。

## 1.Ehcache的原理

Ehcache是基于Java实现的高效缓存框架，其内部采用多线程实现，采用LinkedHashMap存储元素，同时支持将数据持久化到物理磁盘上。

## 2.Ehcache的特点

（1）快速：Ehcache内部采用多线程机制实现，数据存取性能高。

（2）轻量：Ehcache的安装包大小只有1.6MB，可以被快速、方便地继承到系统中。

（3）可伸缩：Ehcache缓存在内存和硬盘的存储可以伸缩到数几十GB，可轻松应对大数据场景。

（4）操作灵活：Ehcache提供了丰富的API接口，可实现基于主键、条件进行数据读取等。同时，Ehcache支持在运行时修改缓存配置（存活时间、空闲时间、内存的最大数据、磁盘的最大数量），提高了系统维护的灵活性。

（5）支持多种淘汰算法：Ehcache支持最近最少被使用、最少被使用和先进先出缓存策略。

（6）支持持久化：Ehcache支持将缓存数据持久化到磁盘上，在机器重启后从磁盘上重新加载缓存数据。

## 3.Ehcache的架构

Ehcache在架构上由Cache Replication、In-Process API和Core组成。其中，Cache Replication存储缓存副本；In-Process API封装操作缓存数据的API，包括Hibernate API、JMX API、Servlet Cacheing FilterAPI等；Core是Ehcache的核心部分，包括用于管理缓存的CacheManger、用于存储缓存的Store和用于操作缓存的Cache API等；NetWork APIs提供RESTful API、SOAP API等Web API接口

## 4.Ehcache的存储方式

Ehcache的存储方式包括堆存储、堆外存储和磁盘存储。

（1）堆存储：将缓存数据存储在Java堆内存中，其特点是存取速度快，但容量有限。

（2）堆外存储：基于NIO的DirectByteBuffers实现，将缓存数据存储在堆外内存上。其特点是比磁盘存取速度快，而且不受GC的影响，可以保证响应时间的稳定性，在内存分配上开销比堆内存大，而且要求必须以字节数组方式存储，因此对象必须在存储过程中进行序列化，对读取操作则进行反序列化，数据存取速度比堆内存慢一个数量级。

（3）磁盘存储：将数据存储在磁盘上，保障服务重启后内存数据能够重新从磁盘上加载，其读取效率最低，是内存数据持久化的一种方式。

## 5.Ehcache的扩展模块

Ehcache是开放的缓存系统，除自身的实现外还有其他扩展模型，这些扩展模型是相互独立的库，每个都为Ehcache添加新的功能。

- ehcache-core：API，标准缓存引擎，RMI复制和Hibernate支持
- ehcache：分布式Ehcache，包括Ehcache的核心和Terracotta的库
- ehcache-monitor：企业级监控和管理
- ehcache-web：为Java Servlet Container提供缓存，gzip压缩支持的filters
- ehcache-jcache：JSR 107 JCACHE的实现
- ehcache-jgroupsreplication：使用JGroup的复制
- ehcache-jmsreplication：使用JMS的复制
- ehcache-openjpa：OpenJPA插件
- ehcache-server：在war内部署或者单独部署的RESTful cache server
- ehcache-unlockedreadsview：允许Terracotta cache 的无锁读
- ehcache-debugger：记录RMI分布式调用事件
- Ehcache for Ruby：Jruby和Rails支持

## 6.Ehcache的应用

在Spring Boot中使用Ehcache组件比较简单，分为引入jar包、配置ehcache.xml和使用Ehcache缓存。

（1）引入jar包。

（2）设置ehcache.xml。在项目resource的目录下新建ehcache.xml配置文件，

```
<ehcache>
	<cache name="user" eternal="true" overflowToDisk="true" maxElementsInMemory="1000"/>
</ehcache>
```

以上代码在ehcache.xml配置文件中声明了一个名称为user的缓存，其中eternal=true表示缓存对象永不过期，maxElementsInMemory表示内存中该Cache可存储最大的数据量，overflowToDisk=true表示在内存缓存的对象数量达到了maxElementsInMemory界限后，会把溢出的对象写到磁盘缓存中。注意：如果需要将缓存的对象写入磁盘中，则该对象必须实现了Serializable接口。

（3）使用Ehcache缓存：

```
@CachePut(value="user",key="#user.id")
```

以上代码定义了名为UserService的类，同时定义了保存用户数据的方法save()和查找用户数据的方法findOne()，并分别在方法上通过@Cacheable(value = "user", key ="#user.id")开启Ehcache缓存。

在用户调用save()保存数据时会在Ehcache内存中也保存一份User对象，其key为User对象的id属性。在用户调用findOne()查询该数据时，首先会去Ehcache缓存中查找数据，如果在缓存中存在该数据，则将该数据返回，如果在缓存中不存在该数据，则会去数据库中查询并返回结果。