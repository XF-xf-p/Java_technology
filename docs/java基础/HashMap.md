## 1.HashMap

HashMap的数据结构其内部是一个数组，数组中的每个元素都是一个单向链表，链表中的每个元素都是嵌套类Entry的实例，Entry实例

包含4个属性：key、value、hash值和用于指向单向链表下一个元素的next。

![](D:\workspace\Java-Interview-Offer\images\集合004.png)



HashMap常用的参数如下:

- capacity：当前数组的容量，默认为16，可以扩容，扩容后数组的大小为当前的两倍，因此该值始终为2n。
- loadFactor：负载因子，默认为0.75。
- threshold：扩容的阈值，其值等于capacity×loadFactor。

HashMap在查找数据时，根据HashMap的Hash值可以快速定位到数组的具体下标，但是在找到数组下标后需要对链表进行顺序遍历直

到找到需要的数据，时间复杂度为O(n)。

为了减少链表遍历的开销，Java 8对HashMap进行了优化，将数据结构修改为数组+链表或红黑树。在链表中的元素超过8个以后，

HashMap会将链表结构转换为红黑树结构以提高查询效率，因此其时间复杂度为O(logN)。

![](D:\workspace\Java-Interview-Offer\images\集合005.png)

## 2.LinkedHashMap：基于HashTable数据结构，使用链表保存插入顺序

LinkedHashMap为HashMap的子类，其内部使用链表保存元素的插入顺序，在通过Iterator遍历LinkedHashMap时，会按照元素的插入

顺序访问元素。也可以在构造时带参数，按照访问次序排序。