## 1.Map

#### 1.HashMap：数组+链表存储数据，线程不安全

HashMap基于键的HashCode值唯一标识一条数据，同时基于键的HashCode值进行数据的存取，因此可以快速地更新和查询数据，但其

每次遍历的顺序无法保证相同。HashMap的key和value允许为null。

HashMap是非线程安全的，即在同一时刻有多个线程同时写HashMap时将可能导致数据的不一致。如果需要满足线程安全的条件，则可

以用Collections的synchronizedMap方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap。

#### 2.HashTable：线程安全

HashTable是遗留类，很多映射的常用功能都与HashMap类似，不同的是它继承自Dictionary类，并且是线程安全的，同一时刻只有一个

线程能写HashTable，并发性不如ConcurrentHashMap。因为ConcurrentHashMap引入了分段锁。Hashtable不建议在新代码中使用，

不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用ConcurrentHashMap替换。

#### 3.TreeMap：基于二叉树数据结构

TreeMap基于二叉树数据结构存储数据，同时实现了SortedMap接口以保障元素的顺序存取，默认按键值的升序排序，也可以自定义排

序比较器。

TreeMap常用于实现排序的映射列表。在使用TreeMap时其key必须实现Comparable接口或采用自定义的比较器，否则会抛出

java.lang.ClassCastException异常。

## 2.HashMap和HashTable区别

#### 线程安全：

HashTable 中的方法是同步的，而HashMap中的方法在默认情况下是非同步的。在多线程并发的环境下，可以直接使用HashTable，但是要使用HashMap的话就要自己增加同步处理了。

#### 继承关系：

HashTable是基于陈旧的Dictionary类继承来的。 HashMap继承的抽象类AbstractMap实现了Map接口。

#### 允不允许null值：

HashTable中，key和value都不允许出现null值，否则会抛出NullPointerException异常。 HashMap中，null可以作为键，这样的键只有一个；可以有一个或多个键所对应的值为null。

#### 默认初始容量和扩容机制：

HashTable中的hash数组初始大小是11，增加的方式是 old*2+1。HashMap中hash数组的默认大小是16，而且一定是2的指数。

#### 哈希值的使用不同

HashTable直接使用对象的hashCode。 HashMap重新计算hash值。

#### 遍历方式的内部实现上不同

Hashtable、HashMap都使用了 Iterator。而由于历史原因，Hashtable还使用了Enumeration的方式 。 HashMap 实现 Iterator，支持fast-fail，Hashtable的 Iterator 遍历支持fast-fail，用 Enumeration 不支持 fast-fail

## 3.HashMap 和 ConcurrentHashMap 的区别？

ConcurrentHashMap和HashMap的实现方式不一样，虽然都是使用桶数组实现的，但是还是有区别，ConcurrentHashMap对桶数组进行了分段，而HashMap并没有。

ConcurrentHashMap在每一个分段上都用锁进行了保护。HashMap没有锁机制。所以，前者线程安全的，后者不是线程安全的。