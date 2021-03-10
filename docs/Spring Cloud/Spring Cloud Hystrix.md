## 1.Spring Cloud Hystrix 

Netflix Hystrix为SOA( Service Oriented Architecture，面向服务的架构）和微服务架构提供一整套服务隔离、服务熔断和服务降级的解决方案。它是熔断器的一种实现，主要应用于微服务架构的高可用，防止出现服务雪崩等问题。

微服务架构将传统的单体服务根据业务功能和模块的不同，分解为多个独立的子服务，每个子服务都独立开发、部署和发布，子服务之间通过RPC接口实现服务间的接口调用。每个子服务都可以根据自己的需要进行独立的技术选型，服务之间相互独立，实现敏捷开发和部署。

由于业务之间往往具有复杂的依赖和调用关系，因此，微服务中的各个子服务之间的依赖关系也较为复杂。比如上游某个子服务因网络故障或者操作系统资源不足出现接口调用异常，则将导致下游服务也出现服务异常；此时，如果上游服务没有很好的请求拒绝策略，则会导致请求不断增加，大量的请求积压不但会导致当前服务宕机，还可能导致下游服务宕机，继而引起雪崩效应。

为了避免雪崩效应，提高关键业务的可靠性，可使用熔断器对部分非核心、低服务级别的业务进行服务降级。Netflix Hystrix实现了服务熔断器的功能，具体的做法是通过监控远程接口调用的状态，统计分析远程接口调用的数据，一旦发现某个服务出现宕机或故障过多的情况，则自动进行服务降级，不再调用远程接口服务，而是直接返回错误状态，避免集群雪崩效应，这样既有效保障了集群的安全性，也为恢复服务争取了时间。

## 2.Hystrix的特性

### 服务熔断

Hystrix熔断器就像家中的安全阀一样，一旦某个服务不可用，熔断器会直接切断该链路上的请求，避免大量的元效请求影响系统稳定，并且熔断器有自我检测和恢复的功能，在服务状态恢复正常后会自动关闭。

### 服务降级

Hystrix通过fallback实现服务降级。在需要进行服务降级的类中定义一个fallback 方法，当请求的远程服务出现异常时，可以直接使用fallback方法返回异常信息，而不调用远程服务。fallback方法的返回值一般是系统默认的错误消息或者来向缓存中的数据，用以告知服务消费者当前服务处于不可用状态。Hystrix通过HystrixCommand实现服务降级，熔断器有闭路、开路和半开路3种状态。

( 1）当调用远程服务请求的失败数量超过一定比例（默认为50%)时，熔断器会切换到开路状态，这时所有请求都会直接返回失败信息而不调用远程服务。

( 2 ）熔断器保持开路状态一段时间后（默认为5s），会自动切换到半开路状态。

( 3 ）熔断器判断下一次请求的返回情况，如果请求成功，则熔断器切换回闭路状态，服务进入正常链路调用流程；否则重新切换到开路状态，并保持开路状态。

### 依赖隔离

Hystrix通过线程池和信号量两种方式实现服务之间的依赖隔离，这样即使其中一个服务出现异常，资源迟迟不能释放，也不会影响其他业务线程的正常运行。

( 1 ）线程池的隔离策略

Hystrix线程池的资源隔离为每个依赖的服务都分配一个线程池，每个线程池都处理特定的服务，多个服务之间的线程资源互不影向，以达到资源隔离的目标。当突然发生流量洪峰、请求增多时，来不及处理的任务将在线程队列中排队等候，这样做的好处是不会丢弃客户端请求，保障所有数据最终都会得到处理。

( 2）信号量的隔离策略

Hystrix信号量的隔离策略是为每个依赖的服务都分配一个信号量（原子数数器），当接收到用户请求时，先判断该请求依赖的服务所在的信号量值是否超过最大线程设置。若超过最大线程设置，则丢弃该类型的请求；若不超过，则在处理请求前执行"信号量+1"的操作，在请求返回后执行“信号量-1”的操作。当流量洪峰来临，收到的请求数量超过设置的最大值时，这种方式会直接将错误状态返回给客户端，不继续去请求依赖的服务。

### 请求缓存

Hytrix按照请求参数把请求结果缓存起来，当后面有相同的请求时不会再走完整的调用链流程，而是把上次缓存的结果直接返回，以达到服务快速响应和性能优化的目的；同时，缓存可作为服务降级的数据源，当远程服务不可用时，直接返回缓存数据。对于消费者来说，只是可能获取了过期的数据，这样就优雅地处理了系统异常。

### 请求合并

当微服务需要调用多个远程服务做结果的汇总时，需要使用请求合并。Hystrix采用异步消息订阅的方式进行请求合并。当应用程序需要请求多个接口时，采用异步调用的方式提交请求，然后订阅返回值，这时应用程序的业务可以接着执行其他任务而不用阻塞等待，当所有请求都返回时，应用程序会得到一个通知，取出返回值合并即可。

## 3.Hystrix的服务降级流程

Hystrix的服务熔断是依赖HystrixCommand指令来实现的，具体流程如下：

( 1 ）当有服务请求时，首先会根据注解创建一个HystrixCommand指令对象，该对象设置了服务调用失败的场景（如服务请求超时等）和调用失败后服务降级的业务逻辑方法。

( 2 ）熔断器判断状态，当熔断器处于开路状态时，直接调用服务降级的业务逻辑方法返回调用失败的反馈信息。

( 3 ）当熔断器处于半开路或者闭路状态时，服务会进行线程池和信号量等资源检查，如果有可用资源，则调用正常业务逻辑。如果调用正常业务逻辑成功，则返回成功后的消息；如果失败，则调用服务降级的业务逻辑，进行服务降级。

( 4 ）当熔断器处于半开路或者闭路状态时，如果当前服务线程池和信号量中没有可用资源，则执行服务降级的业务逻辑，返回失败信息。

( 5 ）当熔断器处于半开路状态并且本次服务执行失败时，熔断器会进入开路状态。

( 6）当正常业务逻辑处理超时或者出现错误时，HystrixCommand会执行服务降级的业务逻辑，返回失败信息.

( 7 ）线程池和信号量的资源检查及正常业务逻辑会将自己的状态和调用结果反馈给监控，监控将服务状态反馈给熔断器，以便熔断器判断熔断状态

![](D:\workspace\Java-Interview-Offer\images\springcloudHystrix001.png)

## 4.Hystrix的使用

Hystrix的使用主要分为服务熔断、服务降级和服务监控3个方面

( 1 ) pom.xml添加依赖

在pom.xml中加入Hystrix依赖，其中，spring-cloud-starter-netflix-hystrix和javanica为Hystrix服务熔断所需的依赖，spring-cloud-netflix-hystrix-dash board为Hystrix服务监控所需的依赖。

( 2 ）通过@EnableHystrix注解开启对服务熔断的支持，通过@EnableHysrixDashboard注解开启对服务监控的支持。注意，Hystrix一般和服务发现配合使用，这里使用@EnableEurekaClient开启了对服务发现客户端的支持。

( 3 ) application. properties配置

配置服务名称、端口和需要连接的服务注册中心的地址，这里注册中使用Eureka服务

( 4)服务熔断和降级

通过@HystrixCommand(fallbackMethod=”exceptionHandler＂）在方法上定义了一个服务降级的命令。当远程方法调用失败时，Hystrix会向动调用fallbackMethod来完成服务熔断和降级，这里会调用exceptionHandler（）方法。

( 5 ）服务验证

当关闭EUREKA-CLIENT远程服务时，远程服务将不可用，Hystrix的熔断器打开，程序直接调用fallbackMethod方法实现服务降级。

## 5.异步请求

Hystrix为非阻塞I/O提供了两种实现方式，分别是表示将来式的Future和表示回调式的Callable。

### Future 

当用Future去请求一个网络IO的任务时，Future会以多线程的形式异步实现服务调用，主线程不必阻塞等待，在结果返回后再通过Future的get（）方法获取Future的返回结果。具体实现如下：

( 1 ）定义HystrixCommand

通过继承HystrixCommand定义一个CommandFuture来实现异步请求，其中，正常业务执行的逻辑在覆写的run（）方体体中被执行，服务降级方法在getFallback()中被执行。需要注意的是，这里使用andCommandKey(HystrixCommandKey.Factory.asKey(”Hello World＂））实现了使用HystrixCommandKey工厂定义依赖名称，每个CommandKey都代表一个依赖抽象，相同的依赖要使用相同的CommandKey名称。依赖隔离的本质就实对相同CommandKey的依赖进行隔离。使用andThreadPoolKey(HystrixThreadPoolKey.Factory.asKey（”HelloWorldPool”））实现了基于HystrixThreadPoolKey工厂定义线程池名称。

当对同一业务的依赖进行资源隔离时，使用CommandGroup进行区分，但是当对同一依赖进行不同远程调用时（例如。一个是Redis服务，一个是HTTP服务），则可以使用HystrixThreadPoolKey进行隔离区分，虽然在业务上都是相同的组，但是当需要在源上进行隔离时，则可以使用HystrixThreadPoolKey进行区分。

(2 ）使用HystrixCommand

通过new CommandFuture（”future”，restTemplate).queue（）定义一个异步服务请求，请求在提交后会以队列的形式在线程池中等待被执行，在执行完成后通过get（）方法获取即可。

### Callable

预定义一个回调任务，Callable在发出请求后，主线程继续执行，在请求被执行完成并返回结果后，Callable会自动调用回调任务。具体使用如下。

( 1 ）定义HystrixObservableCommand

定义名为CommandObservable的类，该类继承自HystrixObservableCommand接口，并通过覆写HystrixObservableCommand接口中的construct（）方法实现观察者模。具体实现为通过Observable.create（）创建并返回一个Observable<String＞对象，在创建对象时，通过new Observable.OnSubscribe<String>（）方法实现消息的监听和处理。其中，call方法用于消息的接收和业务的处理，在消息处理完成后通过subscriber.onNext( result）将调用结果传递下去，当所有任务都执行完成时通过subscriber.onCompleted（）将总体执行结果发布出去。

resumeWithFallback方法是服务降级处理逻辑，当服务出现异常时，通过subscriber.onN ext（”hystrix：远程服务异常”）进行服务熔断和异常消息的发布，实现服务降级处理。

( 2 ）使用HystrixObservabeCommand

通过new CommandObservable(”observer" ,restTemplate).observe（）定义了一个实现服务发布的命令。通过调用observable.subscribe（）方法来实现基于观察者模式的请求结果订阅，其中，订阅的数据结果在onNext（）方法中被通知，总体调用结果在onCompleted（）中被通知，服务处理异常结果在onError（）中被通知。

## 6.Hystrix的常用配置

### 熔断的配置参数

( 1 ) circuitBreakerEnabled：是否允许熔断（默认为true）。 

( 2 ) circuitBreakerRequestVolumeThreshold：最小熔断请求数（默认为20）。当一个统计窗口内处理的请求数达到该阈值时，Hystrix会触发是否需要熔断的判断。

( 3) circuitBreakerErrorThresholdPercentage：熔断的阈值百分比（默认为50）。当一个统计窗口内有50%的请求处理失败时，Hystrix会触发熔断。

( 4) circuitBreakerForceOpen：是否强制开启熔断器（默认为false）。如果为true，则会拒绝所有请求。

( 5 ) circuitBreakerForceClosed：强制熔断器进入closed状态（默认为false）。如果设置为true，则会忽略错误百分比（circuitBreakerErrorThresholdPercentage），优先级比强制开启熔断器（circuitBreakerForceOpen）低。也就是说，当circuitBreakerForceOpen设置为true时，circuitBreakerForceClosed这个设置是无效的。

( 6) circuitBreakerSleepWindowInMilliseconds：熔断时间（默认为5s）。当满足熔断条件时，熔断器在中断请求5s后会自动进入半开路状态，允许部分流量进行重试。

### 执行的配置参数

( I ) executionIsolationSemaphoreMaxConcurrentRequests ：当隔离策略为ExecutionIsolationStrategy.SEMAPHORE时，允许进入HystrixCommand.run（）方法的最大并发请求数量。

( 2) executionlsolationStrategy：隔离策略。HystrixCommand的默认值为THREAD,HystrixObservableCommand的默认值为SEMAPHORE。

( 3) executionlsolationThreadInterruptOnTimeout：在隔离执行超时后是否中断（默认true)。

( 4) executionTimeoutlnMilliseconds：执行超时时间（单位为ms）。当任务执行超过指定时间时，执行fallback函数进行熔断。

( 5) executionTimeoutEnabled：是否允许超时执行。

( 6) executionlsolationThreadPoolKeyOverride：指定任务执行的线程池。Hystrix通过线程池名称来判断使用哪一个线程池来执行，如果没有对应的线程池，则会为其创建一个线程池。

( 7 ) fallbacklsolationSemaphoreMaxConcurrentRequests:HystrixCommand.getFallback() 执行的最大并发数（默认为10），如果超过该并发数，则直接抛出异常，不执行fallback 函数。

( 8) fallbackEnabled：是否允许fallback。

( 9) metricsRollingStatisticalWindowlnMilliseconds：熔断统计时间窗口（默认为10s), 也就是以10s为单位统计信息.

( 10 ) metricsRollingStatisticalWindowBuckets：一个熔断统计窗口内的Bucket数量（默认为10）。

( 11 ) metricsRollingPercentileEnabled：是否开启执行延迟统计（默认为true），如果为true，则服务的执行延迟会被追踪计算；如果为false，则所有统计均值百分比都会返回-1.

( 12) metricsRollingPercentileWindowlnMilliseconds：执行时间统计窗口（默认为60s）。

( 13) metricsRollingPercentileWindowBuckets：执行时间统计窗口内的Bucket数量（默认为6）。

( 14) metricsRolIingPercentileBucketSize：执行时间内保留Bucket的最大值。

( 15 ) metricsHealthSnapshotlntervalInMilliseconds：计算成功失败百分比的时间间隔（默认为500ms).

( 16 ) requestCacheEnabled：是否开启请求缓存（默认为true）。

( 17 ) requestLogEnabled：是否开启请求日志（默认为true）。

( 18 ) maxRequestslnBatch：批量执行命令的最大值（默认为Integer.MAX_VALUE）。

( 19) timerDelaylnMilliseconds：执行命令延迟时间（默认为10ms）。

( 20) requestCacheEnabled：是否开启请求缓存（默认为true）。

## 7.Hystix Dashboard

Hystrix Dashboard主要用来实时监控Hystrix的各项运行指标。通过Hystrix Dashboard可以查询Hystrix的实时信息，用于快速定位和发现问题。Hystrix Dashboard 的使用简单方便，首先在pom.xml中引人spring-cloud-netflix-hystrix-dashboard服务依赖，然后使用@EnableHystrixDashboard注解开启Dashboard功能即可。

## 8.设计原则

- 阻止任何一个依赖服务耗尽所有的资源，比如 tomcat 中的所有线程资源。
- 避免请求排队和积压，采用限流和 `fail fast` 来控制故障。
- 提供 fallback 降级机制来应对故障。
- 使用资源隔离技术，比如 `bulkhead`（舱壁隔离技术）、`swimlane`（泳道技术）、`circuit breaker`（断路技术）来限制任何一个依赖服务的故障的影响。
- 通过近实时的统计/监控/报警功能，来提高故障发现的速度。
- 通过近实时的属性和配置**热修改**功能，来提高故障处理和恢复的速度。
- 保护依赖服务调用的所有故障情况，而不仅仅只是网络故障情况。

## 9.基于 Hystrix 线程池技术实现资源隔离

Hystrix 实现资源隔离，主要有两种技术：

- 线程池
- 信号量

默认情况下，Hystrix 使用线程池模式。

资源隔离，就是说，你如果要把对某一个依赖服务的所有调用请求，全部隔离在同一份资源池内，不会去用其它资源了，这就叫资源隔离。哪怕对这个依赖服务，比如说商品服务，现在同时发起的调用量已经到了 1000，但是分配给商品服务线程池内就 10 个线程，最多就只会用这 10 个线程去执行。不会因为对商品服务调用的延迟，将 Tomcat 内部所有的线程资源全部耗尽。

Hystrix 进行资源隔离，其实是提供了一个抽象，叫做 Command。这也是 Hystrix 最最基本的资源隔离技术。

### 利用 HystrixCommand 获取单条数据

我们通过将调用商品服务的操作封装在 HystrixCommand 中，限定一个 key，比如下面的 `GetProductInfoCommandGroup`，在这里我们可以简单认为这是一个线程池，每次调用商品服务，就只会用该线程池中的资源，不会再去用其它线程资源了。

```html
 public class GetProductInfoCommand extends HystrixCommand<ProductInfo> {
 
     private Long productId;
 
     public GetProductInfoCommand(Long productId) {
         super(HystrixCommandGroupKey.Factory.asKey("GetProductInfoCommandGroup"));
         this.productId = productId;
     }
 
     @Override
     protected ProductInfo run() {
         String url = "http://localhost:8081/getProductInfo?productId=" + productId;
         // 调用商品服务接口
         String response = HttpClientUtils.sendGetRequest(url);
         return JSONObject.parseObject(response, ProductInfo.class);
     }
 }
```

我们在缓存服务接口中，根据 productId 创建 Command 并执行，获取到商品数据。

```html
 @RequestMapping("/getProductInfo")
 @ResponseBody
 public String getProductInfo(Long productId) {
     HystrixCommand<ProductInfo> getProductInfoCommand = new GetProductInfoCommand(productId);
 
     // 通过command执行，获取最新商品数据
     ProductInfo productInfo = getProductInfoCommand.execute();
     System.out.println(productInfo);
     return "success";
 }
```

上面执行的是 execute() 方法，其实是同步的。也可以对 command 调用 queue() 方法，它仅仅是将 command 放入线程池的一个等待队列，就立即返回，拿到一个 Future 对象，后面可以继续做其它一些事情，然后过一段时间对 Future 调用 get() 方法获取数据。这是异步的。

### 利用 HystrixObservableCommand 批量获取数据

只要是获取商品数据，全部都绑定到同一个线程池里面去，我们通过 HystrixObservableCommand 的一个线程去执行，而在这个线程里面，批量把多个 productId 的 productInfo 拉回来。

```html
 public class GetProductInfosCommand extends HystrixObservableCommand<ProductInfo> {
 
     private String[] productIds;
 
     public GetProductInfosCommand(String[] productIds) {
         // 还是绑定在同一个线程池
         super(HystrixCommandGroupKey.Factory.asKey("GetProductInfoGroup"));
         this.productIds = productIds;
     }
 
     @Override
     protected Observable<ProductInfo> construct() {
         return Observable.unsafeCreate((Observable.OnSubscribe<ProductInfo>) subscriber -> {
 
             for (String productId : productIds) {
                 // 批量获取商品数据
                 String url = "http://localhost:8081/getProductInfo?productId=" + productId;
                 String response = HttpClientUtils.sendGetRequest(url);
                 ProductInfo productInfo = JSONObject.parseObject(response, ProductInfo.class);
                 subscriber.onNext(productInfo);
             }
             subscriber.onCompleted();
 
         }).subscribeOn(Schedulers.io());
     }
 }
```

在缓存服务接口中，根据传来的 id 列表，比如是以 `,` 分隔的 id 串，通过上面的 HystrixObservableCommand，执行 Hystrix 的一些 API 方法，获取到所有商品数据。

```html
 public String getProductInfos(String productIds) {
     String[] productIdArray = productIds.split(",");
     HystrixObservableCommand<ProductInfo> getProductInfosCommand = new GetProductInfosCommand(productIdArray);
     Observable<ProductInfo> observable = getProductInfosCommand.observe();
 
     observable.subscribe(new Observer<ProductInfo>() {
         @Override
         public void onCompleted() {
             System.out.println("获取完了所有的商品数据");
         }
 
         @Override
         public void onError(Throwable e) {
             e.printStackTrace();
         }
 
         /**
          * 获取完一条数据，就回调一次这个方法
          * @param productInfo
          */
         @Override
         public void onNext(ProductInfo productInfo) {
             System.out.println(productInfo);
         }
     });
     return "success";
 }
```

## 10.基于 Hystrix 信号量机制实现资源隔离

### 信号量机制

信号量的资源隔离只是起到一个开关的作用，比如，服务 A 的信号量大小为 10，那么就是说它同时只允许有 10 个 tomcat 线程来访问服务 A，其它的请求都会被拒绝，从而达到资源隔离和限流保护的作用。

### 线程池与信号量区别

线程池隔离技术，并不是说去控制类似 tomcat 这种 web 容器的线程。更加严格的意义上来说，Hystrix 的线程池隔离技术，控制的是 tomcat 线程的执行。Hystrix 线程池满后，会确保说，tomcat 的线程不会因为依赖服务的接口调用延迟或故障而被 hang 住，tomcat 其它的线程不会卡死，可以快速返回，然后支撑其它的事情。

线程池隔离技术，是用 Hystrix 自己的线程去执行调用；而信号量隔离技术，是直接让 tomcat 线程去调用依赖服务。信号量隔离，只是一道关卡，信号量有多少，就允许多少个 tomcat 线程通过它，然后去执行。

**适用场景**：

- **线程池技术**，适合绝大多数场景，比如说我们对依赖服务的网络请求的调用和访问、需要对调用的 timeout 进行控制（捕捉 timeout 超时异常）。
- **信号量技术**，适合说你的访问不是对外部依赖的访问，而是对内部的一些比较复杂的业务逻辑的访问，并且系统内部的代码，其实不涉及任何的网络请求，那么只要做信号量的普通限流就可以了，因为不需要去捕获 timeout 类似的问题。

## 11.Hystrix 隔离策略细粒度控制

### execution.isolation.strategy

指定了 HystrixCommand.run() 的资源隔离策略：`THREAD` or `SEMAPHORE`，一种基于线程池，一种基于信号量。

```html
 // to use thread isolation
 HystrixCommandProperties.Setter().withExecutionIsolationStrategy(ExecutionIsolationStrategy.THREAD)
 
 // to use semaphore isolation
 HystrixCommandProperties.Setter().withExecutionIsolationStrategy(ExecutionIsolationStrategy.SEMAPHORE)
```

线程池机制，每个 command 运行在一个线程中，限流是通过线程池的大小来控制的；信号量机制，command 是运行在调用线程中（也就是 Tomcat 的线程池），通过信号量的容量来进行限流。

如何在线程池和信号量之间做选择？

**默认的策略**就是线程池。

**线程池**其实最大的好处就是对于网络访问请求，如果有超时的话，可以避免调用线程阻塞住。

而使用信号量的场景，通常是针对超大并发量的场景下，每个服务实例每秒都几百的 `QPS`，那么此时你用线程池的话，线程一般不会太多，可能撑不住那么高的并发，如果要撑住，可能要耗费大量的线程资源，那么就是用信号量，来进行限流保护。一般用信号量常见于那种基于纯内存的一些业务逻辑服务，而不涉及到任何网络访问请求。

### command key & command group

每一个 command，都可以设置一个自己的名称 command key，同时可以设置一个自己的组 command group。

command group 是一个非常重要的概念，默认情况下，就是通过 command group 来定义一个线程池的，而且还会通过 command group 来聚合一些监控和报警信息。同一个 command group 中的请求，都会进入同一个线程池中。

### command thread pool

ThreadPoolKey 代表了一个 HystrixThreadPool，用来进行统一监控、统计、缓存。默认的 ThreadPoolKey 就是 command group 的名称。每个 command 都会跟它的 ThreadPoolKey 对应的 ThreadPool 绑定在一起。

如果不想直接用 command group，也可以手动设置 ThreadPool 的名称。

### command key & command group & command thread pool

**command key** ，代表了一类 command，一般来说，代表了下游依赖服务的某个接口。

**command group** ，代表了某一个下游依赖服务，这是很合理的，一个依赖服务可能会暴露出来多个接口，每个接口就是一个 command key。command group 在逻辑上对一堆 command key 的调用次数、成功次数、timeout 次数、失败次数等进行统计，可以看到某一个服务整体的一些访问情况。**一般来说，推荐根据一个服务区划分出一个线程池，command key 默认都是属于同一个线程池的。**

比如说有一个服务 A，你估算出来服务 A 每秒所有接口加起来的整体 `QPS` 在 100 左右，你有一个服务 B 去调用服务 A。你的服务 B 部署了 10 个实例，每个实例上，用 command group 去对应下游服务 A。给一个线程池，量大概是 10 就可以了，这样服务 B 对服务 A 整体的访问 QPS 就大概是每秒 100 了。

但是，如果说 command group 对应了一个服务，而这个服务暴露出来的几个接口，访问量很不一样，差异非常之大。你可能就希望在这个服务对应 command group 的内部，包含对应多个接口的 command key，做一些细粒度的资源隔离。**就是说，希望对同一个服务的不同接口，使用不同的线程池。**

```html
 command key -> command group
 
 command key -> 自己的 thread pool key
```

逻辑上来说，多个 command key 属于一个 command group，在做统计的时候，会放在一起统计。每个 command key 有自己的线程池，每个接口有自己的线程池，去做资源隔离和限流。

说白点，就是说如果你的 command key 要用自己的线程池，可以定义自己的 thread pool key，就 ok 了。

### coreSize

设置线程池的大小，默认是 10。一般来说，用这个默认的 10 个线程大小就够了。

```html
 HystrixThreadPoolProperties.Setter().withCoreSize(int value);
```

### queueSizeRejectionThreshold

如果说线程池中的 10 个线程都在工作中，没有空闲的线程来做其它的事情，此时再有请求过来，会先进入队列积压。如果说队列积压满了，再有请求过来，就直接 reject，拒绝请求，执行 fallback 降级的逻辑，快速返回。

控制 queue 满了之后 reject 的 threshold，因为 maxQueueSize 不允许热修改，因此提供这个参数可以热修改，控制队列的最大大小。

```html
 HystrixThreadPoolProperties.Setter().withQueueSizeRejectionThreshold(int value);
```

### execution.isolation.semaphore.maxConcurrentRequests

设置使用 SEMAPHORE 隔离策略的时候允许访问的最大并发量，超过这个最大并发量，请求直接被 reject。

这个并发量的设置，跟线程池大小的设置，应该是类似的，但是基于信号量的话，性能会好很多，而且 Hystrix 框架本身的开销会小很多。

默认值是 10，尽量设置的小一些，因为一旦设置的太大，而且有延时发生，可能瞬间导致 tomcat 本身的线程资源被占满。

```html
HystrixCommandProperties.Setter().withExecutionIsolationSemaphoreMaxConcurrentRequests(int value);
```

## 12.Hystrix 执行时内部原理

Hystrix 最基本的支持高可用的技术：**资源隔离** + **限流**。

- 创建 command；
- 执行这个 command；
- 配置这个 command 对应的 group 和线程池。

### 步骤一：创建 command

一个 HystrixCommand 或 HystrixObservableCommand 对象，代表了对某个依赖服务发起的一次请求或者调用。创建的时候，可以在构造函数中传入任何需要的参数。

- HystrixCommand 主要用于仅仅会返回一个结果的调用。
- HystrixObservableCommand 主要用于可能会返回多条结果的调用。

```html
// 创建 HystrixCommand
HystrixCommand hystrixCommand = new HystrixCommand(arg1, arg2);

// 创建 HystrixObservableCommand
HystrixObservableCommand hystrixObservableCommand = new HystrixObservableCommand(arg1, arg2);
```

### 步骤二：调用 command 执行方法

执行 command，就可以发起一次对依赖服务的调用。

要执行 command，可以在 4 个方法中选择其中的一个：execute()、queue()、observe()、toObservable()。

其中 execute() 和 queue() 方法仅仅对 HystrixCommand 适用。

- execute()：调用后直接 block 住，属于同步调用，直到依赖服务返回单条结果，或者抛出异常。
- queue()：返回一个 Future，属于异步调用，后面可以通过 Future 获取单条结果。
- observe()：订阅一个 Observable 对象，Observable 代表的是依赖服务返回的结果，获取到一个那个代表结果的 Observable 对象的拷贝对象。
- toObservable()：返回一个 Observable 对象，如果我们订阅这个对象，就会执行 command 并且获取返回结果。

```html
K             value    = hystrixCommand.execute();
Future<K>     fValue   = hystrixCommand.queue();
Observable<K> oValue   = hystrixObservableCommand.observe();
Observable<K> toOValue = hystrixObservableCommand.toObservable();
```

execute() 实际上会调用 queue().get() 方法，而在 queue() 方法中，会调用 toObservable().toBlocking().toFuture()。

也就是说，先通过 toObservable() 获得 Future 对象，然后调用 Future 的 get() 方法。那么，其实无论是哪种方式执行 command，最终都是依赖于 toObservable() 去执行的。

### 步骤三：检查是否开启缓存（不太常用）

从这一步开始，就进入到 Hystrix 底层运行原理啦，看一下 Hystrix 一些更高级的功能和特性。

如果这个 command 开启了请求缓存 Request Cache，而且这个调用的结果在缓存中存在，那么直接从缓存中返回结果。否则，继续往后的步骤。

### 步骤四：检查是否开启了断路器

检查这个 command 对应的依赖服务是否开启了断路器。如果断路器被打开了，那么 Hystrix 就不会执行这个 command，而是直接去执行 fallback 降级机制，返回降级结果。

### 步骤五：检查线程池/队列/信号量是否已满

如果这个 command 线程池和队列已满，或者 semaphore 信号量已满，那么也不会执行 command，而是直接去调用 fallback 降级机制，同时发送 reject 信息给断路器统计。

### 步骤六：执行 command

调用 HystrixObservableCommand 对象的 construct() 方法，或者 HystrixCommand 的 run() 方法来实际执行这个 command。

- HystrixCommand.run() 返回单条结果，或者抛出异常。
- HystrixObservableCommand.construct() 返回一个 Observable 对象，可以获取多条结果。

如果是采用线程池方式，并且 HystrixCommand.run() 或者 HystrixObservableCommand.construct() 的执行时间超过了 timeout 时长的话，那么 command 所在的线程会抛出一个 TimeoutException，这时会执行 fallback 降级机制，不会去管 run() 或 construct() 返回的值了。另一种情况，如果 command 执行出错抛出了其它异常，那么也会走 fallback 降级。这两种情况下，Hystrix 都会发送异常事件给断路器统计。

**注意**，我们是不可能终止掉一个调用严重延迟的依赖服务的线程的，只能说给你抛出来一个 TimeoutException。

如果没有 timeout，也正常执行的话，那么调用线程就会拿到一些调用依赖服务获取到的结果，然后 Hystrix 也会做一些 logging 记录和 metric 度量统计。

### 步骤七：断路健康检查

Hystrix 会把每一个依赖服务的调用成功、失败、Reject、Timeout 等事件发送给 circuit breaker 断路器。断路器就会对这些事件的次数进行统计，根据异常事件发生的比例来决定是否要进行断路（熔断）。如果打开了断路器，那么在接下来一段时间内，会直接断路，返回降级结果。

如果在之后，断路器尝试执行 command，调用没有出错，返回了正常结果，那么 Hystrix 就会把断路器关闭。

### 步骤八：调用 fallback 降级机制

在以下几种情况中，Hystrix 会调用 fallback 降级机制。

- 断路器处于打开状态；
- 线程池/队列/semaphore 满了；
- command 执行超时；
- run() 或者 construct() 抛出异常。

一般在降级机制中，都建议给出一些默认的返回值，比如静态的一些代码逻辑，或者从内存中的缓存中提取一些数据，在这里尽量不要再进行网络请求了。

在降级中，如果一定要进行网络调用的话，也应该将那个调用放在一个 HystrixCommand 中进行隔离。

- HystrixCommand 中，实现 getFallback() 方法，可以提供降级机制。
- HystrixObservableCommand 中，实现 resumeWithFallback() 方法，返回一个 Observable 对象，可以提供降级结果。

如果没有实现 fallback，或者 fallback 抛出了异常，Hystrix 会返回一个 Observable，但是不会返回任何数据。

不同的 command 执行方式，其 fallback 为空或者异常时的返回结果不同。

- 对于 execute()，直接抛出异常。
- 对于 queue()，返回一个 Future，调用 get() 时抛出异常。
- 对于 observe()，返回一个 Observable 对象，但是调用 subscribe() 方法订阅它时，立即抛出调用者的 onError() 方法。
- 对于 toObservable()，返回一个 Observable 对象，但是调用 subscribe() 方法订阅它时，立即抛出调用者的 onError() 方法。

### 不同的执行方式

- execute()，获取一个 Future.get()，然后拿到单个结果。
- queue()，返回一个 Future。
- observe()，立即订阅 Observable，然后启动 8 大执行步骤，返回一个拷贝的 Observable，订阅时立即回调给你结果。
- toObservable()，返回一个原始的 Observable，必须手动订阅才会去执行 8 大步骤。

## 13.基于本地缓存的 fallback 降级机制

Hystrix 出现以下四种情况，都会去调用 fallback 降级机制：

- 断路器处于打开的状态。
- 资源池已满（线程池+队列 / 信号量）。
- Hystrix 调用各种接口，或者访问外部依赖，比如 MySQL、Redis、Zookeeper、Kafka 等等，出现了任何异常的情况。
- 访问外部依赖的时候，访问时间过长，报了 TimeoutException 异常。

### 两种最经典的降级机制

- 纯内存数据 在降级逻辑中，你可以在内存中维护一个 ehcache，作为一个纯内存的基于 LRU 自动清理的缓存，让数据放在缓存内。如果说外部依赖有异常，fallback 这里直接尝试从 ehcache 中获取数据。
- 默认值 fallback 降级逻辑中，也可以直接返回一个默认值。

在 `HystrixCommand`，降级逻辑的书写，是通过实现 getFallback() 接口；而在 `HystrixObservableCommand` 中，则是实现 resumeWithFallback() 方法。

## 14.Hystrix 断路器执行原理

### 状态机

Hystrix 断路器有三种状态，分别是关闭（Closed）、打开（Open）与半开（Half-Open）。

1. `Closed` 断路器关闭：调用下游的请求正常通过
2. `Open` 断路器打开：阻断对下游服务的调用，直接走 Fallback 逻辑
3. `Half-Open` 断路器处于半开状态：SleepWindowInMilliseconds

### Enabled

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerEnabled(boolean)
```

控制断路器是否工作，包括跟踪依赖服务调用的健康状况，以及对异常情况过多时是否允许触发断路。默认值 `true`。

### circuitBreaker.requestVolumeThreshold

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerRequestVolumeThreshold(int)
```

表示在一次统计的**时间滑动窗口中（这个参数也很重要，下面有说到）**，至少经过多少个请求，才可能触发断路，默认值 20。**经过 Hystrix 断路器的流量只有在超过了一定阈值后，才有可能触发断路。**比如说，要求在 10s 内经过断路器的流量必须达到 20 个，而实际经过断路器的请求有 19 个，即使这 19 个请求全都失败，也不会去判断要不要断路。

### circuitBreaker.errorThresholdPercentage

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerErrorThresholdPercentage(int)
```

表示异常比例达到多少，才会触发断路，默认值是 50(%)。

### circuitBreaker.sleepWindowInMilliseconds

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerSleepWindowInMilliseconds(int)
```

断路器状态由 Close 转换到 Open，在之后 `SleepWindowInMilliseconds` 时间内，所有经过该断路器的请求会被断路，不调用后端服务，直接走 Fallback 降级机制，默认值 5000(ms)。

而在该参数时间过后，断路器会变为 `Half-Open` 半开闭状态，尝试让一条请求经过断路器，看能不能正常调用。如果调用成功了，那么就自动恢复，断路器转为 Close 状态。

### ForceOpen

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerForceOpen(boolean)
```

如果设置为 true 的话，直接强迫打开断路器，相当于是手动断路了，手动降级，默认值是 `false`。

### ForceClosed

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerForceClosed(boolean)
```

如果设置为 true，直接强迫关闭断路器，相当于手动停止断路了，手动升级，默认值是 `false`。

## 15.Hystrix 线程池隔离与接口限流

Hystrix 通过判断线程池或者信号量是否已满，超出容量的请求，直接 Reject 走降级，从而达到限流的作用。

限流是限制对后端的服务的访问量，比如说你对 MySQL、Redis、Zookeeper 以及其它各种后端中间件的资源的访问的限制，其实是为了避免过大的流量直接打死后端的服务。

### 线程池隔离技术的设计

Hystrix 采用了 Bulkhead Partition 舱壁隔离技术，来将外部依赖进行资源隔离，进而避免任何外部依赖的故障导致本服务崩溃。

**舱壁隔离**，是说将船体内部空间区隔划分成若干个隔舱，一旦某几个隔舱发生破损进水，水流不会在其间相互流动，如此一来船舶在受损时，依然能具有足够的浮力和稳定性，进而减低立即沉船的危险。

Hystrix 对每个外部依赖用一个单独的线程池，这样的话，如果对那个外部依赖调用延迟很严重，最多就是耗尽那个依赖自己的线程池而已，不会影响其他的依赖调用。

### 线程池机制的优点

- 任何一个依赖服务都可以被隔离在自己的线程池内，即使自己的线程池资源填满了，也不会影响任何其他的服务调用。
- 服务可以随时引入一个新的依赖服务，因为即使这个新的依赖服务有问题，也不会影响其他任何服务的调用。
- 当一个故障的依赖服务重新变好的时候，可以通过清理掉线程池，瞬间恢复该服务的调用，而如果是 tomcat 线程池被占满，再恢复就很麻烦。
- 如果一个 client 调用库配置有问题，线程池的健康状况随时会报告，比如成功/失败/拒绝/超时的次数统计，然后可以近实时热修改依赖服务的调用配置，而不用停机。
- 基于线程池的异步本质，可以在同步的调用之上，构建一层异步调用层。

简单来说，最大的好处，就是资源隔离，确保说任何一个依赖服务故障，不会拖垮当前的这个服务。

### 线程池机制的缺点

- 线程池机制最大的缺点就是增加了 CPU 的开销。 除了 tomcat 本身的调用线程之外，还有 Hystrix 自己管理的线程池。
- 每个 command 的执行都依托一个独立的线程，会进行排队，调度，还有上下文切换。

我们可以用 Hystrix semaphore 技术来实现对某个依赖服务的并发访问量的限制，而不是通过线程池/队列的大小来限制流量。

semaphore 技术可以用来限流和削峰，但是不能用来对调用延迟的服务进行 timeout 和隔离。

`execution.isolation.strategy` 设置为 `SEMAPHORE`，那么 Hystrix 就会用 semaphore 机制来替代线程池机制，来对依赖服务的访问进行限流。如果通过 semaphore 调用的时候，底层的网络调用延迟很严重，那么是无法 timeout 的，只能一直 block 住。一旦请求数量超过了 semaphore 限定的数量之后，就会立即开启限流。

## 16.基于 timeout 机制为服务接口调用超时提供安全保护

### TimeoutMilliseconds

在 Hystrix 中，我们可以手动设置 timeout 时长，如果一个 command 运行时间超过了设定的时长，那么就被认为是 timeout，然后 Hystrix command 标识为 timeout，同时执行 fallback 降级逻辑。

`TimeoutMilliseconds` 默认值是 1000，也就是 1000ms。

```html
HystrixCommandProperties.Setter()
    ..withExecutionTimeoutInMilliseconds(int)
```

### TimeoutEnabled

这个参数用于控制是否要打开 timeout 机制，默认值是 true。

```html
HystrixCommandProperties.Setter()
    .withExecutionTimeoutEnabled(boolean)
```

