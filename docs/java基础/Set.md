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