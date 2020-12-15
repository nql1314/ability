### 线程状态
![](../resources/thread.jpg)
### 新建(new),可运行(runnable),阻塞(blocked),无限期等待(waiting),限期等待(timed waiting),死亡(terminated)
* 线程可以通过实现runnable,callable,继承thread.并且实现接口会更好一些,因为:
  Java 不支持多重继承,因此继承了 Thread 类就无法继承其它类,但是可以实现多个接口;
  类可能只要求可执行就行,继承整个 Thread 类开销过大.
### Executor线程池框架
    1. CachedThreadPool:一个任务创建一个线程;
    2. FixedThreadPool:所有任务只能使用固定大小的线程;
    3. SingleThreadExecutor:相当于大小为 1 的 FixedThreadPool.
* ExecutorService定义了一些生命周期的方法
* 用ThreadPoolExecutor类自定义线程池
### 线程
* Daemon 守护线程,当所有非守护线程结束后,程序终止,杀死所有守护线程
* yield() 对线程调度器的一个建议,而且也只是建议具有相同优先级的其它线程可以运行.
### 中断
* InterruptedException 调用thread.interrupt()中断,如果该线程处于阻塞、限期等待或者无限期等待状态,那么就会抛出 InterruptedException,从而提前结束该线程.但是不能中断 I/O 阻塞和 synchronized 锁阻塞
* interrupted() 在循环体中使用 interrupted() 方法来判断线程是否处于中断状态,从而提前结束线程
* 调用 Executor 的 shutdown() 方法会等待线程都执行完毕之后再关闭,但是如果调用的是 shutdownNow() 方法,则相当于调用每个线程的 interrupt() 方法
### 互斥同步
* synchronized 可以同步代码块,方法,类,静态方法,基于JVM实现,不可中断
* 方法同步调用指令读取运行时常量池中方法的 ACC_SYNCHRONIZED 标志来隐式实现的,其他是基于进入和退出管程(Monitor)对象实现,monitorenter 和 monitorexit 指令
* 锁的状态总共有四种,无锁状态、偏向锁、轻量级锁和重量级锁.单向升级
* ReentrantLock 是 JDK 实现的,可中断,可以是公平锁
### 线程协作
* join() 挂起当前线程,直到目标线程结束
* wait() notify() notifyAll() ,在同步方法或同步代码块使用
* await() signal() signalAll() java.util.concurrent 类库中提供了 Condition 类来实现线程之间的协调
### cas
* compareAndSet Atomic原子类
## Actor模型
* scala

## concurrent并发包
* ReentrantLock 同一线程可多次获得锁

## Excutors
### Callable,Future
https://blog.csdn.net/qq_19431333/article/details/77483763
 * 带返回值,能抛出异常
 * Future接口代表运算的结果,get方法阻塞,直到返回结果
 * Future接口是提供异步计算的结果的任务
 * FutureTask 状态:NEW(0),COMPLETING(1),NORMAL(2),EXCEPTIONAL(3),CANCELLED(4),INTERRUPTING(5),INTERRUPTED(6).

### 线程中断
https://www.cnblogs.com/yangming1996/p/7612653.html
  * java 的支持:public boolean isInterrupted(), public void interrupt(),public static boolean interrupted()
    isInterrupted不会清除中断标志位,而interrupted会
  * 线程一共6种状态,分别是NEW,RUNNABLE,BLOCKED,WAITING,TIMED_WAITING,TERMINATED（Thread类中有一个State枚举类型列举了线程的所有状态） 
  * NEW 和 TERMINATED状态不会设置中断标志位,中断操作会无效
  * RUNNABLE状态中断会设置中断标志位但不会实际中断,具体操作是在run方法中交给程序去判断
  * BLOCKED,阻塞状态中断无影响,但仍会设置中断标志位
  * WAITING/TIMED_WAITING,中断会抛出InterruptedException,并清空中断标志位
  
### ThreadFactory
线程工厂
https://www.cnblogs.com/zhujiabin/p/5404771.html
 * newCachedThreadPool
    * 创建一个可缓存线程池,如果线程池长度超过处理需要,可灵活回收空闲线程,若无可回收,则新建线程.
 * newFixedThreadPool
    * 创建一个定长线程池,可控制线程最大并发数,超出的线程会在队列中等待.
 * newScheduledThreadPool
    * 创建一个定长线程池,支持定时及周期性任务执行.
 * ExcutorCompletionService
    * 创建一个单线程化的线程池,它只会用唯一的工作线程来执行任务,保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行.
### Folk/Join
http://ifeve.com/talk-concurrency-forkjoin/
 * 切分任务,合并结果

### Lock
* ReentrantLock 
    * 可重入锁,通过lock()加锁,finally里unlock解锁
    * lockInterruptibly(),加可中断锁,在等待时可以interrupt()
    * tryLock()或者tryLock(long timeout, TimeUtil unit)进行一次限时的锁等待,没获得就返回
    * new ReentrantLock(true)创建公平锁,按先后顺序获取锁
    * 通过condition 进行wait(),notify()等操作
    
     
### Atomic
### volatile
### ThreadLocal