# Java并发编程基础

## 进程与线程

* 进程是代码在数据集合上的一次运行活动， 是系统进行资源分配和调度的基本单位，进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。在 Java 中，当我们启动 main 函数时其实就是启动了一个 JVM 的进程，而 main 函数所在的线程就是这个进程中的一个线程，也称主线程。
* 线程则是进程的一个执行路径， 一个进程中至少有一个线程，进程中的多个线程共享进程的资源。 

与进程不同的是同类的多个线程共享进程的**堆**和**方法区**资源，但每个线程有自己的**程序计数器**、**虚拟机栈**和**本地方法栈**，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。 

**线程的状态与生命周期**

* 新建：当一个`Thread`类或其子类的对象被声明并创建时，新生的线程对象处于新建状态。此时它已经有了相应的内存空间和其他资源
* 运行：线程创建之后仅仅是占有了内存资源，在JVM管理的线程中还没有这个线程，此线程调用`start()`方法通知JVM，这样JVM就会知道又有一个新线程排队等候切换了。
* 中断：有4种原因的中断。**a.**线程使用CPU资源期间，执行了`sleep(int millsecond)`方法，是当前线程处于休眠状态。**b.**执行了`wait()`方法，使当前线程进入等待状态。**c.**线程执行某个操作进入阻塞状态**d.**JVM将CPU的资源从当前线程切换给其他线程，使本线程让出CPU的使用权处于中断状态。
* 死亡

> 几个方法的比较
> Thread.sleep(long millis)，一定是当前线程调用此方法，当前线程进入TIMED_WAITING状态，但不释放对象锁，millis后线程自动苏醒进入就绪状态。作用：给其它线程执行机会的最佳方式。
> Thread.yield()，一定是当前线程调用此方法，当前线程放弃获取的CPU时间片，但不释放锁资源，由运行状态变为就绪状态，让OS再次选择线程。作用：让相同优先级的线程轮流执行，但并不保证一定会轮流执行。实际中无法保证yield()达到让步目的，因为让步的线程还有可能被线程调度程序再次选中。Thread.yield()不会导致阻塞。该方法与sleep()类似，只是不能由用户指定暂停多长时间。
> thread.join()/thread.join(long millis)，当前线程里调用其它线程t的join方法，当前线程进入WAITING/TIMED_WAITING状态，当前线程不会释放已经持有的对象锁。线程t执行完毕或者millis时间到，当前线程一般情况下进入RUNNABLE状态，也有可能进入BLOCKED状态（因为join是基于wait实现的）。
> obj.wait()，当前线程调用对象的wait()方法，当前线程释放对象锁，进入等待队列。依靠notify()/notifyAll()唤醒或者wait(long timeout) timeout时间到自动唤醒。
> obj.notify()唤醒在此对象监视器上等待的单个线程，选择是任意性的。notifyAll()唤醒在此对象监视器上等待的所有线程。

**线程的创建与运行**

Java 中有三种线程创建方式，分别为实现 Runnable 接口的 run 方法，继承 Thread 类并重写 run 的方法，使用 FutureTask 方式。

```java
public class ThreadTest extends Thread{
    public void run() {
        System.out.println("Hello啊");
    }
    public static void main(String[] args) {
        ThreadTest t = new ThreadTest();
        t.start();
    }
}
```

其实调用 start 方法后线程并没有马上执行而是处于就绪状态， 这个就绪状态是指该已经获取了除 CPU 资源外的其他资源，等待获取 CPU 资源后才会真正处于运行状态。 一旦 run 方法执行完毕，该线程就处于终止状态。

```
public class ThreadRunnable implements Runnable{
    @Override
    public void run() {
        System.out.println("I am a thread");
    }

    public static void main(String[] args) {
        ThreadRunnable t = new ThreadRunnable();
        new Thread(t).start();
    }
}
```

使用 FutureTask 的方式。

```java
public class ThreadCaller implements Callable<String>{
    @Override
    public String call() throws Exception {
        return "Hello";
    }

    public static void main(String[] args) {
        //创建异步任务
        FutureTask<String> futureTask = new FutureTask<>(new ThreadCaller());
        //启动线程
        new Thread(futureTask).start();
        try{
            //等待任务执行完毕，并返回结果
            String result = futureTask.get();
            System.out.println(result);
        }catch(InterruptedException | ExecutionException e){
            e.printStackTrace();
        }
    }
}
```

小结 ： 使用继承方式的好处是方便传参，你可以在子类里面添加成员变量，通过 set 方法设置参数或者通过构造函数进行传递，而如果使用 Runnable 方式，则只能使用主线 程里面被声明为 final 的变量。继承方式不好的地方是 Java 不支持多继承，如果继承了 Thread 类， 那么子类不能再继承其他类，而 Runable 则没有这个限制。前两种方式都没办法拿到任务的返回结果，但是 Futuretask 方式可以。

## 理解线程上下文切换

> 多线程编程中一般线程的个数都大于 CPU 核心的个数，而一个 CPU 核心在任意时刻只能被一个线程使用，为了让这些线程都能得到有效执行，CPU 采取的策略是为每个线程分配时间片并轮转的形式。当一个线程的时间片用完的时候就会重新处于就绪状态让给其他线程使用，这个过程就属于一次上下文切换。
>
> 概括来说就是：当前任务在执行完 CPU 时间片切换到另一个任务之前会先保存自己的状态，以便下次再切换回这个任务时，可以再加载这个任务的状态。**任务从保存到再加载的过程就是一次上下文切换**。
>
> 上下文切换通常是计算密集型的。也就是说，它需要相当可观的处理器时间，在每秒几十上百次的切换中，每次切换都需要纳秒量级的时间。所以，上下文切换对系统来说意味着消耗大量的 CPU 时间，事实上，可能是操作系统中时间消耗最大的操作。
>
> Linux 相比与其他操作系统（包括其他类 Unix 系统）有很多的优点，其中有一项就是，其上下文切换和模式切换的时间消耗非常少。

## 线程死锁

什么是线程死锁

死锁是指两个或两个以上的线程在执行过程中，因争夺资源而造成的互相等待的现象， 在无外力作用的情况下，这些线程会一直相互等待而无法继续运行下去

> 死锁的产生必须具备以 下四个条件。
> · 互斥条件： 指线程对己经获取到的资源进行排它性使用 ， 即该资源同时只由一个线 程占用。如果此时还有其他线程请求获取该资源，则请求者只能等待，直至占有资 源的线程释放该资源。
>
> · 请求并持有条件 ： 指一个线程己经持有了至少一个资源， 但又提出了新的资源请求， 而新资源己被其他线程占有，所以当前线程会被阻塞，但阻塞的同时并不释放自己经获取的资源。
>
> · 不可剥夺条件 ： 指线程获取到的资源在自己使用完之前不能被其他线程抢占， 只有 在自己使用完毕后才由 自 己释放该资源。
>
> · 环路等待条件 ： 指在发生死锁时， 必然存在一个线程→资源的环形链， 即线程集合 {TO , TL T2，…， Tn｝中 的 TO 正在等待一个 Tl 占用 的资源， Tl 正在等待 T2 占 用的资源，……Tn 正在等待己被 TO 占用的资源。

* 如何避免死锁？

**破坏互斥条件**

这个条件我们没有办法破坏，因为我们用锁本来就是想让他们互斥的（临界资源需要互斥访问）。

**破坏请求与保持条件**

一次性申请所有的资源。

**破坏不剥夺条件**

占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。

**破坏循环等待条件**

靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。

## 锁的概述

* **乐观锁**

乐观锁是相对悲观锁来说的，它认为数据在一般情况下不会造成冲突，所以在访问记录前不会加排它锁，而是在进行数据提交更新时，才会正式对数据冲突与否进行检测

* **悲观锁**

悲观锁指对数据被外界修改持保守态度，认为数据很容易就会被其他线程修改，所以 在数据被处理前先对数据进行加锁，并在整个数据处理过程中，使数据处于锁定状态。 悲 观锁的实现往往依靠数据库提供的锁机制，即在数据库中 ，在对数据记录操作前给记录加 排它锁。 

* **公平锁与非公平锁**

根据线程获取锁的抢占机制，锁可以分为公平锁和非公平锁，公平锁表示线程获取锁 的顺序是按照线程请求锁的时间早晚来决定的，也就是最早请求锁的线程将最早获取到锁。 而非公平锁则在运行时闯入，也就是先来不一定先得。

* **独占锁与共享锁**

根据锁只能被单个线程持有还是能被多个线程共同持有，锁可以分为独占锁和共享锁。
独占锁保证任何时候都只有一个线程能得到锁， `ReentrantLock `就是以独占方式实现 的。 共享锁则可以同时由多个线程持有，例如 `ReadWriteLock `读写锁，它允许一个资源可 以被多线程同时进行读操作。
独占锁是一种悲观锁，由于每次访问资源都先加上互斥锁，这限制了并发性，因为读操作并不会影响数据的一致性，而独占锁只允许在同一时间由一个线程读取数据，其他线程必须等待当前线程释放锁才能进行读取。
共享锁则是一种乐观锁，它放宽了加锁的条件，允许多个线程同时进行读操作。

* **什么是可重入锁**

当一个线程要获取一个被其他线程持有的独占锁时，该线程会被阻塞，那么当一个线程再次获取它自己己经获取的锁时是否会被阻塞呢？如果不被阻塞，那么我们说该锁是可 重入的，也就是只要该线程获取了该锁，那么可以无限次数（严格来说是有限次数）地进入被该锁锁住的代码。

* **自旋锁**

由于 Java 中的线程是与操作系统中的线程一一对应的，所以当一个线程在获取锁（比如独占锁）失败后，会被切换到内核状态而被挂起。 当该线程获取到锁时又需要将其切换到内核状态而唤醒该线程。 而从用户状态切换到内核状态的开销是比较大的，在一定程度上会影响并发性能。自旋锁则是，当前线程在获取锁时，如果发现锁已经被其他线程占有， 它不马上阻塞自己，在不放弃 CPU 使用权的情况下，多次尝试获取（默认次数是10，可以使用 －XX:PreB lockS pinsh 参数设置该值），很有可能在后面几次尝试中其他线程己经释 放了锁。 如果尝试指定的次数后仍没有获取到锁则当前线程才会被阻塞挂起。 由此看来自旋锁是使用 CPU 时间换取线程阻塞与调度的开销，但是很有可能这些 CPU 时间白白浪费了 。



## 线程池



## ThreadLocal

ThreadLocal 是 JDK 包提供的，它提供了线程本地变量，也就是如果你创建了 一个 ThreadLocal 变量，那么访问这个变量的每个线程都会有这个变量的一个本地副本。 当多 个线程操作这个变量时，实际操作的是自己本地内存里面的变量，从而避免了线程安全问 题。创建一个 ThreadLocal 变量后，每个线程都会复制一个变量到自己的本地内存。

**ThreadLocal原理：**

```java
public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }
private void set(ThreadLocal<?> key, Object value) {
    	...
        tab[i] = new Entry(key, value);
}
static class Entry extends WeakReference<ThreadLocal<?>> {
            Object value;
            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }
```

在每个线程内部都有一个名 为 threadLocals 的成员变量， 该变量的类型为 HashMap， 其中 key 为我们定义的 ThreadLocal 变量的 this 引用 ，value 则为我们使用 set 方法设置的值。 每个线程的本地变量存放在线程自己的内存变量 threadLocals 中，在`set(ThreadLocal<?> key, Object value)`时，利用Entry实现了对key的弱引用。当`Entry`使用强引用，即使`t=null`,但key的引用依然指`ThreadLocal`对象,所以会有内存的泄露，而使用若引用则不会，但是还是有内存泄露存在，`ThreadLocal`被回收，key 的值变为了`null`，则导致整个value再也无法访问到，因此依旧存在内存泄露，因此使用完毕后要记得调用 ThreadLocal 的 remove 方法删除对应线程的 threadLocals 中的本地变量。

## synchronized 关键字

因为线程共享相同的内存地址空间，且并发的运行，它们可能访问或修改其它线程正在使用的变量。编写线程安全的代码，本质上就是对于共享、可变的状态的管理。	

synchronized 块是 Java 提供的一种原子性内置锁， Java 中的每个对象都可以把它当作 一个同步锁来使用 ， 这些 Java 内置的使用者看不到的锁被称为内部锁，也叫作监视器锁。 线程的执行代码在进入 synchronized 代码块前会自动获取内部锁，这时候其他线程访问该 同步代码块时会被阻塞挂起。拿到内部锁的线程会在正常退出同步代码块或者抛出异常后 或者在同步块内调用了该内置锁资源的 wait 系列方法时释放该内置锁。 内置锁是排它锁， 也就是当一个线程获取这个锁后， 其他线程必须等待该线程释放锁后才能获取该锁。

加锁和释放锁的语义，当获取锁后会清空锁块内本地内存中将会被用到的共享变量，在使用这些共享变量时从主内存进行加载，在释放锁时将本地内存中修改的共享变量刷新到主内存。

```java
public synchronized static void m1(){
        counts--;
        System.out.println(Thread.currentThread().getName() + "counts=" + counts);
 }
//锁在静态方法上，相当于锁定Class对象
public static void m2(){
        synchronized(synchronizedTest.class){
            counts--;
        }
    }
```

## Volatile关键字

上面介绍了使用锁的方式可以解决共享变量内存可见性问题，但是使用锁太笨重，因 为它会带来线程上下文的切换开销。 对于解决内存可见性问题， Java 还提供了一种弱形式 的同步，也就是使用 volatile 关键字。 该关键字可以确保对一个变量的更新对其他线程马 上可见。 当一个变量被声明为 volatile 时，线程在写入变量时不会把值缓存在寄存器或者 其他地方，而是会把值刷新回主内存。 当其他线程读取该共享变量时，会从主内存重新获 取最新值，而不是使用当前线程的工作内存中的值。 volatile 的内存语义和 synchronized 有 相似之处，具体来说就是，当线程写入了 volatile 变量值时就等价于线程退出 synchronized 同步块（把写入工作内存的变量值同步到主内存），读取 volatile 变量值时就相当于进入同 步块 （先清空本地内存变量值，再从主内存获取最新值）。

**synchronized和volatile的区别：**

- volatile本质：是java虚拟机(JVM)当前变量在工作内存中的值是不确定的，需要从主内存中读取；synchronized则是锁定当前的变量，只有当前线程可以访问到该变量，其他的线程将会被阻塞。
- volatile只能实现变量的修改可见性，并不能保证原子性；而synchronized则可以保证变量的修改可见性和原子性。
- volatile只能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的。
- volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。

保证内存可见性

禁止指令重排序

## CAS操作

CAS 即 Compare and Swap，其是 JDK 提供的非阻塞原子性操作，它通过硬件保证了 比较更新操作的原子性。 JDK 里面的 Unsafe 类提供了一系列的 compareAndSwap方法， 下面以 compareAndSwapLong 方法为例进行简单介绍。
boolean compareAndSwapLong(Object obj,long valueOffset,long expect, long update）方法 ： 其中 compareAndSwap 的意思是比较并交换。 CAS 有四个操作数， 分别为 ： 对象内存位置、 对象中的变量的偏移量、 变量预期值和新的值。 其操作含义是， 如果对象 obj 中内存偏移量为 valueOffset 的变量值为 expect，则使用新的值 update 替换旧的值 expect。 这是处理器提供的一个原子性指令。

```java
cas(V,Expected,NewValue)
if V == E
   V = New
otherwise try agin or fail
```

* **ABA问题**

##　ReentrantLock 

ReentrantLock 是可重入的独占锁， 同时只能有一个线程可以获取该锁，其他获取该锁的线程会被阻塞而被放入该锁的 AQS 阻塞队列里面。

## 并发容器类





## AQS

AQS的全称为（AbstractQueuedSynchronizer） 抽象队列同步器

## NIO BIO AIO























