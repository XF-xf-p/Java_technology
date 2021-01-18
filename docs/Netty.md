## 1.Netty

Netty是一个高性能、异步事件驱动的NIO框架，它基于JavaNIO提供的API实现，提供了对TCP( Transmission Control Protocol，传输控制协议）、UDP(User Datagram Protocol，用户数据包协议）和文件传输的支持。作为一个异步NIO框架，Netty的所有I/O 操作都是异步非阻塞的，通过Future-Listener机制，用户可以方便地主动获取或者通过通知机制获取I/O操作结果。

## 2.Reactor线程模型

Reactor是一种并发处理客户端请求响应的事件驱动模型。服务端在接收到客户端请求后采用多路复用策略，通过一个非阻塞的线程来异步接收所有的客户端请求，并将这些请求转发到相关的工作线程组上进行处理。

Reactor模型常常基于异步线程的方式实现，常用的Reactor线程模型有3种：Reactor单线程模型、Reactor多线程模型和Reactor主从多线程模型。

## 3.JavaNIO 

Java NIO的实现主要涉及3个核心概念：Selector（选择器）、Channel（通道）和Buffer （缓冲区）。Selector用于监听多个Channel的事件，比如连接打开或数据到达，因此，一个线程可实现对多个Channel的管理。传统的I/O基于数据流进行I/O读写操作，而Java NIO基于Channel和Buffer进行I/O读写操作，并且数据总是从Channel读取到Buffer，或者从Buffer写入Channel。

### Channel 

Channel与I/O中的Stream（流）类似，只不过Stream是单向的（例如InputStream、OutputStream ），而Channel是双向的，既可以用来进行读操作，也可以用来进行写操作。

NIO中Channel的主要实现有：FileChannel、DatagramChannel、SocketChannel、ServerSocketChannel。FileChannel对应文件的l/O读写操作，DatagramChannel对应UDP的广播事件，SocketChannel对应Socket客户端的TCP的I/O读写操作，ServerSocketChannel对应Socket服务端的TCP的I/O读写操作。

### Buffer 

Buffer实际上是一个容器，其内部通过一个连续的字节数组存储I/O上的数据。在NIO中，Channel在文件、网络上对数据的读取或写入都必须经过Buffer。

客户端在向服务端发送数据时，必须先将数据写入Buffer，然后将Buffer中的数据写入服务端对应的Channel。服务端在接收数据时，必须通过Channel将数据读入Buffer，然后从Buffer读取数据并处理。

在NIO中，Buffer是一个抽象类，对不同的数据类型实现不同的Buffer操作。常用的Buffer实现类有：ByteBuffer、IntBuffer、CharBuffer、LongBuffer、DoubleBuffer、FloatBuffer、ShortBuffer。

### Selector 

Selector用于检测在多个注册的Channel上是否有I/O事件发生，并对检查到的I/O事件进行相应的响应和处理。因此，通过一个Selector线程可以实现对多个Channel的管理，而不必为每个连接都创建一个线程，避免线程资源的浪费和多线程之间的上下文切换导致的开销。同时，Selector只在Channel上有读写事件发生时，才会调用I/O函数进行读写操作，可极大地减少系统开销，提高系统的并发量。

## 4.Reactor单线程模型

Reactor单线程模型指所有的客户端I/O操作请求都在一个线程（Thread）上完成。

Reactor单线程模型的各模块组成及职责。

![](D:\workspace\Java-Interview-Offer\images\netty001.png)

( 1 ) Client: NIO客户端，向服务端发起TCP连接，并发送数据。

( 2 ) Acceptor: NIO服务端，通过Acceptor接收客户端的TCP连接。

( 3 ) Dispatcher：接收客户端的数据并将数据以ByteBuffer的形式发送到对应的编解码器。

( 4 ) DecoderHandler：解码器，读取客户端的数据并进行数据解码、数据处理和消息应答。

( 5 ) EncoderHandler：编码器，将给客户端发送的数据（消息请求或消息应答）进行统一的编码处理，并写入通道。

由于Reactor模式使用的是异步非阻塞I/O，因此一个线程可以独立处理多个I/O相关的操作，Reactor单线程模型将所有I/O操作都集中在一个线程中处理。具体处理流程如下。

( 1) Acceptor接收客户端的TCP连接请求消息.

( 2）链路建立成功后通过Dispatcher将接收到的消息写入ByteBuffer，并派发到对应的DecoderHandler上进行消息解码和处理。

( 3 ）消息处理完成后调用对应的EncoderHandler将请求响应进行消息的编码和下发。

## 5.Reactor多线程模型

![](D:\workspace\Java-Interview-Offer\images\netty002.png)

Reactor多线程模型与单线程模型最大的区别就是由一组线程（Thread Poll ）处理客户端的I/O请求操作。Reactor多线程模型将Acceptor的操作封装在一组线程池中，采用线程池的方式监听服务端端口、接收客户端的TCP连接请求、处理网络I/O读写等操线。线程池一般使用标准的JDK线程池，在该线程池中包含一个任务队列和一系列NIO线程，这些NIO线程负责具体的消息读取、解码、编码和发送。

Reactor多线程模型与单线程模型的区别在于，Reactor多线程模型用于接收客户端请求的Acceptor由一个线程来负责，用于处理客户端消息的Dispatcher由一个线程池负责，这样，基于线程池的调度和线程异步执行的能力，能够在同一时间内接收和处理更多的客户端请求。

## 6.Reactor主从多线程模型

![](D:\workspace\Java-Interview-Offer\images\netty003.png)

在Reactor主从多线程模型中，服务端用于接收客户端连接的不再是一个NIO线程，而是一个独立的NIO线程池。主线程Acceptor在接收到客户端的TCP连接请求并建立完成连接后（可能要经过鉴权、登录等过程），将新创建的SocketChannel注册到子I/O线程池（Sub Reactor Pool ）的某个I/O线程上，由它负责具体的SocketChannel的读写和编解码工作。

Reactor主从多线程模型中的Acceptor线程池（Acceptor Thread Pool ）只用于客户端的鉴权、登录、握手和安全认证，一旦链路建立成功，就将链路注册到后端Sub Reactor 线程池的I/O线程上，由I/O线程负责后续的I/O操作。这样就将客户端连接的建立和消息的响应都以异步线程的方式来实现，大大提高了系统的吞吐量。

## 7.Netty简介

Netty是一个高性能的异步事件驱动的网络应用程序框架，主要用于客户端和服务端网络应用程序的快速搭建和开发。基于Netty API可以非常快速、方便地构建一个高性能的TCP或UDP的网络应用，不但极大地简化了网络编程的复杂度，还提高了网络应用程序的性能和稳定性。Netty支持多种网络通信协议，包括TCP、UDP、FTP、HTTP、SMTP等。

## 8.Netty的架构设计

Netty优秀的架构设计使其能够简单快速地构建网络应用程序。

![](D:\workspace\Java-Interview-Offer\images\netty004.png)

Netty的整体架构分为Transport Services （传输服务层）、Protocol Support （传输协议层）和Core（核心层）3部分.

###  Transport Services

Transport Services指传输服务层，主要定义了数据的传输和通信方式，包括Socket And  Datagram ( Socket协议和数据包）、HTTP Tunnel ( HTTP隧道）、In-VM Pipe （本地传输管道）。

基于Socket的协议有TCP、UDP等。其中，TCP基于报文应答机制实现，主要用于可靠数据的传输，比如移动设备状态信息的传输。UDP发出的数据不需要对方应答，主要用于对数据安全性要求不是很高但是对数据传输吞吐量要求较高的场景，比如实时视频流传输。HTTP Tunnel定义了HTTP的传输方式。In-VM Pipe定义了本地数据的传输方式。

### Protocol Support 

Protocol Support指传输协议层，主要定义数据传输过程中的服务类型、数据安全、数据压缩等。具体包括HTTP And WebSocket ( HTTP和WebSocket服务）、SSL And StartTLS ( SSL和StartTLS协议）、zlib/gzip Compression ( zlib/gzip压缩算法）、Large File Transfer （大文件传输）、Google ProtoBuf ( Google ProtoBuf格式）、RTSP（实时流传输协议）、Legacy Text And Binary Protocols （传统TXT和二进制数据）。

HTTP和WebSocket服务定义了客户端和服务端的数据通信方式。HTTP服务基于HTTP实现，主要用于客户端主动向服务端发起数据包请求。WebSocket主要用于服务端基于消息推送的机制实时将数据包推送到客户端。

SSL ( Secure Sockets Layer，安全套接层）主要用于传输层与应用层之间的直接网络数据的加密，是为网络通信提供安全及数据完整性的一种安全协议。SSL包含记录层( Record Layer ）和传输层（Transport Layer ），记录层协议确定传输层数据的封装格式。传输层安全协议使用X.509认证，之后利用非对称加密演算来对通信方进行身份认证，最后交换对称密钥作为此次会话的密钥。基于该会话密钥来实现通信双方数据的加密，保证两个应用程序间通信的保密性和可靠性，使客户端与服务端之间的应用程序之间的通信不被攻击者窃听。

StartTLS是一种明文通信协议的扩展，能够让明文的通信连线直接成为密连线（使用SSL或TLS加密），而不需要使用另一个特别的端口来进行加密通信，属于机会性加密。StartTLS本身是一个与应用层无关的协议，可以搭配许多应用层协议一同使用。

zlib/gzip Compression定义了消息传递过程中数据的压缩和解压缩算法，主要用于提高批量数据传输过程中网络的吞吐量。zlib是一种数据压缩格式，其使用抽象化的DEFLATE算法实现，应用十分广泛，Linux内核中使用zlib以实现网络协议的压缩、文件系统的压缩。OpenSSH、OpenSSL以zlib达到最优化的加密网络传输。gzip的基础也是DEFLATE算法，gzip也是一种数据压缩格式。

Large File Transfer定义了大文件传输过程中数据的拆包和分发策略。

Google ProtoBuf是Google发布的一款开源的数据传输格式和序列化框架。它是一种语言中立、平台无关、可扩展的序列化数据的格式，可用于通信协议、数据存储等。在序列化数据方面，它提供了灵活高效的序列化实现。ProtoBuf很适合用于数据存储或RPC数据交换格式。

RTSP ( Real Time Streaming Protocol）为实时流传输协议，是一种网络应用协议，专为娱乐和通信系统使用，以控制流媒体服务器。该协议用于创建和控制终端之间的媒体会话。媒体服务器的客户端发布VCR命令，例如，播放、录制和暂停，以便实时控制从服务器到客户端（视频点播）或从客户端到服务器（语音录音）的媒体流。

Legacy Text And Binary Protocols提供了传统文本数据格式和二进制数据格式的传输支持。

### Core

Netty的Core（核心层）封装了Netty框架的核心服服和API，具体包括Extensible Event  Model（可扩展事件模型）、Universal Communication API （通用通信协议API)、Zero-Copy-Capable Rich Byte Buffer （零拷贝字节缓冲区）等。可扩展事件模型为Netty灵活的事件通信提供了基础；通用通信协议API为上层提供了统一的API访问入口，提高了Netty框架的易用性；零拷贝字节缓冲区为数据的快速读取和写入提供了保障。

## 9.Netty的核心组件

Netty的核心组件包括Bootstrap、ServerBootstrap、NioEventLoop、NioEventLoopGroup、Future、ChannelFuture、Channel、Selector、ChannelHandlerContext、ChannelHandler和ChannelPipeline。

( 1 ) Bootstrap/ServerBootstrap: Bootstrap用于客户端服务的启动引导，ServerBootstrap用于服务端服务的启动引导。

( 2 ) NioEventLoop：基于线程队列的方式执行事件操作，具体要执行的事件操作包括连接注册、端口绑定和I/O数据读写等。每个NioEventLoop线程都负责多个Channel的事件处理。

( 3) NioEventLoopGroup: NioEventLoop生命周期的管理。

( 4 ) Future/ChannelFuture:Future和ChannelFuture用于异步通信的实现，基于异步通信方式可以在I/O操作触发后注册一个监听事件，在I/O操作（数据读写完成或失败）完成后自动触发监听事件并完成后续操作。

( 5 ) Channel: Channel是Netty中的网络通信组件，用于执行具体的I/O操作。Netty中所有的数据通信都基于Channel读取或者将数据写入对应的Channel。Channel的主要功能包括网络连接的建立、连接状态的管理（网络连接的打开和关闭）、网络连接参数的配置（每次接收数据的大小）、基于异步NIO的网络数据操作（数据读取、数据写出）等。

( 6 ) Selector: Selector用于多路复用中Channel的管理。在Netty中，一个Selector可以管理多个Channel，在Channel连接建立后将连接注册到Selector，Selector在内部监听每个Channel上I/O事件的变化，当Channel有网络I/O事件发生时通知ChannelHandler执行具体的I/O操作（读取字节流或字节流写入完成）。

( 7) ChannelHandlerContext: Channel上下文信息的管理。每个ChannelHandler都对应一个ChannelHandlerContext。

( 8 ) ChannelHandler: I/O事件的拦截和处理。其中，ChannelInboundHandler用于处理数据接收的I/O操作，ChannelInboundHandler用于处理数据发送的I/O操作。

( 9 ) ChannelPipeline：基于拦截器设计模式实现的事件拦截处理和转发。Netty中的每个Channel都对应一个ChannelPipeline，在ChannelPipeline中维护了一个由ChannelHandlerContext组成的双向链表，每个ChannelHandlerContext都对应一个ChannelHandler，以完成具体Channel事件的拦截和处理。其中，数据入站由Head向Tail依次传递和处理，数据出站从Tail向Head依次传递和处理。

## 10.Netty的原理

Netty的运行核心包含两个NioEventLoopGroup工作组，一个是BossGroup，用于接收客户端连接、接收客户端数据和进行数据转发；另一个是WorkerGroup，用于具体I/O事件的触发和数据处理。

### Netty Server的初始化步骤

Netty Sever的初始化过程如下。

( 1）初始化BossGroup和WorkerGroup。

( 2 ）基于SeverBootstrap配置EventLoopGroup，包括连接参数设置、Channel类型设置、编解码Handler设置等。

( 3 ）绑定端口和服务启动。

###  BossGoup的职责

BossGroup为一个事件循环组，其中包含多个事件循环（NioEventLoop ），每个NioEventLoop都包含一个Selector和一个TaskQueue（事件循环线程）。每个BossNioEventLoop循环都执行以下3个步骤。

( 1 ）轮询监听Accept事件.

( 2 ）接收和处理Accept事件，包括和客户端建立连接并生成NioSocketChannel，将NioSocketChannel注册到某个WorkerNioEventLoop的Selector上。

( 3 ）处理runAllTasks的任务。

### WorkerGroup的职责

WorkerGroup为一个事件循环组，其中包含多个事件循环（NioEventLoop ），每个Work NioEventLoop循环都执行以下3个步骤。

( 1 ）轮询监听NioSocketChannel上的I/O事件（I/O 读写事件）。

( 2 ）当NioSocketChannel有I/O事件触发时执行具体的I/O操作。

( 3 ）处理任务队列中的任务。

BossGroupWorkerGroup工作流程。

![](D:\workspace\Java-Interview-Offer\images\netty005.png)

## 11.Netty的特性

Netty之所以能在高并发的分布式网络环境下实现对数据的快速处理和分发，得益于其优秀的架构设计。Netty架构设计的主要特性有多路复用模型、数据零拷贝、内存重用机制、无锁化设计和高性能的序列化框架。

## 12.I/O多路复用模型

在高并发的I/O编程模型中，一个服务端往往要同时处理成千上万个客户端请求。传统的I/O模型会为每个Socket链路都建立一个线程专门处理该链路上数据的接收和发送。该方案的特点是，当客户端较少时，服务端的数据处理速度很快，但是当客户端的数量较多时，服务端会出现没有足够的线程资源为每个Socket链路分配一个线程的情况，服务端会出现大量的线程资源争抢和调度，性能急速下降。因此，该方案不适合高并发的场景。

当有大量客户端并发请求时，可以采用I/O多路复用模型处理该问题。I/O多路复用模型将I/O的处理分为Selector和Channel。Selector负责多个Channel的监控和管理，Channel负责真正的I/O读写操作。当Selector监控到Channel上有数据发生变化时会通知对应的Channel，Channel接收到对应的通知后处理对应的数据即可。这样一个Selector线程可以同时处理多个客户端请求。与传统的多线程I/O模型相比，I/O多路复用模型的最大优势是系统开销小，系统不需要为每个客户端连接都建立一个新的线程，也不需要维护这些线程的运行，减少了系统的维护工作量，节省了系统资源。

Netty通过在NioEventLoop（事件轮询机制）内封装Selector来实现I/O的多路复用，在一个NioEventLoop上服务端可以同时并发处理成千上万个客户端请求，同时由于Netty的I/O读写操作都是基于ByteBuffer异步非阻塞的，因此大大提高了I/O线程的运行效率，避免由于频繁I/O阻塞导致的线程挂起和系统资源的浪费。

## 13.数据零烤贝

Linux为了实现操作系统程序和应用程序的资源隔离，将系统分为内核态和用户态。操作系统的程序运行在内核态，具体包括进程管理、存储管理、文件系统、网络通信和模块机制；应用程序运行在用户态并且不能访问内核态的地址空间，如果应用程序需要访问系统内核态的资源，则需要通过系统调用接口或本地函数库（Libc）来完成，具体流程为用户程序调用系统应用接口或本地函数库，系统应用接口或本地函数库调用系统进程获取资源，并将其分配到对应的用户程序，用户程序资源在使用完成后通过系统调用接口或本地函数库释放资源。

传统的Socket服务基于JVM堆内存进行Socket读写，也就是说，申请内存资源时，需要通过JVM向操作系统申请堆内存，然后JVM将堆内存复制一份直接内存中，基于直接内存进行Socket读写。这样就存在频繁的JVM内存数据和Socket线程内存数据来回复制的问题，影响系统性能。

Netty的数据接收和发送均采用堆外直接内存进行Socket读写，堆外直接内存可以直接操作系统内存，不需要来回地进行字节缓冲区的二次复制，大大提高了系统的性能。

同时，Netty提供了组合Buffer对象，基于该对象可以聚合多个ByteBuffer对象，使得用户操作多个Buffer与操作一个Buffer一样方便，避免了传统的通过内存复制的方式将多个小Buffer合并为一个大Buffer带来的使用不便和性能损耗。

Netty的文件传输采用transferTo方法。transferTo方法可以直接将文件缓冲区的数据基于内存映射技术发送到目标Channel，避免了通过循环写方式导致的内存复制问题。

## 14.内存重用机制

JVM对于对象内存的分配和回收耗时很小，但Netty数据的接收和发送均采用堆外直接内存缓存的方式实现，而堆外直接内存缓存的分配和回收是一件耗时的操作。为了尽量重用缓冲区，Netty提供了基于内存池的缓冲区重用机制。

## 15.无锁化设计

对于一般程序来说，多线程会提高系统的并发度，但是线程数并不是越多越好，过多的线程会引起CPU的频繁切换而增加系统的负载。Netty内部采用串行无锁化设计的思想对I/O进行操作，避免多线程竞争CPU和资源锁定导致的性能下降。在具体的使用中，可以调整NIO线程池的线程参数，同时启动多个串行化的线程并行运行，这种局部无锁化的串行多线程设计比一个队列结合多个工作线程模型的性能更佳。

Netty的NioEventLoop的设计思路是Channel读取消息后，直接调用ChannelPipeline的fireChannelRead(Object msg）进行消息的处理，如果在运行过程中用户不主动切换线程，Netty的NioEventLoop则一直在该线程上进行消息的处理，这种线程绑定CPU持续执行的方式可以有效减少系统资源的竞争和切换，对于持续高并发的操作来说性能有很大的提升。

## 16.高性能的序列化框架

Netty默认基于Google ProtoBuf实现数据的序列化，通过扩展Netty的编解码接口，用户可以实现自定义的序列化框架，例如Thrift的压缩二进制编解码框架。在使用ProtoBuf序列化的时候需要注意以下几点。

( 1 ) SO_RCVBUF和SO_SNDBUF的设置：SO_RCVBUF为接收数据的Buffer大小，SO_SNDBUF为发送数据的Buffer大小，通常建议值为128KB或者256KB

( 2) SO_TCPNODELAY的设置：将SO_TCPNODELAY 设置为true，表示开启自动粘包操作，该操作采用Nagle算法将缓冲区内字节较少的包前后相连组成一个大包一起发送，防止大量小包频繁发送造成网络的阻塞，从而提高网络的吞吐量。该算法对单条数据延迟的要求不高，但在传输数据量大的情况下能显著提高性能。

( 3 ）软中断：在开启软中断后，Netty会根据数据包的源地址、源端口、目的地址、目的端口计算一个Hash值，根据该Hash值选择对应的CPU执行软中断。即Netty将每个连接都与CPU绑定，并通过Hash值在多个CPU上均衡软中断，以提高CPU的性能。

## 17.Netty的使用

Netty的使用分为客户端和服务端两部分。客户端用于连接服务端上报数据，并接收服务端下发的请求指令等。服务端主要用于接收客户端的数据，并根据协议的规定对客户端的消息进行响应。一般Netty的性能优化主要在服务端，因为服务端需要同时承载成千上万的客户端并发连接。下面介绍一个简单的Netty服务的实现。

( 1 ）定义通用消息格式BaseMessage。

定义客户端和服务端交互的基本消息格式BaseMessage，具体包括创建消息的流水号Messageld、消息创建的时间createTime、消息接收的时间receiveTime和消息体MessageContent。

( 2 ）定义消息处理工具类MessageUtils.

该类义了getBaseMessage（）方法，用于将从Channel中获取的ByteBuf转换成标准的消息格式，这里的ByteBuf是Netty自己封装的Byte流存储格式，客户端上报的数据和服务端下发的数据均被写入ByteBuf。

( 3 ）定义NettyServer。

该类的核心方法是bind（）方，在该方法中定义了ServerBootstrap，并通过bootstrap.bind(port).sync（）启动一个Netty服务，这里核心的部分是通过bootstrap.childHandler（）定义编解码器，当服务端收到消息后，Netty会调用对应的编解码器中的方法，以完成数据的接收、解码、编码和发送工作。

( 4）定义MessageDecoder解码器。

它继承了ChanneHandlerAdapter，并实现了其channelRead（）方法，该方法主要用于接收客户端发送的数据。当Netty通道检测到数据变化时，会将数据写入相应的ByteBuf，并调用channelRead（）方法完成消息的接收，服务端接收到消息后，将其解码转化成标准的消息格式并进行业务处理，处理完成后对消息进行回复。

同时，MessageDecoder实现了channelRegistered（）方法，该方法在服务端接收到新的通道连接后会被调用；实现了handlerRemoved（）方法，该方法在Netty检测到链路断开后会被调用，以便服务端将失效的连接从Session管理中剔除；还实现了exceptionCaught() 方法，该方法在Netty检测到链路异常的时候会被调用，一般引起异常的原因有网络异常、服务端资源不足等。

( 5 ）定义NettyClient。

该类在start（）中实现了对客户端的定义。对客户端的定义和服务端基本相同，也有编码器、解码器和一些网络参数，核心的不同是客户端通过bootstrap.remoteAddress(host，port）来定义要连接的远程服务端的地址，并通过bootstrap. connect(host,port). sync（）实现服务端的连接。

( 6 ）定义NettyClientHandler消息处理器。

该类继承了ChannelHandlerAdapter，并实现了其channelActive（）方法，用来在客户端激活后向服务端发送数据；实现了channelRead（）方法，用来接收服务端下发的消息。

( 7) Netty的使用

分别执行main（）方法启动NettyServer和NettyClient,NettyClient在启动后会向服务端发送消息，服务端在收到消息后会做出消息回应。