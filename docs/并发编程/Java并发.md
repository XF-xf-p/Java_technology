# Java并发

## 1.CountDownLatch（线程计数器）

CountDownLatch类位于java.util.concurrent包下，是一个同步工具类，允许一个或多个线程一直等待其他线程的操作执行完后再执行相关操作。

CountDownLatch基于线程计数器来实现并发访问控制，主要用于主线程等待其他子线程都执行完毕后执行相关操作。其使用过程为：在主线程中定义CountDownLatch，并将线程计数器的初始值设置为子线程的个数，多个子线程并发执行，每个子线程在执行完毕后都会调用countDown函数将计数器的值减1，直到线程计数器为0，表示所有的子线程任务都已执行完毕，此时在CountDownLatch上等待的主线程将被唤醒并继续执行。

我们利用CountDownLatch可以实现类似计数器的功能。比如有一个主任务，它要等待其他两个任务都执行完毕之后才能执行，此时就可以利用CountDownLatch来实现这种功能。

```
//定义大小为1的CountDownLatch
final CountDownLatch latch=new CountDownLatch(1);
new Thread(){
	public void run(){
		try{
			latch.countDown();//在子线程1执行完毕后调用countDown方法
		}catch(Exception e){
		}
	}
}.start();
try{
	latch.await();//在CountDownLatch上等待子程序执行完毕
	//子程序执行完毕，开始执行主程序
}
```

以上代码片段先定义了一个大小为1的CountDownLatch，然后定义了一个子线程并启动该子线程，子线程执行完业务代码后在执行latch.countDown()时减少一个信号量，表示自己已经执行完成。主线程调用latch.await()阻塞等待，在所有线程都执行完成并调用了countDown函数时，表示所有线程均执行完成，这时程序会主动唤醒主线程并开始执行主线程的业务逻辑。

## 2.CyclicBarrier（回环栅栏-等待至barrier状态再全部同时执行）

CyclicBarrier（循环屏障）是一个同步工具，可以实现让一组线程等待至某个状态之后再全部同时执行。在所有等待线程都被释放之后，CyclicBarrier可以被重用。CyclicBarrier的运行状态叫作Barrier状态，在调用await方法后，线程就处于Barrier状态。

CyclicBarrier中最重要的方法是await方法，它有两种实现。

- public int await()：挂起当前线程直到所有线程都为Barrier状态再同时执行后续的任务。
- public int await(long timeout, TimeUnit unit)：设置一个超时时间，在超时时间过后，如果还有线程未达到Barrier状态，则不再等待，让达到Barrier状态的线程继续执行后续的任务。

定义一个CyclicBarrier，然后循环启动了多个线程，每个线程都通过构造函数将CyclicBarrier传入线程中，在线程内部开始执行第1阶段的工作，比如查询数据等；等第1阶段的工作处理完成后，再调用cyclicBarrier.await方法等待其他线程也完成第1阶段的工作（CyclicBarrier让一组线程等待到达某个状态再一起执行）；等其他线程也执行完第1阶段的工作，便可执行并发操作的下一项任务，比如数据分发等。

## 3.Semaphore（信号量-控制同时访问的线程个数）

Semaphore指信号量，用于控制同时访问某些资源的线程个数，具体做法为通过调用acquire()获取一个许可，如果没有许可，则等待，在许可使用完毕后通过release()释放该许可，以便其他线程使用。

Semaphore常被用于多个线程需要共享有限资源的情况，比如办公室有两台打印机，但是有5个员工需要使用，一台打印机同时只能被一个员工使用，其他员工排队等候，且只有该打印机被使用完毕并释放后其他员工方可使用，这时就可以通过Semaphore来实现。

定义了一个数量为 2的Semaphore，然后定义了一个工作线程Worker并通过构造函数将Semaphore传入线程内部。在线程调用semaphore.acquire()时开始申请许可并执行业务逻辑，在线程业务逻辑执行完成后调用semaphore.release()释放许可以便其他线程使用。

在Semaphore类中有以下几个比较重要的方法。

- public void acquire()：以阻塞的方式获取一个许可，在有可用许可时返回该许可，在没有可用许可时阻塞等待，直到获得许可。
- public void acquire(int permits)：同时获取permits个许可。
- public void release()：释放某个许可。
- public void release(int permits)：释放permits个许可。
- public boolean tryAcquire()：以非阻塞方式获取一个许可，在有可用许可时获取该许可并返回true，否则返回false，不会等待。
- public boolean tryAcquire(long timeout,TimeUnit unit)：如果在指定的时间内获取到可用许可，则返回true，否则返回false。
- public boolean tryAcquire(int permits)：如果成功获取permits个许可，则返回true，否则立即返回false。
- public boolean tryAcquire(int permits,longtimeout,TimeUnit unit)：如果在指定的时间内成功获取permits个许可，则返回true，否则返回false。
- availablePermits()：查询可用的许可数量。

## 4.CountDownLatch、CyclicBarrier、Semaphore的区别

- CountDownLatch和CyclicBarrier都用于实现多线程之间的相互等待，但二者的关注点不同。CountDownLatch主要用于主线程等待其他子线程任务均执行完毕后再执行接下来的业务逻辑单元，而CyclicBarrier主要用于一组线程互相等待大家都达到某个状态后，再同时执行接下来的业务逻辑单元。此外，CountDownLatch是不可以重用的，而CyclicBarrier是可以重用的。
- Semaphore和Java中的锁功能类似，主要用于控制资源的并发访问。