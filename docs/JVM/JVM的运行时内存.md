## 1.JVM的运行时内存

JVM的运行时内存也叫作JVM堆，从GC的角度可以将JVM堆分为新生代、老年代和永久代。其中新生代默认占1/3堆空间，老年代默认占2/3堆空间，永久代占非常少的堆空间。新生代又分为Eden区、ServivorFrom区和ServivorTo区，Eden区默认占8/10新生代空间，ServivorFrom区和ServivorTo区默认分别占 1/10新生代空间。

![](D:\workspace\Java-Interview-Offer\images\JVM004.png)

### 新生代：Eden区、ServivorTo区和ServivorFrom区

JVM新创建的对象（除了大对象外）会被存放在新生代，默认占 1/3堆内存空间。由于JVM会频繁创建对象，所以新生代会频繁触发MinorGC进行垃圾回收。新生代又分为Eden区、ServivorTo区和ServivorFrom区，如下所述。

（1）Eden区：Java新创建的对象首先会被存放在Eden区，如果新创建的对象属于大对象，则直接将其分配到老年代。大对象的定义和具体的JVM版本、堆大小和垃圾回收策略有关，一般为2KB～128KB，可通过XX:PretenureSizeThreshold设置其大小。在Eden区的内存空间不足时会触发MinorGC，对新生代进行一次垃圾回收。

（2）ServivorTo区：保留上一次MinorGC时的幸存者。

（3）ServivorFrom区：将上一次MinorGC时的幸存者作为这一次MinorGC的被扫描者。

新生代的GC过程叫作MinorGC，采用复制算法实现，具体过程如下。

（1）把在Eden区和ServivorFrom区中存活的对象复制到ServivorTo区。如果某对象的年龄达到老年代的标准（对象晋升老年代的标准由XX:MaxTenuringThreshold设置，默认为15），则将其复制到老年代，同时把这些对象的年龄加1；如果ServivorTo区的内存空间不够，则也直接将其复制到老年代；如果对象属于大对象（大小为 2KB～128KB的对象属于大对象，例如通过XX:PretenureSizeThreshold

=2097152设置大对象为2MB，1024×1024×2Byte=2097152Byte=2MB），则也直接将其复制到老年代。

（2）清空Eden区和ServivorFrom区中的对象。

（3）ServicorTo和ServicorFrom互换，原来的ServivorTo区成为下一次GC时的ServivorFrom区。

### 老年代

老年代主要存放有长生命周期的对象和大对象。老年代的GC过程叫作MajorGC。在老年代，对象比较稳定，MajorGC不会被频繁触发。在进行MajorGC前，JVM会进行一次MinorGC，在MinorGC过后仍然出现老年代空间不足或无法找到足够大的连续空间分配给新创建的大对象时，会触发MajorGC进行垃圾回收，释放JVM的内存空间。

MajorGC采用标记清除算法。该算法首先会扫描所有对象并标记存活的对象，然后回收未被标记的对象，并释放内存空间。

因为要先扫描老年代的所有对象再回收，所以MajorGC的耗时较长。MajorGC的标记清除算法容易产生内存碎片。在老年代没有内存空间可分配时，会抛出Out Of Memory异常。

### 永久代

永久代指内存的永久保存区域，主要存放Class和Meta（元数据）的信息。Class在类加载时被放入永久代。永久代和老年代、新生代不同，GC不会在程序运行期间对永久代的内存进行清理，这也导致了永久代的内存会随着加载的Class文件的增加而增加，在加载的Class文件过多时会抛出OutOf Memory异常，比如Tomcat引用Jar文件过多导致JVM内存不足而无法启动。

需要注意的是，在Java 8中永久代已经被元数据区（也叫作元空间）取代。元数据区的作用和永久代类似，二者最大的区别在于：**元数据区并没有使用虚拟机的内存，而是直接使用操作系统的本地内存。**因此，元空间的大小不受JVM内存的限制，只和操作系统的内存有关。

在Java 8中，JVM将类的元数据放入本地内存（Native Memory）中，将常量池和类的静态变量放入Java堆中，这样JVM能够加载多少元数据信息就不再由JVM的最大可用内存（MaxPermSize）空间决定，而由操作系统的实际可用内存空间决定。

## 2.为什么要分成年轻代和老年代？

因为这跟垃圾回收有关，对于年轻代里的对象，他们的特点是创建之后很快就会被回收，所以需要用一种垃圾回收算法。

对于老年代里的对象，他们的特点是需要长期存在，所以需要另外一种垃圾回收算法，所以需要分成两个区域来放不同的对象。