## 1.String

一旦一个string对象在内存(堆)中被创建出来，他就无法被修改。特别要注意的是，String类的所有方法都没有改变字符串本身的值，都是返回了一个新的对象。

如果你需要一个可修改的字符串，应该使用StringBuffer 或者 StringBuilder。否则会有大量时间浪费在垃圾回收上，因为每次试图修改都有新的string对象被创建出来。

## 2.JDK 6和JDK 7中substring的原理及区别

### substring() 的作用

substring(int beginIndex, int endIndex)方法截取字符串并返回其[beginIndex,endIndex-1]范围内的内容。

### JDK 6中的substring

String是通过字符数组实现的。在jdk 6 中，String类包含三个成员变量：`char value[]`， `int offset`，`int count`。他们分别用来存储真正的字符数组，数组的第一个位置索引以及字符串中包含的字符个数。

当调用substring方法的时候，会创建一个新的string对象，但是这个string的值仍然指向堆中的同一个字符数组。这两个对象中只有count和offset 的值是不同的。

### JDK 6中的substring导致的问题

如果你有一个很长很长的字符串，但是当你使用substring进行切割的时候你只需要很短的一段。这可能导致性能问题，因为你需要的只是一小段字符序列，但是你却引用了整个字符串（因为这个非常长的字符数组一直在被引用，所以无法被回收，就可能导致内存泄露）。在JDK 6中，一般用以下方式来解决该问题，原理其实就是生成一个新的字符串并引用他。

```markup
x = x.substring(x, y) + ""
```

### JDK 7 中的substring

在jdk 7 中，substring方法会在堆内存中创建一个新的数组。

其使用`new String`创建了一个新字符串，避免对老字符串的引用。从而解决了内存泄露问题。

## 3.replaceFirst、replaceAll、replace区别

replace、replaceAll和replaceFirst是Java中常用的替换字符的方法,它们的方法定义是：

replace(CharSequence target, CharSequence replacement) ，用replacement替换所有的target，两个参数都是字符串。

replaceAll(String regex, String replacement) ，用replacement替换所有的regex匹配项，regex很明显是个正则表达式，replacement是字符串。

replaceFirst(String regex, String replacement) ，基本和replaceAll相同，区别是只替换第一个匹配项。

可以看到，其中replaceAll以及replaceFirst是和正则表达式有关的，而replace和正则表达式无关。

replaceAll和replaceFirst的区别主要是替换的内容不同，replaceAll是替换所有匹配的字符，而replaceFirst()仅替换第一次出现的字符。

## 4.String对“+”的重载

1. String s = "a" + "b"，编译器会进行常量折叠(因为两个都是编译期常量，编译期可知)，即变成 String s = "ab"
2. 对于能够进行优化的(String s = "a" + 变量 等)用 StringBuilder 的 append() 方法替代，最后调用 toString() 方法 (底层就是一个 new String())

## 5.字符串拼接

**String是Java中一个不可变的类**，所以他一旦被实例化就无法被修改。

> 不可变类的实例一旦创建，其成员变量的值就不能被修改。这样设计有很多好处，比如可以缓存hashcode、使用更加便利以及更加安全等。

其实，所有的所谓字符串拼接，都是重新生成了一个新的字符串。

### 字符串拼接方式

**使用`+`拼接字符串**

**concat**

**StringBuffer**

**StringBuilder**

**StringUtils.join**

其实他也是通过`StringBuilder`来实现的。

循环体内，字符串的连接方式，使用 `StringBuilder` 的 `append` 方法进行扩展。而不要使用`+`。

### 效率比较

StringBuilder`<`StringBuffer`<`concat`<`+`<`StringUtils.join

StringBuffer在StringBuilder的基础上，做了同步处理，所以在耗时上会相对多一些。

StringUtils.join也是使用了StringBuilder，并且其中还是有很多其他操作，所以耗时较长，这个也容易理解。其实StringUtils.join更擅长

处理字符串数组或者列表的拼接。

### 总结

1、如果不是在循环体中进行字符串拼接的话，直接使用`+`就好了。

2、如果在并发场景中进行字符串拼接的话，要使用`StringBuffer`来代替`StringBuilder`。

## 6.StringBuffer和StringBuilder

和`String`类类似，`StringBuilder`类也封装了一个字符数组，定义如下：

```text
char[] value;
```

与`String`不同的是，它并不是`final`的，所以他是可以修改的。另外，与`String`不同，字符数组中不一定所有位置都已经被使用，它有一个实例变量，表示数组中已经使用的字符个数，定义如下：

```text
int count;
```

`StringBuffer`和`StringBuilder`类似，最大的区别就是`StringBuffer`是线程安全的。

## 7.switch对String的支持

Java 7中，switch的参数可以是String类型了。

字符串的switch是通过`equals()`和`hashCode()`方法来实现的。**记住，switch中只能使用整型**，比如`byte`。`short`，`char`(ackii码是整型)以及`int`。

### 总结

**其实switch只支持一种数据类型，那就是整型，其他数据类型都是转换成整型之后再使用switch的。**

## 8.intern 

在JVM中，为了减少相同的字符串的重复创建，为了达到节省内存的目的。会单独开辟一块内存，用于保存字符串常量，这个内存区域被叫做字符串常量池。

当代码中出现双引号形式（字面量）创建字符串对象时，JVM 会先对这个字符串进行检查，如果字符串常量池中存在相同内容的字符串对象的引用，则将这个引用返回；否则，创建新的字符串对象，然后将这个引用放入字符串常量池，并返回该引用。

除了以上方式之外，还有一种可以在运行期将字符串内容放置到字符串常量池的办法，那就是使用intern

intern的功能很简单：

在每次赋值的时候使用 String 的 intern 方法，如果常量池中有相同值，就会重复使用该对象，返回对象引用。

## 9.String.valueOf和Integer.toString的区别

```
String i1 = "" + i;
String i2 = String.valueOf(i);
String i3 = Integer.toString(i);
```

第二行代码其实是String i1 = (new StringBuilder()).append(i).toString();，首先创建一个StringBuilder对象，然后再调用append方法，再调用toString方法。

第三行和第四行没有任何区别，因为String.valueOf(i)也是调用Integer.toString(i)来实现的。

