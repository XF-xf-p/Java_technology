# Redis

## 1.Redis

Redis是一个开源（BSD许可）的内存中的数据结构存储系统，可以用作数据库、缓存和消息中间件，支持多种类型的数据结构，例如String（字符串）、Hash（散列）、List（列表）、Set（集合）、ZSet（有序集合）、Bitmap（位图）、HyperLogLog（超级日志）和Geospatial（地理空间）。Redis内置了复制、Lua脚本、LRU驱动事件、事务和不同级别的磁盘持久化，并通过Redis哨兵（Sentinel）模式和集群模式（Cluster）提供高可用性（High Availability）。

Redis不但支持丰富的数据类型，还支持分布式事务、数据分片、数据持久化等功能，是分布式系统中不可或缺的内存数据库服务。

## 2.Redis的数据类型

Redis支持String、Hash、List、Set、ZSet、Bitmap、HyperLogLog和Geospatial这 8种数据类型。

（1）String：String是Redis基本的数据类型，一个key对应一个value。String类型的值最大能存储512MB数据。Redis的String数据类型支持丰富的操作命令。常用的String操作命令。

- Setnx：只有在key不存在时才设置key的值
- Getrange：返回key中字符串值的子字符
- Mset：同时设置一个或多个key-value对
- Setex：将值value关联到key，并将key的过期时间设为seconds（以秒为单位）
- SET：设置指定key的值
- Get：获取指定key的值
- Getbit：获取key所对应的字符串值指定偏移量上的位（bit）
- Setbit：设置或清除key所对应的字符串值指定偏移量上的位（bit）
- Decr：将key中存储的数字值减1
- Decrby：将key所对应的值减去给定的减量值
- Strlen：返回key所储存的字符串值的长度
- Msetnx：同时设置一个或多个key-value对，在且仅在所有给定的key都不存在时
- Incrby：将key所存储的值加上给定的增量值
- Incrbyfloat：将key所存储的值加上给定的浮点增量值
- Setrange：用value参数覆写给定key所存储的字符串值，从偏移量offset开始
- Psetex：和SETEX相似，但它以毫秒为单位设置key的生存时间，而不是像SETEX那样，以秒为单位
- Append：如果key已经存在并且是一个字符串，则append将value追加到该字符串的末尾
- Getset：将给定key的值设为value，并返回key的旧值（old value）
- Mget：获取（一个或多个）给定的key的值
- Incr：将在key中存储的数字值加1

（2）Hash：Redis Hash是一个键值（key->value）对集合。Redis的Hash列表支持的操作。

- Hmset：同时将多个field-value（域-值）对设置到散列表key中
- Hmget：获取所有给定字段的值
- Hset：将散列表key中field字段的值设为value
- Hgetall：获取散列表中指定key的所有字段和值
- Hget：获取存储在散列表中指定字段的值
- Hexists：查看散列表key中指定的字段是否存在
- Hincrby：为散列表key中指定字段的整数值加上增量increment
- Hlen：获取散列表中字段的数量
- Hdel：删除一个或多个散列表字段
- Hvals：获取散列表中的所有值
- Hincrbyfloat：为散列表key中指定字段的浮点数值加上增量increment
- Hkeys：获取所有散列表中的字段
- Hsentnx：只有在字段field不存在时，才设置散列表字段的值

（3）List：Redis List是简单的字符串列表，按照插入顺序排序。我们可以添加一个元素到列表的头部（左边）或者尾部（右边）。列表最多可存储231-1（4 294 967295≈4亿多）个元素。List列表常用的操作。

- Lindex：通过索引获取列表中的元素
- Rpush：在列表中添加一个或多个值
- Lrange：获取列表指定范围内的元素
- Rpoplpush：移除列表的最后一个元素，将该元素添加到另一个列表中并返回
- Blpop：移除并获取列表的第一个元素，如果列表没有元素，则会阻塞列表直到等待超时或发现可移除的元素
- Brpop：移除并获取列表的最后一个元素，如果列表没有元素，则会阻塞列表直到等待超时或发现可移除的元素
- Brpoplpush：从列表中弹出一个值，将弹出的元素插入另一个列表中并返回它，如果列表没有元素，则会阻塞列表直到等待超时或发现可移除的元素
- Lrem：移除列表元素
- Llen：获取列表长度
- Ltrim：对一个列表进行修剪（trim），让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除
- Lpop：移出并获取列表的第一个元素
- Lpushx：将一个或多个值插入已存在的列表头部
- Linsort：在列表的元素前或后插入元素
- Rpop：移除并获取列表的最后一个元素
- Lset：通过索引设置列表元素的值
- Lpush：将一个或多个值插入列表头部
- Rpushx：为已存在的列表添加值

（4）Set：Set是String类型的无序集合。集合是通过散列表实现的，所以添加、删除、查找的复杂度都是O(1)。Set支持的操作。

- Sunion：返回所有给定集合的并集
- Scard：获取集合的成员数
- Srandmember：返回集合中的一个或多个随机数
- Smembers：返回集合中的所有成员
- Sinter：返回给定所有集合的交集
- Srem：移除集合中的一个或多个成员
- Smove：将member元素从source集合移动到destination集合
- Sadd：向集合中添加一个或多个成员
- Sismember：判断member元素是否是集合key的成员
- Sdiffstore：返回给定集合的差集并将其存储在destination中
- Sdiff：返回给定集合的差集
- Sscan：迭代集合中的元素
- Sinterstore：返回给定集合的交集并将其存储在destination中
- Sunionstore：返回给定集合的并集并将其存储在destination中
- Spop：移除并返回集合中的一个随机元素

（5）ZSet：Redis ZSet和Set一样也是String类型元素的集合，且不允许有重复的成员，不同的是，每个元素都会关联一个double类型的分数。Redis正是通过分数来为集合中的成员进行从小到大的排序的。RedisZSet支持的操作。

- Zrevrank：返回有序集合中指定成员的排名，有序集合中的成员按分数值递减（从大到小）排序
- Zlexcount：在有序集合中计算指定字典区间内的成员数量
- Zunionstore：计算给定的一个或多个有序集的并集，并将其存储在新的key中
- Zremrangebyrank：移除有序集合中给定的排名区间的所有成员
- Zoard：获取有序集合的成员数
- Zrem：移除有序集合中的一个或多个成员
- Zinterstore：计算给定的一个或多个有序集合的交集并将结果集存储在新的有序集合key中
- Zrank：返回有序集合中指定成员的索引
- Zincrby：在有序集合中对指定成员的分数加上增量increment
- Zrangebyscore：通过分数返回有序集合指定区间内的成员
- Zrangebylex：通过字典区间返回有序集合的成员
- Zscore：返回有序集合中成员的分数值
- Zremrangebyscore：移除有序集合中给定分数区间内的所有成员
- Zscan：迭代有序集合中的元素（包括元素成员和元素分值）
- Zrevrangebyscore：返回有序集合中指定分数区间内的成员，分数从高到低排序
- Zremrangebylex：移除有序集合中给定字典区间内的所有成员
- Zrevrange：返回有序集合中指定区间内的成员，通过索引按分数从高到底排序
- Zrange：通过索引区间返回有序集合成指定区间内的成员
- Zcount：计算有序集合中指定分数区间内的成员数量
- Zadd：向有序集合添加一个或多个成员，或者更新已存在成员的分数

（6）Bitmap：通过操作二进制位记录数据。Redis Bitmap支持的操作。

- setbit：设置Bitmap值
- getbit：获取Bitmap值
- bitcount：获取指定范围内值为1的个数
- destkey：对Bitmap进行操作，可以是and（交集），or（并集），not（非集）或xor（异或）

（7）HyperLogLog：被用于估计一个Set中元素数量的概率性的数据结构。Redis HyperLogLog支持的操作。

- PFADD：添加指定的元素到HyperLogLog中
- PFCOUNT：返回给定HyperLogLog的基数估算值
- PFMERGE：将多个HyperLogLog合并为一个HyperLogLog

（8）Geospatial：用于地理空间关系计算，支持的操作。

- GEOHASH：返回一个或多个位置元素的Geohash表示
- GEOPOS：从key里返回所有给定位置的元素的位置（经度和纬度）
- GEODIST：返回两个给定位置之间的距离
- GEORADIUS：以给定的经纬度为中心，找出某一半径内的元素
- GEOADD：将指定的地理空间位置（维度，经度，名称）添加到指定的key中
- GEORADIUSBYMEMBER：找出位于指定范围内的元素，中心点由给定的位置元素决定

## 3.Redis管道

Redis是基于请求/响应协议的TCP服务。在客户端向服务器发送一个查询请求后，需要监听Socket的返回，该监听过程一直阻塞，直到服务器有结果返回。由于Redis集群是部署在多个服务器上的，所以Redis的请求/响应模型在每次请求时都要跨网络在不同的服务器之间传输数据，这样每次查询都存在一定的网络延迟（服务器之间的网络延迟一般在 20ms左右）。由于服务器一般采用多线程处理业务，并且内存操作效率很高，所以如果一次请求延迟 20ms，则多次请求的网络延迟会不断累加。也就是说，在分布式环境下，Redis的性能瓶颈主要体现在网络延迟上。

![](D:\workspace\Java-Interview-Offer\images\redis001.png)

Redis的管道技术指在服务端未响应时，客户端可以继续向服务端发送请求，并最终一次性读取所有服务端的响应。管道技术能减少客户端和服务器交互的次数，将客户端的请求批量发送给服务器，服务器针对批量数据分别查询并统一回复，能显著提高Redis的性能。

![](D:\workspace\Java-Interview-Offer\images\redis002.png)

## 4.Redis管道技术基于Spring Boot的使用

使用redisTemplate.executePipelined()在Spring Boot中实现了基于Redis的管道操作。具体的步骤为：新建RedisCallback对象并覆写doInRedis()；在doInRedis()中通过connection.openPipeline()开启Pipeline操作；在for循环中批量进行Redis数据写操作；最终将批量操作结果返回。

## 5.Redis的事务

Redis支持分布式环境下的事务操作，其事务可以一次执行多个命令，事务中的所有命令都会序列化地顺序执行。事务在执行过程中，不会被其他客户端发送来的命令请求打断。服务器在执行完事务中的所有命令之后，才会继续处理其他客户端的其他命令。Redis的事务操作分为开启事务、命令入队列、执行事务三个阶段。Redis的事务执行流程如下：

（1）事务开启：客户端执行Multi命令开启事务。

（2）提交请求：客户端提交命令到事务。

（3）任务入队列：Redis将客户端请求放入事务队列中等待执行。

（4）入队状态反馈：服务器返回QURUD，表示命令已被放入事务队列。

（5）执行命令：客户端通过Exec执行事务。

（6）事务执行错误：在Redis事务中如果某条命令执行错误，则其他命令会继续执行，不会回滚。可以通过Watch监控事务执行的状态并处理命令执行错误的异常情况。

（7）执行结果反馈：服务器向客户端返回事务执行的结果。

![](D:\workspace\Java-Interview-Offer\images\redis003.png)

Redis事务的相关命令有Multi、Exec、Discard、Watch和Unwatch。

- Multi：标记一个事务块的开始
- Exec：执行所有事务块内二点命令
- Discard：取消事务，放弃执行事务块内的所有命令
- Watch：监视一个（或多个）key，如果在事务执行之前这个（或这些）key被其他命令改动，那么事务将被打断。
- Unwatch：取消Watch命令对所有key的监视

## 6.Redis事务基于Spring Boot的使用

```
public void transactionSet(Map<String,Object> commandList){
	//开启事务权限
	redisTemplate.setEnableTransactionSupport(true);
	try{
		//开启事务
		redisTemplate.multi();
		//执行事务命令
		for(Map.Entry<String,Object>entry:commandList.entrySet()){
			String mapKey=entry.getKey();
			Object mapValue=entry.getValue();
			redisTemplate.opsForValue().set(mapKey,mapValue)
		}
		//成功就提交
		redisTemplate.exec();
	}catch(Exception e){
		//失败就回滚
		redisTemplate.discard();
	}
}
```

以上代码定义了名为transactionSet()的Redis事务操作方法，该方法接收事务命令commandList并以事务命令列表在一个事务中执行。具体步骤为：开启事务权限、开启事务、执行事务命令、提供事务和回滚事务。

## 7.Redis发布、订阅

Redis发布、订阅是一种消息通信模式：发送者（Pub）向频道（Channel）发送消息，订阅者（Sub）接收频道上的消息。Redis客户端可以订阅任意数量的频道，发送者也可以向任意频道发送数据。图8-7展示了1个发送者（pub1）、1个频道（channe0）和3个订阅者（sub1、sub2、sub3）的关系。由于3个订阅者sub1、sub2、sub3都订阅了频道channel0，在发送者pub1向频道channel0发送一条消息后，这条消息就会被发送给订阅它的三个客户端。

![](D:\workspace\Java-Interview-Offer\images\redis004.png)

Redis常用的消息订阅与发布命令:

- PSUBSCRIBE：订阅一个或多个符合给定模式的频道
- PUBSUB：查看订阅与发布系统的状态
- PUBLISH：将信息发送到指定的频道
- SUBSCRIBE：订阅给定的一个或多个频道的信息
- UNSUBSCRIBE：指退订给定的频道

## 8.Redis集群数据复制的原理

Redis提供了复制功能，可以实现在主数据库（Master）中的数据更新后，自动将更新的数据同步到从数据库（Slave）。一个主数据库可以拥有多个从数据库，而一个从数据库只能拥有一个主数据库。

Redis的主从数据复制原理如下。

![](D:\workspace\Java-Interview-Offer\images\redis005.png)

（1）一个从数据库在启动后，会向主数据库发送SYNC命令。

（2）主数据库在接收到SYNC命令后会开始在后台保存快照（即RDB持久化的过程），并将保存快照期间接收到的命令缓存起来。在该持久化过程中会生成一个.rdb快照文件。

（3）在主数据库快照执行完成后，Redis会将快照文件和所有缓存的命令以.rdb快照文件的形式发送给从数据库。

（4）从数据库收到主数据库的.rdb快照文件后，载入该快照文件到本地。

（5）从数据库执行载入后的.rdb快照文件，将数据写入内存中。以上过程被称为复制初始化。

（6）在复制初始化结束后，主数据库在每次收到写命令时都会将命令同步给从数据库，从而保证主从数据库的数据一致。

在Redis中开启复制功能时需要在从数据库配置文件中加入如下配置，对主数据库无须进行任何配置：

```
#slave of master_address master_port
slave of 127.0.0.1 9000
#如果master有密码，则需要设置masterauth
masterauth=123
```

在上述配置中，slaveof后面的配置分别为主数据库的IP地址和端口，在主数据库开启了密码认证后需要将masterauth设置为主数据库的密码，在配置完成后重启Redis，主数据库上的数据就会同步到从数据库上。

## 9.Redis的持久化

Redis支持RDB和AOF两种持久化方式。

（1）RDB（Redis DataBase）：RDB在指定的时间间隔内对数据进行快照存储。RDB的特点在于：文件格式紧凑，方便进行数据传输和数据恢复；在保存.rdb快照文件时父进程会fork出一个子进程，由子进程完成具体的持久化工作，所以可以最大化Redis的性能；同时，与AOF相比，在恢复大的数据集时会更快一些。

（2）AOF（Append Of Flie）：AOF记录对服务器的每次写操作，在Redis重启时会重放这些命令来恢复原数据。AOF命令以Redis协议追加和保存每次写操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。AOF的特点有：可以使用不同的fsync策略（无fsync、每秒fsync、每次写的时候fsync），只有某些操作追加命令到文件中，操作效率高；同时，AOF文件是日志的格式，更容易被操作。

## 10.Redis的集群模式及工作原理

Redis有三种集群模式：主从模式、哨兵模式和集群模式。

（1）主从模式：所有的写请求都被发送到主数据库上，再由主数据库将数据同步到从数据库上。主数据库主要用于执行写操作和数据同步，从数据库主要用于执行读操作缓解系统的读压力。

![](D:\workspace\Java-Interview-Offer\images\redis006.png)

Redis的一个主库可以拥有多个从库，从库还可以作为其他数据库的主库。如图 8-7所示，Master的从库有Slave-0和Slave-1，同时Slave-1作为Slave-1-0和Slave-1-1的主库。

（2）哨兵模式：在主从模式上添加了一个哨兵的角色来监控集群的运行状态。哨兵通过发送命令让Redis服务器返回其运行状态。哨兵是一个独立运行的进程，在监测到Master宕机时会自动将Slave切换成Master，然后通过发布与订阅模式通知其他从服务器修改配置文件，完成主备热切。

![](D:\workspace\Java-Interview-Offer\images\redis007.png)

（3）集群模式：Redis集群实现了在多个Redis节点之间进行数据分片和数据复制。基于Redis集群的数据自动分片能力，我们能够方便地对Redis集群进行横向扩展，以提高Redis集群的吞吐量。基于Redis集群的数据复制能力，在集群中的一部分节点失效或者无法进行通信时，Redis仍然可以基于副本数据对外提供服务，这提高了集群的可用性。

![](D:\workspace\Java-Interview-Offer\images\redis008.png)

Redis集群遵循如下原则。

- 所有Redis节点彼此都通过PING-PONG 机制互联，内部使用二进制协议优化传输速度和带宽。
- 在集群中超过半数的节点检测到某个节点Fail后将该节点设置为Fail状态。
- 客户端与Redis节点直连，客户端连接集群中任何一个可用节点即可对集群进行操作。
- Redis-Cluster把所有的物理节点都映射到0～16383的slot（槽）上，Cluster负责维护每个节点上数据槽的分配。Redis的具体数据分配策略为：在Redis集群中内置了16384个散列槽；在需要在Redis集群中放置一个Key-Value时，Redis会先对Key使用CRC16 算法算出一个结果，然后把结果对16384 求余数，这样每个Key都会对应一个编号为0～16383的散列槽；Redis会根据节点的数量大致均等地将散列槽映射到不同的节点。

## 11.在Spring Boot中使用Redis

在Spring Boot中使用Redis的步骤为：引入jar包、配置application.properties，以及配置和使用RedisTemplate.

（1）引入jar包。

```
<dependency>
	<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

（2）配置application.properties：在resource目录的application.properties加入以下Redis配置：

```
#启动redis命令：redis-server
#redis的数据库索引（默认为0）
spring.redis.database=0
#redis的服务器地址
#spring.redis.host=127.0.0.1
#redis的服务器连接端口
#spring.redis.port=7000
#redis的服务器连接密码（默认为空）
spring.redis.password=
#连接池的最大连接数（使用负值表示没有限制）
spring.redis.jedis.pool.max-active=2000
#连接池的最大阻塞等待时间（使用负值表示没有限制）
spring.redis.jedis.pool.max-wait=-1
#连接池的最大空闲连接
spring.redis.jedis.pool.max-idle=100
#连接池的最小空间连接
spring.redis.jedis.pool.min-idle=50
#连接超时时间（毫秒）
spring.redis.timeout=1000
#哨兵模式配置
#spring.redis.sentinel.master=mymaster
#spring.redis.sentinel.nodes=127.0.0.1
#集群模式配置
spring.redis.cluster.nodes=127.0.0.1:7000,127.0.0.1:7001,127.0.0.1:7002,127.0.0.1:7003,127.0.0.1:7004,127.0.0.1:7005
```

在以上配置中，spring.redis.cluster.nodes为Redis集群节点的服务地址，在多个服务地址之间使用逗号隔开；spring.redis.password为Redis服务密码，如果没有密码，则将其设置为空即可。

需要注意的是，以上是集群模式下的Redis配置，如果Redis是主从模式，则将spring.redis.cluster.nodes地址修改为主从节点的服务地址即可；如果是哨兵模式，则注释掉spring.redis.cluster.nodes配置，在spring.redis.sentinel.master和spring.redis.sentinel.nodes中分别配置哨兵的名称和哨兵的节点即可；如果是单机模式，则注释掉spring.redis.sentinel.nodes的配置，通过spring.redis.host配置Redis服务的地址，并通过spring.redis.port配置Redis服务的端口即可。

（3）配置RedisTemplate。Spring Boot默认配置了RedisTemplate，在应用时注入、使用即可，也可以创建自定义的RedisTemplate。

（4）使用RedisTemplate。

```
//redis key-value插入
redisTemplate.opsForValue().set("key","value");
//redis 根据key查询
Object result=redisTemplate.opsForValue().get("key");
//redis 根据key删除
redisTemplate.delete("key");
```

RedisTemplate基于Jedis对Redis数据库的操作进行了二次封装，使得操作Redis数据库更加方便。以上代码在测试类中依赖注入了RedisTemplate，并通过redisTemplate. opsForValue()实现了对Redis数据的插入、查询和删除操作。