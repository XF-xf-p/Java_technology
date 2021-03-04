## 1.什么是AQS

AQS（Abstract Queued Synchronizer）是一个抽象的队列同步器，通过维护一个共享资源状态（Volatile Int State）和一个先进先出（FIFO）的线程等待队列来实现一个多线程访问共享资源的同步框架。

### AQS的原理

AQS为每个共享资源都设置一个共享资源锁，线程在需要访问共享资源时首先需要获取共享资源锁，如果获取到了共享资源锁，便可以在当前线程中使用该共享资源，如果获取不到，则将该线程放入线程等待队列，等待下一次资源调度，许多同步类的实现都依赖于AQS，例如常用的ReentrantLock、Semaphore和CountDownLatch。

### state：状态

Abstract Queued Synchronizer维护了一个volatile int state（代表共享资源）和一个FIFO线程等待队列（多线程争用资源被阻塞时会进入此队列）。Volatile虽然不能保证操作的原子性，但是能保证当前变量state的可见性。

state的访问方式有三种：getState()、setState()和compareAndSetState()，均是原子操作，其中，compareAndSetState的实现依赖于Unsafe的compareAndSwapInt()。

## 2.AQS共享资源的方式：独占式和共享式

AQS定义了两种资源共享方式：独占式（Exclusive）和共享式（Share）。

- 独占式：只有一个线程能执行，具体的Java实现有ReentrantLock。
- 共享式：多个线程可同时执行，具体的Java实现有Semaphore和CountDownLatch。

AQS只是一个框架，只定义了一个接口，具体资源的获取、释放都交由自定义同步器去实现。不同的自定义同步器争用共享资源的方式也不同，自定义同步器在实现时只需实现共享资源state的获取与释放方式即可，至于具体线程等待队列的维护，如获取资源失败入队、唤醒出队等，AQS已经在顶层实现好，不需要具体的同步器再做处理。

- isHeldExclusively()：查询该线程是否正在独占资源，只有用到condition才需要去实现它
- try Acquire(int)：独占方式，尝试获取资源，成功则返回true，失败则返回false
- tryRelease(int)：独占方式，尝试是否资源，成功则返回true，失败则返回false
- tryAcquireShared(int)：共享方式，尝试获取资源，负数表示失败，0表示成功，但没有剩余可用资源，正数表示成功，且有剩余资源
- tryReleaseShared(int)：共享方式，尝试释放资源，如果释放资源后允许唤醒后续等待线程，则返回true，否则返回false

同步器的实现是AQS的核心内存。ReentrantLock对AQS的独占方式实现为：ReentrantLock中的state初始值为0时表示无锁状态。在线程执行tryAcquire()获取该锁后ReentrantLock中的state+1，这时该线程独占ReentrantLock锁，其他线程在通过tryAcquire()获取锁时均会失败，直到该线程释放锁后state再次为 0，其他线程才有机会获取该锁。该线程在释放锁之前可以重复获取此锁，每获取一次便会执行一次state+1，因此ReentrantLock也属于可重入锁。但获取多少次锁就要释放多少次锁，这样才能保证state最终为 0。如果获取锁的次数多于释放锁的次数，则会出现该线程一直持有该锁的情况；如果获取锁的次数少于释放锁的次数，则运行中的程序会报锁异常。

CountDownLatch对AQS的共享方式实现为：CountDownLatch将任务分为N个子线程去执行，将state也初始化为N，N与线程的个数一致，N个子线程是并行执行的，每个子线程都在执行完成后countDown()一次，state会执行CAS操作并减1。在所有子线程都执行完成（state=0）时会unpark()主线程，然后主线程会从await()返回，继续执行后续的动作。

一般来说，自定义同步器要么采用独占方式，要么采用共享方式，实现类只需实现tryAcquire、tryRelease或tryAcquireShared、tryReleaseShared中的一组即可。但AQS也支持自定义同步器同时实现独占和共享两种方式，例如ReentrantReadWriteLock在读取时采用了共享方式，在写入时采用了独占方式。