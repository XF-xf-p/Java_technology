## 1.ConcurrentHashMap：分段锁实现，线程安全

与HashMap不同，ConcurrentHashMap采用分段锁的思想实现并发操作，因此是线程安全的。ConcurrentHashMap由多个Segment组

成（Segment的数量也是锁的并发度），Segment 代表”部分“或”一段“的意思，所以很多地方都会将其描述为分段锁。

每个Segment均继承自ReentrantLock并单独加锁，所以每次进行加锁操作时锁住的都是一个Segment，这样只要保证每个Segment都是

线程安全的，也就实现了全局的线程安全。

在ConcurrentHashMap中有个concurrencyLevel参数表示并行级别，默认是 16，也就是说ConcurrentHashMap默认由 16个Segments

组成，在这种情况下最多同时支持 16个线程并发执行写操作，只要它们的操作分布在不同的Segment上即可。并行级别

concurrencyLevel可以在初始化时设置，一旦初始化就不可更改。ConcurrentHashMap的每个Segment内部的数据结构都和HashMap

相同。

![](D:\workspace\Java-Interview-Offer\images\集合006.png)

Java 8在ConcurrentHashMap中引入了红黑树。

![](D:\workspace\Java-Interview-Offer\images\集合007.png)