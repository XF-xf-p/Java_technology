# **Eureka**

## 1.服务发现（Service Discovery）

它抽象出来了一个注册中心，当一个新的服务上线时，它会将自己的 IP 和端口注册到注册中心去，会对注册的服务进行定期的心跳检测，当发现服务状态异常时将其从注册中心剔除下线。服务 A 只要从注册中心中获取服务 B 的信息即可，即使当服务 B 的 IP 或者端口变更了，服务 A 也无需修改，从一定程度上解耦了服务。服务发现目前业界有很多开源的实现，比如 `apache` 的 [zookeeper](https://github.com/apache/zookeeper)、 `Netflix` 的 [eureka](https://github.com/Netflix/eureka)、 `hashicorp` 的 [consul](https://github.com/hashicorp/consul)、 `CoreOS` 的 [etcd](https://github.com/etcd-io/etcd)。

## 2.Eureka 是什么

采用的是 Client / Server 模式进行设计，基于 http 协议和使用 Restful Api 开发的服务注册与发现组件，提供了完整的服务注册和服务发现，可以和 `Spring Cloud` 无缝集成。其中 Server 端扮演着服务注册中心的角色，主要是为 Client 端提供服务注册和发现等功能，维护着 Client 端的服务注册信息，同时定期心跳检测已注册的服务当不可用时将服务剔除下线，Client 端可以通过 Server 端获取自身所依赖服务的注册信息，从而完成服务间的调用。

## 3.服务注册中心（Eureka Server）

我们在项目中引入 `Eureka Server` 的相关依赖，然后在启动类加上注解 `@EnableEurekaServer` ，就可以将其作为注册中心。

我们继续添加两个模块 `service-provider` ， `service-consumer` ，然后在启动类加上注解 `@EnableEurekaClient` 并指定注册中心地址为我们刚刚启动的 `Eureka Server` ，再次访问可以看到两个服务都已经注册进来了。

可以看到 `Eureka` 的使用非常简单，只需要添加几个注解和配置就实现了服务注册和服务发现，接下来我们看看它是如何实现这些功能的。

### 服务注册（Register）

注册中心提供了服务注册接口，用于当有新的服务启动后进行调用来实现服务注册，或者心跳检测到服务状态异常时，变更对应服务的状态。服务注册就是发送一个 `POST` 请求带上当前实例信息到类 `ApplicationResource` 的 `addInstance` 方法进行服务注册。

可以看到方法调用了类 `PeerAwareInstanceRegistryImpl` 的 `register` 方法，该方法主要分为两步：

1. 调用父类 `AbstractInstanceRegistry` 的 `register` 方法把当前服务注册到注册中心
2. 调用 `replicateToPeers` 方法使用异步的方式向其它的 `Eureka Server` 节点同步服务注册信息

服务注册信息保存在一个嵌套的 `map` 中，

第一层 `map` 的 `key` 是应用名称（对应 `Demo` 里的 `SERVICE-PROVIDER` ），第二层 `map` 的 `key` 是应用对应的实例名称（对应 `Demo` 里的 `mghio-mbp:service-provider:9999` ），一个应用可以有多个实例。

### 服务续约（Renew）

服务续约会由服务提供者（比如 `Demo` 中的 `service-provider` ）定期调用，类似于心跳，用来告知注册中心 `Eureka Server` 自己的状态，避免被 `Eureka Server` 认为服务时效将其剔除下线。服务续约就是发送一个 `PUT` 请求带上当前实例信息到类 `InstanceResource` 的 `renewLease` 方法进行服务续约操作。

进入到 `PeerAwareInstanceRegistryImpl` 的 `renew` 方法可以看到，服务续约步骤大体上和服务注册一致，先更新当前 `Eureka Server` 节点的状态，服务续约成功后再用异步的方式同步状态到其它 `Eureka Server` 节上。

### 服务下线（Cancel）

当服务提供者（比如 `Demo` 中的 `service-provider` ）停止服务时，会发送请求告知注册中心 `Eureka Server` 进行服务剔除下线操作，防止服务消费者从注册中心调用到不存在的服务。服务下线就是发送一个 `DELETE` 请求带上当前实例信息到类 `InstanceResource` 的 `cancelLease` 方法进行服务剔除下线操作。

进入到 `PeerAwareInstanceRegistryImpl` 的 `cancel` 方法可以看到，服务续约步骤大体上和服务注册一致，先在当前 `Eureka Server` 节点剔除下线该服务，服务下线成功后再用异步的方式同步状态到其它 `Eureka Server` 节上。

### 服务剔除（Eviction）

服务剔除是注册中心 `Eureka Server` 在启动时就启动一个守护线程 `evictionTimer` 来定期（默认为 `60` 秒）执行检测服务的，判断标准就是超过一定时间没有进行 `Renew` 的服务，默认的失效时间是 `90` 秒，也就是说当一个已注册的服务在 `90` 秒内没有向注册中心 `Eureka Server` 进行服务续约（Renew），就会被从注册中心剔除下线。失效时间可以通过配置 `eureka.instance.leaseExpirationDurationInSeconds` 进行修改，定期执行检测服务可以通过配置 `eureka.server.evictionIntervalTimerInMs` 进行修改。

## 4.服务提供者（Service Provider）

对于服务提供方（比如 `Demo` 中的 `service-provider` 服务）来说，主要有三大类操作，分别为 `服务注册（Register）` 、 `服务续约（Renew）` 、 `服务下线（Cancel）` ，接下来看看这三个操作是如何实现的。

### 服务注册（Register）

一个服务要对外提供服务，首先要在注册中心 `Eureka Server` 进行服务相关信息注册，能进行这一步的前提是你要配置 `eureka.client.register-with-eureka=true` ，这个默认值为 `true` ，注册中心不需要把自己注册到注册中心去，把这个配置设为 `false`。

### 服务续约（Renew）

服务续约是由服务提供者方定期（默认为 `30` 秒）发起心跳的，主要是用来告知注册中心 `Eureka Server` 自己状态是正常的还活着，可以通过配置 `eureka.instance.lease-renewal-interval-in-seconds` 来修改，当然服务续约的前提是要配置 `eureka.client.register-with-eureka=true` ，将该服务注册到注册中心中去。

### 服务下线（Cancel）

当服务提供者方服务停止时，要发送 `DELETE` 请求告知注册中心 `Eureka Server` 自己已经下线，好让注册中心将自己剔除下线，防止服务消费方从注册中心获取到不可用的服务。这个过程实现比较简单，在类 `DiscoveryClient` 的 `shutdown` 方法加上注解 `@PreDestroy` ，当服务停止时会自动触发服务剔除下线，执行服务下线逻辑。

## 5.服务消费者（Service Consumer）

这里的服务消费者如果不需要被其它服务调用的话，其实只会涉及到两个操作，分别是从注册中心 `获取服务列表（Fetch）` 和 `更新服务列表（Update）` 。如果同时也需要注册到注册中心对外提供服务的话，那么剩下的过程和上文提到的服务提供者是一致的，这里不再阐述，接下来看看这两个操作是如何实现的。

### 获取服务列表（Fetch）

服务消费者方启动之后首先肯定是要先从注册中心 `Eureka Server` 获取到可用的服务列表同时本地也会缓存一份。这个获取服务列表的操作是在服务启动后 `DiscoverClient` 类实例化的时候执行的。

能发生这个获取服务列表的操作前提是要保证配置了 `eureka.client.fetch-registry=true` ，该配置的默认值为 `true`。

### 更新服务列表（Update）

由上面的 `获取服务列表（Fetch）` 操作过程可知，本地也会缓存一份，所以这里需要定期的去到注册中心 `Eureka Server` 获取服务的最新配置，然后比较更新本地缓存，这个更新的间隔时间可以通过配置 `eureka.client.registry-fetch-interval-seconds` 修改，默认为 `30` 秒，能进行这一步更新服务列表的前提是你要配置 `eureka.client.register-with-eureka=true` ，这个默认值为 `true` 。

## 6.**注册表存储结构**

![](D:\workspace\Java-Interview-Offer\images\Eureka001.webp)

- 这个名字叫做**registry**的**CocurrentHashMap**，就是注册表的核心结构。
- Eureka Server的注册表直接基于**纯内存**，即在内存里维护了一个数据结构。各个服务的注册、服务下线、服务故障，全部会在内存里维护和更新这个注册表。
- 各个服务每隔30秒拉取注册表的时候，Eureka Server就是直接提供内存里存储的有变化的注册表数据给他们就可以了。同样，每隔30秒发起心跳时，也是在这个纯内存的Map数据结构里更新心跳时间。
- 这个ConcurrentHashMap的key就是服务名称,value则代表了一个服务的多个服务实例。
- **Map<String, Lease<InstanceInfo**>>,这个Map的key就是**服务实例的id**,value是一个叫做**Lease**的类，它的泛型是一个叫做**InstanceInfo**的东西,这个InstanceInfo就代表了**服务实例的具体信息**，比如机器的ip地址、hostname以及端口号。而这个Lease，里面则会维护每个服务**最近一次发送心跳的时间**.

## 7.**Eureka Server端优秀的多级缓存机制**

- 在拉取注册表的时候：

  - 首先从**ReadOnlyCacheMap**里查缓存的注册表。

  - 若没有，就找**ReadWriteCacheMap**里缓存的注册表。

  - 如果还没有，就从**内存中获取实际的注册表数据。**

- 在注册表发生变更的时候：

  - 会在内存中更新变更的注册表数据，同时**过期掉ReadWriteCacheMap**。

  - 此过程不会影响ReadOnlyCacheMap提供人家查询注册表。

  - 一段时间内（默认30秒），各服务拉取注册表会直接读ReadOnlyCacheMap

  - 30秒过后，Eureka Server的后台线程发现ReadWriteCacheMap已经清空了，也会清空ReadOnlyCacheMap中的缓存

  - 下次有服务拉取注册表，又会从内存中获取最新的数据了，同时填充各个缓存。

### **多级缓存机制的优点是什么？**

- 尽可能保证了内存注册表数据不会出现频繁的读写冲突问题。

- 并且进一步保证对Eureka Server的大量请求，都是快速从纯内存走，性能极高。



