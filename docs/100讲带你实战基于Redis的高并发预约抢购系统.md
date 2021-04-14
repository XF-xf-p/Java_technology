 

### 01_课程介绍以及高并发高可用复杂系统中的缓存架构有哪些东西？

第一讲，简单来跟大家说一下，4块 

1、现在常见的java工程师/架构师对缓存技术的了解和掌握程度 

我常年在一些国内最大的那些互联网公司里吧，负责招人，java这块，我们也会招，各种各样的人，我都见过 

大型的互联网公司的人，传统行业的一些人，初级的人，高阶的架构师，高级工程师，技术经理，技术总监，带几十个人 

缓存技术，复杂的场景的时候，很复杂的缓存架构 

工作中都会用到一些缓存技术，redis/memcached基础使用，初步的集群知识 

我面试过的人里，能掌握到很少的缓存架构的人，屈指可数，个位数，而且都是在大公司有过类似的大型复杂系统架构经验的人 

2、缓存架构/技术掌握的不够，对你的发展带来了哪些阻碍？ 

工作中 

如果你这块技术掌握不够，然后你的公司的项目遇到了一些相关的难题，高并发+高性能的场景，hold不住类似的这种高并发的系统 

因为缓存架构做得不好，不到位，实际在公司的项目里，出了一些大case，导致系统崩溃，巨大的经济损失 

职业发展中 

redis，memcached，activemq，zookeeper，kafka，lucene，activiti，爬虫，或者等等，各种技术，写了都几十种技术 

没有一样是精通的，redis就会简单的操作，memcached操作，activemq，zookeeper，爬虫，全都是简单的操作 

架构设计思路，有没有一些考量的点，高并发的中场景，高可用的场景，说不出来 

不可能做到更高级的一个职位了，因为很多公司的人也不傻，技术一看就平平庸庸，怎么给你一个很好的职位呢？职业发展怎么做上去呢？ 

亮点，技术亮点，高人一筹 

java高工，java资深工，java架构，技术亮点，造诣 

如果你的技术很牛，各种技术都有深度，架构面临过一些复杂的场景，别人搞不定的高并发高可用的系统架构，你都能搞定，职业发展就会做的很好 

3、课程的一个简单的介绍 

基于高并发的口罩预约抢购系统，业务背景，简化，贯穿起来，学习到百万流量的电商网站，商品详情页的整体架构设计，学到的 

复杂的缓存架构：才是我们最真实要讲解的东西，支撑高并发，高可用 

缓存架构过程中：我们会讲解各种高并发场景下的各种难题，怎么去解决这些难题，缓存架构的过程，各种技术和解决方案，高可用性，解决缓存架构中面临的一些高可用的问题，包括怎么去解决，技术，解决方案 

这套课程，学到很多的全网独家的技术 

大型电商网站的商品详情页系统的架构

复杂的缓存架构

如何用复杂的缓存架构去支撑高并发

利用将缓存架构做成高可用机会，也可以学到高可用系统架构构建的技术 

4、真正能支撑高并发以及高可用的复杂系统中的缓存架构有哪些东西？ 

（1）如何让redis集群支撑几十万QPS高并发+99.99%高可用+TB级海量数据+企业级数据备份与恢复？：redis企业级集群架构 

（2）如何支撑高性能以及高并发到极致？

### 02_基于大型电商网站中的口罩预约抢购系统贯穿的授课思路介绍？

我授课的一个思路，龙果另外两个课程，es的快速入门+es高手进阶 

讲解一个技术，一个课程，就是纯讲技术，按照一个一个的技术点去讲解 

决定拿一个从真实的系统中抽离出来的，简化过后的一个项目，去贯穿整个课程 

项目去讲解，提供了连续而且仿真的一个业务场景 

各种各样的业务场景，以及在业务场景中面临的难题和问题，去学习一个又一个的技术或者解决方案，或者架构设计思想 

基于大型电商网站中的口罩预约抢购系统 

最最核心的架构就是缓存架构，商品详情页系统整体有自己整体的架构 

一步一步的去分析商品详情页系统中的一些核心的部分，涉及到最最主要的就是缓存架构，高并发 

缓存架构，一步一步讲解各种各样支撑高并发场景的缓存技术，解决方案，架构设计 

如何将缓存架构本身做成高可用的架构，缓存架构本身面临的可用性的问题 

### 03_单机版redis的安装以及redis生产环境启动方案 

课程大纲 

1、安装单机版redis

2、redis的生产环境启动方案

3、redis cli的使用 

\------------------------------------------------------------------------ 

1、安装单机版redis 

大家可以自己去官网下载，当然也可以用课程提供的压缩包 

wget http://downloads.sourceforge.net/tcl/tcl8.6.1-src.tar.gz

tar -xzvf tcl8.6.1-src.tar.gz

cd /usr/local/tcl8.6.1/unix/

./configure 

make && make install 

使用redis-3.2.8.tar.gz（截止2017年4月的最新稳定版）

tar -zxvf redis-3.2.8.tar.gz

cd redis-3.2.8

make && make test && make install 

\------------------------------------------------------------------------ 

2、redis的生产环境启动方案 

如果一般的学习课程，你就随便用redis-server启动一下redis，做一些实验，这样的话，没什么意义 

要把redis作为一个系统的daemon进程去运行的，每次系统启动，redis进程一起启动 

（1）redis utils目录下，有个redis_init_script脚本

（2）将redis_init_script脚本拷贝到linux的/etc/init.d目录中，将redis_init_script重命名为redis_6379，6379是我们希望这个redis实例监听的端口号

（3）修改redis_6379脚本的第6行的REDISPORT，设置为相同的端口号（默认就是6379）

（4）创建两个目录：/etc/redis（存放redis的配置文件），/var/redis/6379（存放redis的持久化文件）

（5）修改redis配置文件（默认在根目录下，redis.conf），拷贝到/etc/redis目录中，修改名称为6379.conf 

（6）修改redis.conf中的部分配置为生产环境 

daemonize yes           让redis以daemon进程运行

pidfile    /var/run/redis_6379.pid 设置redis的pid文件位置

port    6379           设置redis的监听端口号

dir     /var/redis/6379       设置持久化文件的存储位置 

（7）启动redis，执行cd /etc/init.d, chmod 777 redis_6379，./redis_6379 start 

（8）确认redis进程是否启动，ps -ef | grep redis 

（9）让redis跟随系统启动自动启动 

在redis_6379脚本中，最上面，加入两行注释 

\# chkconfig:  2345 90 10 

\# description: Redis is a persistent key-value database 

chkconfig redis_6379 on 

\------------------------------------------------------------------------ 

3、redis cli的使用 

redis-cli SHUTDOWN，连接本机的6379端口停止redis进程 

redis-cli -h 127.0.0.1 -p 6379 SHUTDOWN，制定要连接的ip和端口号 

redis-cli PING，ping redis的端口，看是否正常 

redis-cli，进入交互式命令行 

SET k1 v1

GET k1 

redis的技术，包括4块 

redis各种数据结构和命令的使用，包括java api的使用

redis一些特殊的解决方案的使用，pub/sub消息系统，分布式锁，输入的自动完成，等等

redis日常的管理相关的命令

redis企业级的集群部署和架构 

我们这套课程，实际上是针对企业级的大型缓存架构，用得项目，真实的高并发口罩预约抢购系统（缓存） 

我们首先讲解的第一块，其实就是企业级的大型缓存架构中的，redis集群架构（海量数据、高并发、高可用），最最流行，最最常用的分布式缓存系统 

后面我们做商品详情页系统的业务开发的时候，当然也会去用redis的一些命令 

redis基础知识：教程，书籍，视频 

redis持久化、主从架构、复制原理、集群架构、数据分布式存储原理、哨兵原理、高可用架构 

网上一些redis的教程，持久化，集群，哨兵，也讲了，都是泛泛而讲，简单带你搭建一下 

我会深入集群架构的底层原理，哨兵的底层原理，用一线的经验，告诉你，redis的大规模的架构师如何去支撑海量数据、高并发、高可用的  

### 04_redis持久化机对于生产环境中的灾难恢复的意义

课程大纲 

1、故障发生的时候会怎么样

2、如何应对故障的发生 

很多同学，自己也看过一些redis的资料和书籍，当然可能也看过一些redis视频课程 

所有的资料，其实都会讲解redis持久化，但是有个问题，我到目前为止，没有看到有人很仔细的去讲解，redis的持久化意义 

redis的持久化，RDB，AOF，区别，各自的特点是什么，适合什么场景 

redis的企业级的持久化方案是什么，是用来跟哪些企业级的场景结合起来使用的？？？ 

redis持久化的意义，在于故障恢复 

比如你部署了一个redis，作为cache缓存，当然也可以保存一些较为重要的数据 

如果没有持久化的话，redis遇到灾难性故障的时候，就会丢失所有的数据 

如果通过持久化将数据搞一份儿在磁盘上去，然后定期比如说同步和备份到一些云存储服务上去，那么就可以保证数据不丢失全部，还是可以恢复一部分数据回来的 

### 05_图解分析redis的RDB和AOF两种持久化机制的工作原理 

现代操作系统中，写文件不是直接写入磁盘，会先写os cache，然后到一定时候再从os cache到disk file

redis中的数据，是有一定数量的，不可能说redis内存中的数据无限增长，从而导致AOF无限增长。内存大小是一定的，到一定时候，redis就会用缓存淘汰算法，LRU，自动将一部分数据从内存中清除。

AOF，是存放每条写入命令的，所以会不断地膨胀，当大到一定的时候，AOF做rewrite操作，AOF rewrite操作就会基于当时redis内存中的数据，来重新构造一个更小的AOF文件，然后将旧的膨胀的很大的文件给删了。

每隔1秒，调用一次操作系统fsync操作，强制将os cacche中的数据，刷入磁盘文件中

### 06_redis的RDB和AOF两种持久化机制的优劣势对比 

课程大纲 

1、RDB和AOF两种持久化机制的介绍

2、RDB持久化机制的优点

3、RDB持久化机制的缺点

4、AOF持久化机制的优点

5、AOF持久化机制的缺点

6、RDB和AOF到底该如何选择 

我们已经知道对于一个企业级的redis架构来说，持久化是不可减少的 

企业级redis集群架构：海量数据、高并发、高可用

持久化主要是做灾难恢复，数据恢复，也可以归类到高可用的一个环节里面去 

比如你redis整个挂了，然后redis就不可用了，你要做的事情是让redis变得可用，尽快变得可用 

重启redis，尽快让它对外提供服务，但是就像上一讲说，如果你没做数据备份，这个时候redis启动了，也不可用啊，数据都没了 

很可能说，大量的请求过来，缓存全部无法命中，在redis里根本找不到数据，这个时候就死定了，缓存雪崩问题，所有请求，没有在redis命中，就会去mysql数据库这种数据源头中去找，一下子mysql承接高并发，然后就挂了 

mysql挂掉，你都没法去找数据恢复到redis里面去，redis的数据从哪儿来？从mysql来。。。 

具体的完整的缓存雪崩的场景，还有企业级的解决方案，到后面讲 

如果你把redis的持久化做好，备份和恢复方案做到企业级的程度，那么即使你的redis故障了，也可以通过备份数据，快速恢复，一旦恢复立即对外提供服务 

redis的持久化，跟高可用，是有关系的，企业级redis架构中去讲解 

redis持久化：RDB，AOF 

\------------------------------------------------------------------------------------- 

1、RDB和AOF两种持久化机制的介绍 

RDB持久化机制，对redis中的数据执行周期性的持久化 

AOF机制对每条写入命令作为日志，以append-only的模式写入一个日志文件中，在redis重启的时候，可以通过回放AOF日志中的写入指令来重新构建整个数据集 

如果我们想要redis仅仅作为纯内存的缓存来用，那么可以禁止RDB和AOF所有的持久化机制 

通过RDB或AOF，都可以将redis内存中的数据给持久化到磁盘上面来，然后可以将这些数据备份到别的地方去，比如说阿里云，云服务 

如果redis挂了，服务器上的内存和磁盘上的数据都丢了，可以从云服务上拷贝回来之前的数据，放到指定的目录中，然后重新启动redis，redis就会自动根据持久化数据文件中的数据，去恢复内存中的数据，继续对外提供服务 

如果同时使用RDB和AOF两种持久化机制，那么在redis重启的时候，会使用AOF来重新构建数据，因为AOF中的数据更加完整 

\------------------------------------------------------------------------------------- 

2、RDB持久化机制的优点 

（1）RDB会生成多个数据文件，每个数据文件都代表了某一个时刻中redis的数据，这种多个数据文件的方式，非常适合做冷备，可以将这种完整的数据文件发送到一些远程的安全存储上去，比如说Amazon的S3云服务上去，在国内可以是阿里云的ODPS分布式存储上，以预定好的备份策略来定期备份redis中的数据 

RDB也可以做冷备，生成多个文件，每个文件都代表了某一个时刻的完整的数据快照

AOF也可以做冷备，只有一个文件，但是你可以，每隔一定时间，去copy一份这个文件出来 

RDB做冷备，优势在哪儿呢？由redis去控制固定时长生成快照文件的事情，比较方便; AOF，还需要自己写一些脚本去做这个事情，各种定时

RDB数据做冷备，在最坏的情况下，提供数据恢复的时候，速度比AOF快 

（2）RDB对redis对外提供的读写服务，影响非常小，可以让redis保持高性能，因为redis主进程只需要fork一个子进程，让子进程执行磁盘IO操作来进行RDB持久化即可 

RDB，每次写，都是直接写redis内存，只是在一定的时候，才会将数据写入磁盘中

AOF，每次都是要写文件的，虽然可以快速写入os cache中，但是还是有一定的时间开销的,速度肯定比RDB略慢一些 

（3）相对于AOF持久化机制来说，直接基于RDB数据文件来重启和恢复redis进程，更加快速 

AOF，存放的指令日志，做数据恢复的时候，其实是要回放和执行所有的指令日志，来恢复出来内存中的所有数据的

RDB，就是一份数据文件，恢复的时候，直接加载到内存中即可 

结合上述优点，RDB特别适合做冷备份，冷备 

\------------------------------------------------------------------------------------- 

3、RDB持久化机制的缺点 

（1）如果想要在redis故障时，尽可能少的丢失数据，那么RDB没有AOF好。一般来说，RDB数据快照文件，都是每隔5分钟，或者更长时间生成一次，这个时候就得接受一旦redis进程宕机，那么会丢失最近5分钟的数据 

这个问题，也是rdb最大的缺点，就是不适合做第一优先的恢复方案，如果你依赖RDB做第一优先恢复方案，会导致数据丢失的比较多 

（2）RDB每次在fork子进程来执行RDB快照数据文件生成的时候，如果数据文件特别大，可能会导致对客户端提供的服务暂停数毫秒，或者甚至数秒 

一般不要让RDB的间隔太长，否则每次生成的RDB文件太大了，对redis本身的性能可能会有影响的 

\------------------------------------------------------------------------------------- 

4、AOF持久化机制的优点

 （1）AOF可以更好的保护数据不丢失，一般AOF会每隔1秒，通过一个后台线程执行一次fsync操作，最多丢失1秒钟的数据 

每隔1秒，就执行一次fsync操作，保证os cache中的数据写入磁盘中 

redis进程挂了，最多丢掉1秒钟的数据 

（2）AOF日志文件以append-only模式写入，所以没有任何磁盘寻址的开销，写入性能非常高，而且文件不容易破损，即使文件尾部破损，也很容易修复 

（3）AOF日志文件即使过大的时候，出现后台重写操作，也不会影响客户端的读写。因为在rewrite log的时候，会对其中的指导进行压缩，创建出一份需要恢复数据的最小日志出来。再创建新日志文件的时候，老的日志文件还是照常写入。当新的merge后的日志文件ready的时候，再交换新老日志文件即可。 

（4）AOF日志文件的命令通过非常可读的方式进行记录，这个特性非常适合做灾难性的误删除的紧急恢复。比如某人不小心用flushall命令清空了所有数据，只要这个时候后台rewrite还没有发生，那么就可以立即拷贝AOF文件，将最后一条flushall命令给删了，然后再将该AOF文件放回去，就可以通过恢复机制，自动恢复所有数据 

\------------------------------------------------------------------------------------- 

5、AOF持久化机制的缺点 

（1）对于同一份数据来说，AOF日志文件通常比RDB数据快照文件更大 

（2）AOF开启后，支持的写QPS会比RDB支持的写QPS低，因为AOF一般会配置成每秒fsync一次日志文件，当然，每秒一次fsync，性能也还是很高的 

如果你要保证一条数据都不丢，也是可以的，AOF的fsync设置成没写入一条数据，fsync一次，那就完蛋了，redis的QPS大降 

（3）以前AOF发生过bug，就是通过AOF记录的日志，进行数据恢复的时候，没有恢复一模一样的数据出来。所以说，类似AOF这种较为复杂的基于命令日志/merge/回放的方式，比基于RDB每次持久化一份完整的数据快照文件的方式，更加脆弱一些，容易有bug。不过AOF就是为了避免rewrite过程导致的bug，因此每次rewrite并不是基于旧的指令日志进行merge的，而是基于当时内存中的数据进行指令的重新构建，这样健壮性会好很多。 

（4）唯一的比较大的缺点，其实就是做数据恢复的时候，会比较慢，还有做冷备，定期的备份，不太方便，可能要自己手写复杂的脚本去做，做冷备不太合适 

\------------------------------------------------------------------------------------- 

6、RDB和AOF到底该如何选择 

（1）不要仅仅使用RDB，因为那样会导致你丢失很多数据 

（2）也不要仅仅使用AOF，因为那样有两个问题，第一，你通过AOF做冷备，没有RDB做冷备，来的恢复速度更快; 第二，RDB每次简单粗暴生成数据快照，更加健壮，可以避免AOF这种复杂的备份和恢复机制的bug 

（3）综合使用AOF和RDB两种持久化机制，用AOF来保证数据不丢失，作为数据恢复的第一选择; 用RDB来做不同程度的冷备，在AOF文件都丢失或损坏不可用的时候，还可以使用RDB来进行快速的数据恢复

###  07_redis的RDB持久化配置以及数据恢复实验

课程大纲

1、如何配置RDB持久化机制
2、RDB持久化机制的工作流程
3、基于RDB持久化机制的数据恢复实验

------------------------------------------------------------------------

1、如何配置RDB持久化机制

redis.conf文件，也就是/etc/redis/6379.conf，去配置持久化

save 60 1000

每隔60s，如果有超过1000个key发生了变更，那么就生成一个新的dump.rdb文件，就是当前redis内存中完整的数据快照，这个操作也被称之为snapshotting，快照

也可以手动调用save或者bgsave命令，同步或异步执行rdb快照生成

save可以设置多个，就是多个snapshotting检查点，每到一个检查点，就会去check一下，是否有指定的key数量发生了变更，如果有，就生成一个新的dump.rdb文件

------------------------------------------------------------------------

2、RDB持久化机制的工作流程

（1）redis根据配置自己尝试去生成rdb快照文件
（2）fork一个子进程出来
（3）子进程尝试将数据dump到临时的rdb快照文件中
（4）完成rdb快照文件的生成之后，就替换之前的旧的快照文件

dump.rdb，每次生成一个新的快照，都会覆盖之前的老快照

------------------------------------------------------------------------

3、基于RDB持久化机制的数据恢复实验

（1）在redis中保存几条数据，立即停掉redis进程，然后重启redis，看看刚才插入的数据还在不在

数据还在，为什么？

带出来一个知识点，通过redis-cli SHUTDOWN这种方式去停掉redis，其实是一种安全退出的模式，redis在退出的时候会将内存中的数据立即生成一份完整的rdb快照

/var/redis/6379/dump.rdb

（2）在redis中再保存几条新的数据，用kill -9粗暴杀死redis进程，模拟redis故障异常退出，导致内存数据丢失的场景

这次就发现，redis进程异常被杀掉，数据没有进dump文件，几条最新的数据就丢失了

（2）手动设置一个save检查点，save 5 1
（3）写入几条数据，等待5秒钟，会发现自动进行了一次dump rdb快照，在dump.rdb中发现了数据
（4）异常停掉redis进程，再重新启动redis，看刚才插入的数据还在

rdb的手动配置检查点，以及rdb快照的生成，包括数据的丢失和恢复，全都演示过了

### 08_redis的AOF持久化深入讲解各种操作和相关实验

课程大纲 

1、AOF持久化的配置

2、AOF持久化的数据恢复实验

3、AOF rewrite

4、AOF破损文件的修复

5、AOF和RDB同时工作 

\------------------------------------------------------------------------------ 

1、AOF持久化的配置 

AOF持久化，默认是关闭的，默认是打开RDB持久化 

appendonly yes，可以打开AOF持久化机制，在生产环境里面，一般来说AOF都是要打开的，除非你说随便丢个几分钟的数据也无所谓 

打开AOF持久化机制之后，redis每次接收到一条写命令，就会写入日志文件中，当然是先写入os cache的，然后每隔一定时间再fsync一下 

而且即使AOF和RDB都开启了，redis重启的时候，也是优先通过AOF进行数据恢复的，因为aof数据比较完整 

可以配置AOF的fsync策略，有三种策略可以选择，一种是每次写入一条数据就执行一次fsync; 一种是每隔一秒执行一次fsync; 一种是不主动执行fsync 

always: 每次写入一条数据，立即将这个数据对应的写日志fsync到磁盘上去，性能非常非常差，吞吐量很低; 确保说redis里的数据一条都不丢，那就只能这样了 

mysql -> 内存策略，大量磁盘，QPS到多少，一两k。QPS，每秒钟的请求数量

redis -> 内存，磁盘持久化，QPS到多少，单机，一般来说，上万QPS没问题 

everysec: 每秒将os cache中的数据fsync到磁盘，这个最常用的，生产环境一般都这么配置，性能很高，QPS还是可以上万的

no: 仅仅redis负责将数据写入os cache就撒手不管了，然后后面os自己会时不时有自己的策略将数据刷入磁盘，不可控了 

\------------------------------------------------------------------------------ 

2、AOF持久化的数据恢复实验 

（1）先仅仅打开RDB，写入一些数据，然后kill -9杀掉redis进程，接着重启redis，发现数据没了，因为RDB快照还没生成

（2）打开AOF的开关，启用AOF持久化

（3）写入一些数据，观察AOF文件中的日志内容 

其实你在appendonly.aof文件中，可以看到刚写的日志，它们其实就是先写入os cache的，然后1秒后才fsync到磁盘中，只有fsync到磁盘中了，才是安全的，要不然光是在os cache中，机器只要重启，就什么都没了 

（4）kill -9杀掉redis进程，重新启动redis进程，发现数据被恢复回来了，就是从AOF文件中恢复回来的 

redis进程启动的时候，直接就会从appendonly.aof中加载所有的日志，把内存中的数据恢复回来 

\------------------------------------------------------------------------------ 

3、AOF rewrite 

redis中的数据其实有限的，很多数据可能会自动过期，可能会被用户删除，可能会被redis用缓存清除的算法清理掉 

redis中的数据会不断淘汰掉旧的，就一部分常用的数据会被自动保留在redis内存中 

所以可能很多之前的已经被清理掉的数据，对应的写日志还停留在AOF中，AOF日志文件就一个，会不断的膨胀，到很大很大 

所以AOF会自动在后台每隔一定时间做rewrite操作，比如日志里已经存放了针对100w数据的写日志了; redis内存只剩下10万; 基于内存中当前的10万数据构建一套最新的日志，到AOF中; 覆盖之前的老日志; 确保AOF日志文件不会过大，保持跟redis内存数据量一致 

redis 2.4之前，还需要手动，开发一些脚本，crontab，通过BGREWRITEAOF命令去执行AOF rewrite，但是redis 2.4之后，会自动进行rewrite操作 

在redis.conf中，可以配置rewrite策略 

auto-aof-rewrite-percentage 100

auto-aof-rewrite-min-size 64mb 

比如说上一次AOF rewrite之后，是128mb 

然后就会接着128mb继续写AOF的日志，如果发现增长的比例，超过了之前的100%，256mb，就可能会去触发一次rewrite 

但是此时还要去跟min-size，64mb去比较，256mb > 64mb，才会去触发rewrite 

（1）redis fork一个子进程

（2）子进程基于当前内存中的数据，构建日志，开始往一个新的临时的AOF文件中写入日志

（3）redis主进程，接收到client新的写操作之后，在内存中写入日志，同时新的日志也继续写入旧的AOF文件

（4）子进程写完新的日志文件之后，redis主进程将内存中的新日志再次追加到新的AOF文件中

（5）用新的日志文件替换掉旧的日志文件 

\------------------------------------------------------------------------------ 

4、AOF破损文件的修复 

如果redis在append数据到AOF文件时，机器宕机了，可能会导致AOF文件破损 

用redis-check-aof --fix命令来修复破损的AOF文件 

\------------------------------------------------------------------------------ 

5、AOF和RDB同时工作 

（1）如果RDB在执行snapshotting操作，那么redis不会执行AOF rewrite; 如果redis再执行AOF rewrite，那么就不会执行RDB snapshotting

（2）如果RDB在执行snapshotting，此时用户执行BGREWRITEAOF命令，那么等RDB快照生成之后，才会去执行AOF rewrite

（3）同时有RDB snapshot文件和AOF日志文件，那么redis重启的时候，会优先使用AOF进行数据恢复，因为其中的日志更完整 

\------------------------------------------------------------------------------ 

6、最后一个小实验，让大家对redis的数据恢复有更加深刻的体会 

（1）在有rdb的dump和aof的appendonly的同时，rdb里也有部分数据，aof里也有部分数据，这个时候其实会发现，rdb的数据不会恢复到内存中

（2）我们模拟让aof破损，然后fix，有一条数据会被fix删除

（3）再次用fix得aof文件去重启redis，发现数据只剩下一条了 

数据恢复完全是依赖于底层的磁盘的持久化的，主要rdb和aof上都没有数据，那就没了

### 09_在项目中部署redis企业级数据备份方案以及各种踩坑的数据恢复容灾演练

到这里为止，其实还是停留在简单学习知识的程度，学会了redis的持久化的原理和操作，但是在企业中，持久化到底是怎么去用得呢？ 

企业级的数据备份和各种灾难下的数据恢复，是怎么做得呢？ 

1、企业级的持久化的配置策略 

在企业中，RDB的生成策略，用默认的也差不多 

save 60 10000：如果你希望尽可能确保说，RDB最多丢1分钟的数据，那么尽量就是每隔1分钟都生成一个快照，低峰期，数据量很少，也没必要 

10000->生成RDB，1000->RDB，这个根据你自己的应用和业务的数据量，你自己去决定 

AOF一定要打开，fsync，everysec 

auto-aof-rewrite-percentage 100: 就是当前AOF大小膨胀到超过上次100%，上次的两倍

auto-aof-rewrite-min-size 64mb: 根据你的数据量来定，16mb，32mb 

2、企业级的数据备份方案 

RDB非常适合做冷备，每次生成之后，就不会再有修改了 

数据备份方案 

（1）写crontab定时调度脚本去做数据备份

（2）每小时都copy一份rdb的备份，到一个目录中去，仅仅保留最近48小时的备份

（3）每天都保留一份当日的rdb的备份，到一个目录中去，仅仅保留最近1个月的备份

（4）每次copy备份的时候，都把太旧的备份给删了

（5）每天晚上将当前服务器上所有的数据备份，发送一份到远程的云服务上去 

/usr/local/redis 

每小时copy一次备份，删除48小时前的数据 

crontab -e 

0 * * * * sh /usr/local/redis/copy/redis_rdb_copy_hourly.sh 

redis_rdb_copy_hourly.sh 

\#!/bin/sh  

cur_date=`date +%Y%m%d%k`

rm -rf /usr/local/redis/snapshotting/$cur_date

mkdir /usr/local/redis/snapshotting/$cur_date

cp /var/redis/6379/dump.rdb /usr/local/redis/snapshotting/$cur_date 

del_date=`date -d -48hour +%Y%m%d%k`

rm -rf /usr/local/redis/snapshotting/$del_date 

每天copy一次备份 

crontab -e 

0 0 * * * sh /usr/local/redis/copy/redis_rdb_copy_daily.sh 

redis_rdb_copy_daily.sh 

\#!/bin/sh  

cur_date=`date +%Y%m%d`

rm -rf /usr/local/redis/snapshotting/$cur_date

mkdir /usr/local/redis/snapshotting/$cur_date

cp /var/redis/6379/dump.rdb /usr/local/redis/snapshotting/$cur_date 

del_date=`date -d -1month +%Y%m%d`

rm -rf /usr/local/redis/snapshotting/$del_date 

每天一次将所有数据上传一次到远程的云服务器上去 

3、数据恢复方案 

（1）如果是redis进程挂掉，那么重启redis进程即可，直接基于AOF日志文件恢复数据 

不演示了，在AOF数据恢复那一块，演示了，fsync everysec，最多就丢一秒的数 

（2）如果是redis进程所在机器挂掉，那么重启机器后，尝试重启redis进程，尝试直接基于AOF日志文件进行数据恢复 

AOF没有破损，也是可以直接基于AOF恢复的 

AOF append-only，顺序写入，如果AOF文件破损，那么用redis-check-aof fix 

（3）如果redis当前最新的AOF和RDB文件出现了丢失/损坏，那么可以尝试基于该机器上当前的某个最新的RDB数据副本进行数据恢复 

当前最新的AOF和RDB文件都出现了丢失/损坏到无法恢复，一般不是机器的故障，人为 

大数据系统，hadoop，有人不小心就把hadoop中存储的大量的数据文件对应的目录，rm -rf一下，我朋友的一个小公司，运维不太靠谱，权限也弄的不太好 

/var/redis/6379下的文件给删除了 

找到RDB最新的一份备份，小时级的备份可以了，小时级的肯定是最新的，copy到redis里面去，就可以恢复到某一个小时的数据 

容灾演练 

我跟大家解释一下，我其实上课，为什么大量的讲师可能讲课就是纯PPT，或者是各种复制粘贴，都不是现场讲解和写代码演示的 

很容易出错，为了避免出错，一般就会那样玩儿 

吐槽，念PPT，效果很差 

真实的，备课，讲课不可避免，会出现一些问题，但是我觉得还好，真实 

appendonly.aof + dump.rdb，优先用appendonly.aof去恢复数据，但是我们发现redis自动生成的appendonly.aof是没有数据的 

然后我们自己的dump.rdb是有数据的，但是明显没用我们的数据 

redis启动的时候，自动重新基于内存的数据，生成了一份最新的rdb快照，直接用空的数据，覆盖掉了我们有数据的，拷贝过去的那份dump.rdb 

你停止redis之后，其实应该先删除appendonly.aof，然后将我们的dump.rdb拷贝过去，然后再重启redis 

很简单，就是虽然你删除了appendonly.aof，但是因为打开了aof持久化，redis就一定会优先基于aof去恢复，即使文件不在，那就创建一个新的空的aof文件 

停止redis，暂时在配置中关闭aof，然后拷贝一份rdb过来，再重启redis，数据能不能恢复过来，可以恢复过来 

脑子一热，再关掉redis，手动修改配置文件，打开aof，再重启redis，数据又没了，空的aof文件，所有数据又没了 

在数据安全丢失的情况下，基于rdb冷备，如何完美的恢复数据，同时还保持aof和rdb的双开 

停止redis，关闭aof，拷贝rdb备份，重启redis，确认数据恢复，直接在命令行热修改redis配置，打开aof，这个redis就会将内存中的数据对应的日志，写入aof文件中 

此时aof和rdb两份数据文件的数据就同步了 

redis config set热修改配置参数，可能配置文件中的实际的参数没有被持久化的修改，再次停止redis，手动修改配置文件，打开aof的命令，再次重启redis 

（4）如果当前机器上的所有RDB文件全部损坏，那么从远程的云服务上拉取最新的RDB快照回来恢复数据 

（5）如果是发现有重大的数据错误，比如某个小时上线的程序一下子将数据全部污染了，数据全错了，那么可以选择某个更早的时间点，对数据进行恢复 

举个例子，12点上线了代码，发现代码有bug，导致代码生成的所有的缓存数据，写入redis，全部错了 

找到一份11点的rdb的冷备，然后按照上面的步骤，去恢复到11点的数据，不就可以了吗

### 10_redis如何通过读写分离来承载读请求QPS超过10万+？ 

1、redis高并发跟整个系统的高并发之间的关系 

redis，你要搞高并发的话，不可避免，要把底层的缓存搞得很好 

mysql，高并发，做到了，那么也是通过一系列复杂的分库分表，订单系统，事务要求的，QPS到几万，比较高了 

要做一些电商的商品详情页，真正的超高并发，QPS上十万，甚至是百万，一秒钟百万的请求量 

光是redis是不够的，但是redis是整个大型的缓存架构中，支撑高并发的架构里面，非常重要的一个环节 

首先，你的底层的缓存中间件，缓存系统，必须能够支撑的起我们说的那种高并发，其次，再经过良好的整体的缓存架构的设计（多级缓存架构、热点缓存），支撑真正的上十万，甚至上百万的高并发 

2、redis不能支撑高并发的瓶颈在哪里？ 

单机 

3、如果redis要支撑超过10万+的并发，那应该怎么做？ 

单机的redis几乎不太可能说QPS超过10万+，除非一些特殊情况，比如你的机器性能特别好，配置特别高，物理机，维护做的特别好，而且你的整体的操作不是太复杂 

单机在几万 

读写分离，一般来说，对缓存，一般都是用来支撑读高并发的，写的请求是比较少的，可能写请求也就一秒钟几千，一两千 

大量的请求都是读，一秒钟二十万次读 

读写分离 

主从架构 -> 读写分离 -> 支撑10万+读QPS的架构 

4、接下来要讲解的一个topic 

redis replication 

redis主从架构 -> 读写分离架构 -> 可支持水平扩展的读高并发架构

### 11_redis replication以及master持久化对主从架构的安全意义

课程大纲 

1、图解redis replication基本原理

2、redis replication的核心机制

3、master持久化对于主从架构的安全保障的意义 

redis replication -> 主从架构 -> 读写分离 -> 水平扩容支撑读高并发 

redis replication的最最基本的原理，铺垫 

\------------------------------------------------------------------------ 

1、图解redis replication基本原理 

\------------------------------------------------------------------------ 

2、redis replication的核心机制 

（1）redis采用异步方式复制数据到slave节点，不过redis 2.8开始，slave node会周期性地确认自己每次复制的数据量

（2）一个master node是可以配置多个slave node的

（3）slave node也可以连接其他的slave node

（4）slave node做复制的时候，是不会block master node的正常工作的

（5）slave node在做复制的时候，也不会block对自己的查询操作，它会用旧的数据集来提供服务; 但是复制完成的时候，需要删除旧数据集，加载新数据集，这个时候就会暂停对外服务了

（6）slave node主要用来进行横向扩容，做读写分离，扩容的slave node可以提高读的吞吐量 

slave，高可用性，有很大的关系 

\------------------------------------------------------------------------ 

3、master持久化对于主从架构的安全保障的意义 

如果采用了主从架构，那么建议必须开启master node的持久化！ 

不建议用slave node作为master node的数据热备，因为那样的话，如果你关掉master的持久化，可能在master宕机重启的时候数据是空的，然后可能一经过复制，salve node数据也丢了 

master -> RDB和AOF都关闭了 -> 全部在内存中 

master宕机，重启，是没有本地数据可以恢复的，然后就会直接认为自己IDE数据是空的 

master就会将空的数据集同步到slave上去，所有slave的数据全部清空 

100%的数据丢失 

master节点，必须要使用持久化机制 

第二个，master的各种备份方案，要不要做，万一说本地的所有文件丢失了; 从备份中挑选一份rdb去恢复master; 这样才能确保master启动的时候，是有数据的 

即使采用了后续讲解的高可用机制，slave node可以自动接管master node，但是也可能sentinal还没有检测到master failure，master node就自动重启了，还是可能导致上面的所有slave node数据清空故障

### 12_redis主从复制原理、断点续传、无磁盘化复制、过期key处理

课程大纲 

1、主从架构的核心原理 

当启动一个slave node的时候，它会发送一个PSYNC命令给master node 

如果这是slave node重新连接master node，那么master node仅仅会复制给slave部分缺少的数据; 否则如果是slave node第一次连接master node，那么会触发一次full resynchronization 

开始full resynchronization的时候，master会启动一个后台线程，开始生成一份RDB快照文件，同时还会将从客户端收到的所有写命令缓存在内存中。RDB文件生成完毕之后，master会将这个RDB发送给slave，slave会先写入本地磁盘，然后再从本地磁盘加载到内存中。然后master会将内存中缓存的写命令发送给slave，slave也会同步这些数据。 

slave node如果跟master node有网络故障，断开了连接，会自动重连。master如果发现有多个slave node都来重新连接，仅仅会启动一个rdb save操作，用一份数据服务所有slave node。 

2、主从复制的断点续传 

从redis 2.8开始，就支持主从复制的断点续传，如果主从复制过程中，网络连接断掉了，那么可以接着上次复制的地方，继续复制下去，而不是从头开始复制一份 

master node会在内存中常见一个backlog，master和slave都会保存一个replica offset还有一个master id，offset就是保存在backlog中的。如果master和slave网络连接断掉了，slave会让master从上次的replica offset开始继续复制 

但是如果没有找到对应的offset，那么就会执行一次resynchronization 

3、无磁盘化复制 

master在内存中直接创建rdb，然后发送给slave，不会在自己本地落地磁盘了 

repl-diskless-sync

repl-diskless-sync-delay，等待一定时长再开始复制，因为要等更多slave重新连接过来 

4、过期key处理 

slave不会过期key，只会等待master过期key。如果master过期了一个key，或者通过LRU淘汰了一个key，那么会模拟一条del命令发送给slave。

### 13_redis replication的完整流运行程和原理的再次深入剖析

1、复制的完整流程 

（1）slave node启动，仅仅保存master node的信息，包括master node的host和ip，但是复制流程没开始 

master host和ip是从哪儿来的，redis.conf里面的slaveof配置的 

（2）slave node内部有个定时任务，每秒检查是否有新的master node要连接和复制，如果发现，就跟master node建立socket网络连接

（3）slave node发送ping命令给master node

（4）口令认证，如果master设置了requirepass，那么salve node必须发送masterauth的口令过去进行认证

（5）master node第一次执行全量复制，将所有数据发给slave node

（6）master node后续持续将写命令，异步复制给slave node 

2、数据同步相关的核心机制 

指的就是第一次slave连接msater的时候，执行的全量复制，那个过程里面你的一些细节的机制 

（1）master和slave都会维护一个offset 

master会在自身不断累加offset，slave也会在自身不断累加offset

slave每秒都会上报自己的offset给master，同时master也会保存每个slave的offset 

这个倒不是说特定就用在全量复制的，主要是master和slave都要知道各自的数据的offset，才能知道互相之间的数据不一致的情况 

（2）backlog 

master node有一个backlog，默认是1MB大小

master node给slave node复制数据时，也会将数据在backlog中同步写一份

backlog主要是用来做全量复制中断候的增量复制的 

（3）master run id 

info server，可以看到master run id

如果根据host+ip定位master node，是不靠谱的，如果master node重启或者数据出现了变化，那么slave node应该根据不同的run id区分，run id不同就做全量复制

如果需要不更改run id重启redis，可以使用redis-cli debug reload命令 

（4）psync 

从节点使用psync从master node进行复制，psync runid offset

master node会根据自身的情况返回响应信息，可能是FULLRESYNC runid offset触发全量复制，可能是CONTINUE触发增量复制 

3、全量复制 

（1）master执行bgsave，在本地生成一份rdb快照文件

（2）master node将rdb快照文件发送给salve node，如果rdb复制时间超过60秒（repl-timeout），那么slave node就会认为复制失败，可以适当调节大这个参数

（3）对于千兆网卡的机器，一般每秒传输100MB，6G文件，很可能超过60s

（4）master node在生成rdb时，会将所有新的写命令缓存在内存中，在salve node保存了rdb之后，再将新的写命令复制给salve node

（5）client-output-buffer-limit slave 256MB 64MB 60，如果在复制期间，内存缓冲区持续消耗超过64MB，或者一次性超过256MB，那么停止复制，复制失败

（6）slave node接收到rdb之后，清空自己的旧数据，然后重新加载rdb到自己的内存中，同时基于旧的数据版本对外提供服务

（7）如果slave node开启了AOF，那么会立即执行BGREWRITEAOF，重写AOF 

rdb生成、rdb通过网络拷贝、slave旧数据的清理、slave aof rewrite，很耗费时间 

如果复制的数据量在4G~6G之间，那么很可能全量复制时间消耗到1分半到2分钟 

4、增量复制 

（1）如果全量复制过程中，master-slave网络连接断掉，那么salve重新连接master时，会触发增量复制

（2）master直接从自己的backlog中获取部分丢失的数据，发送给slave node，默认backlog就是1MB

（3）msater就是根据slave发送的psync中的offset来从backlog中获取数据的 

5、heartbeat 

主从节点互相都会发送heartbeat信息 

master默认每隔10秒发送一次heartbeat，salve node每隔1秒发送一个heartbeat 

6、异步复制 

master每次接收到写命令之后，现在内部写入数据，然后异步发送给slave node

### 14_在项目中部署redis的读写分离架构（包含节点间认证口令） 

之前几讲都是在铺垫各种redis replication的原理，和知识，主从，读写分离，画图 

知道了这些东西，关键是怎么搭建呢？？？ 

一主一从，往主节点去写，在从节点去读，可以读到，主从架构就搭建成功了 

1、启用复制，部署slave node 

wget http://downloads.sourceforge.net/tcl/tcl8.6.1-src.tar.gz

tar -xzvf tcl8.6.1-src.tar.gz

cd /usr/local/tcl8.6.1/unix/

./configure 

make && make install 

使用redis-3.2.8.tar.gz（截止2017年4月的最新稳定版）

tar -zxvf redis-3.2.8.tar.gz

cd redis-3.2.8

make && make test && make install 

（1）redis utils目录下，有个redis_init_script脚本

（2）将redis_init_script脚本拷贝到linux的/etc/init.d目录中，将redis_init_script重命名为redis_6379，6379是我们希望这个redis实例监听的端口号

（3）修改redis_6379脚本的第6行的REDISPORT，设置为相同的端口号（默认就是6379）

（4）创建两个目录：/etc/redis（存放redis的配置文件），/var/redis/6379（存放redis的持久化文件）

（5）修改redis配置文件（默认在根目录下，redis.conf），拷贝到/etc/redis目录中，修改名称为6379.conf 

（6）修改redis.conf中的部分配置为生产环境 

daemonize yes           让redis以daemon进程运行

pidfile    /var/run/redis_6379.pid 设置redis的pid文件位置

port    6379           设置redis的监听端口号

dir     /var/redis/6379       设置持久化文件的存储位置 

（7）让redis跟随系统启动自动启动 

在redis_6379脚本中，最上面，加入两行注释 

\# chkconfig:  2345 90 10 

\# description: Redis is a persistent key-value database 

chkconfig redis_6379 on 

在slave node上配置：slaveof 192.168.1.1 6379，即可 

也可以使用slaveof命令 

2、强制读写分离 

基于主从复制架构，实现读写分离 

redis slave node只读，默认开启，slave-read-only 

开启了只读的redis slave node，会拒绝所有的写操作，这样可以强制搭建成读写分离的架构 

3、集群安全认证 

master上启用安全认证，requirepass

master连接口令，masterauth 

4、读写分离架构的测试 

先启动主节点，eshop-cache01上的redis实例

再启动从节点，eshop-cache02上的redis实例 

刚才我调试了一下，redis slave node一直说没法连接到主节点的6379的端口 

在搭建生产环境的集群的时候，不要忘记修改一个配置，bind 

bind 127.0.0.1 -> 本地的开发调试的模式，就只能127.0.0.1本地才能访问到6379的端口 

每个redis.conf中的bind 127.0.0.1 -> bind自己的ip地址

在每个节点上都: iptables -A INPUT -ptcp --dport 6379 -j ACCEPT 

redis-cli -h ipaddr

info replication 

在主上写，在从上读

### 15_对项目的主从redis架构进行QPS压测以及水平扩容支撑更高QPS

你如果要对自己刚刚搭建好的redis做一个基准的压测，测一下你的redis的性能和QPS（query per second） 

redis自己提供的redis-benchmark压测工具，是最快捷最方便的，当然啦，这个工具比较简单，用一些简单的操作和场景去压测 

1、对redis读写分离架构进行压测，单实例写QPS+单实例读QPS 

redis-3.2.8/src 

./redis-benchmark -h 192.168.31.187 

-c <clients>    Number of parallel connections (default 50)

-n <requests>   Total number of requests (default 100000)

-d <size>     Data size of SET/GET value in bytes (default 2) 

根据你自己的高峰期的访问量，在高峰期，瞬时最大用户量会达到10万+，-c 100000，-n 10000000，-d 50 

各种基准测试，直接出来 

1核1G，虚拟机 

====== PING_INLINE ======

 100000 requests completed in 1.28 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

99.78% <= 1 milliseconds

99.93% <= 2 milliseconds

99.97% <= 3 milliseconds

100.00% <= 3 milliseconds

78308.54 requests per second 

====== PING_BULK ======

 100000 requests completed in 1.30 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

99.87% <= 1 milliseconds

100.00% <= 1 milliseconds

76804.91 requests per second 

====== SET ======

 100000 requests completed in 2.50 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

5.95% <= 1 milliseconds

99.63% <= 2 milliseconds

99.93% <= 3 milliseconds

99.99% <= 4 milliseconds

100.00% <= 4 milliseconds

40032.03 requests per second 

====== GET ======

 100000 requests completed in 1.30 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

99.73% <= 1 milliseconds

100.00% <= 2 milliseconds

100.00% <= 2 milliseconds

76628.35 requests per second 

====== INCR ======

 100000 requests completed in 1.90 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

80.92% <= 1 milliseconds

99.81% <= 2 milliseconds

99.95% <= 3 milliseconds

99.96% <= 4 milliseconds

99.97% <= 5 milliseconds

100.00% <= 6 milliseconds

52548.61 requests per second 

====== LPUSH ======

 100000 requests completed in 2.58 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

3.76% <= 1 milliseconds

99.61% <= 2 milliseconds

99.93% <= 3 milliseconds

100.00% <= 3 milliseconds

38684.72 requests per second 

====== RPUSH ======

 100000 requests completed in 2.47 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

6.87% <= 1 milliseconds

99.69% <= 2 milliseconds

99.87% <= 3 milliseconds

99.99% <= 4 milliseconds

100.00% <= 4 milliseconds

40469.45 requests per second 

====== LPOP ======

 100000 requests completed in 2.26 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

28.39% <= 1 milliseconds

99.83% <= 2 milliseconds

100.00% <= 2 milliseconds

44306.60 requests per second 

====== RPOP ======

 100000 requests completed in 2.18 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

36.08% <= 1 milliseconds

99.75% <= 2 milliseconds

100.00% <= 2 milliseconds

45871.56 requests per second 

====== SADD ======

 100000 requests completed in 1.23 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

99.94% <= 1 milliseconds

100.00% <= 2 milliseconds

100.00% <= 2 milliseconds

81168.83 requests per second 

====== SPOP ======

 100000 requests completed in 1.28 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

99.80% <= 1 milliseconds

99.96% <= 2 milliseconds

99.96% <= 3 milliseconds

99.97% <= 5 milliseconds

100.00% <= 5 milliseconds

78369.91 requests per second 

====== LPUSH (needed to benchmark LRANGE) ======

 100000 requests completed in 2.47 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

15.29% <= 1 milliseconds

99.64% <= 2 milliseconds

99.94% <= 3 milliseconds

100.00% <= 3 milliseconds

40420.37 requests per second 

====== LRANGE_100 (first 100 elements) ======

 100000 requests completed in 3.69 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

30.86% <= 1 milliseconds

96.99% <= 2 milliseconds

99.94% <= 3 milliseconds

99.99% <= 4 milliseconds

100.00% <= 4 milliseconds

27085.59 requests per second 

====== LRANGE_300 (first 300 elements) ======

 100000 requests completed in 10.22 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

0.03% <= 1 milliseconds

5.90% <= 2 milliseconds

90.68% <= 3 milliseconds

95.46% <= 4 milliseconds

97.67% <= 5 milliseconds

99.12% <= 6 milliseconds

99.98% <= 7 milliseconds

100.00% <= 7 milliseconds

9784.74 requests per second 

====== LRANGE_500 (first 450 elements) ======

 100000 requests completed in 14.71 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

0.00% <= 1 milliseconds

0.07% <= 2 milliseconds

1.59% <= 3 milliseconds

89.26% <= 4 milliseconds

97.90% <= 5 milliseconds

99.24% <= 6 milliseconds

99.73% <= 7 milliseconds

99.89% <= 8 milliseconds

99.96% <= 9 milliseconds

99.99% <= 10 milliseconds

100.00% <= 10 milliseconds

6799.48 requests per second 

====== LRANGE_600 (first 600 elements) ======

 100000 requests completed in 18.56 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

0.00% <= 2 milliseconds

0.23% <= 3 milliseconds

1.75% <= 4 milliseconds

91.17% <= 5 milliseconds

98.16% <= 6 milliseconds

99.04% <= 7 milliseconds

99.83% <= 8 milliseconds

99.95% <= 9 milliseconds

99.98% <= 10 milliseconds

100.00% <= 10 milliseconds

5387.35 requests per second 

====== MSET (10 keys) ======

 100000 requests completed in 4.02 seconds

 50 parallel clients

 3 bytes payload

 keep alive: 1 

0.01% <= 1 milliseconds

53.22% <= 2 milliseconds

99.12% <= 3 milliseconds

99.55% <= 4 milliseconds

99.70% <= 5 milliseconds

99.90% <= 6 milliseconds

99.95% <= 7 milliseconds

100.00% <= 8 milliseconds

24869.44 requests per second 

我们这个读写分离这一块的第一讲 

大部分情况下来说，看你的服务器的机器性能和配置，机器越牛逼，配置越高 

单机上十几万，单机上二十万 

很多公司里，给一些低配置的服务器，操作复杂度 

大公司里，都是公司会提供统一的云平台，比如京东、腾讯、BAT、其他的一些、小米、美团 

虚拟机，低配 

搭建一些集群，专门为某个项目，搭建的专用集群，4核4G内存，比较复杂的操作，数据比较大 

几万，单机做到，差不多了 

redis提供的高并发，至少到上万，没问题 

几万~十几万/二十万不等 

QPS，自己不同公司，不同服务器，自己去测试，跟生产环境还有区别 

生产环境，大量的网络请求的调用，网络本身就有开销，你的redis的吞吐量就不一定那么高了 

QPS的两个杀手：一个是复杂操作，lrange，挺多的; value很大，2 byte，我之前用redis做大规模的缓存 

做商品详情页的cache，可能是需要把大串数据，拼接在一起，作为一个json串，大小可能都几k，几个byte 

2、水平扩容redis读节点，提升度吞吐量 

就按照上一节课讲解的，再在其他服务器上搭建redis从节点，单个从节点读请QPS在5万左右，两个redis从节点，所有的读请求打到两台机器上去，承载整个集群读QPS在10万+  

### 16_redis主从架构下如何才能做到99.99%的高可用性？ 

1、什么是99.99%高可用？ 

架构上，高可用性，99.99%的高可用性 

讲的学术，99.99%，公式，系统可用的时间 / 系统故障的时间，365天，在365天 * 99.99%的时间内，你的系统都是可以哗哗对外提供服务的，那就是高可用性，99.99% 

系统可用的时间 / 总的时间 = 高可用性，然后会对各种时间的概念，说一大堆解释 

2、redis不可用是什么？单实例不可用？主从架构不可用？不可用的后果是什么？ 

3、redis怎么才能做到高可用？  

### 17_redis哨兵架构的相关基础知识讲解

1、哨兵的介绍 

sentinal，中文名是哨兵 

哨兵是redis集群架构中非常重要的一个组件，主要功能如下 

（1）集群监控，负责监控redis master和slave进程是否正常工作

（2）消息通知，如果某个redis实例有故障，那么哨兵负责发送消息作为报警通知给管理员

（3）故障转移，如果master node挂掉了，会自动转移到slave node上

（4）配置中心，如果故障转移发生了，通知client客户端新的master地址 

哨兵本身也是分布式的，作为一个哨兵集群去运行，互相协同工作 

（1）故障转移时，判断一个master node是宕机了，需要大部分的哨兵都同意才行，涉及到了分布式选举的问题

（2）即使部分哨兵节点挂掉了，哨兵集群还是能正常工作的，因为如果一个作为高可用机制重要组成部分的故障转移系统本身是单点的，那就很坑爹了 

目前采用的是sentinal 2版本，sentinal 2相对于sentinal 1来说，重写了很多代码，主要是让故障转移的机制和算法变得更加健壮和简单 

2、哨兵的核心知识 

（1）哨兵至少需要3个实例，来保证自己的健壮性

（2）哨兵 + redis主从的部署架构，是不会保证数据零丢失的，只能保证redis集群的高可用性

（3）对于哨兵 + redis主从这种复杂的部署架构，尽量在测试环境和生产环境，都进行充足的测试和演练 

3、为什么redis哨兵集群只有2个节点无法正常工作？ 

哨兵集群必须部署2个以上节点 

如果哨兵集群仅仅部署了个2个哨兵实例，quorum=1 

+----+     +----+

| M1 |---------| R1 |

| S1 |     | S2 |

+----+     +----+ 

Configuration: quorum = 1 

master宕机，s1和s2中只要有1个哨兵认为master宕机就可以还行切换，同时s1和s2中会选举出一个哨兵来执行故障转移 

同时这个时候，需要majority，也就是大多数哨兵都是运行的，2个哨兵的majority就是2（2的majority=2，3的majority=2，5的majority=3，4的majority=2），2个哨兵都运行着，就可以允许执行故障转移 

但是如果整个M1和S1运行的机器宕机了，那么哨兵只有1个了，此时就没有majority来允许执行故障转移，虽然另外一台机器还有一个R1，但是故障转移不会执行 

4、经典的3节点哨兵集群 

​    +----+

​    | M1 |

​    | S1 |

​    +----+

​     |

+----+  |  +----+

| R2 |----+----| R3 |

| S2 |     | S3 |

+----+     +----+ 

Configuration: quorum = 2，majority 

如果M1所在机器宕机了，那么三个哨兵还剩下2个，S2和S3可以一致认为master宕机，然后选举出一个来执行故障转移 

同时3个哨兵的majority是2，所以还剩下的2个哨兵运行着，就可以允许执行故障转移  

### 18_详解redis哨兵主备切换的数据丢失问题：异步复制、集群脑裂 

课程大纲 

1、两种数据丢失的情况

2、解决异步复制和脑裂导致的数据丢失 

\------------------------------------------------------------------ 

1、两种数据丢失的情况 

主备切换的过程，可能会导致数据丢失 

（1）异步复制导致的数据丢失 

因为master -> slave的复制是异步的，所以可能有部分数据还没复制到slave，master就宕机了，此时这些部分数据就丢失了 

（2）脑裂导致的数据丢失 

脑裂，也就是说，某个master所在机器突然脱离了正常的网络，跟其他slave机器不能连接，但是实际上master还运行着 

此时哨兵可能就会认为master宕机了，然后开启选举，将其他slave切换成了master 

这个时候，集群里就会有两个master，也就是所谓的脑裂 

此时虽然某个slave被切换成了master，但是可能client还没来得及切换到新的master，还继续写向旧master的数据可能也丢失了 

因此旧master再次恢复的时候，会被作为一个slave挂到新的master上去，自己的数据会清空，重新从新的master复制数据 

\------------------------------------------------------------------ 

2、解决异步复制和脑裂导致的数据丢失 

min-slaves-to-write 1

min-slaves-max-lag 10 

要求至少有1个slave，数据复制和同步的延迟不能超过10秒 

如果说一旦所有的slave，数据复制和同步的延迟都超过了10秒钟，那么这个时候，master就不会再接收任何请求了 

上面两个配置可以减少异步复制和脑裂导致的数据丢失 

（1）减少异步复制的数据丢失 

有了min-slaves-max-lag这个配置，就可以确保说，一旦slave复制数据和ack延时太长，就认为可能master宕机后损失的数据太多了，那么就拒绝写请求，这样可以把master宕机时由于部分数据未同步到slave导致的数据丢失降低的可控范围内 

（2）减少脑裂的数据丢失 

如果一个master出现了脑裂，跟其他slave丢了连接，那么上面两个配置可以确保说，如果不能继续给指定数量的slave发送数据，而且slave超过10秒没有给自己ack消息，那么就直接拒绝客户端的写请求 

这样脑裂后的旧master就不会接受client的新数据，也就避免了数据丢失 

上面的配置就确保了，如果跟任何一个slave丢了连接，在10秒后发现没有slave给自己ack，那么就拒绝新的写请求 

因此在脑裂场景下，最多就丢失10秒的数据  

### 19_redis哨兵的多个核心底层原理深入解析（包含slave选举算法） 

1、sdown和odown转换机制 

sdown和odown两种失败状态 

sdown是主观宕机，就一个哨兵如果自己觉得一个master宕机了，那么就是主观宕机 

odown是客观宕机，如果quorum数量的哨兵都觉得一个master宕机了，那么就是客观宕机 

sdown达成的条件很简单，如果一个哨兵ping一个master，超过了is-master-down-after-milliseconds指定的毫秒数之后，就主观认为master宕机 

sdown到odown转换的条件很简单，如果一个哨兵在指定时间内，收到了quorum指定数量的其他哨兵也认为那个master是sdown了，那么就认为是odown了，客观认为master宕机 

2、哨兵集群的自动发现机制 

哨兵互相之间的发现，是通过redis的pub/sub系统实现的，每个哨兵都会往__sentinel__:hello这个channel里发送一个消息，这时候所有其他哨兵都可以消费到这个消息，并感知到其他的哨兵的存在 

每隔两秒钟，每个哨兵都会往自己监控的某个master+slaves对应的__sentinel__:hello channel里发送一个消息，内容是自己的host、ip和runid还有对这个master的监控配置 

每个哨兵也会去监听自己监控的每个master+slaves对应的__sentinel__:hello channel，然后去感知到同样在监听这个master+slaves的其他哨兵的存在 

每个哨兵还会跟其他哨兵交换对master的监控配置，互相进行监控配置的同步 

3、slave配置的自动纠正 

哨兵会负责自动纠正slave的一些配置，比如slave如果要成为潜在的master候选人，哨兵会确保slave在复制现有master的数据; 如果slave连接到了一个错误的master上，比如故障转移之后，那么哨兵会确保它们连接到正确的master上 

4、slave->master选举算法 

如果一个master被认为odown了，而且majority哨兵都允许了主备切换，那么某个哨兵就会执行主备切换操作，此时首先要选举一个slave来 

会考虑slave的一些信息 

（1）跟master断开连接的时长

（2）slave优先级

（3）复制offset

（4）run id 

如果一个slave跟master断开连接已经超过了down-after-milliseconds的10倍，外加master宕机的时长，那么slave就被认为不适合选举为master 

(down-after-milliseconds * 10) + milliseconds_since_master_is_in_SDOWN_state 

接下来会对slave进行排序 

（1）按照slave优先级进行排序，slave priority越低，优先级就越高

（2）如果slave priority相同，那么看replica offset，哪个slave复制了越多的数据，offset越靠后，优先级就越高

（3）如果上面两个条件都相同，那么选择一个run id比较小的那个slave 

5、quorum和majority 

每次一个哨兵要做主备切换，首先需要quorum数量的哨兵认为odown，然后选举出一个哨兵来做切换，这个哨兵还得得到majority哨兵的授权，才能正式执行切换 

如果quorum < majority，比如5个哨兵，majority就是3，quorum设置为2，那么就3个哨兵授权就可以执行切换 

但是如果quorum >= majority，那么必须quorum数量的哨兵都授权，比如5个哨兵，quorum是5，那么必须5个哨兵都同意授权，才能执行切换 

6、configuration epoch 

哨兵会对一套redis master+slave进行监控，有相应的监控的配置 

执行切换的那个哨兵，会从要切换到的新master（salve->master）那里得到一个configuration epoch，这就是一个version号，每次切换的version号都必须是唯一的 

如果第一个选举出的哨兵切换失败了，那么其他哨兵，会等待failover-timeout时间，然后接替继续执行切换，此时会重新获取一个新的configuration epoch，作为新的version号 

7、configuraiton传播 

哨兵完成切换之后，会在自己本地更新生成最新的master配置，然后同步给其他的哨兵，就是通过之前说的pub/sub消息机制 

这里之前的version号就很重要了，因为各种消息都是通过一个channel去发布和监听的，所以一个哨兵完成一次新的切换之后，新的master配置是跟着新的version号的 

其他的哨兵都是根据版本号的大小来更新自己的master配置的 

### 20_在项目中以经典的3节点方式部署哨兵集群》 

动手实操，练习如何操作部署哨兵集群，如何基于哨兵进行故障转移，还有一些企业级的配置方案 

1、哨兵的配置文件 

sentinel.conf 

最小的配置 

每一个哨兵都可以去监控多个maser-slaves的主从架构 

因为可能你的公司里，为不同的项目，部署了多个master-slaves的redis主从集群 

相同的一套哨兵集群，就可以去监控不同的多个redis主从集群 

你自己给每个redis主从集群分配一个逻辑的名称 

sentinel monitor mymaster 127.0.0.1 6379 2

sentinel down-after-milliseconds mymaster 60000

sentinel failover-timeout mymaster 180000

sentinel parallel-syncs mymaster 1

 

sentinel monitor resque 192.168.1.3 6380 4

sentinel down-after-milliseconds resque 10000

sentinel failover-timeout resque 180000

sentinel parallel-syncs resque 5

 

sentinel monitor mymaster 127.0.0.1 6379  

类似这种配置，来指定对一个master的监控，给监控的master指定的一个名称，因为后面分布式集群架构里会讲解，可以配置多个master做数据拆分 

sentinel down-after-milliseconds mymaster 60000

sentinel failover-timeout mymaster 180000

sentinel parallel-syncs mymaster 1 

上面的三个配置，都是针对某个监控的master配置的，给其指定上面分配的名称即可 

上面这段配置，就监控了两个master node 

这是最小的哨兵配置，如果发生了master-slave故障转移，或者新的哨兵进程加入哨兵集群，那么哨兵会自动更新自己的配置文件 

sentinel monitor master-group-name hostname port quorum 

quorum的解释如下： 

（1）至少多少个哨兵要一致同意，master进程挂掉了，或者slave进程挂掉了，或者要启动一个故障转移操作

（2）quorum是用来识别故障的，真正执行故障转移的时候，还是要在哨兵集群执行选举，选举一个哨兵进程出来执行故障转移操作

（3）假设有5个哨兵，quorum设置了2，那么如果5个哨兵中的2个都认为master挂掉了; 2个哨兵中的一个就会做一个选举，选举一个哨兵出来，执行故障转移; 如果5个哨兵中有3个哨兵都是运行的，那么故障转移就会被允许执行 

down-after-milliseconds，超过多少毫秒跟一个redis实例断了连接，哨兵就可能认为这个redis实例挂了 

parallel-syncs，新的master别切换之后，同时有多少个slave被切换到去连接新master，重新做同步，数字越低，花费的时间越多 

假设你的redis是1个master，4个slave 

然后master宕机了，4个slave中有1个切换成了master，剩下3个slave就要挂到新的master上面去 

这个时候，如果parallel-syncs是1，那么3个slave，一个一个地挂接到新的master上面去，1个挂接完，而且从新的master sync完数据之后，再挂接下一个 

如果parallel-syncs是3，那么一次性就会把所有slave挂接到新的master上去 

failover-timeout，执行故障转移的timeout超时时长 

2、在eshop-cache03上再部署一个redis 

只要安装redis就可以了，不需要去部署redis实例的启动 

wget http://downloads.sourceforge.net/tcl/tcl8.6.1-src.tar.gz

tar -xzvf tcl8.6.1-src.tar.gz

cd /usr/local/tcl8.6.1/unix/

./configure 

make && make install

 

使用redis-3.2.8.tar.gz（截止2017年4月的最新稳定版）

tar -zxvf redis-3.2.8.tar.gz

cd redis-3.2.8

make && make test

make install 

2、正式的配置 

哨兵默认用26379端口，默认不能跟其他机器在指定端口连通，只能在本地访问 

mkdir /etc/sentinal

mkdir -p /var/sentinal/5000

 

/etc/sentinel/5000.conf

 

port 5000

bind 192.168.31.187

dir /var/sentinal/5000

sentinel monitor mymaster 192.168.31.187 6379 2

sentinel down-after-milliseconds mymaster 30000

sentinel failover-timeout mymaster 60000

sentinel parallel-syncs mymaster 1

 

port 5000

bind 192.168.31.19

dir /var/sentinal/5000

sentinel monitor mymaster 192.168.31.187 6379 2

sentinel down-after-milliseconds mymaster 30000

sentinel failover-timeout mymaster 60000

sentinel parallel-syncs mymaster 1

 

port 5000

bind 192.168.31.227

dir /var/sentinal/5000

sentinel monitor mymaster 192.168.31.187 6379 2

sentinel down-after-milliseconds mymaster 30000

sentinel failover-timeout mymaster 60000

sentinel parallel-syncs mymaster 1 

3、启动哨兵进程 

在eshop-cache01、eshop-cache02、eshop-cache03三台机器上，分别启动三个哨兵进程，组成一个集群，观察一下日志的输出 

redis-sentinel /etc/sentinal/5000.conf

redis-server /etc/sentinal/5000.conf --sentinel 

日志里会显示出来，每个哨兵都能去监控到对应的redis master，并能够自动发现对应的slave 

哨兵之间，互相会自动进行发现，用的就是之前说的pub/sub，消息发布和订阅channel消息系统和机制 

4、检查哨兵状态 

redis-cli -h 192.168.31.187 -p 5000 

sentinel master mymaster

SENTINEL slaves mymaster

SENTINEL sentinels mymaster 

SENTINEL get-master-addr-by-name mymaster 

### 21_对项目中的哨兵节点进行管理以及高可用redis集群的容灾演练

1、哨兵节点的增加和删除 

增加sentinal，会自动发现 

删除sentinal的步骤 

（1）停止sentinal进程

（2）SENTINEL RESET *，在所有sentinal上执行，清理所有的master状态

（3）SENTINEL MASTER mastername，在所有sentinal上执行，查看所有sentinal对数量是否达成了一致 

2、slave的永久下线 

让master摘除某个已经下线的slave：SENTINEL RESET mastername，在所有的哨兵上面执行 

3、slave切换为Master的优先级 

slave->master选举优先级：slave-priority，值越小优先级越高 

4、基于哨兵集群架构下的安全认证 

每个slave都有可能切换成master，所以每个实例都要配置两个指令 

master上启用安全认证，requirepass

master连接口令，masterauth 

sentinal，sentinel auth-pass <master-group-name> <pass> 

5、容灾演练 

通过哨兵看一下当前的master：SENTINEL get-master-addr-by-name mymaster 

把master节点kill -9掉，pid文件也删除掉 

查看sentinal的日志，是否出现+sdown字样，识别出了master的宕机问题; 然后出现+odown字样，就是指定的quorum哨兵数量，都认为master宕机了

（1）三个哨兵进程都认为master是sdown了

（2）超过quorum指定的哨兵进程都认为sdown之后，就变为odown

（3）哨兵1是被选举为要执行后续的主备切换的那个哨兵

（4）哨兵1去新的master（slave）获取了一个新的config version

（5）尝试执行failover

（6）投票选举出一个slave区切换成master，每隔哨兵都会执行一次投票

（7）让salve，slaveof noone，不让它去做任何节点的slave了; 把slave提拔成master; 旧的master认为不再是master了

（8）哨兵就自动认为之前的187:6379变成了slave了，19:6379变成了master了

（9）哨兵去探查了一下187:6379这个salve的状态，认为它sdown了

所有哨兵选举出了一个，来执行主备切换操作 

如果哨兵的majority都存活着，那么就会执行主备切换操 

再通过哨兵看一下master：SENTINEL get-master-addr-by-name mymaster 

尝试连接一下新的master 

故障恢复，再将旧的master重新启动，查看是否被哨兵自动切换成slave节点 

（1）手动杀掉master

（2）哨兵能否执行主备切换，将slave切换为master

（3）哨兵完成主备切换后，新的master能否使用

（4）故障恢复，将旧的master重新启动

（5）哨兵能否自动将旧的master变为slave，挂接到新的master上面去，而且也是可以使用的 

6、哨兵的生产环境部署 

daemonize yes

logfile /var/log/sentinal/5000 

mkdir -p /var/log/sentinal/5000  

### 22_redis如何在保持读写分离+高可用的架构下，还能横向扩容支撑1T+海量数据》

1、单机redis在海量数据面前的瓶颈 

2、怎么才能够突破单机瓶颈，让redis支撑海量数据？ 

3、redis的集群架构 

redis cluster 

支撑N个redis master node，每个master node都可以挂载多个slave node 

读写分离的架构，对于每个master来说，写就写到master，然后读就从mater对应的slave去读 

高可用，因为每个master都有salve节点，那么如果mater挂掉，redis cluster这套机制，就会自动将某个slave切换成master 

redis cluster（多master + 读写分离 + 高可用） 

我们只要基于redis cluster去搭建redis集群即可，不需要手工去搭建replication复制+主从架构+读写分离+哨兵集群+高可用 

4、redis cluster vs. replication + sentinal 

如果你的数据量很少，主要是承载高并发高性能的场景，比如你的缓存一般就几个G，单机足够了 

replication，一个mater，多个slave，要几个slave跟你的要求的读吞吐量有关系，然后自己搭建一个sentinal集群，去保证redis主从架构的高可用性，就可以了 

redis cluster，主要是针对海量数据+高并发+高可用的场景，海量数据，如果你的数据量很大，那么建议就用redis cluster  

### 23_数据分布算法：hash+一致性hash+redis cluster的hash slot 

讲解分布式数据存储的核心算法，数据分布的算法 

hash算法 -> 一致性hash算法（memcached） -> redis cluster，hash slot算法 

用不同的算法，就决定了在多个master节点的时候，数据如何分布到这些节点上去，解决这个问题 

1、redis cluster介绍 

redis cluster 

（1）自动将数据进行分片，每个master上放一部分数据

（2）提供内置的高可用支持，部分master不可用时，还是可以继续工作的 

在redis cluster架构下，每个redis要放开两个端口号，比如一个是6379，另外一个就是加10000的端口号，比如16379 

16379端口号是用来进行节点间通信的，也就是cluster bus的东西，集群总线。cluster bus的通信，用来进行故障检测，配置更新，故障转移授权 

cluster bus用了另外一种二进制的协议，主要用于节点间进行高效的数据交换，占用更少的网络带宽和处理时间 

2、最老土的hash算法和弊端（大量缓存重建） 

3、一致性hash算法（自动缓存迁移）+虚拟节点（自动负载均衡） 

4、redis cluster的hash slot算法 

redis cluster有固定的16384个hash slot，对每个key计算CRC16值，然后对16384取模，可以获取key对应的hash slot 

redis cluster中每个master都会持有部分slot，比如有3个master，那么可能每个master持有5000多个hash slot 

hash slot让node的增加和移除很简单，增加一个master，就将其他master的hash slot移动部分过去，减少一个master，就将它的hash slot移动到其他master上去 

移动hash slot的成本是非常低的 

客户端的api，可以对指定的数据，让他们走同一个hash slot，通过hash tag来实现 

讲解分布式数据存储的核心算法，数据分布的算法 

hash算法 -> 一致性hash算法（memcached） -> redis cluster，hash slot算法 

用不同的算法，就决定了在多个master节点的时候，数据如何分布到这些节点上去，解决这个问题 

1、redis cluster介绍 

redis cluster 

（1）自动将数据进行分片，每个master上放一部分数据

（2）提供内置的高可用支持，部分master不可用时，还是可以继续工作的 

在redis cluster架构下，每个redis要放开两个端口号，比如一个是6379，另外一个就是加10000的端口号，比如16379 

16379端口号是用来进行节点间通信的，也就是cluster bus的东西，集群总线。cluster bus的通信，用来进行故障检测，配置更新，故障转移授权 

cluster bus用了另外一种二进制的协议，主要用于节点间进行高效的数据交换，占用更少的网络带宽和处理时间 

2、最老土的hash算法和弊端（大量缓存重建） 

3、一致性hash算法（自动缓存迁移）+虚拟节点（自动负载均衡） 

4、redis cluster的hash slot算法 

redis cluster有固定的16384个hash slot，对每个key计算CRC16值，然后对16384取模，可以获取key对应的hash slot 

redis cluster中每个master都会持有部分slot，比如有3个master，那么可能每个master持有5000多个hash slot 

hash slot让node的增加和移除很简单，增加一个master，就将其他master的hash slot移动部分过去，减少一个master，就将它的hash slot移动到其他master上去 

移动hash slot的成本是非常低的 

客户端的api，可以对指定的数据，让他们走同一个hash slot，通过hash tag来实现  

### 24_在项目中重新搭建一套读写分离+高可用+多master的redis cluster集群》 

redis cluster最最基础的一些知识 

redis cluster: 自动，master+slave复制和读写分离，master+slave高可用和主备切换，支持多个master的hash slot支持数据分布式存储 

停止之前所有的实例，包括redis主从和哨兵集群

1、redis cluster的重要配置 

cluster-enabled <yes/no> 

cluster-config-file <filename>：这是指定一个文件，供cluster模式下的redis实例将集群状态保存在那里，包括集群中其他机器的信息，比如节点的上线和下限，故障转移，不是我们去维护的，给它指定一个文件，让redis自己去维护的 

cluster-node-timeout <milliseconds>：节点存活超时时长，超过一定时长，认为节点宕机，master宕机的话就会触发主备切换，slave宕机就不会提供服务 

2、在三台机器上启动6个redis实例 

（1）在eshop-cache03上部署目录 

/etc/redis（存放redis的配置文件），/var/redis/6379（存放redis的持久化文件） 

（2）编写配置文件 

redis cluster集群，要求至少3个master，去组成一个高可用，健壮的分布式的集群，每个master都建议至少给一个slave，3个master，3个slave，最少的要求 

正式环境下，建议都是说在6台机器上去搭建，至少3台机器 

保证，每个master都跟自己的slave不在同一台机器上，如果是6台自然更好，一个master+一个slave就死了 

3台机器去搭建6个redis实例的redis cluster 

mkdir -p /etc/redis-cluster

mkdir -p /var/log/redis

mkdir -p /var/redis/7001

 

port 7001

cluster-enabled yes

cluster-config-file /etc/redis-cluster/node-7001.conf

cluster-node-timeout 15000

daemonize yes           

pidfile    /var/run/redis_7001.pid          

dir     /var/redis/7001   

logfile /var/log/redis/7001.log

bind 192.168.31.187   

appendonly yes 

至少要用3个master节点启动，每个master加一个slave节点，先选择6个节点，启动6个实例 

将上面的配置文件，在/etc/redis下放6个，分别为: 7001.conf，7002.conf，7003.conf，7004.conf，7005.conf，7006.conf 

（3）准备生产环境的启动脚本 

在/etc/init.d下，放6个启动脚本，分别为: redis_7001, redis_7002, redis_7003, redis_7004, redis_7005, redis_7006 

每个启动脚本内，都修改对应的端口号 

（4）分别在3台机器上，启动6个redis实例 

将每个配置文件中的slaveof给删除 

3、创建集群 

下面方框内的内容废弃掉 

======================================================================= 

wget https://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.1.tar.gz

tar -zxvf ruby-2.3.1.tar.gz

./configure -prefix=/usr/local/ruby

make && make install

cd /usr/local/ruby

cp bin/ruby /usr/local/bin

cp bin/gem /usr/local/bin

 

wget http://rubygems.org/downloads/redis-3.3.0.gem

gem install -l ./redis-3.3.0.gem

gem list --check redis gem 

======================================================================= 

因为，以前比如公司里面搭建集群，公司里的机器的环境，运维会帮你做好很多事情 

在讲课的话，我们手工用从零开始装的linux虚拟机去搭建，那肯定会碰到各种各样的问题 

yum install -y ruby

yum install -y rubygems

gem install redis 

cp /usr/local/redis-3.2.8/src/redis-trib.rb /usr/local/bin 

redis-trib.rb create --replicas 1 192.168.31.187:7001 192.168.31.187:7002 192.168.31.19:7003 192.168.31.19:7004 192.168.31.227:7005 192.168.31.227:7006 

--replicas: 每个master有几个slave 

6台机器，3个master，3个slave，尽量自己让master和slave不在一台机器上 

yes 

redis-trib.rb check 192.168.31.187:7001 

4、读写分离+高可用+多master 

读写分离：每个master都有一个slave

高可用：master宕机，slave自动被切换过去

多master：横向扩容支持更大数据量 

### 25_动手实验：对项目的redis cluster进行多master写入、读写分离、高可用验证

redis cluster搭建起来了 

redis cluster，提供了多个master，数据可以分布式存储在多个master上; 每个master都带着slave，自动就做读写分离; 每个master如果故障，那么久会自动将slave切换成master，高可用 

redis cluster的基本功能，来测试一下 

1、实验多master写入 -> 海量数据的分布式存储 

你在redis cluster写入数据的时候，其实是你可以将请求发送到任意一个master上去执行 

但是，每个master都会计算这个key对应的CRC16值，然后对16384个hashslot取模，找到key对应的hashslot，找到hashslot对应的master 

如果对应的master就在自己本地的话，set mykey1 v1，mykey1这个key对应的hashslot就在自己本地，那么自己就处理掉了 

但是如果计算出来的hashslot在其他master上，那么就会给客户端返回一个moved error，告诉你，你得到哪个master上去执行这条写入的命令 

什么叫做多master的写入，就是每条数据只能存在于一个master上，不同的master负责存储不同的数据，分布式的数据存储 

100w条数据，5个master，每个master就负责存储20w条数据，分布式数据存储 

大型的java系统架构，还专注在大数据系统架构，分布式，分布式存储，hadoop hdfs，分布式资源调度，hadoop yarn，分布式计算，hadoop mapreduce/hive 

分布式的nosql数据库，hbase，分布式的协调，zookeeper，分布式通用计算引擎，spark，分布式的实时计算引擎，storm 

如果你要处理海量数据，就涉及到了一个名词，叫做大数据，只要涉及到大数据，那么其实就会涉及到分布式 

redis cluster，分布式 

因为我来讲java系统的架构，有时候跟其他人不一样，纯搞java，但是我因为工作时间很长，早期专注做java架构，好多年，大数据兴起，就一直专注大数据系统架构 

大数据相关的系统，也涉及很多的java系统架构，高并发、高可用、高性能、可扩展、分布式系统 

会给大家稍微拓展一下知识面，从不同的角度去讲解一块知识 

redis，高并发、高性能、每日上亿流量的大型电商网站的商品详情页系统的缓存架构，来讲解的，redis是作为大规模缓存架构中的底层的核心存储的支持 

高并发、高性能、每日上亿流量，redis持久化 -> 灾难的时候，做数据恢复，复制 -> 读写分离，扩容slave，支撑更高的读吞吐，redis怎么支撑读QPS超过10万，几十万; 哨兵，在redis主从，一主多从，怎么保证99.99%可用性; redis cluster，海量数据 

java架构课，架构思路和设计是很重要的，但是另外一点，我希望能够带着大家用真正java架构师的角度去看待一些技术，而不是仅仅停留在技术的一些细节的点 

给大家从一些大数据的角度，去分析一下我们java架构领域中的一些技术 

天下武功，都出自一脉，研究过各种大数据的系统，redis cluster讲解了很多原理，跟elasticsearch，很多底层的分布式原理，都是类似的 

redis AOF，fsync 

elasticsearch建立索引的时候，先写内存缓存，每秒钟把数据刷入os cache，接下来再每隔一定时间fsync到磁盘上去 

redis cluster，写可以到任意master，任意master计算key的hashslot以后，告诉client，重定向，路由到其他mater去执行，分布式存储的一个经典的做法 

elasticsearch，建立索引的时候，也会根据doc id/routing value，做路由，路由到某个其他节点，重定向到其他节点去执行 

分布式的一些，hadoop，spark，storm里面很多核心的思想都是类似的 

后面，马上把redis架构给讲完之后，就开始讲解业务系统的开发，包括高并发的商品详情页系统的大型的缓存架构，jedis cluster相关api去封装和测试对redis cluster的访问 

jedis cluster api，就可以自动针对多个master进行写入和读取 

2、实验不同master各自的slave读取 -> 读写分离 

在这个redis cluster中，如果你要在slave读取数据，那么需要带上readonly指令，get mykey1 

redis-cli -c启动，就会自动进行各种底层的重定向的操作 

实验redis cluster的读写分离的时候，会发现有一定的限制性，默认情况下，redis cluster的核心的理念，主要是用slave做高可用的，每个master挂一两个slave，主要是做数据的热备，还有master故障时的主备切换，实现高可用的 

redis cluster默认是不支持slave节点读或者写的，跟我们手动基于replication搭建的主从架构不一样的 

slave node，readonly，get，这个时候才能在slave node进行读取 

redis cluster，主从架构是出来，读写分离，复杂了点，也可以做，jedis客户端，对redis cluster的读写分离支持不太好的 

默认的话就是读和写都到master上去执行的 

如果你要让最流行的jedis做redis cluster的读写分离的访问，那可能还得自己修改一点jedis的源码，成本比较高 

要不然你就是自己基于jedis，封装一下，自己做一个redis cluster的读写分离的访问api 

核心的思路，就是说，redis cluster的时候，就没有所谓的读写分离的概念了 

读写分离，是为了什么，主要是因为要建立一主多从的架构，才能横向任意扩展slave node去支撑更大的读吞吐量 

redis cluster的架构下，实际上本身master就是可以任意扩展的，你如果要支撑更大的读吞吐量，或者写吞吐量，或者数据量，都可以直接对master进行横向扩展就可以了 

也可以实现支撑更高的读吞吐的效果 

不会去跟大家直接讲解的，很多东西都要带着一些疑问，未知，实际经过一些实验和操作之后，让你体会的更加深刻一些 

redis cluster，主从架构，读写分离，没说错，没有撒谎 

redis cluster，不太好，server层面，jedis client层面，对master做扩容，所以说扩容master，跟之前扩容slave，效果是一样的 

3、实验自动故障切换 -> 高可用性 

redis-trib.rb check 192.168.31.187:7001 

比如把master1，187:7001，杀掉，看看它对应的19:7004能不能自动切换成master，可以自动切换 

切换成master后的19:7004，可以直接读取数据 

再试着把187:7001给重新启动，恢复过来，自动作为slave挂载到了19:7004上面去  

### 26_redis cluster如何通过master水平扩容来支撑更高的读写吞吐+海量数据

实验，演示过了 

redis cluster模式下，不建议做物理的读写分离了 

我们建议通过master的水平扩容，来横向扩展读写吞吐量，还有支撑更多的海量数据 

redis单机，读吞吐是5w/s，写吞吐2w/s 

扩展redis更多master，那么如果有5台master，不就读吞吐可以达到总量25/s QPS，写可以达到10w/s QPS 

redis单机，内存，6G，8G，fork类操作的时候很耗时，会导致请求延时的问题 

扩容到5台master，能支撑的总的缓存数据量就是30G，40G -> 100台，600G，800G，甚至1T+，海量数据 

redis是怎么扩容的 

1、加入新master 

mkdir -p /var/redis/7007 

port 7007

cluster-enabled yes

cluster-config-file /etc/redis-cluster/node-7007.conf

cluster-node-timeout 15000

daemonize yes           

pidfile    /var/run/redis_7007.pid          

dir     /var/redis/7007   

logfile /var/log/redis/7007.log

bind 192.168.31.227   

appendonly yes 

搞一个7007.conf，再搞一个redis_7007启动脚本 

手动启动一个新的redis实例，在7007端口上 

redis-trib.rb add-node 192.168.31.227:7007 192.168.31.187:7001 

redis-trib.rb check 192.168.31.187:7001 

连接到新的redis实例上，cluster nodes，确认自己是否加入了集群，作为了一个新的master 

2、reshard一些数据过去 

resharding的意思就是把一部分hash slot从一些node上迁移到另外一些node上 

redis-trib.rb reshard 192.168.31.187:7001 

要把之前3个master上，总共4096个hashslot迁移到新的第四个master上去 

How many slots do you want to move (from 1 to 16384)? 

1000 

3、添加node作为slave 

eshop-cache03 

mkdir -p /var/redis/7008

 

port 7008

cluster-enabled yes

cluster-config-file /etc/redis-cluster/node-7008.conf

cluster-node-timeout 15000

daemonize yes           

pidfile    /var/run/redis_7008.pid          

dir     /var/redis/7008   

logfile /var/log/redis/7008.log

bind 192.168.31.227   

appendonly yes

 

redis-trib.rb add-node --slave --master-id 28927912ea0d59f6b790a50cf606602a5ee48108 192.168.31.227:7008 192.168.31.187:7001 

4、删除node 

先用resharding将数据都移除到其他节点，确保node为空之后，才能执行remove操作 

redis-trib.rb del-node 192.168.31.187:7001 bd5a40a6ddccbd46a0f4a2208eb25d2453c2a8db 

2个是1365，1个是1366 

当你清空了一个master的hashslot时，redis cluster就会自动将其slave挂载到其他master上去 

这个时候就只要删除掉master就可以了  

### 27_实战redis cluster的自动化slave迁移高可用架构

slave的自动迁移 

比如现在有10个master，每个有1个slave，然后新增了3个slave作为冗余，有的master就有2个slave了，有的master出现了salve冗余 

如果某个master的slave挂了，那么redis cluster会自动迁移一个冗余的slave给那个master 

只要多加一些冗余的slave就可以了 

为了避免的场景，就是说，如果你每个master只有一个slave，万一说一个slave死了，然后很快，master也死了，那可用性还是降低了 

但是如果你给整个集群挂载了一些冗余slave，那么某个master的slave死了，冗余的slave会被自动迁移过去，作为master的新slave，此时即使那个master也死了 

还是有一个slave会切换成master的 

之前有一个master是有冗余slave的，直接让其他master其中的一个slave死掉，然后看有冗余slave会不会自动挂载到那个master 

### 28_redis cluster的核心原理分析：gossip通信、jedis smart定位、主备切换 

一、节点间的内部通信机制 

1、基础通信原理 

（1）redis cluster节点间采取gossip协议进行通信 

跟集中式不同，不是将集群元数据（节点信息，故障，等等）集中存储在某个节点上，而是互相之间不断通信，保持整个集群所有节点的数据是完整的 

维护集群的元数据用得，集中式，一种叫做gossip 

集中式：好处在于，元数据的更新和读取，时效性非常好，一旦元数据出现了变更，立即就更新到集中式的存储中，其他节点读取的时候立即就可以感知到; 不好在于，所有的元数据的跟新压力全部集中在一个地方，可能会导致元数据的存储有压力 

gossip：好处在于，元数据的更新比较分散，不是集中在一个地方，更新请求会陆陆续续，打到所有节点上去更新，有一定的延时，降低了压力; 缺点，元数据更新有延时，可能导致集群的一些操作会有一些滞后 

我们刚才做reshard，去做另外一个操作，会发现说，configuration error，达成一致 

（2）10000端口 

每个节点都有一个专门用于节点间通信的端口，就是自己提供服务的端口号+10000，比如7001，那么用于节点间通信的就是17001端口 

每隔节点每隔一段时间都会往另外几个节点发送ping消息，同时其他几点接收到ping之后返回pong 

（3）交换的信息 

故障信息，节点的增加和移除，hash slot信息，等等 

2、gossip协议 

gossip协议包含多种消息，包括ping，pong，meet，fail，等等 

meet: 某个节点发送meet给新加入的节点，让新节点加入集群中，然后新节点就会开始与其他节点进行通信 

redis-trib.rb add-node 

其实内部就是发送了一个gossip meet消息，给新加入的节点，通知那个节点去加入我们的集群 

ping: 每个节点都会频繁给其他节点发送ping，其中包含自己的状态还有自己维护的集群元数据，互相通过ping交换元数据 

每个节点每秒都会频繁发送ping给其他的集群，ping，频繁的互相之间交换数据，互相进行元数据的更新 

pong: 返回ping和meet，包含自己的状态和其他信息，也可以用于信息广播和更新 

fail: 某个节点判断另一个节点fail之后，就发送fail给其他节点，通知其他节点，指定的节点宕机了 

3、ping消息深入 

ping很频繁，而且要携带一些元数据，所以可能会加重网络负担 

每个节点每秒会执行10次ping，每次会选择5个最久没有通信的其他节点 

当然如果发现某个节点通信延时达到了cluster_node_timeout / 2，那么立即发送ping，避免数据交换延时过长，落后的时间太长了 

比如说，两个节点之间都10分钟没有交换数据了，那么整个集群处于严重的元数据不一致的情况，就会有问题 

所以cluster_node_timeout可以调节，如果调节比较大，那么会降低发送的频率 

每次ping，一个是带上自己节点的信息，还有就是带上1/10其他节点的信息，发送出去，进行数据交换 

至少包含3个其他节点的信息，最多包含总节点-2个其他节点的信息 

\------------------------------------------------------------------------------------------------------- 

二、面向集群的jedis内部实现原理 

开发，jedis，redis的java client客户端，redis cluster，jedis cluster api 

jedis cluster api与redis cluster集群交互的一些基本原理 

1、基于重定向的客户端 

redis-cli -c，自动重定向 

（1）请求重定向 

客户端可能会挑选任意一个redis实例去发送命令，每个redis实例接收到命令，都会计算key对应的hash slot 

如果在本地就在本地处理，否则返回moved给客户端，让客户端进行重定向 

cluster keyslot mykey，可以查看一个key对应的hash slot是什么 

用redis-cli的时候，可以加入-c参数，支持自动的请求重定向，redis-cli接收到moved之后，会自动重定向到对应的节点执行命令 

（2）计算hash slot 

计算hash slot的算法，就是根据key计算CRC16值，然后对16384取模，拿到对应的hash slot 

用hash tag可以手动指定key对应的slot，同一个hash tag下的key，都会在一个hash slot中，比如set mykey1:{100}和set mykey2:{100} 

（3）hash slot查找 

节点间通过gossip协议进行数据交换，就知道每个hash slot在哪个节点上 

2、smart jedis 

（1）什么是smart jedis 

基于重定向的客户端，很消耗网络IO，因为大部分情况下，可能都会出现一次请求重定向，才能找到正确的节点 

所以大部分的客户端，比如java redis客户端，就是jedis，都是smart的 

本地维护一份hashslot -> node的映射表，缓存，大部分情况下，直接走本地缓存就可以找到hashslot -> node，不需要通过节点进行moved重定向 

（2）JedisCluster的工作原理 

在JedisCluster初始化的时候，就会随机选择一个node，初始化hashslot -> node映射表，同时为每个节点创建一个JedisPool连接池 

每次基于JedisCluster执行操作，首先JedisCluster都会在本地计算key的hashslot，然后在本地映射表找到对应的节点 

如果那个node正好还是持有那个hashslot，那么就ok; 如果说进行了reshard这样的操作，可能hashslot已经不在那个node上了，就会返回moved 

如果JedisCluter API发现对应的节点返回moved，那么利用该节点的元数据，更新本地的hashslot -> node映射表缓存 

重复上面几个步骤，直到找到对应的节点，如果重试超过5次，那么就报错，JedisClusterMaxRedirectionException 

jedis老版本，可能会出现在集群某个节点故障还没完成自动切换恢复时，频繁更新hash slot，频繁ping节点检查活跃，导致大量网络IO开销 

jedis最新版本，对于这些过度的hash slot更新和ping，都进行了优化，避免了类似问题 

（3）hashslot迁移和ask重定向 

如果hash slot正在迁移，那么会返回ask重定向给jedis 

jedis接收到ask重定向之后，会重新定位到目标节点去执行，但是因为ask发生在hash slot迁移过程中，所以JedisCluster API收到ask是不会更新hashslot本地缓存 

已经可以确定说，hashslot已经迁移完了，moved是会更新本地hashslot->node映射表缓存的 

\------------------------------------------------------------------------------------------------------- 

三、高可用性与主备切换原理 

redis cluster的高可用的原理，几乎跟哨兵是类似的 

1、判断节点宕机 

如果一个节点认为另外一个节点宕机，那么就是pfail，主观宕机 

如果多个节点都认为另外一个节点宕机了，那么就是fail，客观宕机，跟哨兵的原理几乎一样，sdown，odown 

在cluster-node-timeout内，某个节点一直没有返回pong，那么就被认为pfail 

如果一个节点认为某个节点pfail了，那么会在gossip ping消息中，ping给其他节点，如果超过半数的节点都认为pfail了，那么就会变成fail 

2、从节点过滤 

对宕机的master node，从其所有的slave node中，选择一个切换成master node 

检查每个slave node与master node断开连接的时间，如果超过了cluster-node-timeout * cluster-slave-validity-factor，那么就没有资格切换成master 

这个也是跟哨兵是一样的，从节点超时过滤的步骤 

3、从节点选举 

哨兵：对所有从节点进行排序，slave priority，offset，run id 

每个从节点，都根据自己对master复制数据的offset，来设置一个选举时间，offset越大（复制数据越多）的从节点，选举时间越靠前，优先进行选举 

所有的master node开始slave选举投票，给要进行选举的slave进行投票，如果大部分master node（N/2 + 1）都投票给了某个从节点，那么选举通过，那个从节点可以切换成master 

从节点执行主备切换，从节点切换为主节点 

4、与哨兵比较 

整个流程跟哨兵相比，非常类似，所以说，redis cluster功能强大，直接集成了replication和sentinal的功能 

没有办法去给大家深入讲解redis底层的设计的细节，核心原理和设计的细节，那个除非单独开一门课，redis底层原理深度剖析，redis源码 

对于咱们这个架构课来说，主要关注的是架构，不是底层的细节，对于架构来说，核心的原理的基本思路，是要梳理清晰的  

### 29_redis在实践中的一些常见问题以及优化思路 

基本讲解到现在，大家其实直接到公司里，就可以去搭建redis了 

因为其实有些东西，也许没有讲解到台细节的一些东西，比如一些参数的设置 

不同的公司，不同的业务，不同的数据量，可能要调节的参数不同 

到这里为止，大家就差不多了，按照这个思路，去搭建redis支撑高并发、高可用、海量数据的架构，部署 

可以用公司里的一些已有的数据，导入进去，几百万，一千万，进去 

做各种压力测试，性能，redis-benchmark，并发，QPS，高可用的演练，每台机器最大能存储多少数据量，横向扩容支撑更多数据 

基于测试环境还有测试数据，做各种演练，去摸索一些最适合自己的一些细节的东西 

你说你靠一套课程，搞定一个技术100%的所有的东西，几乎是不可能的 

师傅领进门，修行在个人 

一套好的课程，唯一的判断标准，就是在这个价格下，能教会你值得这个价格的一些技术和架构等等知识，是你从其他地方没法学到的，或者自己去学要耗费几倍的时间摸索的 

这个课程的价值就已经达到了 

你说你花了几百块钱，买了个课程，要求，课程，学完，立即就是独孤九剑，直接到公司里各种问题都能轻松解决 

这个世界上，不存在这种课程，合理的价值观，大家才能有一个非常好的良性的互动的过程 

spark等等课程 

实际学了课程去做项目，100%会遇到大量自己没想到的问题，遇到了首先就自己尝试去解决，遇到问题，才是你的经验积累 

遇到了问题，加我的QQ，然后跟我咨询咨询，我给你看看，也是可以的 

spark，elasticsearch，java架构课程 

70%~80%的问题，我都可以帮你搞定，我能做到的 

1、fork耗时导致高并发请求延时 

RDB和AOF的时候，其实会有生成RDB快照，AOF rewrite，耗费磁盘IO的过程，主进程fork子进程 

fork的时候，子进程是需要拷贝父进程的空间内存页表的，也是会耗费一定的时间的 

一般来说，如果父进程内存有1个G的数据，那么fork可能会耗费在20ms左右，如果是10G~30G，那么就会耗费20 * 10，甚至20 * 30，也就是几百毫秒的时间 

info stats中的latest_fork_usec，可以看到最近一次form的时长 

redis单机QPS一般在几万，fork可能一下子就会拖慢几万条操作的请求时长，从几毫秒变成1秒 

优化思路 

fork耗时跟redis主进程的内存有关系，一般控制redis的内存在10GB以内，slave -> master，全量复制 

2、AOF的阻塞问题 

redis将数据写入AOF缓冲区，单独开一个现场做fsync操作，每秒一次 

但是redis主线程会检查两次fsync的时间，如果距离上次fsync时间超过了2秒，那么写请求就会阻塞 

everysec，最多丢失2秒的数据 

一旦fsync超过2秒的延时，整个redis就被拖慢 

优化思路 

优化硬盘写入速度，建议采用SSD，不要用普通的机械硬盘，SSD，大幅度提升磁盘读写的速度 

3、主从复制延迟问题 

主从复制可能会超时严重，这个时候需要良好的监控和报警机制 

在info replication中，可以看到master和slave复制的offset，做一个差值就可以看到对应的延迟量 

如果延迟过多，那么就进行报警 

4、主从复制风暴问题 

如果一下子让多个slave从master去执行全量复制，一份大的rdb同时发送到多个slave，会导致网络带宽被严重占用 

如果一个master真的要挂载多个slave，那尽量用树状结构，不要用星型结构 

5、vm.overcommit_memory 

0: 检查有没有足够内存，没有的话申请内存失败

1: 允许使用内存直到用完为止

2: 内存地址空间不能超过swap + 50% 

如果是0的话，可能导致类似fork等操作执行失败，申请不到足够的内存空间 

cat /proc/sys/vm/overcommit_memory

echo "vm.overcommit_memory=1" >> /etc/sysctl.conf

sysctl vm.overcommit_memory=1 

6、swapiness 

cat /proc/version，查看linux内核版本 

如果linux内核版本<3.5，那么swapiness设置为0，这样系统宁愿swap也不会oom killer（杀掉进程）

如果linux内核版本>=3.5，那么swapiness设置为1，这样系统宁愿swap也不会oom killer 

保证redis不会被杀掉 

echo 0 > /proc/sys/vm/swappiness

echo vm.swapiness=0 >> /etc/sysctl.conf 

7、最大打开文件句柄 

ulimit -n 10032 10032 

自己去上网搜一下，不同的操作系统，版本，设置的方式都不太一样 

8、tcp backlog 

cat /proc/sys/net/core/somaxconn

echo 511 > /proc/sys/net/core/somaxconn 

### 30_redis阶段性总结：1T以上海量数据 + 10万以上QPS高并发 + 99.99%高可用

1、讲解redis是为了什么？ 

topic：高并发、亿级流量、高性能、海量数据的场景，电商网站的商品详情页系统的缓存架构 

商品详情页系统，大型电商网站，会有很多部分组成，但是支撑高并发、亿级流量的，主要就是其中的大型的缓存架构 

在这个大型的缓存架构中，redis是最最基础的一层 

高并发，缓存架构中除了redis，还有其他的组成部分，但是redis至关重要 

大量的离散请求，随机请求，各种你未知的用户过来的请求，上千万用户过来访问，每个用户访问10次; 集中式的请求，1个用户过来，一天访问1亿次 

支撑商品展示的最重要的，就是redis cluster，去抗住每天上亿的请求流量，支撑高并发的访问 

redis cluster在整个缓存架构中，如何跟其他几个部分搭配起来组成一个大型的缓存系统，后面再讲 

2、讲解的redis可以实现什么效果？ 

我之前一直在redis的各个知识点的讲解之前都强调一下，我们要讲解的每个知识点，要解决的问题是什么？？？ 

redis：持久化、复制（主从架构）、哨兵（高可用，主备切换）、redis cluster（海量数据+横向扩容+高可用/主备切换） 

持久化：高可用的一部分，在发生redis集群灾难的情况下（比如说部分master+slave全部死掉了），如何快速进行数据恢复，快速实现服务可用，才能实现整个系统的高可用 

复制：主从架构，master -> slave 复制，读写分离的架构，写master，读slave，横向扩容slave支撑更高的读吞吐，读高并发，10万，20万，30万，上百万，QPS，横向扩容 

哨兵：高可用，主从架构，在master故障的时候，快速将slave切换成master，实现快速的灾难恢复，实现高可用性 

redis cluster：多master读写，数据分布式的存储，横向扩容，水平扩容，快速支撑高达的数据量+更高的读写QPS，自动进行master -> slave的主备切换，高可用 

让底层的缓存系统，redis，实现能够任意水平扩容，支撑海量数据（1T+，几十T，10G * 600 redis = 6T），支撑很高的读写QPS（redis单机在几万QPS，10台，几十万QPS），高可用性（给我们每个redis实例都做好AOF+RDB的备份策略+容灾策略，slave -> master主备切换） 

1T+海量数据、10万+读写QPS、99.99%高可用性 

3、redis的第一套企业级的架构 

如果你的数据量不大，单master就可以容纳，一般来说你的缓存的总量在10G以内就可以，那么建议按照以下架构去部署redis 

redis持久化+备份方案+容灾方案+replication（主从+读写分离）+sentinal（哨兵集群，3个节点，高可用性） 

可以支撑的数据量在10G以内，可以支撑的写QPS在几万左右，可以支撑的读QPS可以上10万以上（随你的需求，水平扩容slave节点就可以），可用性在99.99% 

4、redis的第二套企业级架构 

如果你的数据量很大，比如我们课程的topic，大型电商网站的商品详情页的架构（对标那些国内排名前三的大电商网站，*宝，*东，*宁易购），数据量是很大的 

海量数据 

redis cluster 

多master分布式存储数据，水平扩容 

支撑更多的数据量，1T+以上没问题，只要扩容master即可 

读写QPS分别都达到几十万都没问题，只要扩容master即可，redis cluster，读写分离，支持不太好，readonly才能去slave上读 

支撑99.99%可用性，也没问题，slave -> master的主备切换，冗余slave去进一步提升可用性的方案（每个master挂一个slave，但是整个集群再加个3个slave冗余一下） 

我们课程里，两套架构都讲解了，后续的业务系统的开发，主要是基于redis cluster去做 

5、我们现在课程讲解的项目进展到哪里了？ 

我们要做后续的业务系统的开发，redis的架构部署好，是第一件事情，也是非常重要的，也是你作为一个架构师而言，在对系统进行设计的时候，你必须要考虑到底层的redis的并发、性能、能支撑的数据量、可用性 

redis：水平扩容，海量数据，上10万的读写QPS，99.99%高可用性 

从架构的角度，我们的redis是可以做到的，水平扩容，只要机器足够，到1T数据量，50万读写QPS，99.99% 

正式开始做大型电商网站的商品详情页系统，大规模的缓存架构设计

### 31_百万流量商品详情页的多级缓存架构以及架构中每一层的意义（一） 

### 32_百万流量商品详情页的多级缓存架构以及架构中每一层的意义（二）

我们之前的三十讲，主要是在讲解redis如何支撑海量数据、高并发读写、高可用服务的架构，redis架构 

redis架构，在我们的真正类似商品详情页读高并发的系统中，redis就是底层的缓存存储的支持 

从这一讲开始，我们正式开始做业务系统的开发 

亿级流量以上的电商网站的商品详情页的系统，商品详情页系统，大量的业务，十几个人做一两年，堆出来复杂的业务系统 

几十个小时的课程，讲解复杂的业务 

把整体的架构给大家讲解清楚，然后浓缩和精炼里面的业务，提取部分业务，做一些简化，把整个详情页系统的流程跑出来 

架构，骨架，有少量的业务，血和肉，把整个项目串起来，在业务背景下，去学习架构 

讲解商品详情页系统，缓存架构，90%大量的业务代码（没有什么技术含量），10%的最优技术含量的就是架构，上亿流量，每秒QPS几万，上十万的，读并发 

读并发，缓存架构 

1、上亿流量的商品详情页系统的多级缓存架构 

很多人以为，做个缓存，其实就是用一下redis，访问一下，就可以了，简单的缓存 

做复杂的缓存，支撑电商复杂的场景下的高并发的缓存，遇到的问题，非常非常之多，绝对不是说简单的访问一下redsi就可以了 

采用三级缓存：nginx本地缓存+redis分布式缓存+tomcat堆缓存的多级缓存架构 

时效性要求非常高的数据：库存 

一般来说，显示的库存，都是时效性要求会相对高一些，因为随着商品的不断的交易，库存会不断的变化 

当然，我们就希望当库存变化的时候，尽可能更快将库存显示到页面上去，而不是说等了很长时间，库存才反应到页面上去 

时效性要求不高的数据：商品的基本信息（名称、颜色、版本、规格参数，等等） 

时效性要求不高的数据，就还好，比如说你现在改变了商品的名称，稍微晚个几分钟反应到商品页面上，也还能接受 

商品价格/库存等时效性要求高的数据，而且种类较少，采取相关的服务系统每次发生了变更的时候，直接采取数据库和redis缓存双写的方案，这样缓存的时效性最高 

商品基本信息等时效性不高的数据，而且种类繁多，来自多种不同的系统，采取MQ异步通知的方式，写一个数据生产服务，监听MQ消息，然后异步拉取服务的数据，更新tomcat jvm缓存+redis缓存 

nginx+lua脚本做页面动态生成的工作，每次请求过来，优先从nginx本地缓存中提取各种数据，结合页面模板，生成需要的页面 

如果nginx本地缓存过期了，那么就从nginx到redis中去拉取数据，更新到nginx本地 

如果redis中也被LRU算法清理掉了，那么就从nginx走http接口到后端的服务中拉取数据，数据生产服务中，现在本地tomcat里的jvm堆缓存中找，ehcache，如果也被LRU清理掉了，那么就重新发送请求到源头的服务中去拉取数据，然后再次更新tomcat堆内存缓存+redis缓存，并返回数据给nginx，nginx缓存到本地 

2、多级缓存架构中每一层的意义 

nginx本地缓存，抗的是热数据的高并发访问，一般来说，商品的购买总是有热点的，比如每天购买iphone、nike、海尔等知名品牌的东西的人，总是比较多的 

这些热数据，利用nginx本地缓存，由于经常被访问，所以可以被锁定在nginx的本地缓存内 

大量的热数据的访问，就是经常会访问的那些数据，就会被保留在nginx本地缓存内，那么对这些热数据的大量访问，就直接走nginx就可以了 

那么大量的访问，直接就可以走到nginx就行了，不需要走后续的各种网络开销了 

redis分布式大规模缓存，抗的是很高的离散访问，支撑海量的数据，高并发的访问，高可用的服务 

redis缓存最大量的数据，最完整的数据和缓存，1T+数据; 支撑高并发的访问，QPS最高到几十万; 可用性，非常好，提供非常稳定的服务 

nginx本地内存有限，也就能cache住部分热数据，除了各种iphone、nike等热数据，其他相对不那么热的数据，可能流量会经常走到redis那里 

利用redis cluster的多master写入，横向扩容，1T+以上海量数据支持，几十万的读写QPS，99.99%高可用性，那么就可以抗住大量的离散访问请求 

tomcat jvm堆内存缓存，主要是抗redis大规模灾难的，如果redis出现了大规模的宕机，导致nginx大量流量直接涌入数据生产服务，那么最后的tomcat堆内存缓存至少可以再抗一下，不至于让数据库直接裸奔 

同时tomcat jvm堆内存缓存，也可以抗住redis没有cache住的最后那少量的部分缓存  

### 33_Cache Aside Pattern缓存+数据库读写模式的深入剖析

最经典的缓存+数据库读写的模式，cache aside pattern 

1、Cache Aside Pattern 

（1）读的时候，先读缓存，缓存没有的话，那么就读数据库，然后取出数据后放入缓存，同时返回响应 

（2）更新的时候，先删除缓存，然后再更新数据库 

2、为什么是删除缓存，而不是更新缓存呢？ 

原因很简单，很多时候，复杂点的缓存的场景，因为缓存有的时候，不简单是数据库中直接取出来的值 

商品详情页的系统，修改库存，只是修改了某个表的某些字段，但是要真正把这个影响的最终的库存计算出来，可能还需要从其他表查询一些数据，然后进行一些复杂的运算，才能最终计算出 

现在最新的库存是多少，然后才能将库存更新到缓存中去 

比如可能更新了某个表的一个字段，然后其对应的缓存，是需要查询另外两个表的数据，并进行运算，才能计算出缓存最新的值的 

更新缓存的代价是很高的 

是不是说，每次修改数据库的时候，都一定要将其对应的缓存去跟新一份？也许有的场景是这样的，但是对于比较复杂的缓存数据计算的场景，就不是这样了 

如果你频繁修改一个缓存涉及的多个表，那么这个缓存会被频繁的更新，频繁的更新缓存 

但是问题在于，这个缓存到底会不会被频繁访问到？？？ 

举个例子，一个缓存涉及的表的字段，在1分钟内就修改了20次，或者是100次，那么缓存跟新20次，100次; 但是这个缓存在1分钟内就被读取了1次，有大量的冷数据 

28法则，黄金法则，20%的数据，占用了80%的访问量 

实际上，如果你只是删除缓存的话，那么1分钟内，这个缓存不过就重新计算一次而已，开销大幅度降低 

每次数据过来，就只是删除缓存，然后修改数据库，如果这个缓存，在1分钟内只是被访问了1次，那么只有那1次，缓存是要被重新计算的，用缓存才去算缓存 

其实删除缓存，而不是更新缓存，就是一个lazy计算的思想，不要每次都重新做复杂的计算，不管它会不会用到，而是让它到需要被使用的时候再重新计算 

mybatis，hibernate，懒加载，思想 

查询一个部门，部门带了一个员工的list，没有必要说每次查询部门，都里面的1000个员工的数据也同时查出来啊 

80%的情况，查这个部门，就只是要访问这个部门的信息就可以了 

先查部门，同时要访问里面的员工，那么这个时候只有在你要访问里面的员工的时候，才会去数据库里面查询1000个员工

### 34_高并发场景下的缓存 + 数据库双写不一致问题分析与解决方案

马上开始去开发业务系统 

从哪一步开始做，从比较简单的那一块开始做，实时性要求比较高的那块数据的缓存去做 

实时性比较高的数据缓存，选择的就是库存的服务 

库存可能会修改，每次修改都要去更新这个缓存数据; 每次库存的数据，在缓存中一旦过期，或者是被清理掉了，前端的nginx服务都会发送请求给库存服务，去获取相应的数据 

库存这一块，写数据库的时候，直接更新redis缓存 

实际上没有这么的简单，这里，其实就涉及到了一个问题，数据库与缓存双写，数据不一致的问题 

围绕和结合实时性较高的库存服务，把数据库与缓存双写不一致问题以及其解决方案，给大家讲解一下 

数据库与缓存双写不一致，很常见的问题，大型的缓存架构中，第一个解决方案 

大型的缓存架构全部讲解完了以后，整套架构是非常复杂，架构可以应对各种各样奇葩和极端的情况 

也有一种可能，不是说，来讲课的就是超人，万能的 

讲课，就跟写书一样，很可能会写错，也可能有些方案里的一些地方，我没考虑到 

也可能说，有些方案只是适合某些场景，在某些场景下，可能需要你进行方案的优化和调整才能适用于你自己的项目 

大家觉得对这些方案有什么疑问或者见解，都可以找我，沟通一下 

如果的确我觉得是我讲解的不对，或者有些地方考虑不周，那么我可以在视频里补录，更新到网站上面去 

多多包涵 

1、最初级的缓存不一致问题以及解决方案 

问题：先修改数据库，再删除缓存，如果删除缓存失败了，那么会导致数据库中是新数据，缓存中是旧数据，数据出现不一致 

解决思路 

先删除缓存，再修改数据库，如果删除缓存成功了，如果修改数据库失败了，那么数据库中是旧数据，缓存中是空的，那么数据不会不一致 

因为读的时候缓存没有，则读数据库中旧数据，然后更新到缓存中 

2、比较复杂的数据不一致问题分析 

数据发生了变更，先删除了缓存，然后要去修改数据库，此时还没修改 

一个请求过来，去读缓存，发现缓存空了，去查询数据库，查到了修改前的旧数据，放到了缓存中 

数据变更的程序完成了数据库的修改 

完了，数据库和缓存中的数据不一样了。。。。 

3、为什么上亿流量高并发场景下，缓存会出现这个问题？ 

只有在对一个数据在并发的进行读写的时候，才可能会出现这种问题

其实如果说你的并发量很低的话，特别是读并发很低，每天访问量就1万次，那么很少的情况下，会出现刚才描述的那种不一致的场景 

但是问题是，如果每天的是上亿的流量，每秒并发读是几万，每秒只要有数据更新的请求，就可能会出现上述的数据库+缓存不一致的情况 

高并发了以后，问题是很多的 

4、数据库与缓存更新与读取操作进行异步串行化 

更新数据的时候，根据数据的唯一标识，将操作路由之后，发送到一个jvm内部的队列中 

读取数据的时候，如果发现数据不在缓存中，那么将重新读取数据+更新缓存的操作，根据唯一标识路由之后，也发送同一个jvm内部的队列中 

一个队列对应一个工作线程 

每个工作线程串行拿到对应的操作，然后一条一条的执行 

这样的话，一个数据变更的操作，先执行，删除缓存，然后再去更新数据库，但是还没完成更新 

此时如果一个读请求过来，读到了空的缓存，那么可以先将缓存更新的请求发送到队列中，此时会在队列中积压，然后同步等待缓存更新完成 

这里有一个优化点，一个队列中，其实多个更新缓存请求串在一起是没意义的，因此可以做过滤，如果发现队列中已经有一个更新缓存的请求了，那么就不用再放个更新请求操作进去了，直接等待前面的更新操作请求完成即可 

待那个队列对应的工作线程完成了上一个操作的数据库的修改之后，才会去执行下一个操作，也就是缓存更新的操作，此时会从数据库中读取最新的值，然后写入缓存中 

如果请求还在等待时间范围内，不断轮询发现可以取到值了，那么就直接返回; 如果请求等待的时间超过一定时长，那么这一次直接从数据库中读取当前的旧值 

5、高并发的场景下，该解决方案要注意的问题 

（1）读请求长时阻塞 

由于读请求进行了非常轻度的异步化，所以一定要注意读超时的问题，每个读请求必须在超时时间范围内返回 

该解决方案，最大的风险点在于说，可能数据更新很频繁，导致队列中积压了大量更新操作在里面，然后读请求会发生大量的超时，最后导致大量的请求直接走数据库 

务必通过一些模拟真实的测试，看看更新数据的频繁是怎样的 

另外一点，因为一个队列中，可能会积压针对多个数据项的更新操作，因此需要根据自己的业务情况进行测试，可能需要部署多个服务，每个服务分摊一些数据的更新操作 

如果一个内存队列里居然会挤压100个商品的库存修改操作，每隔库存修改操作要耗费10ms区完成，那么最后一个商品的读请求，可能等待10 * 100 = 1000ms = 1s后，才能得到数据 

这个时候就导致读请求的长时阻塞 

一定要做根据实际业务系统的运行情况，去进行一些压力测试，和模拟线上环境，去看看最繁忙的时候，内存队列可能会挤压多少更新操作，可能会导致最后一个更新操作对应的读请求，会hang多少时间，如果读请求在200ms返回，如果你计算过后，哪怕是最繁忙的时候，积压10个更新操作，最多等待200ms，那还可以的 

如果一个内存队列可能积压的更新操作特别多，那么你就要加机器，让每个机器上部署的服务实例处理更少的数据，那么每个内存队列中积压的更新操作就会越少 

其实根据之前的项目经验，一般来说数据的写频率是很低的，因此实际上正常来说，在队列中积压的更新操作应该是很少的 

针对读高并发，读缓存架构的项目，一般写请求相对读来说，是非常非常少的，每秒的QPS能到几百就不错了 

一秒，500的写操作，5份，每200ms，就100个写操作 

单机器，20个内存队列，每个内存队列，可能就积压5个写操作，每个写操作性能测试后，一般在20ms左右就完成 

那么针对每个内存队列中的数据的读请求，也就最多hang一会儿，200ms以内肯定能返回了 

写QPS扩大10倍，但是经过刚才的测算，就知道，单机支撑写QPS几百没问题，那么就扩容机器，扩容10倍的机器，10台机器，每个机器20个队列，200个队列 

大部分的情况下，应该是这样的，大量的读请求过来，都是直接走缓存取到数据的 

少量情况下，可能遇到读跟数据更新冲突的情况，如上所述，那么此时更新操作如果先入队列，之后可能会瞬间来了对这个数据大量的读请求，但是因为做了去重的优化，所以也就一个更新缓存的操作跟在它后面 

等数据更新完了，读请求触发的缓存更新操作也完成，然后临时等待的读请求全部可以读到缓存中的数据 

（2）读请求并发量过高 

这里还必须做好压力测试，确保恰巧碰上上述情况的时候，还有一个风险，就是突然间大量读请求会在几十毫秒的延时hang在服务上，看服务能不能抗的住，需要多少机器才能抗住最大的极限情况的峰值 

但是因为并不是所有的数据都在同一时间更新，缓存也不会同一时间失效，所以每次可能也就是少数数据的缓存失效了，然后那些数据对应的读请求过来，并发量应该也不会特别大 

按1:99的比例计算读和写的请求，每秒5万的读QPS，可能只有500次更新操作 

如果一秒有500的写QPS，那么要测算好，可能写操作影响的数据有500条，这500条数据在缓存中失效后，可能导致多少读请求，发送读请求到库存服务来，要求更新缓存 

一般来说，1:1，1:2，1:3，每秒钟有1000个读请求，会hang在库存服务上，每个读请求最多hang多少时间，200ms就会返回 

在同一时间最多hang住的可能也就是单机200个读请求，同时hang住 

单机hang200个读请求，还是ok的 

1:20，每秒更新500条数据，这500秒数据对应的读请求，会有20 * 500 = 1万 

1万个读请求全部hang在库存服务上，就死定了 

（3）多服务实例部署的请求路由 

可能这个服务部署了多个实例，那么必须保证说，执行数据更新操作，以及执行缓存更新操作的请求，都通过nginx服务器路由到相同的服务实例上 

（4）热点商品的路由问题，导致请求的倾斜 

万一某个商品的读写请求特别高，全部打到相同的机器的相同的队列里面去了，可能造成某台机器的压力过大 

就是说，因为只有在商品数据更新的时候才会清空缓存，然后才会导致读写并发，所以更新频率不是太高的话，这个问题的影响并不是特别大 

但是的确可能某些机器的负载会高一些

### 35_高并发场景下恐怖的缓存雪崩现象以及导致系统全盘崩溃的灾难性后果 

缓存雪崩这种场景，缓存架构中非常重要的一个环节，应对缓存雪崩的解决方案，避免缓存雪崩的时候，造成整个系统崩溃，带来巨大的经济损失 

1、redis集群彻底崩溃 

2、缓存服务大量对redis的请求hang住，占用资源 

3、缓存服务大量的请求打到源头服务去查询mysql，直接打死mysql 

4、源头服务因为mysql被打死也崩溃，对源服务的请求也hang住，占用资源 

5、缓存服务大量的资源全部耗费在访问redis和源服务无果，最后自己被拖死，无法提供服务 

6、nginx无法访问缓存服务，redis和源服务，只能基于本地缓存提供服务，但是缓存过期后，没有数据提供 

7、网站崩溃 

行业里真实的缓存雪崩的经验和教训 

某电商，之前就是出现过，整个缓存的集群彻底崩溃了，因为主要是集群本身的bug，导致自己把自己给弄死了，虽然当时也是部署了双机房的，但是还是死了 

电商大量的，几乎所有的应用都是基于那个缓存集群去开发的 

导致各种服务的线程资源全部被耗尽，然后用在了访问那个缓存集群时的等待、超时和报错上了 

然后导致各种服务就没有资源对外提供服务咯 

然后各种降级措施也没做好，直接就是整体系统的全盘崩溃 

导致网站就没法对外出售商品咯，导致了很大数额的经济的损失 

java架构师，资深java工程师，对自己技术有点要求，多学一些，多思考一些各种场景下的缓存架构，用来解决各种各样的问题 

自己做系统架构设计的时候，多留个心眼儿，考虑一下各种高并发场景下可能出现的问题，数据不一致，热点缓存，重建并发冲突，redis高可用性，缓存雪崩， 缓存穿透，缓存失效 

架构设计做好一些，稳定性也做好一些 

你的系统能够承载各种各样的故障，才能在真正发生故障的时候，减少对公司的损失，保住大家的饭碗 

你说你用过redis，系统里面涉及过这种缓存的架构，高并发场景下的各种问题，结合你的业务，怎么去设计整套缓存架构的，跳槽面试的时候，说点牛逼的出来 

### 36_缓存雪崩的基于事前+事中+事后三个层次的完美解决方案》

相对来说，考虑的比较完善的一套方案，分为事前，事中，事后三个层次去思考怎么来应对缓存雪崩的场景 

1、事前解决方案 

发生缓存雪崩之前，事情之前，怎么去避免redis彻底挂掉 

redis本身的高可用性，复制，主从架构，操作主节点，读写，数据同步到从节点，一旦主节点挂掉，从节点跟上 

双机房部署，一套redis cluster，部分机器在一个机房，另一部分机器在另外一个机房 

还有一种部署方式，两套redis cluster，两套redis cluster之间做一个数据的同步，redis集群是可以搭建成树状的结构的 

一旦说单个机房出了故障，至少说另外一个机房还能有些redis实例提供服务 

2、事中解决方案 

redis cluster已经彻底崩溃了，已经开始大量的访问无法访问到redis了 

（1）ehcache本地缓存 

所做的多级缓存架构的作用上了，ehcache的缓存，应对零散的redis中数据被清除掉的现象，另外一个主要是预防redis彻底崩溃 

多台机器上部署的缓存服务实例的内存中，还有一套ehcache的缓存 

ehcache的缓存还能支撑一阵 

（2）对redis访问的资源隔离 

（3）对源服务访问的限流以及资源隔离 

3、事后解决方案 

（1）redis数据可以恢复，做了备份，redis数据备份和恢复，redis重新启动起来 

（2）redis数据彻底丢失了，或者数据过旧，快速缓存预热，redis重新启动起来 

redis对外提供服务 

缓存服务里，熔断策略，自动可以恢复，half-open，发现redis可以访问了，自动恢复了，自动就继续去访问redis了 

基于hystrix的高可用服务这块技术之后，先讲解缓存服务如何设计成高可用的架构 

缓存架构应对高并发下的缓存雪崩的解决方案，基于hystrix去做缓存服务的保护 

要带着大家去实现的有什么东西？事前和事后不用了吧 

事中，ehcache本身也做好了 

基于hystrix对redis的访问进行保护，对源服务的访问进行保护，讲解hystrix的时候，也说过对源服务的访问怎么怎么进行这种高可用的保护 

但是站的角度不同，源服务如果自己本身不知道什么原因出了故障，我们怎么去保护，调用商品服务的接口大量的报错、超时 

限流，资源隔离，降级 

### 37_为redis集群崩溃时的访问失败增加fail silent容错机制 

上一节课，我们已经通过hystrix command对redis的访问进行了资源隔离 

资源隔离，避免说redis访问频繁失败，或者频繁超时的时候，耗尽大量的tomcat容器的资源去hang在redis的访问上 

限定只有一部分线程资源可以用来访问redis 

你是不是说，如果redis集群彻底崩溃了，这个时候，可能command对redis的访问大量的报错和timeout超时，熔断（短路） 

降级机制，fallback 

fail silent模式，fallback里面直接返回一个空值，比如一个null，最简单了 

在外面调用redis的代码（CacheService类），是感知不到redis的访问异常的，只要你把timeout、熔断、熔断恢复、降级，都做好了 

可能会出现的情况是，当redis集群崩溃的时候，CacheService获取到的是大量的null空值 

根据这个null空值，我们还可以去做多级缓存的降级访问，nginx本地缓存，redis分布式集群缓存，ehcache本地缓存，CacheController 

### 38_高并发场景下的缓存穿透导致MySQL压力倍增问题以及其解决方案 

缓存穿透 

缓存穿透的现象是什么呢？

 相当于说，你的缓存里没有数据，根本就没有用，缓存根本没有帮mysql抗住多少流量，这样的话，就会导致每次请求都会直接穿透所有层的缓存，然后大量的高并发请求打到mysql上去，执行各种sql语句，但是每次查询出来的数据都为空

这种情况，如果不好好处理，高并发访问大量进入mysql上，打死了mysql 

### 039_案例实战：最普通的基于Redis实现的缓存机制》 

redis如果说简单了，里面存放的就是一大堆的key-value对，set key value，往里面放一堆数据，然后通过get key，从里面获取一堆数据，往简单了用，基本上来说redis就是这么用的，第一种最经典的场景 

你可以用redis来应对高并发和高性能 

高性能，你有一些很复杂的查询，或者是一些很大的数据资源（比如说图片之类的东西，超长文本之类的东西），可能平时执行的话呢是很慢的，复杂查询，上百行的SQL语句，从数据库里出，可能很慢，或者是一个大html页面，从磁盘上加载 

你就可以把复杂查询以后的结果放入redis里作为缓存，下次再查询的时候就直接从redis缓存里出，不需要再次从数据库里执行上百行的SQL语句去查询了，只要10毫秒就可以做到了 

html页面，可以放在缓存里，每次就直接从缓存里出就可以了，不需要每次都做动态的渲染，比如说基于jsp动态渲染页面，再从磁盘上读取出来返回给浏览器，其实你也可以把一些静态资源放到redis缓存里去 

高并发，假设你每次都查询数据库，数据库最多每秒抗4000个请求，但是你每秒有1万次请求，此时你可以把一些查询结果放在缓存里，后续可能每秒有9500次请求都是走redis缓存的，500次请求走数据库 

同样的机器配置下，数据库每秒也就抗几千次请求，redis每秒可以抗几万次请求到十万次的请求都可以做到  

### 040_案例实战：实现一个最简单的分布式锁 

企业级分布式锁，基于redis的分布式锁的企业级实现，在redisson框架里的源码，我们很早就分析过了，基于复杂的lua脚本去实现的，有一整套的实现锁的逻辑在里面的，根本不是给大家讲解分布式锁 

nx 

set key value nx，必须是这个key此时是不存在的，才能设置成功，如果说key要是存在了，此时设置是失败的 

比如说有很多台机器，要竞争一把锁，此时你可以让他们对同一个锁的key，比如lock_key，设置一个value，而且都是要加上nx选项的，此时redis可以通过nx选项，保证说只有一台机器可以设置成功，可以加锁成功，其他机器都是会失败的

### 041_案例实战：博客网站的文章发布与查看》 

mset，mget，msetnx，m -> multi的意思，mset一下子设置多个key-value对，mget就是一下子获取多个key的value，msetnx就是在多个key都不存在的情况下，一次性设置多个key的value 

mset和mget，相当于是batch批量设置和查询，比如说假设你一次性要往redis里塞入20条数据，假设你是通过for循环加上set，执行20次的set，每一次set操作都要一次完整的网络通信过程 

每一次set就算他是10ms，200ms 

mset，一次性把20个key-value对通过一次网络通信，交给redis去执行，此时就是10ms就可以了，mget也是一个意思，批量查询，msetnx，必须所有key都不存在，然后才可以完成本次的设置 

平时写一篇博客，一方面落入数据库，一方面把数据双写一份到缓存里去 

### 042_案例实战：博客字数统计与文章预览 

strlen，getrange  

### 043_案例实战：用户操作日志审计功能 

append 

平时我们对系统上的一些用户的一些核心操作，比如更新一些数据的操作，都是会有一个审计日志的功能，其实就是把你用户在系统里的一些操作记录下来，每一次操作都记录成一条日志，审计日志 

后续可以通过一个列表来进行一个查询，分页查询 

我们如果说要基于redis来存储这种操作审计日志，应该如何来做呢？也可以在redis里，搞一个key，每天都有一个key，每天的这个key里就把当天所有的操作日志都串联在一起，查询的时候，都是按天来查询你的操作审计日志就可以了 

key -> value，value取出来，做一下字符串拼接 

redis的append这个api，就是说可以不停的把你的日志追加到指定的key里去

### 044_案例实战：实现一个简单的唯一ID生成器 

ID，都是通过数据库的自增主键来实现的，有一些场景下，分库分表的时候，你同一个表里的数据都会分散在多个库的多个表里，这个时候就不能光是靠数据库来生成自增长的主键了，此时就需要生成唯一ID 

snowflake算法，还有一些大厂开源的唯一ID生成组件，他是会解决一些原生的snowflake算的缺陷 

incr  

### 045_案例实战：实现博客点赞次数计数器 

incr，decr   

### 046_案例实战：社交网站的网址点击追踪机制 

开胃菜，初步讲解了一下redis里的一些字符串的用法，最最简单的key-value对，加上一些数字的操作方法，增加/减少；hash，list，set，sorted set，把核心的redis的四大数据结构的各种用法结合案例讲解清楚；下周，hyperloglog、流、事务、lua、流水线、过期时间，高阶的功能，结合案例 

hash数据结构，社交网站的网址点击追踪机制，类似微博 

hset hash field value

hsetnx

hget hash field 

社交网站（微博）一般会把你发表的一些微博里的长连接转换为短连接，这样可以利用短连接进行点击数量追踪，然后再让你进入短连接对应的长连接地址里去，所以可以利用hash数据结构去实现网址点击追踪机制 

http://t.cn/XsGGA9d -> http://redis.com/index.html?dfd=sf& dfd=sf& dfd=sf& dfd=sf& dfd=sf& dfd=sf& 

利用redis的incr自增长，然后10进制转36进制，接着hset存放在hash数据结构里，再提供一个映射转换的hget获取方法，hash数据结构，说白了就是我们的Java里的HashMap，redis里的hash就是一个map，一个map里可以放一些key-value对 

short_url_access_count: {

​    http://t.cn/XsGGA9d: 152,

​    http://t.cn/I93yUUaF: 269

} 

public class Ten2Thirty { 

  private static final String X36 = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";

  private static final String[] X36_ARRAY = "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z".split(","); 

  public static String tenTo36(int num) {

​    StringBuffer sBuffer = new StringBuffer(); 

​    if(num == 0) {

​      sBuffer.append("0");

​    }

​    while(num > 0) {

​      sBuffer.append(X36_ARRAY[num % 36]);

​      num = num / 36;

​    } 

​    return sBuffer.reverse().toString();

  } 

}  

### 047_案例实战：博客网站文章浏览次数统计 

博客网站，发布一篇文章，修改一篇文章，查看一篇文章，有人查看了一下这篇文章，就可以维护一下这篇文章他的一个浏览次数，对于这个浏览次数就可以统一都放在一个hash数据结构里，每个key就是一篇文章，value就是文章的浏览次数 

hincrby hash view_count 1

hget hash view_count 

### 48_案例实战：基于hash数据结构重构博客网站案例》 

redis的hash数据结构，特别适合存放什么呢，就是类似于我们自己Java代码里搞的一些对象，很适合用hash数据结构来存储的，如果说你的一些对象不用hash结构来存储的话，比如你直接用json串把java对象序列化成json，然后用key-value对的字符串形势放在redis里，但是操作起来不是太方便 

hexists判断博客是否存在，不存在可以发表新博客，用hmset一次性设置多个key-value对在hash数据结构里 

查看博客的时候，直接用hgetall hash，一次性把一个hash里全部key-value对拿出来，然后返回就可以了 

更新博客，也是用hmset就可以了 

getrange，setrange，append，key-value的字符串形式，可以用到过期时间，过期时间key-value对在适当的时候是会过期的  

### 49_案例实战：基于令牌的用户登录会话机制 

用户平时会访问我们的系统，在处理任何一个请求之前，必须检查一下，这个请求是否带上了一个令牌，如果带了一个令牌，那么此时就必须在redis里检查一下，这个令牌是否有在redis里合法的、有效的一个session会话 

如果有这个session会话，此时就可以允许这个请求被处理，因为说明这个人之前已经登录过我们的系统了，登录过后才会在redis里放一个有效的session会话；如果说没有这个session的话，此时就会导致用户必须强制被迫登录 

如果用户登录通过之后，就会返回给浏览器或者客户端一块令牌，同时在redis里初始化好一个session会话，后续客户端就会在指定时间范围内发送请求的时候带上一块令牌，每次令牌和服务器端的session校验通过就可以执行请求 

过一段时间过后，服务端的redis里的session会话就会过期，过期了之后，又会导致你必须要重新登录，虽然你可能带上了令牌，但是一检查发现这块令牌对应的redis里的session已经过期了 

hset把用户id和令牌存储一下，hset把用户id和过期令牌过期时间存储一下 

每次访问系统都让用户带上令牌，如果令牌不存在就是没登录，hget获取存储的令牌和过期时间，如果令牌过期了也要强制登录，如果令牌校验通过，这次请求就可以通过 

如果令牌要是过期了，就用hdel把存储的令牌和过期时间都删了  

### 50_案例实战：秒杀活动下的公平队列抢购机制 

通过一些案例，已经给大家把hash数据结构如何来使用，结合案例已经讲解的非常清楚了，接下来的话呢，就是讲解list数据结构，同样的，也是通过各种案例来讲解，讲解list是如何来使用的 

秒杀系统有很多实现方案，其中有一种技术方案，就是对所有涌入系统的秒杀抢购请求，都放入redis的一个list数据结构里去，进行公平队列排队，然后入队之后就等待秒杀结果，专门搞一个消费者从list里按顺序获取抢购请求，按顺序进行库存扣减，扣减成功了就让你抢购成功 

如果说你要是不用公平队列的话，可能就会导致你很多抢购请求进来，大家都在尝试扣减库存，此时可能先涌入进来的请求并没有先对redis执行抢购请求，此时可能后涌入进来的请求先执行了抢购请求，此时就是不公平的 

公平队列，基于redis里的list数据结构，搞一个队列，抢购请求都进队列，先入先出，先出来的人先抢购，此时就是公平的 

list数据结构，你可以把他理解为是Java里的ArrayList，LinkedList，就是一种有序的数据结构，也可以把他作为队列来使用也是可以的 

对于抢购请求入队列，就用lpush list request就可以了，然后对于出队列进行抢购，就用rpop list就可以了，lpush就是左边推入，rpush就是右边推入，lpop就是左边弹出，rpop就是右边弹出 

所以你lpush+rpop，就是做了一个左边推入和右边弹出的先入先出的公平队列 

[第3个请求，第2个请求，第1个请求]  

### 51_案例实战：实现OA系统中的待办事项列表管理》 

OA系统，自动化办公系统，说白了就是把企业日常运行的办公的日常事务都在OA系统里来做，请假，审批，开会，项目，任务，待办事项列表 

lindex，lset，linsert，ltrim，lrem 

新增待办事项，lpush list event 

插入待办事项，linsert list index event 

[待办事项3，待办事项2，插入待办事项，待办事项1] 

查询待办事项列表，lrange list 0 -1，所有都查询出来 

完成待办事项，lrem list 0 event，就把这个待办事项给删了，然后lpush done_list event，添加一个已办事项 

批量完成待办事项，ltrim list start_index end_index，然后lpush done_list event1 event2 event3 

修改待办事项，lindex和lset 

查询已办事项列表，lrange done_list 0 -01 

### 52_案例实战：网站用户注册时的邮件验证机制 

填写完注册信息，然后提交注册，此时后台完成注册校验和用户数据入库，返回一个消息告诉你说注册成功，但是稍后发送了一封验证邮件到你的邮箱，希望你去邮箱里点击一个链接确认你的邮箱 

一般来说在注册的后台里，是否在注册的逻辑里直接就发送一封邮件呢？发送邮件是比较耗费时间的，所以通常来说，你后台注册成功之后，都会把一个发送邮件的任务给放到一个队列里去，然后直接返回给你了 

接着lpush将发送邮件任务放入list，然后发送邮件的系统就用brpop阻塞式的等待从队列里获取任务，这就可以把list做成阻塞队列的效果了，加一个timeout超时时间即可 

### 53_案例实战：网站每日UV数据指标去重统计 

set数据结构，跟Java里的set是一样的，就是说放无序的、不重复的数据集合，list一般来说做一个操作，时间复杂度都是O(n)，set一般的操作，时间复杂度是O(1)，无序的，不重复的数据集合，数据结构上的优化，可以做到 

如果说你把重复的数据放到一个set里，他会自动给你进行去重 

网站的uv，有多少用户访问了你的网站，但是一个用户可能会访问多次，此时要对多次访问进行去重，保留完整的不重复的访问过你网站的用户集合，然后算出集合的元素数量，就会知道你当日的uv，sadd把uid放入集合，scard可以拿到uv值，当然这个仅仅是演示，实际实现是不可能这么做的  

### 54_案例实战：博客网站的文章标签管理 

在博客网站发表一篇文章的时候，都可以自定义给这篇文章打一些标签的，java、大数据、软件架构、心灵鸡汤，对于一篇文章来说，标签其实是不能重复的，就直接可以用set来保存 

sadd是添加标签，srem是删除标签，sismember是判断该文章是否包含某个标签，smembers是返回这个文章的全部标签，scard是这个文章的标签数量 

### 55_案例实战：朋友圈点赞功能的实现 

假设说你发了一条朋友圈，此时可能你的好友会对你的朋友圈进行点赞，还可以取消点赞，你的好友在刷朋友圈的时候可以查看到自己是否对你点赞过，你自己还可以查看到你的朋友圈有哪些人点赞了，有多少人给你点赞了 

sadd给某一条朋友圈添加点赞的一个好友，用户取消点赞的话，那就是srem删除某个好友的点赞，查看你是否对某条朋友圈进行过点赞，sismember，你发出的朋友圈被哪些人点赞了，smembers，你的朋友圈的点赞次数，scard 

### 56_案例实战：实现一个网站投票统计程序 

sadd把用户添加到某个投票项的投票用户集合里去，sismember可以检查用户是否已经对任何一个投票项发起过投票，scard可以统计每个投票箱的投票人数，smembers可以拿到每个投票项的投票人    

### 57_案例实战：实现类似微博的社交关系 

关注目标用户，sadd把你加入到人家的关注用户集合里去，sadd把别人加入到你正关注的用户集合里去，取消关注，srem取消两个用户集合的关注，smembers获取你关注的所有人和你被哪些人关注了，scard获取你关注的人数和关注你的人数  

### 58_案例实战：微博的共同关注与推荐关注 

sinter set1 set2，取交集，就是共同关注好友；推荐好友关注的人，sdiff获取差集，然后用差集再和你的好友集合sdiff一下，再取差集，就可以得到你没关注的但是你好友关注的人，此时就可以推荐一下； 

sunion，如果+store还可以存储   

### 59_案例实战：实现网站上的抽奖程序 

srandommember，spop，前者是随机从set里返回几个元素，后者是随机从set里弹出几个元素，sadd可以加入待抽奖的人，smembers返回所有待抽奖人，scard返回参与抽奖的人数，srandmember返回随机抽中奖的人    

### 60_案例实战：为商品搜索构建反向索引 

为商品添加索引，sadd，给商品添加一个关键词索引集合，sadd把商品添加到每个关键词的商品集合里去，删除商品是一个反向的过程，走srem，获取一个商品所有的关键词，smembers，根据某几个关键词去搜索商品，对每个关键词都smembers一下拿到商品集合，然后走一个sintern对多个集合进行交集 

仅仅是演示，实际上如果你要考虑到分页，那就更加复杂了 

### 61_案例实战：实现音乐网站的排行榜程序

sorted set，不能有重复的数据，加入进去的每个数据都可以带一个分数，他里面的数据都会按照分数进行排序，有序的set，他自动按照分数来排序，相当于你可以定制他里面的排序规则了

zadd，把音乐加入排行榜中，刚开始分数可能就是0；zscore可以获取音乐的分数；zrem可以删除某个音乐；zincrby可以给某个音乐增加分数，这个增加分数可能就是说有人下载了，或者是有人播放了，或者有人分享了， 有人点赞了，此时可以按照规则去加分，那么排序就会移动了；当然也可以减去分数；zrevrank获取音乐在排行榜里的排名；zrevrange set 0 100 withscores，可以获取排名前100的热门歌曲

### 62_案例实战：实现一个新闻推荐机制

我们可以把一个sorted set里的数据倒序排序，选择其中我们指定的分数区间范围内的数据，对这块数据还可以进行分页查询，我们可以维护一个新闻数据集合，里面的分数都是新闻的时间戳

zadd，把当日最新的新闻加入到一个集合里，zrem是删除某个新闻，zcard是统计当日最新新闻，，zrevrangebyscore max_time min_time start_index count withscores，是说按照他的时间分数进行倒序排序，然后获取指定的分页，zcount 可以获取指定分数范围的数量

### 063_案例实战：大数据商品关系推荐机制

zremrangebyscore，zunionstore，zinterstore

买了一个商品，他会给你推荐一个列表，购买过此商品的顾客也同时购买了其他XX商品，这个也是可以用这个sorted set来实现的

对每个顾客，每次购买了之后，都会遍历他之前购买过的所有的商品，对每个商品都维护一个同时购买的其他商品的数量，可以用zincrby set 1 其他商品，然后后续如果要看到某个商品购买的人还购买了其他哪些商品，就可以用zrevrange set start_index end_index withscores，

### 64_案例实战：网站搜索框的自动补全功能

zrangebylex、zrevrangebylex、zlexcount、zremrangebylex、zpopmax、zpopmin、bzpopmax、bzpopmin

对于输入的每个搜索词，都会遍历一下，拼接出来，然后zincrby auto_complete::潜在搜索词 权重 完整搜索词，这样的话，就是每个潜在搜索词都会有一个集合，集合里是各种可能对应的搜索词和权重分数

然后真正你搜的时候，把你输入的潜在搜索词去执行，zrevrange set 0 9，就是对这个潜在搜索词按照权重分数倒序拿出最近的10个搜索词，做一个自动提示和补全

string和number

list、hash、set、sorted set（自己定制他的排序规则，定制搜索浏览的规则）

### 065_基于HyperLogLog的网站UV统计程序

hyperloglog，数据结构+概率算法，组合而成的，去重统计，近似数

1023468个用户来访问过你

1011325，算出来这样的一个近似值，对于很多大数据统计类的程序，精准也没太大的必要，CEO层面，100万的日活

如果基于set来计数，太耗费内存了，基于HyperLogLog算法来计数，是近似计数，有0.8%的误差，但是误差不会太大，可以给出一个相对准确的近似计数，而且就占用12kb的内存，不需要存一个set来统计uv

pfadd key 一大堆item，可以对数据进行计数，如果计算过这个元素，就返回0，没计算过就返回1，他也是可以去重的

pfcount key，可以获取计数结果

这个其实是很有用的，其实一些网站数据分析，什么pv、uv之类的，没必要过于精准，直接就是用redis的hyperloglog计数就挺好的

### 066_网站重复垃圾数据的快速去重和过滤

对于你的一些网站，垃圾数据，多的是一些不良广告，在一些论坛或者别的，有的时候你会看到一些在帖子下面的评论里，是一些广告，都有，可能会遇到有人频繁的提交相同的垃圾信息到你的站点里

pfadd key content，如果返回的是1，那么说明之前没见过这条数据，如果返回的是0，说明之前见过这条数据了 

### 067_周活跃用户数、月活跃用户数、年活跃用户数的统计

pfmerge 多个hyperloglog，可以算出来周、月和年的活跃用户数 

### 068_基于位图的网站用户行为记录程序

如果说你要记录一下，在系统 里执行一些特殊的操作，每天执行过某个操作的用户有多少个人，操作日志，审计日志，记录下来每个用户每天做了哪些操作，每个用户每天搞一个set，里面放他做的一些操作日志

也可以对每种操作都搞一个set，里面放执行过这些操作的用户

bitmap，位图，二进制里的一位一位的，字符串，int，long，double，二进制，都是对应多少多少位的，一个字节是8位的二进制数，int就是4个字节就是32位，你直接可以在redis里操作二进制的位数据

可以把网站里每一种操作每天执行过的用户都放在一个位图里，一个用户仅仅对应了一位而已

setbit key user_id 1

getbit key user_id

bitcount key

一个位图统计100万用户的行为，也不过一百多kb的内存

### 069_基于GeoHash的你与商铺距离计算的程序

redis里还有一种特殊的数据结构，叫做Geo，可以让你在里面添加很多的地理位置，每个位置对应一个名称和一个经纬度，然后可以利用这个数据结构计算各种距离的位置，获取方圆半径多少1公里内的店铺

geoadd key longitude latitude user 116.49428833935545 39.86700462665782

geoadd key longitude latitude shop 116.45961274121092 39.87517301328063

geodist key user shop unit=’km’ 

### 070_案例实战：陌生人社交APP里的查找附近的人功能实现

geoadd key longitude latitude user，记录每个用户的地理位置

georadiusbymember key user radius unit=’km’，radius设置为1，就是附近1公里的用户都找出来，再从返回的set里删除user自己，就可以了 

### 071_带有自动过期时间的分布式缓存实现

redis，我们之前已经讲解了他的一些数据结构，过期时间、pipeline、事务，过期时间，就是说你可以对一个key指定一个过期时间，到了时间之后，自动就让这个数据就过期了，不再生效了

set key value

expire key timeout 

### 072_支持超时自动释放的分布式锁实现

set key value ex=timeout nx=true 

### 073_支持自动过期的用户登录会话实现

set key token ex=timeout 

### 074_支持冷数据自动淘汰的自动补全程序

expire key timeout 

### 075_支持身份验证功能的分布式锁释放机制

pipeline = pipeline() // 流水线里的命令，都是一次性打包发送执行的

try {

pipeline.watch(key)

// key如果有变化，后续提交事务直接失败，unwatch可以取消监控

lock_value = pipeline.get(key)

if(lock_value == null) {

// 锁已经被释放了

} else if(lock_value == user_id) {

// 是自己加的锁，可以释放

// 先用multi()打开一个事务，事务里的命令一起成功或者一起失败

pipeline.multi()

pipeline.delete(key)

pipeline.execute() // 提交事务，discard可以放弃事务

} else {

// 不是自己加的锁，不能让你去释放

}

} catch() {

// 如果有异常说明你提交事务的时候有人更新了key

} finally {

// 每个流水线都绑定了一个连接

// 执行完一个流水线，必须执行reset把连接还给连接池

pipeline.reset()

}

stream，发布/订阅，lua 

### 076_项目案例：百万流量大型电商网站商品详情页系统架构的整体设计（一）

#### 1、案例背景及相关业务知识介绍

现代社会网购已经成为我们生活的一部分，即使你很少网购，你也一定会因为家眷的网购需求而有所支出。所以你也一定对下面这种页面司空见惯，如图1所示：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmdkw7yy0p0g.png)

图1

你一定会觉得，“这有啥？不就是个展示商品的页面吗？” 但请注意就这一张小小的页面，包含的数据信息可以说是极为丰富的。如图2所示：

![图片2.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmdky82v07sv.png)

图2

图2中我用不同颜色将图划分了一些区域，每一块区域都代表了在这个商品展示页面里的一块数据维度：

- 蓝色代表商家信息的数据维度，包括：商家名称，商家评分
- 绿色代表产品的分类
- 红色代表所浏览的商品信息 包括：商品标题，商品价格，商品分类
- 那么此时我们用屁股想想都应该知道这些数据肯定来自于多个数据源，从而才使得这一张页面的页面里所包含的数据如此的丰富。这种仅仅以一件单品信息为用户关注主体，却又包含如此丰富其他维度信息的页面，被称作商品详情页

#### 2、商品详情页架构v1.0

正如前面所说，这些数据来自于多个数据源，如果我们用最传统的SSM架构来做这套系统大概是这样子的 如图3所示：

![图片3.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmdl0pd00rg4.png) 图3

这个架构十分简单，问题自然也就十分的明显：

问题一：如图4所示，每次用户对一个商品详情页进行访问，都会直接查询MySQL数据库，性能之差那是可想而知的，并且mysql抗并发能力是极其有限的，让它直接正面迎接巨额流量是极其危险的。

![图片4.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmdl2mz30nn1.png)图4

问题二：大家可以预想一下mysql一旦耍起性子来玩起了罢工，就问你怕不怕？所以解放mysql是现在的当务之急，商品详情页系统与其依赖的数据源系统高度耦合了，但凡其中有一个依赖服务出现网络波动等外界因素，会直接导致商品详情页系统无法正常提供服务。如图5所示：

![图片5.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmdl5a070dw8.png)图5

由此可见，商品详情页系统与各个源数据系统解耦是我们必须做的事情。回头看看，这样的架构想要支撑起百万流量的商品详情页系统简直就是痴人说梦。

#### 3、商品详情页架构v2.0

为了解决上述的两个问题，我们会一步一步的做改动：

首先我们先来思考一下，让mysql面临巨额流量的根本原因是什么？

根本原因是有着百万流量的电商详情页系统对mysql直接发起请求，来查询用户需要的数据，对页面进行渲染。用户的需求仅仅是一份美观的数据详情页，用于浏览即可，往往对数据的时效性，要求并不是那么的严格。

所以我们可以这样，让公司内部的运营人员对商品详情页系统发起通知，当商品详情页系统接收到运营人员的通知，就去各个源数据系拉取数据，同于渲染html页面，渲染完成一个html页面就将其推送到一个ngnix服务器上。那么之后的用户直接从ngnix这一层就能浏览到商品详情页了。此时的架构变化如图6所示：

![架构2.0释放mysql.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmdo3jnr05pt.png)图6

此时可以看到真正对数据库访问的是我们的内部运营系统，数据库的负载自然小的不是一星半点，而用户在数据访问层的ngnix就能获取到商品详情页的内容了。

接着我们要解决商品详情页系统和各个数据源系统高度耦合的问题。上文说到耦合的副作用就是当商品详情页系统访问各个数据源系统时一旦其中一个系统发生网络波动就直接影响到商品详情页对html的渲染。

那么我们为何不让这些数据源服务自己主动把最新的消息推送过来，此时商品详情页就只要关注html的渲染和页面的推送就可以了。就不需要“传话人”的运营系统了，岂不是美滋滋？既然说到了消息推送，自然想到解耦那么对于MQ的引入也就随之呼之欲出了。按照这个思路我们可以做以下改动，如图7所示：

​                               ![架构2.0mq引入.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmdndo91005i.png)如图7

至此架构v1.0提出的两个问题，我们都顺利得以解决。下一节，我会带大家继续对v2.0的架构进行剖析。咱们下期见，拜拜。

### 076_项目案例：百万流量大型电商网站商品详情页系统架构的整体设计（二）

#### 1、改善架构2.0

经过一番改造系统的性能有了较大的改善，但是大家仔细看我下面对图7标记的部分 如图8：

![架构2.0mq引入标红新 (7).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/85226500_1616071330.png)

图8

图中圈出的一块问题是显而易见的，就是只要任意维度的数据发生了变更，那么对应商品详情页就要进行完整的重新渲染，这无疑是极其浪费性能的。

由开头的电商截图可以知道，每一个维度的数据反应在商品详情页上其实就是在在一块各自的固定区域里填上相关的的内容而已，那么在，布局已经确立的情况下，布局已经确立的情况下，布局已经确立的情况下 ，重要的事情说三遍！！！此时我们完全可以根据现在的商品详情页抽出一套模板，只要填充对应部分的数据即可，就像图9所示：

![骨架概念引出 (1).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/14014000_1616071331.png)

图9

那么现在不管哪个维度发生改变，只要更新对应的商品详情页的维度所在区域即可，如图10所示：

![基于骨架骨架更新的好处 (1).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/73864900_1616071331.png)

图10

根据这个思路我们可以把我们的html按照维度拆成一个一个的html片段,各个消费端只需要关注自己的维度，渲染和上传相关的html片段即可，如图11所示：

![架构2.0mq拆分片段 (3).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/36138200_1616071332.png)图11

但是现在又有一个新的问题，上传到ngnix服务器上是一个一个的html片段，我们又如何将这些片段组装在一起成为一个完整的html页面呢？这时候我们就需要SSI动态内容缓存技术帮我们完成页面的“组装”工作，如图12所示：



![架构2.0mq合并片段 (1).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/68315400_1616071332.png)图12

好了，咱们现在的页面也实现了局部渲染，问题看似得已解决了。

#### 2、迫在眉睫得扩容问题

我们现在案例是以百万流量大型电商的商品详情页为背景，那么不能只把眼光放在抗流量上，因为我们的数据访问层的ngnix实际上扮演了一个存储设备的角色，上面存放着电商网站所有的商品详情页的片段，那么一个电商网站肯定有数以万计的商品给用户挑选，而且以后还得不断发布新的商品，以现在的架构迟早会发生如图13所示的情况：

![架构2.0mq容量问题 (6).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/32443300_1616071333.png)图13

看到这，你可能要问这有何难？加个机器不就行了吗？是的看上去是这样的，但是你怎么让你推送的页面，均匀的分配到各个ngnix上呢？如果想不清楚这个问题，就会出现如图14的问题：



![架构2.0扩容，无路由 (2).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/2099400_1616071334.png)

图14

那么此时我们必须制定一个路由规则，来引导消费端将商品详情页平均的推送到各个ngnix 上,并且保证同一个商品的html片段必须分配到同一台ngnix服务器上，那么如图15所示：

![架构2.0扩容路由加入 (2).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/77755400_1616071334.png)图15

那么相对的我们也要保证用户访问的时候，也遵循和推送一致的路由算法进行页面定位，这个时候我们就要数据访问层的ngnix前面增设一台用于路由分发的ngnix,如图16所示：

![架构2.0扩容分发加入了 (3).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/16849200_1616071336.png)图16

至此ngnix的扩容和路由定位的问题也被我们解决了。

#### 3、架构3.0

看到这里大家肯定有些郁闷了，你在这唠叨半天，这和redis有半毛钱关系啊？这可是redis专栏啊喂！

大家稍安勿躁，咱们儒猿学院至始至终不忘初心，以打造精品为己任，自然不会犯这种低级错误。既然是我们有意为之，那么事情绝不会像看上去那么简单，我们的主角redis马上就要登场了。

现在的架构看上去十分完美，但是还是有一些问题的，请同学们回看一下，我在前面强调三遍的内容，因为强调的内容是能够使用SSI技术解决全量渲染的前题，再讨论这个问题之前大家先明确的知道一点那就是SSI技术在这里起到的作用是对html文件的组装，所以当整个骨架都发生变化的时候原先组装在这套模板里的所有html文件都要重新排列组合，那就必然会导致全量的重新渲染，场景大致如图17：

![骨架变更引发的全局渲染 (2).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/58345300_1616071336.png)图17

那么现在问题就比较明显了，因为作为一套后端架构，他的性能直接与前端页面布局有一定的联系，这是比较不合适的，本质上我们只关心数据的填充，页面怎么布局我应该不需要care，况且ssi是基于磁盘让html进行组装，这个方案性能本身就不算多好。

那我们应该怎么才能即保证高性能的数据读取，又能保证高性能页面渲染呢？

既然提到高性能的数据读取，自然就该轮到我们的主角redis登场了，那我们有什么技术手段，能让ngnix加载到redis中的数据用，并且渲染到html页面里呢？我们就要利用 lua脚本以及动态渲染模板来完成这些功能。

并且此时我们不涉及到什么页面的推送，最终我们的架构大致就是如图18所示：



![架构3.0新 (1).png](http://wechatapppro-1252524126.file.myqcloud.com/image/ueditor/20719700_1616071337.png)图18

这一次优化过后优化就比较到位了,商品详情页系统在接收到数据源的数据，根据数据维度的不同将数据写入对应redis的中就可以了，在用户发起请求路由到对应的商品详情页所在的ngnix上，通过lua脚本从各个维度的redis中读取到对应的数据，接收到数据之后 再由ngnix动态渲染到html模板中，再也不用囤积大量的html碎片了，最终一个完整的html就这样展现在我们眼前了。

并且由于ngnix只存放页面模板，分发层起到的不仅是负载均衡的作用，也保证了数据访问层ngnix的高可用·。这绝对是符合百万流量电商的架构标准，真正的顶尖硬货！

没说错吧？redis的在这套架构里扮演的绝对是举足轻重的主角，无论是读还是写，它都是最为核心的一环，所以在这个专栏里 可能会迟到，但绝不会缺席，而且必须是这篇文章里最亮的仔。

### 077_项目案例：某大型电商平台疫情期间预约+秒杀预约营销活动的架构设计（一）

#### 1、案例背景介绍

2020年新冠疫情席卷全球，全世界人民都加入到这场无硝烟的战争，中国在众多医务工作者的不懈努力下，在这场战役里取得的成绩，可以说是独占鳌头，让祖国母亲在全世界面前是那么的熠熠生辉。让我们把记忆拉回到疫情初期那段日子，开始今天的案例讲述。

突如其来的疫情，使得口罩成为全国人民争相抢购的必需品。在疫情最严重的那段日子里，某电商组织力量采购了一批口罩，面向全国人民发售。由于库存十分有限，运营采用了预约+秒杀的营销模式，也就是用户先预约，到了指定时间才能通过秒杀系统抢购。

由于大家对口罩的需求太过强烈，活动一上线就遭到用户热捧，一度还冲上微博热搜 ，如图1：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmjb9z7n03ao.png)图1

#### 2、系统面临的挑战

由于这个系统过于复杂，我们将用好几讲的时间来向大家讲述这个案例。为了能让绝大部分同学更好的学习本专栏，今天仅仅讲解一些必要的前置内容，作为这个案例讲解的开胃小菜。

首先大家看一下上节课商品详情页的最终版本架构图，如图2所示：

![图片2.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmjbabkg03s1.png)

 图2

上图中通过lua脚本语言，从redis服务器加载用户所需的商品信息，然后通过lua脚本将加载的数据渲染到页面模板上这个是纯粹的动态渲染。之所以采用动态渲染的方式是因为商品千千万万，没法穷举，所以只好通过加载数据去填充了。 如图3所示：

![图片3.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmjban9d0r52.png)

 图3

从图3中我们可以看到，如果现在是一个口罩抢购的专场，实在没有必要每一次从redis那边读取数据，自己还得把这些数据填充进去，可以说是吃力不讨好，我们完全可以将页面直接静态化，然后推送到ngnix上就可以，那又该如何实现呢？

这时候就需要静态化页面的模板引擎出场了，现在主流的静态化模板引擎主流的是freemarker和thymeleaf ，下面我将提供一个freemarker静态化页面的demo，大家可以自行尝试观察结果。

#### 3、编写pom文件

<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"

​     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

​     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <groupId>org.example</groupId>

  <artifactId>freemarker_test</artifactId>

  <version>1.0-SNAPSHOT</version> 

  <dependencies>

​    <dependency>

​      <groupId>org.freemarker</groupId>

​      <artifactId>freemarker</artifactId>

​      <version>2.3.23</version>

​    </dependency>

  </dependencies>

  <build>

​    <plugins>

​      <plugin>

​        <groupId>org.apache.maven.plugins</groupId>

​        <artifactId>maven-compiler-plugin</artifactId>

​        <configuration>

​          <source>1.8</source>

​          <target>1.8</target>

​        </configuration>

​      </plugin>

​    </plugins>

  </build>

</project>

#### 3、编写thymeleaf模板test.ftl

<html>

<head>

    <meta charset="UTF-8">

</head>

<body>

姓名：${name}<br/>

年龄：${age}<br/>

</body>

</htm

#### 4、编码主函数

public class Main {

  public static void main(String[] args) throws Exception {

​    Configuration cfg = new Configuration(Configuration.VERSION_2_3_23);

​    // 设置编码

​    cfg.setDefaultEncoding("UTF-8");

​    // 模板文件的存放目录

​    cfg.setDirectoryForTemplateLoading(new File("F:/"));

​    // 获取模板文件

​    Template tpl = cfg.getTemplate("test.ftl");

​    // 数据

​    Map<String, Object> data = new HashMap<>();

​    data.put("age", "18");

​    data.put("name", "张翠花");

​    try (FileWriter out = new FileWriter(new File("F:\\test.html"))) {

​      tpl.process(data, out);

​    }

  }

}

#### 5、场景描述图

![图片4.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmjbb1gy030u.png)

 图4

#### 6、总结

今天带大家介绍了一下业务场景，下节课我们会分析如何让用户更快捷的访问静态资源。

### 077_项目案例：某大型电商平台疫情期间预约+秒杀预约营销活动的架构设计（二）

#### 1、如何让用户更快捷的访问静态资源 

身为一个程序员你一定有访问外网的经历，那你是否觉得访问外网时，要比国内网站慢的不是一星半点，常言道远亲不如近邻，远水救不了近火，距离近了自然可以更好的呼应和响应，那资源访问也是同理，如图5：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmkqqlks0qf5.png)

 图5

那在这种情况下我们怎样才能更快更稳定的访问国外网站呢？如果一个网站考虑到让四面八方的国际友人，都能流畅访问，可以在不同的国家增设静态资源站点，将用于展示的静态资源推送到对应的站点上，这样国外的用户就可以就地取材，看到自己想要的信息了，如图6：

 ![图片6.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmkqr8st0x4x.png)图6

增设了资源站点以后，用户体验大幅度提升，这就是所谓的CDN 全称是Content Delivery Network，即内容分发网络。有了它，每位用户都可以依靠距离最近的服务资源站点，更快更好的访问静态资源了。

#### 2、仔细想想：这和口罩预约系统，有什么关系呢？

如果没有给口罩预约系统，场景会像如图7所示：



![图片7.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmkqtw180jd8.png)

 图7

增设了CDN之后，场景如图8所示：

![图片8.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmkqubub0cnx.png)

图8

大家及可以看到，在增设了CDN之后，上海和北京这样的一线城市，都被各自地区CDN给承接，大大减轻了数据访问层nginx的压力。

#### 3、跟紧脚步，看一下用户是如何访问到距离自己最近的cdn节点的呢？ 

不过在讲解这些内容之前希望家一定要清楚以下点：

- 当你有了一个CDN服务器时，你必须设置一个CNAME,用来映射CDN的域名，CNAME是你在DNS里为你实际要访问的域名的一个别名，你通过CNAME定位到你要访问的实际域名，再由DNS将这个实际域名解析成IP地址并进行访问，例如：

1. 1. CDN域名：shishan.com
   2. ip: 180.101.49.12
   3. CNAME: ruyuan.com

解析顺序如下：shishan.com -> ruyuan.com -> 180.101.49.12

- cdn是由特定运营商提供给我们的，例如BAT就是CDN的运营商，这些运营商在全国、乃至全球的各个大枢纽城市都建立了机房，并且部署了大量拥有高存储容量，高带宽的服务器，构建了一个跨地域，跨网络运营商的专用网。
- 这就意味着CDN是以大规模的集群形式提供服务的，有集群那肯定的有用作负载均衡的机器来分发请求，这个分发器可不一般，它有一个响亮的称呼——全局负载均衡器，马上就带大家看看它特殊在哪。

不废话，先来一张图，图9如下所示：

![图片9.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmkqx5bj07jf.png)

 图9

好了，图先画到这里，相信有一定网络基础的同学，应该都能明白图上画的东西，不明白的同学学习一下石杉老师在面试突击第三季中讲解有关网络的内容。

我现在想问大家一个问题从第三步开始当对shishan.com 这个域名发起访问的时候，这个域名所在的DNS返回给我们用户的是什么？是一台CDN站点的ip，还是CDN站点的域名呢？其实都不是，返回的正是全局负载均衡器的域名，如图10：

 ![图片10.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmkqxun30m86.png)图10

好了 这个全局负载均衡器正式登场了，所谓全局负载均衡器是能够综合判断多种因素，为当前用户分配最合适的IP地址，这些评判因素大致包括以下几点：

1. 根据用户 IP 地址，判断哪一台服务器距用户最近
2. 用户所处的网络运营商
3. 根据用户所请求的 URL 中携带的内容名称，判断哪一台服务器上有用户想要获取的内容
4. 查询各个服务器当前的负载情况，判断哪一台服务器有较强的服务能力

#### 4、回头看看：怎么样？听起来是不是很酷？

为了让大家更好的了解到它的强大，以及方便我对后续讲述内容的讲解，请大家看图11（为了保证图的清晰度，我免去了一些网络相关的通信细节，这样也会让大家更好的理解CDN核心的一些概念）

![图片11.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmkqziob0n9u.png) 图11

上图大致描述了全局负载均衡器的作用，大家应该也都注意到了图中绘制了好几个CDN节点，并且有一些似乎是有层级关系，并且下一层是上一层的冗余备份。完全正确，这些不在同一行的CDN节点氏父子关系。

当用户没能在当前的CDN服务器找到需要的资源，就会从自己的父节点寻找所需资源。如果最终未能以这样的方式在CDN上找到资源，就从网站的主站上搜索资了，这个过程被称为回源，如图12：

 ![图片12.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmkr09xb0c7v.png)图12

那么想必大家都清楚了CDN的主要搜索原理了，此时我再给大家解释一些名词就简单很多了。

1. 边缘节点：没有子节点的CDN节点 也就是图中淡黄色节点
2. 区域节点：有父节点也有子节点的CDN节点，也就是图中橙色节点
3. 中心节点：拥有最为全面的CDN数据，是所有其他CDN的祖先节点，也就是图中绿色节点
4. 源站：中心节点的CDN的数据源，也就是图中浅蓝色部分

技术铺垫的差不多了，结合这些知识我们完善一下图7后如图13所示： 

![图片13.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmkr1dtk0sh0.png)

 图13

从上图中我们可以看到，如果用户只是正常的浏览页面，不进行任何业务操作的前提下，从上海和北京这样人口密集的一线城市产生的访问流量，直接由各地的CDN抗下；此时就只有未在本地搭建CDN服务器的扬州买家会直接访问主站nginx，可以说给这套案例的建设开了个不错的头。

### 077_项目案例：某大型电商平台疫情期间预约+秒杀运营架构系统的设计（三）

#### 1、上节回顾

上一次我们讲到用模板引擎技术+CDN的设备搭建很好的减轻了了ngnix和redis的负载，也很好的解决了用户频繁刷新页面造成的不必要的开销。

#### 2、令人始料未及的预约火爆程度

虽然某电商平台经历了很多次大促的考验，整个系统可以说是固若金汤，这次的口罩预约情形任然打他们了个措手不及，主要是两方面的原因：

预约情形之火爆，远远超过预期，口罩预约系统一上线，预约用户就像江河决堤一般，倾泻而来，预约人数急速增长，远远超过可供应库存的几百倍 如图1所示：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmm36fyh0rou.png)图1

联想到用户对口罩的需求之迫切，如果走正常的交易链路，让巨额流量涌入核心的关系型数据库中，将会对整个系统造成什么样的冲击是不敢想象的，因这样的情形俨然就是成为一场史无前例的爆款秒杀，如图2所示：

![图片2.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmm37fw20w24.png)

 图2

你看，一旦mysql承接了巨额流量，订单和物流两个核心系统将直接受影响。和业务方磋商之后决定提前结束这一场口罩预约，并为口罩预约成功的用户增设了一场秒杀活动，即后台走秒杀抢购链路，确保核心交易链路的安全 大致情形如图1：

![图片3.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmm37vph03x4.png)

图3

看着监控中逐渐降低的增长态势，大家稍微松了口气，但是也不敢有丝毫的懈怠，因为大家都知道将要面对一场史无前例的单品爆款秒杀。接下来就为大家介绍一下，这套秒杀架构。

#### 3、秒杀系统的架构简介

就像前文说的那样由于预约的人数，远远超过口罩的供给库存，如果不做一点限流和熔断，就会出现图4的情况：

![图片4.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmm38vwu0xqr.png)

图4

 那么我们可以通过lua脚本来进行限流，避免库存系统承接不必要的压力，如图5：

![图片5.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmm39y110y14.png)如图5

但是这样也有一个问题，放行的流量里在对库存进行操作的过程中，一旦发生了网络波动，机会导致有口罩未能正常被秒杀完如图6所示：

 ![图片6.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmm3agty0sm5.png)图6

那么在实际场景中为了避免这种球情况，限流的时候就需要多放比秒杀库存多10%的流量才行如图7所示：

![图片7.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmm3azba07kf.png)图7 

想必大家都应该明白秒杀场景下库存的扣减不会说基于mysql进行库存的更新，如果你打算用mysql去扛这些流量，我觉得你可以考虑准备一下面试题了。而且库存服务一定是电商平台多个服务依赖的核心链路，我们不应该让让如此核心的系统直接面对怎大的流量，如果不做改进会出现以下情况，如图8所示：

![图片8.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmm3br4k0qhe.png)图8

### 077_项目案例：某大型电商平台疫情期间预约+秒杀运营架构系统的设计（四）

#### **1、循序渐进** 

那么如果要改进的话，需要引进什么，还需要多说吗？如图9所示： ![图片9.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmnbnbhu04ab.png)

图9

也就是说在秒杀开始之前，可以让运营人员发布秒杀场次的时候，提前把秒杀时所售卖的库存预热到redis中，这样秒杀开始的时候时就不会像现在这样承受这巨大的并发量。

那么问题又来啦 秒杀只是为少数几个单品举办的一场限时限量打折促销的活动，并不是说 一件单品在其他时候不能售卖，只是花的钱更多，那这个时候就会出现如下的情况,如图10所示：

![图片10.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmnc92k80j8u.png)

 如图10

那么从中可以看到 如果运营人员在今天发布了口罩秒杀活动，但非秒杀时段，也可以通过正常的销售渠道买到口罩，那么就有可能导致用于秒杀的库存不足，这是极其坑爹的；从这个事情可以看出，先来后到的规矩还是挺重要的，那么就得让库存系统遵守这个约定才行，这个就要所涉及到库表结构的一个设计了，如图11所示：

![图片11.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmnca0tf09sk.png)图11

那么现在就比较稳妥了，明确的给库存系统设置了明确不能售出的量，既然设置了锁定的库存，那么根据它也可以延伸出已销售的库存量，方便各业务线进行查询及相关业务操作。

此时我们就知道了口罩秒杀系统可操作的库存是从redis里获取的；换句话说，此刻的redis就是口罩秒杀系统的库存系统，那么性质确定了，数据结构也就明确了——redis中维护库存的数据结构也要和库存系统里一样。如图12所示：

![图片12.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmncaoas0crs.png)

 图12

难道到这里，你就觉得已经高枕无忧了？不是的，因为现在有多余秒杀售卖库存的流量涌入，那么就会引发一个棘手的问题——库存超卖问题。大家学了这么久的redis专栏，想必一定知道redis是单线程执行的，貌似不会有多线程引发的问题，实则不然，在并发情况下大家都是单纯的写操作，没有任何逻辑判断，自然是没问题，但是有了逻辑判断情况就完全不一样了，如图13所示：

![图片13.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmncb6t007o8.png)

 图13

那么怎么解决这个问题呢？想必大家一定会不假思索地说，那加把锁不就行了吗？是的没错，这是一种解决问题的方法，但有比较明显的问题，且听我细细道来。

#### 2、细细道来，锁有哪些实现方式？

- 悲观锁

想必大家都能理解，一旦用它，就意味着锁的争抢，以及锁的释放，对于秒杀这种，对并发吞吐量要求高，对时间较为敏感的场景可以说几乎不考虑。

- 乐观锁

这个可以借助redis中的 pipeline实现，对与这套方案，前面专门有一节视频课程进行讲解，具体实现可以参考之前课程内容的代码，缺点其实也很明显，不断的watch一个key的值是否发生了变化，大家仔细想想这是不是和并发编程里CAS的机制，可以说是一摸一样，那么缺点也就比较明显了——CPU负载过高，尤其是秒杀这种场景，库存变更非常的快，体现的更为明显。

那么锁不行，还有别的方案吗？大家可能会想到我以前讲解过的，内存队列的那种方案，使得请求可以顺序串行化处理，理论上也是可以实现的，如图14所示：

![图片14.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmnccyf60dz4.png)图14

缺点也比较明显，秒杀的瞬时请求量巨大，堆内存空间有着极其巨大的风险，这也是为啥不建议采用的缘故。

最好的永远放最后，也是大家的老朋友采取lua脚本的方式来解决 ，因为lua脚本的执行是能够保证原子性的，并且脚本与脚本的执行是串行化的，伪代码大致如下：

\#获取商品库存信息       

local counts = redis.call("HMGET", KEYS[1], "totalCount", "selledCount","lockedCount");

\#将总库存转换为数值

local totalCount = tonumber(counts[1])

\#将已销售库存转换为数值

local selledCount = tonumber(counts[2])

\#将已锁定库存转换为数值

local lockedCount = tonumber(counts[3])  

\#如果已销售库存量加上锁定库存再加一仍然小于或等于总库存量，就可以更新库存     

if selledCount + lockedCount + 1 <= totalCount then

  \#更新已秒杀的库存量

  redis.call("HINCRBY",KEYS[1],"lockedCount",1)

 return 1;  

end        

return 0

你想想类似于这样的代码 既能原子性又可以保证一套完整的逻辑执行起来是串行化的，性能还很高，是不是美滋滋？

#### 3、步步紧逼：是否还有优化的空间了？

文章开头提到，由于口罩预约情形过于火爆，使他变为了一个热点产品，那么就有可能导致，潮水般的流量都集中在一台redis上，导致单台redis负载过高，那么这样肯定也是不行的，这个时候我们可以使用库存分片的策略来解决，如图15所示：

![图片15.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmnce70t0tpk.png)

 图15

#### 4、结束语

花了大概四讲的时间向大家展示了一个秒杀系统最基础的前端架构，后端限流以及如何基于redis去做秒杀产品的。

库存更新方案，当然一个完整的秒杀系统是不可能怎么简单的，其中还涉及到时钟同步，tomcat调优，拦截恶意请求，等等。

都讲述出来未免有些喧宾夺主，如果想了解更多的秒杀课程相关的知识，可以关注儒猿学院各位老师呕心沥血正在研发的儒猿实战云平台，最后与我架构班的秒杀课程梦幻联动起来。

### 078_项目案例：某大型电商平台交易系统架构设计

#### 1、开篇词 

上一节我们讲到由于口罩预约系统异常的火爆，我们不得不提前终止了口罩预约活动，鉴于大家对口罩的迫切需求我们不得不为其增设了秒杀专场，让口罩抢购流程通过电商平台为应对超高并发场景。

建设的秒杀系统，以保证核心交易系统及交易链路的绝对安全，大家兴许好奇这个交系统到底是个啥，听了我的描述大家兴许好奇，这个交易系统到底是个啥？听起来好像高大上，同时又非常的抽象，让人觉得颇为的神秘。大家稍安勿躁，这篇文章我就会缓缓揭开交易系统的神秘面纱。

#### 2、仔细想想交易系统有啥用？

大家首先要思考一个问题，何为交易？ 说白了就是一笔买卖。从商家的角度看，把自己货架上的商品推销出去，就算一比是一笔买卖达成了。我们暂且不讨论售出之后会不会产生任何的退款纠纷，因为严格来说那属于售后范畴，也改变不了这件商品曾经被商家推销出去的事实。

那么从电商的角度来看，一件商品是否被成功推销出去最最核心的依据就是一笔订单有没有被创建出来。无论用户最后是否付款成功，只要用户按下“提交订单”这个按钮，就说明把产品推销出去了，也就产生了一笔交易。明白了这个概念，我们现在可以知道交易系统能是什么了——能够促进用户下订单的系统就可称为交易系统。如图1：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmp1yyi10kb8.png)图1

图1中我们看到，交易系统展现了商品价格的低廉，将商品推销出去，促进了用户下单，但往往推销商品的手段和方式又何止凭借价格低廉这一种 如图2：

![图片2.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmp1zhq00ycl.png)如图2

那么我们可以看到一个交易系统，可以对产品进行各种维度的推销包装，来吸引用户下单。上面列举的促销维度大致包括：广告，结算策略，支付方式，商品详情页，秒杀场次

这些维度的信息往往由对应的服务来维护，大家可以想象这么多维度的数据都交易系统来维护的话，那么所有请求都在交易系统进行复杂的逻辑处理，交易系统一定会不堪重负的 如图3：

![图片3.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmp1zzp90ssp.png)图3

#### 3、交易系统服务架构长啥样？

所以此时想必大家都知道了，这个时候就需要依据各个维度的数据进行服务的拆分，也就是划分微服务，此时一个交易系统是由多个微服务提供相应的功能来完成自身所负责的业务功能，反过来说交易系统对各个微服务进行整合 如图4：

![图片4.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmp221jq094n.png)图4

其中值得大家注意的一点是我在图中我用蓝色填充的购物车模块，原则上它不能算是一个促销维度，但是当你对购物车里的商品进行变更时，也会触发例如，运费以及折扣的计算，影响着订单的提交金额，顾也算是交易系统的一部分。

通过图4大家就可以知道，在这样的架构下，交易系统就是一个业务中台，那么此时我们要对一件商品进行下单时，就要通过交易系统，将这个过程中涉及到的一些服务，根据业务需求的不同进行串联从而形成一条交易链路，如图5：

![图片5.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmp23l0t0n3w.png)图5

看了图5想必大叫都知道了，为什么当发现预约人数严重超标时，宁可提前结束预约活动，都要保证交易链路的安全，因为这条链路上只要一个服务被巨额流量打垮，直接就有可能影响整个电商网站的正常运转。因此需要为口罩增设秒杀专场，此时交易链路就很不一样了，如图6： ![图片6.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmp2449c09qn.png)图6

从图6中我们可以看到秒杀的链路，商品详情页的页面都是直接做了静态化处理，收银台也只牵扯到了金额计算相关的服务，自然不影响正常的交易链路，中间的秒杀系统我在上一讲文章中提到如何进行建设，在此不多赘述。

#### 4、总结

大家是否注意到我用黄色标出的链路节点没有？这些节点都是有对应的前端页面，正所谓爱美之心人皆有之，一个美观的前端界面也是吸引客户下单的重要因素，那必然是交易系统重要组成部分。

不仅如此，还要保证任何一种客户端访问都能提供良好的观感体验，在电商竞争环境如此激烈的当下，通过前端页面，或者是有对应界面的前台系统不断的更新迭代。让电商平台有更加出色的用户体验。

这类系统往往自身的业务功能并不复杂，主要是依赖更多后端服务来完成与客户端的交互，也是与客户端最为贴近的服务系统，被称为前台。为前台提供数据支撑和接受前台业务请求的服务，这类系统被称为中台。所以前台系统和中台系统完美搭配才能构建一个完整的系统 如图7：

![图片7.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmp26mfq0j3a.png)图7

### 079_项目案例：某大型电商平台预约中台系统的整体架构设计

**1、上节回顾**

上一讲我给大家讲述了，交易系统的意义，以及其架构设计，今天我打算和大家讲一讲此次爆品事件的吹哨人—预约系统，如果没有对预约系统的观测，就有可能导致一场P0级别的事故。现在想来也是脊背放凉，后怕不已。

#### 2、预约的意义何在？

老规矩在讲一个系统架构之前，先给大家解释清楚一个系统的意义，这也是为了方便大家理解系统架构的设计思路，预约的本质就是获得一件商品的购买权，没预约不让买，预约了也会买不到，场景一般适用于，新品发布，靠商品首发在竟对环境中凸显优势，如图1：

 ![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqd60wt0g8x.png)图1

以及因为某件商品库存紧缺，通过预约的方式把有限的库存尽可能合理分配，如图2：

![图片2.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqdbozg04c3.png)图2

也就是说预约本身并不能简单理解为一种促销手段，它更像是一种市场调研，亦或是凸显首发优势增强用户好感及黏性。所以对于预约商品来说与普通售卖商品的交易链路本质是一样的，只是在它的链路上多了自己业务处理如图3：

![图片3.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqdc7eo0o8b.png)

图3

那么现在大家就明白，此次口罩预约活动的本意是为祖国母亲尽一份绵薄之力，没想到却引发了一场史诗级的秒杀，还造成了一定的社会反响。。。。

#### 3、预约中台的架构介绍

大家想一下如果一件商品有用户已经预约了，那么此时商户如果把商品的款式，甚至是价格进行修改的话，那么商户的信誉势必会有不小的损伤，所以一旦一件商品发布了相关的预约场次是几乎不可能修改的，是典型的读多谢少的场景。

可以将这些有预约场次的商品信息写入redis，以保证读取数据的性能，但是不怕一万就怕万一，商品信息毕竟是核心的业务数据，一定要保证其查询的时效性，如果做不好的话就会出现如下的情况，如图4：
![图片4.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqdewgz0u52.png)图4

所以在更新数据前一定要删除缓存，避免用户读取到脏数据如图5：

![图片5.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqdg8pa060c.png) 图5

那么现在是否就万无一失了？还差一点，因为如果用户并发访问系统，一个进行写，一个进行读会出现如下情况，如图6：

 ![图片6.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqfpsmc0ovu.png)图6

那么此时我们可以采用内存队列的方式来解决，如图7：

 ![图片7.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqfqeqc09jo.png)图7

大家注意一下图7我始终没有两个读请求，道理很简单，如果前一个是读请求的话，就意味着没有发生数据更新，没有任何必要进行redis和mysql的同步操作，自然也没有必要入队列了。

当然预约系统中，还要记录预约用户信息，但他们业务处理逻辑是一样的，但由于数据的特质不同架构上是有一些区别的，这在之后的文章里在向大家介绍。

这套预约系统的架构根据我们现在学到的知识大致如图8所示：

 ![图片8.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqfqyzq0dcv.png)

如图8

好了预约系统架构大家也清楚了，现在和秒杀链路拼接起来如图9：

![图片9.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqfrk9304ft.png)图9

### 080_项目案例：一张图剖析某大型电商平台预约中台的黄金链路

#### 1、上节回顾

上一讲我向大家讲解了预约中台的架构设计，本讲我将给大家讲解清楚预约业务的相关核心要素并梳理预约业务的处理链路。

#### 2、时间统一的重要性

大家先看图1内容：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqg3znr04bb.png)图1

刨除掉预约商品本身的信息不谈，你觉得图中哪些信息是你作为商品的预约用户所关心的呢？那毫无疑问的就是预约活动何时开始抢购，也就是说作为预约用户你关心的是这场预约活动的状态，而决定预约活动状态最为关键因素就是时间如图2所示：

![图片2.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqg6aud0bss.png)图2

那么时间和预约活动的状态有着密不可分的关系，那就意味着前端的展示时间，一定要和后端进行逻辑处理时所获取的时间一样，并且现在都是前后端分离，并且后端系统架构也都是分布式的，所以一定要确保预约系统所涉及到的机器，用于处理业务的时间一定要统一，否则就会出现图3的状况：

![图片3.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqg6y3i02i5.png)图3

为了防止这样的情况，就不能让前端系统和后端系统依据他们的本机时间，必须有一个方案确保它们的时间没有误差。如图4：

 ![图片4.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqg7e5n05vq.png)图4

由此大家可以看到，预约自身重点就是在做数据采集，并没有太多复杂的业务牵涉，状态机的流转直接和时间挂钩，基本不涉及到其他复杂的逻辑判断，所以如果仅仅是预约商品的的话，业务处理链路大致如图5：

![图片5.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmqg85t303vz.png)

 图5

#### 3、总结

看上去一切都显得那么的简单，没错，预约的业务链并不复杂，但是别忘了，我们正处于百年一遇的疫情环境下，一切习以为常的事情，总会有或多或少的变化。

口罩本该是最为普通的生活用品，我们也仅仅想在这样的环境下为全国人民做一些事情，买家最为普通的生活诉求加上卖家最为单纯的赤子之心，两者相遇本该刮起阵阵暖流，却没想到在疫情的催化下，让某电商的程序员惊出一身冷汗，这就是下一篇文章要讲的内容了。

### 081_项目案例：有条不紊：面对突然其来的百万级流量，预约中台应该如何应对？

#### 1、开篇词

上一篇文章给大家讲述了预约业务的相关链路以及在预约系统中时间同步的重要性并给出了解决方案，看似一切都挺正常。但是随着疫情的到来，一些问题也渐渐暴露出来。

#### 2、预约中台在疫情环境下显露出的问题

前文说过口罩预约情况，出奇的火爆，让所有人大吃一惊，预约系统的问题大概体现在如下几个方面：

首先从业务层面上来说，预约口罩的人数量高出了可供库存，正是因为这种供不应求的情况已经到了无以复加的地步，直接让这场口罩预约活动直接冲上热搜。

这一方面说明了某电商开展这个活动，是非常有益的，吸引了大量用户参与其中，但另一面也是因为参与人数过多，致使大量用户最终抢购失败，虽然广大用户对这样的情况表示理解。

但毕竟是一种非常差的用户体验，也难免大家会在微博上进行小小的吐槽。所以从业务上来说当发现供不应求到了一定的比例，就要以一种全新的购买方式，稀释无效的参与人数以增强用户体验。

其次，这一次口罩预约的问题之所以能够被发现，只是在一次普通的例行检查中，被发现，这么严重的问题，居然没有计入系统告警系统。如果值班人员稍有疏忽，造成的结果是难以预估的，所以对于预约系统，应该增加对爆品SKU的监控才行。

用户的热情本是对这场预约活动的最好的认可，可某电商的程序员同学们，却一点也开心不起来，用户的热情汇聚成了泛滥的洪水，而那个“等待抢购”的置灰按钮就像是洪水的闸门，抢购的按钮一旦亮起，洪水的闸门打开，可以说对整个交易链路的参与者们，足以形成水淹七军之势，他们必将毫无招架之力，被弄得人仰马翻。如图1：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmr519te0yd9.png)图1

可见如果那个闸门一旦打开局面可谓是一地鸡毛。。。。

#### 3、这些问题又该如何解决呢？

首先从从业务说，如果预约人数远远大于商品的供应可供应量时，应该采取摇号的方式这样，参与抢购的人数少了，服务器压力也不会很大，参与抢购的用户也能如愿的购买到需要的商品，未能参与抢购的用户也只是觉得运气不佳，不会有很大的埋怨。

再者就是监控，根据压测结果计算出一个核心交易链路所能承受的TPS阈值，高于这个阈值时发出警告，让相关人员能够引起警觉，尽早发现问题，给与开发人员更多的处理时间。

接着，预约服务的后台逻辑处理上的改动，应该根据预约人数情况，添加熔断机制，即预约参与人数达到一定阈值后，切断之后的请求通向交易链路的通道，如图2：

![图片2.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kmr51wvi0ldg.png)图2

#### 4、现在为之奈何？

以上提的方案都不错，但说这些为时已晚，不能抱着事后诸葛亮的心态看问题。现在的“洪水”就在“闸门”之外，涌进交易链路只是时间问题，既然将流量看作是洪水，那么现在就必须对“洪水”进行“引流”，避免当抢购时间一到核心链路受到毁灭性打击，这就是我们下节课要讲的内容，大家敬请期待。

### 082_项目案例：停下脚步：重新思考一下预约系统的性能瓶颈！

#### 1、开篇词

上一讲我们讲到千万用户对口罩的抢购热情，本该是一件让某电商平台感到欣慰的事情，但是没想到这份热情，最后会变成一头洪水猛兽，一旦抢购时间到了，这头猛兽将会把某电商的核心交易链路无情的吞噬，不由得让人心惊肉跳。不过今天我为了今后讲课的流畅程度，我们先来思考一下预约系统有啥问题没有？

#### 2、思考一下现有的预约系统应对热门爆品时有啥缺陷？

我们在前面讲过，用户进行预约会涉及到两个维度的数据变更一个是用户信息，一个是SKU信息，如图1所示：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kn59a4210gdv.png)图1

正常来说这么搞一点问题没有，即便涉及到写数据库，但是每个sku以及用户信息，都可以通过分库分表中间件将各种不同的SKU和用户读写请求分摊到各个机器上，那么每台机器在正常情况下负载都不会很高。但是现在大家有意识的将请求集中在某几个SKU，甚至是某一个SKU这就有问题了，如图2所示：

![图片2.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kn59ap740sgc.png)图2 

类似于上图有一个热点爆品出现时，流量就都会聚集到某几个甚至是某一个机器上，能参与到其中分摊流量的节点少的不是一星半点，如果说擅自的改变路由策略，且不说会不会影响到之前正常的业务流转，当下时间也是非常的紧迫，从开发，测试，在上线根本不现实。

现在更严重的是，预约系统作为交易的发生源之一，也就是说预约系统是整个交易链路中比较靠前的一个环节，那么此时在预约系统这里发生了流量倾斜问题，势必也会对整个交易链路中的后续的服务节点产生影响，如图3所示：

![图片3.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kn59bf5p0q4n.png)图3

#### 3、总结

可见当一个节点有了负载有压力时，作为同一根绳上的蚂蚱，其他节点也很难幸免遇难，那么在预约服务节点发生流量倾斜时，一切还有挽回的余地吗？大家且听下回分解！

### 083_项目案例：打开脑洞：营销活动如何无缝切换为秒杀活动？

#### 1、开篇词

上一讲我们讲到由于预约系统发生了流量倾斜，极有可能导致整个核心交易链路的安全受到威胁，但是事实摆在眼前，得想办法把事情解决才是啊，现在唯一的办法就是找到一种引流的方式，将这巨额的流量迁移走。

#### 2、无奈之举——提前取消这场抢购活动

现在口罩的巨额流量还在不断地增长，并且距离这场预约活动结束还有数个小时，此时如果在不采取措施预约的人数完全有可能达到几千万。

经过和业务方的磋商，由于口罩的可供应库存已经远远小于预约人数，活动开展下去也没有什么实际意义，遂业务方同意，将活动提前取消，但是要保证已经预约口罩的用户能够正常参与抢购。

在业务方的授意下，我们提前停止了这一场预约活动。看着监控里的预约人数逐渐走低，悬着的心终于可以放一放了，即便如此大家也丝毫不敢懈怠，因为现在必须将现有的巨额流量进行迁移才行并且要保证这些用户能够正常的参与抢购。

这时候就必须把这些流量迁移到秒杀系统了，怎么迁移呢？简单点说就是为口罩SKU新建单独的秒杀场次并且需要将相关的预约的用户数据迁移到相关秒杀场次的数据库里。如图1所示：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kn5avb4p03t8.png)

​                           如图1

#### 3、总结

看到这大家可能就要问了，就这么容易吗？这个导数工具又是个啥？请听下回分解。

### 084_项目案例：悄无声息：用户如何无感知迁移历史数据到秒杀系统？

#### 1、开篇词

上一讲我们讲到，在和业务方沟通后我们最终采取了将本场预约活动提前取消的决定，并且为了保证已经预约成功的用户能够正常的参与抢购，我们为这批预约成功的用户单独发布了一场秒杀活动，这其中就一定会涉及到数据迁移的问题，本节课就好好和大家聊一聊吧。

#### 2、古老的迁移方案

再多年以前，互联网发展还不像如今这般迅猛，用户对网站的用户体验还没有要求如此之高，数据量也不像现在如此之大，那时通常还是关机备份，如图1所示：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kn5h9cck06a1.png)

图1

但你想一下，在竞争环境如此惨烈的当下，用户对网站要求更多，也更为挑剔了，如图2：

![图片2.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kn5ha0bs0gl0.png)

如图2那样，不能说怎么搞一定会导致用户，但也至少是给竞争对手以可乘之机，这也是大忌。那么有没有什么技术手段，能够做到为用户提供服务的同时，又能进行数据迁移呢？

#### 3、canal是什么

在讲解方案之前我想给大家介绍一个由阿里巴巴开源的一款binLog采集中间件，大致的实现原理，如图3所示：

![图片3.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kn5haijr0mn4.png)图3

这里就用阿里巴巴官方的图好了，从图中可以看出，它的原理非常的显而易见，就是将自己伪装成Slave,让mysql和它进行主从复制，然后通过Canal将数据同步给其他中间件，至于mysql主从复制的原理大家可以看看我的面试突击第一季的内容，讲解的非常详细，可以不谦虚地说，那里面的内容在所有的免费课程里都算得上一等一的顶尖硬货了。

那么此时就非常明了了，因为Canal可以将数据推送给MQ我们完全可以通过自己编写Consumer进行逻辑处理，再同步给redis就行了，如图4所示：

![图片4.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kn5hb0570lxz.png)

图4 

#### 4、Canal相关知识小补

接下来还是给大家介绍一下有关Canal的一些知识，如图5：

![图片5.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kn5hbilw0uc1.png)图5

- deployer：对canal进行部署
- paser:对采集到的binlog进行解析
- skin:对解析之后的binlog进行加工转换
- store:对加工之后的数据的存储策略
- meta:源数据维护，例如binlog日志消费到的下标等等
- protocol:canal服务端与客户端的通讯协议
- instance:一个canal服务可以对多台mysql binlog日志进行采集，每一台mysql主实例，都对应一个instance
- destination:决定用哪一个instance为客户端提供服务的核心参数

### 085_项目案例：争分夺秒：面对百万级流量，预约系统如何快速搭建熔断机制？

#### 1、开篇词

上一篇文章我给大家讲述了预约系统数据的迁移方案，但是这一次预约活动是强行的提前终止预约活动才限制住了流量的增长，但是接下来还有好几场口罩的预约活动，我们必须制定一个熔断限流的的方案才行。

#### 2、方案的确立

导致这次事件的根本原因是预约口罩的人数远远大于口罩的可供应库存，所以导致了大量无意的流量涌入，并且在已经确认巨额流量继续涌入已无意义的时候，服务窗口仍然继续为用户提供服务，这明显是非常不合理的。

那么问题已经明确了，方案也就呼之欲出，当口罩的预约人数和口罩库存达到一定的比例范围，就自动结束预约状态，这样把参与抢购的人数控制在一个核心交易链路可以承受的负载范围内，也避免大量占用秒杀系统的资源。

所以经过磋商，现在的业务流程大致如图1所示：

![图片1.png](http://wechatapppro-1252524126.file.myqcloud.com/apppuKyPtrl1086/image/b_u_5efbf242e79a5_klVaiiYb/kn5kgc7r0xln.png)

图1

#### 3、总结

看似现在一切都已搞定，但谁也没想到反而引发了另一个棘手的问题，这个问题值得好好说说，我将花好几讲的时间来阐述，敬请期待！



