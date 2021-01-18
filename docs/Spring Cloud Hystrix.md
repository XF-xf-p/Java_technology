## 1.Spring Cloud Hystrix 

Netflix Hystrix为SOA( Service Oriented Architecture，面向服务的架构）和微服务架构提供一整套服务隔离、服务熔断和服务降级的解决方案。它是熔断器的一种实现，主要应用于微服务架构的高可用，防止出现服务雪崩等问题。

微服务架构将传统的单体服务根据业务功能和模块的不同，分解为多个独立的子服务，每个子服务都独立开发、部署和发布，子服务之间通过RPC接口实现服务间的接口调用。每个子服务都可以根据自己的需要进行独立的技术选型，服务之间相互独立，实现敏捷开发和部署。

由于业务之间往往具有复杂的依赖和调用关系，因此，微服务中的各个子服务之间的依赖关系也较为复杂。比如上游某个子服务因网络故障或者操作系统资源不足出现接口调用异常，则将导致下游服务也出现服务异常；此时，如果上游服务没有很好的请求拒绝策略，则会导致请求不断增加，大量的请求积压不但会导致当前服务宕机，还可能导致下游服务宕机，继而引起雪崩效应。

为了避免雪崩效应，提高关键业务的可靠性，可使用熔断器对部分非核心、低服务级别的业务进行服务降级。Netflix Hystrix实现了服务熔断器的功能，具体的做法是通过监控远程接口调用的状态，统计分析远程接口调用的数据，一旦发现某个服务出现宕机或故障过多的情况，则自动进行服务降级，不再调用远程接口服务，而是直接返回错误状态，避免集群雪崩效应，这样既有效保障了集群的安全性，也为恢复服务争取了时间。

## 2.Hystrix的特性

### 服务熔断

Hystrix熔断器就像家中的安全阀一样，一旦某个服务不可用，熔断器会直接切断该链路上的请求，避免大量的元效请求影响系统稳定，并且熔断器有自我检测和恢复的功能，在服务状态恢复正常后会自动关闭

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