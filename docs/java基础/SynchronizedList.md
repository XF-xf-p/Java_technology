## SynchronizedList

 SynchronizedList是java.util.Collections中的一个静态内部类。

在多线程的场景中可以直接使用Vector类，也可以使用Collections.synchronizedList(List list)方法来返回一个线程安全的List。

```
List<String> list = new ArrayList<String>();
List list2 =  Collections.synchronizedList(list);
```

### SynchronizedList和Vector区别

 **1.SynchronizedList有很好的扩展和兼容功能。他可以将所有的List的子类转成线程安全的类。** 

**2.使用SynchronizedList的时候，进行遍历时要手动进行同步处理**。 

**3.SynchronizedList可以指定锁定的对象。**