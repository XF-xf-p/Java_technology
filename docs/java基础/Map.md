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