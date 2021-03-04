# Mybatis

## 1.MyBatis的缓存

MyBatis缓存分为一级缓存和二级缓存，在默认情况下，一级缓存是开启的，而且不能被关闭。

![](D:\workspace\Java-Interview-Offer\images\mybatis001.png)

- 一级缓存：SqlSession级别的缓存，当在同一个SqISession中执行相同的SQL语句查询时将查询结果集缓存，第二次以后的查询不会从数据库中查询，而是直接从缓存中获取，一级缓存最多能缓存1024条SQL语句。
- 二级缓存：指跨SqlSession的缓存，即Mapper级别的缓存。在Mapper级别的缓存内，不同的SqlSession缓存可以共享。

![](D:\workspace\Java-Interview-Offer\images\mybatis002.png)

## 2.MyBatis的一级缓存原理

当客户端第一次发出一个SQL查询语句时，MyBatis执行SQL查询并将查询结果写入SqlSession的一级缓存，当第二次有相同的SQL查询语句时，则直接从缓存中获取数据。在缓存中使用的数据结构是Map，其中，Key为Mapperld+Offset+Limit+SQL＋所有的入参。value：用户信息。

当同一个SqlSession多次发出相同的SQL查询语句时，MyBatis直接从缓存中获取数据。如果两次查询中间出现Commit操作（修改、添加、删除），则认为数据发生了变化，MyBatis会把该SqlSession中的一级缓存区域全部清空，当下次再到缓存中查询时将找不到对应的缓存数据，因此要再次从数据库中查询数据并将查询的结果写入缓存。

## 3.MyBatis的二级缓存原理

MyBatis二级缓存的范围是Mapper级别的，Mapper以命名空间为单位创建缓存数据结构，数据结构是Map类型，Map中Key为Mapperld+Offset+Limit+SQL＋所有的入参。My Batis的二级缓存是通过CacheExecuto实现的。CaceExecutor是Executor的代理对象。当MyBatis接收到用户的查询请求时，首先会根据Map的Key在CacheExecutor缓存中查询数据是否存在，如果不存在则在数据库中查询。

开启二级缓存需要做以下配置

( 1）在MyBatis全局配置中启用二级缓存配置。

( 2）在对应的Mapper.xml中配置Cache节点。

( 3 ）在对应的Select查询节点中添加useCache=true。

