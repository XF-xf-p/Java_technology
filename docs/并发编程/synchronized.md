## 1.synchronized

synchronized关键字用于为Java对象、方法、代码块提供线程安全的操作。synchronized它可以把任意一个非NULL的对象当作锁。synchronized属于独占式的悲观锁，同时属于可重入锁。在使用synchronized修饰对象时，同一时刻只能有一个线程对该对象进行访问；在synchronized修饰方法、代码块时，同一时刻只能有一个线程执行该方法体或代码块，其他线程只有等待当前线程执行完毕并释放锁资源后才能访问该对象或执行同步代码块。

Java中的每个对象都有个monitor对象，加锁就是在竞争monitor对象。对代码块加锁是通过在前后分别加上monitorenter和monitorexit指令实现的，对方法是否加锁是通过一个标记位来判断的。

### synchronized的作用范围

- synchronized作用于成员变量和非静态方法时，锁住的是对象的实例，即this对象。
- 当作用于静态方法时，锁住的是Class实例，又因为Class的相关数据存储在永久带PermGen（jdk1.8则是metaspace），永久带是全局共享的，因此静态方法锁相当于类的一个全局锁，会锁所有调用该方法的线程；
- synchronized作用于一个对象实例时，锁住的是所有以该对象为锁的代码块。它有多个队列，当多个线程一起访问某个对象监视器的时候，对象监视器会将这些线程存储在不同的容器中。

### synchronized的用法

synchronized作用于成员变量和非静态方法时，锁住的是对象的实例。

synchronized作用于静态同步方法，锁住的是当前类的Class对象。

synchronized作用于一个代码块时，锁住的是在代码块中配置的对象。

## 2.synchronized的实现原理

在synchronized内部包括ContentionList、EntryList、WaitSet、OnDeck、Owner、!Owner这6个区域，每个区域的数据都代表锁的不同状态。

- ContentionList：锁竞争队列，所有请求锁的线程都被放在竞争队列中。
- EntryList：竞争候选列表，在Contention List中有资格成为候选者来竞争锁资源的线程被移动到了Entry List中。
- WaitSet：等待集合，调用wait方法后被阻塞的线程将被放在WaitSet中。
- OnDeck：竞争候选者，在同一时刻最多只有一个线程在竞争锁资源，该线程的状态被称为OnDeck。
- Owner：竞争到锁资源的线程被称为Owner状态线程。
- !Owner：在Owner线程释放锁后，会从Owner的状态变成!Owner。

![](D:\workspace\java\images\synchronized001.png)

1.JVM每次从队列的尾部取出一个数据用于锁竞争候选者（OnDeck），但是并发情况下，ContentionList会被大量的并发线程进行CAS访问，为了降低对尾部元素的竞争，JVM会将一部分线程移动到EntryList中作为候选竞争线程。

2.Owner线程会在unlock时，将ContentionList中的部分线程迁移到EntryList中，并指定EntryList中的某个线程为OnDeck线程（一般是最先进去的那个线程）。

3.Owner线程并不直接把锁传递给OnDeck线程，而是把锁竞争的权利交给OnDeck，OnDeck需要重新竞争锁。这样虽然牺牲了一些公平性，但是能极大的提升系统的吞吐量，在JVM中，也把这种选择行为称之为“竞争切换”。

4.OnDeck线程获取到锁资源后会变为Owner线程，而没有得到锁资源的仍然停留在EntryList中。如果Owner线程被wait方法阻塞，则转移到WaitSet队列中，直到某个时刻通过notify或者notifyAll唤醒，会重新进去EntryList中。

5.处于ContentionList、EntryList、WaitSet中的线程都处于阻塞状态，该阻塞是由操作系统来完成的（Linux内核下采用pthread_mutex_lock内核函数实现的）。

6.Synchronized是非公平锁。Synchronized在线程进入ContentionList时，等待的线程会先尝试自旋获取锁，如果获取不到就进入ContentionList，这明显对于已经进入队列的线程是不公平的，还有一个不公平的事情就是自旋获取锁的线程还可能直接抢占OnDeck线程的锁资源。

7.每个对象都有个monitor对象，加锁就是在竞争monitor对象，代码块加锁是在前后分别加上monitorenter和monitorexit指令来实现的，方法加锁是通过一个标记位来判断的。

8.synchronized是一个重量级操作，需要调用操作系统相关接口，性能是低效的，有可能给线程加锁消耗的时间比有用操作消耗的时间更多。

9.Java1.6，synchronized进行了很多的优化，有适应自旋、锁消除、锁粗化、轻量级锁及偏向锁等，效率有了本质上的提高。在之后推出的Java1.7与1.8中，均对该关键字的实现机理做了优化。引入了偏向锁和轻量级锁。都是在对象头中有标记位，不需要经过操作系统加锁。

10.锁可以从偏向锁升级到轻量级锁，再升级到重量级锁。这种升级过程叫做锁膨胀；

11.JDK 1.6中默认是开启偏向锁和轻量级锁，可以通过-XX:-UseBiasedLocking来禁用偏向锁。

## 3.synchronized和ReentrantLock的比较

synchronized和ReentrantLock的共同点如下。

- 都用于控制多线程对共享对象的访问。
- 都是可重入锁。
- 都保证了可见性和互斥性。

synchronized和ReentrantLock的不同点如下。

- ReentrantLock显式获取和释放锁；synchronized隐式获取和释放锁。为了避免程序出现异常而无法正常释放锁，在使用ReentrantLock时必须在finally控制块中进行解锁操作。
- ReentrantLock可响应中断、可轮回，为处理锁提供了更多的灵活性。
- ReentrantLock是API级别的，synchronized是JVM级别的。
- ReentrantLock可以定义公平锁。
- ReentrantLock通过Condition可以绑定多个条件。
- 二者的底层实现不一样：synchronized是同步阻塞，采用的是悲观并发策略；Lock是同步非阻塞，采用的是乐观并发策略。
- Lock是一个接口，而synchronized是Java中的关键字，synchronized是由内置的语言实现的。
- synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁。
- Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断。
- 我们通过Lock可以知道有没有成功获取锁，通过synchronized却无法做到。
- Lock可以通过分别定义读写锁提高多个线程读操作的效率。