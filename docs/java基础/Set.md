## 1.Set：不可重复

Set核心是独一无二的性质，适用于存储无序且值不相等的元素。对象的相等性在本质上是对象的HashCode值相同，Java依据对象的内存

地址计算出对象的HashCode值。如果想要比较两个对象是否相等，则必须同时覆盖对象的hashCode方法和equals方法，并且hashCode

方法和equals方法的返回值必须相同。

**1.HashSet：HashTable实现，无序**

HashSet存放的是散列值，它是按照元素的散列值来存取元素的。元素的散列值是通过元素的hashCode方法计算得到的，HashSet首先

判断两个元素的散列值是否相等，如果散列值相等，则接着通过equals方法比较，如果equls方法返回的结果也为true，HashSet就将其

视为同一个元素；如果equals方法返回的结果为false，HashSet就不将其视为同一个元素。

**2.TreeSet：二叉树实现**

TreeSet基于二叉树的原理对新添加的对象按照指定的顺序排序（升序、降序），每添加一个对象都会进行排序，并将对象插入二叉树指

定的位置。

Integer和String等基础对象类型可以直接根据TreeSet的默认排序进行存储，而自定义的数据类型必须实现Comparable接口，并且覆写

其中的compareTo函数才可以按照预定义的顺序存储。若覆写compare函数，则在升序时在this.对象小于指定对象的条件下返回-1，在

降序时在this.对象大于指定对象的条件下返回1。

**3.LinkHashSet：HashTable实现数据存储，双向链表记录顺序**

LinkedHashSet在底层使用LinkedHashMap存储元素，它继承了HashSet，所有的方法和操作都与HashSet相同，因此LinkedHashSet的

实现比较简单，只提供了4个构造方法，并通过传递一个标识参数调用父类的构造器，在底层构造一个LinkedHashMap来记录数据访问，

其他相关操作与父类HashSet相同，直接调用父类HashSet的方法即可。

## 2.Set如何保证元素不重复?

在Java的Set体系中，根据实现方式不同主要分为两大类。HashSet和TreeSet。

1、TreeSet 是二叉树实现的，TreeSet中的数据是自动排好序的，不允许放入 null值
2、HashSet 是哈希表实现的，HashSet中的数据是无序的，可以放入 null值，但只能放入一个null，两者中的值都不能重复，就如数据库中的唯一约束。

在HashSet中，基本的操作都是由HashMap底层实现的，因为HashSet底层是用HashMap存储数据的。当向HashSet中添加元素的时候，首先计算元素的hashCode值，然后通过扰动计算和按位与的方式计算出这个元素的存储位置，如果这个位置为空，就将元素添加进去；如果不为空，则用equals方法比较元素是否相等，相等就不添加，否则找一个空位添加。

TreeSet的底层是TreeMap的keySet()，而TreeMap是基于红黑树实现的，红黑树是一种平衡二叉查找树，它能保证任何一个节点的左右子树的高度差不会超过较矮的那棵的一倍。

TreeMap是按key排序的，元素在插入TreeSet时compareTo()方法要被调用，所以TreeSet中的元素要实现Comparable接口。TreeSet作为一种Set，它不允许出现重复元素。TreeSet是用compareTo()来判断重复元素的。