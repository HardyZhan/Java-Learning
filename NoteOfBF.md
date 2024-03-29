# Java并发编程

## 一、并发编程线程基础

### 1.线程和进程

* 线程是进程中的一个实体，线程本身是不会独立存在的，线程是进程的一个执行路径
* 进程是代码在数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，一个进程中至少有一个线程，进程中的多个线程共享进程的资源(堆和方法区资源)

#### 资源分配

* 操作系统在分配资源时是把资源分配给进程的
* 但是CPU资源比较特殊，它是被分配到线程的，因为真正要占用CPU运行的是线程，所以也说线程是CPU分配的基本单位

### 2.线程的创建与运行

Java中有三种线程创建方式，分别为Runnable接口的run方法，继承Thread类并重写run的方法，使用FutureTask方式。

#### （1） 继承Thread类方式实现

```java
public class ThreadTest {
    public static class myThread extends Thread {
        @Override
        public void run() {
            System.out.println("I am a child thread");
        }
    }
    public static void main(String[] args) {
        MyThread thread = enw MyThread();
        thread.start();
    }
}
```

* 使用继承的好处是，在`run()`方法内获取到当前线程直接使用this就可以了，无须使用`Thread.currentThread()`方法；可以在子类里面添加成员变量，通过set方法设置参数或者通过构造函数进行传递
* 不好的地方是Java不支持多继承，如果继承了Thread类，那么就不能再继承其他类。另外任务与代码没有分离，当多个线程执行一样的任务时需要多份任务代码，而Runnable没有这个限制。

#### （2） 实现Runnable接口的run方法方式实现

```java
public static class RunnableTask implements Runnable {
    @Override
    public void run() {
        System.out.println("I am a child thread");
    }
}
public static void main(String[] args) throws InterruptedException {
    Runnable task = new RunnableTask();
    new Thread(task).start();
    new Thread(task).start();
}
```

* 上述代码中，两个线程共用一个task代码逻辑，如果需要，可以给RunnableTask添加参数进行任务区分。
* 另外，RunnbaleTask可以继承其他类
* 但是只能使用主线程里面被声明为final的变量

#### （3） 使用FutureTask的方式实现

```java
public static class CallerTask implements Callable<String> {
    @Override
    public String call() throws Exception {
        return "hello";
    }
}
public static void main(String[] args) throws InterruptedException {
    FutureTask<String> futureTask = new FutureTask<>(new CallerTask());
    new Thread(futureTask).start();
    try {
        String result = futureTask.get();
        System.out.println(result);
    } catch (ExecutionException e) {
        e.printStackTrace();
    }
}
```

* 前两种方式都无法拿到任务的返回结果，但是Futuretask方式可以

### 3. 线程通知与等待

下面的所有方法都是Object类中的方法

#### （1） wait()函数

* 当一个线程调用一个共享变量的`wait()`方法时，该调用线程会被阻塞挂起，知道发生下面几件事情之一才返回

  * 其它线程调用了该共享对象的`notify()`方法或者`notifyAll()`方法
  * 其它线程调用了该线程的`interrupt()`方法，该方法抛出`InterruptedException`异常返回

  **注意**——如果调用`wait()`方法的线程没有事先获取该对象的监视器锁，则调用`wait()`方法时调用线程会抛出`IllegalMonitorStateException`异常

* 一个线程如何获取一个共享变量的监视器锁

  * 执行`synchronized`同步代码块时，使用该共享变量作为参数

  ```java
  synchronized (共享变量) {
      // do something
  }
  ```

  * 调用该共享变量的方法，并且该方法使用了`synchronized`修饰

  ```java
  synchronized void add (int a, int b) {
      // do something
  }
  ```

  ##### 虚假唤醒

  一个线程可以从挂起状态变为可运行状态，即使该线程没有被其他线程调用`notify()`、`notifyAll()`方法进行通知，或者被中断，或者等待超时

  ##### 解决办法

  不停地去测试该线程被唤醒的条件是否满足，不满足则继续等待

  ```java
  synchronized (obj) {
      while (条件不满足) {
          obj.wait();
      }
  }
  ```

* 当前线程调用共享变量的`wait()`方法后，只会释放当前共享变量上的锁，如果当前线程还持有其他共享变量的锁，则这些锁是不会被释放的。

* 当一个线程调用共享对象的`wait()`方法被阻塞挂起后，如果其他线程中断了该线程，则该线程会抛出`InterruptedException`异常并返回。

#### （2） wait(long timeout)函数

* 如果一个线程调用共享对象的该方法挂起后，没有在指定时间的timeout ms时间内被其它线程调用该共享变量的`notify()`或者`notifyAll()`方法唤醒，那么该函数还是会因为超时而返回。
* 如果在调用该函数时，传递了一个负的timeout则会抛出`IllegalArgumentException`异常。

#### （3） wait(long timeout, int nanos)函数

* 在其内部调用的是`wait(long timeout)`函数，如下代码只有在nanos > 0时才使参数timeout递增1.

```java
public final void wait(long timeout, int nanos) throws InterruptedException {
    if (timeout < 0) {
        throw new InterruptedException("timeout value is negative");
    }
    if (nanos < 0 || nanos > 999999) {
        throw new InterruptedException("nanosencond timeout value out of range");
    }
    if (nanos > 0) {
        timeout ++;
    }
    wait(timeout);
}
```

#### （4） notify()函数

* 一个线程调用共享对象的`notify()`方法后，会唤醒一个在该共享变量上调用wait系列方法后被挂起的线程。一个共享变量上可能会有多个线程在等待，具体唤醒哪个等待的线程是随机的。
* 被唤醒的线程不能马上从wait方法返回并继续执行，它必须在获取了共享对象的监视器锁后才可以返回，也就是唤醒它的线程释放了共享变量上的监视器锁，被唤醒的线程也不一定会获取到共享对象的监视器锁，这是因为该线程还需要和其他线程一起竞争该锁，只有该线程竞争到了共享变量的监视器锁后才可以继续执行。
* 类似wait系列方法，只有当前线程获取到了共享变量的监视器锁后，才可以调用共享变量的`notify()`方法，否则会抛出`InterruptedException`异常。

#### （5） notifyAll()函数

* 会唤醒所有在该共享变量上由于调用wait系列方法而被挂起的线程
* 如果调用`notifyAll()`方法后一个线程调用了该共享变量的`wait()`方法而被放入阻塞集合，则该线程是不会被唤醒的

### 4. 等待线程执行终止的join方法

* join方法是Thread类直接提供的
* join是无参且返回值为void的方法
* 当某个线程的join方法被调用后，当前调用该方法的线程会被阻塞，等待其结束再执行之后的语句。若在阻塞的过程中其他线程调用了当前线程的`interrupted()`方法中断了该线程，该线程则会抛出`InterruptedException`异常而返回

### 5. 让线程睡眠的sleep方法

* Thread类中有一个静态的sleep方法，当一个执行中的线程调用了Thread的sleep方法后，调用线程会暂时让出指定时间的执行权，也就是在这期间不参与CPU的调度，但是该线程所拥有的监视器资源，比如锁还是**持有不让出的**。
* 指定的睡眠时间到了之后，该函数会正常返回，线程就处于就绪状态，然后参与CPU调度，获取到CPU资源后就可以继续运行了。
* 如果在睡眠期间其他线程调用了该线程的`interrupted()`方法中断了该线程，则该线程会在调用sleep方法的地方抛出`InterruptedException`异常而返回。

### 6. 让出CPU执行权的yield方法

* Thread类中有一个静态的yield方法，当一个线程调用yield方法后，实际就是在暗示线程调度器当前线程请求让出自己的CPU使用，但是线程调度器可以无条件忽略这个暗示。

#### sleep与yield区别

* 当线程调用sleep方法时调用线程会被阻塞挂起指定时间，在这期间线程调度器不会去调度该线程。
* 而调用yield方法时，线程只是让出自己剩余的时间片，并没有被阻塞挂起，而是处于就绪状态，线程调度器下一次调度时就有可能调度到当前线程执行。

### 7. 线程中断

Java中的线程中断是一种线程间的协作模式，通过设置线程的中断标志并不能直接终止该线程的执行，而是被中断的线程根据中断状态自行处理。

#### （1） void interrupt()方法

* 中断线程，例如，当线程A运行时，线程B可以调用线程A的interrupt()方法来设置线程A的中断标志为true并立即返回。设置标志仅仅是设置标志，线程A实际并没有被中断，它会继续往下执行。如果线程A因为调用了wait系列函数、join方法或者sleep方法而被阻塞挂起，这时候若线程B调用线程A的interrupt()方法，线程A会在调用这些方法的地方抛出`InterruptedException`异常而返回。

#### （2） boolean isInterrupted()方法

* 检测当前线程是否被中断，如果是返回true，否则返回false。不清楚中断标志。

#### （3） boolean interrupted()方法

* 检测**当前线程**是否被中断，如果是返回true，否则返回false。
* 与isInterrupted()方法不同的是，该方法如果发现当前线程被中断，则会清除中断标志，并且该方法是static方法，可以通过Thread类直接调用。

### 8. 线程死锁

死锁是指两个或两个以上的线程在执行过程中，因争夺资源而造成的互相等待的现象，在无外力作用的情况下，这些线程会一直相互等待而无法继续运行下去。

#### 产生死锁的四个条件

* **互斥条件**。指线程对已经获取到的资源进行排他性使用，即该资源同时只有一个线程占用。如果此时还有其它线程请求获取该资源，则请求者只能等待，直到占有资源的线程释放该资源。
* **请求并持有条件**。指一个线程已经持有了至少一个资源，但又提出了新的资源请求，而新资源已被其它线程占有，所以当前线程会被阻塞，但阻塞的同时并不释放自己已经获取到的资源。
* **不可剥夺条件**。指线程获取到的资源在自己使用完之前不能被其它线程抢占，只有在自己使用完毕后才由自己释放该资源。
* **环路等待条件**。指在发生死锁时，不然存在一个线程——资源的环形链，即线程集合{T0,T1,T2,...,Tn}中的T0正在等待一个T1占用的资源，T1正在等待T2占用的资源，...Tn正在等待已被T0占用的资源。

### 9. 守护线程和用户线程

* Java中的线程分为两类，分别为daemon线程(守护线程)和user线程(用户线程)。
* 守护线程和用户线程的区别之一就是，当最后一个非守护线程结束时，JVM会正常退出，而不管当前是否有守护线程，也就是说，守护线程是否结束并不影响JVM的退出。言外之意就是，只要有一个用户线程还没结束，正常情况下JVM就不会退出。

#### 如何创建守护线程

```java
public static void mian(String[] args) {
    Thread daemonThread = new Thread(new Runnable() {
       public void run() {
           
       } 
    });
    
    // 设置为守护线程
    daemonThread.setDaemon(true);
    daemonThread.start();
}
```

### 10. ThreadLocal

**问题**：多线程访问同一个共享变量时特别容易出现并发问题，特别是在多个线程需要对一个共享变量进行写入时。为了保证线程安全，一般使用者在访问共享变量时需要进行适当的同步。同步的措施一般是加锁，那么有没有另一种方式可以做到？

**方案**：ThreadLocal是JDK包提供的，它提供了线程本地变量，也就是如果你创建了一个ThreadLocal变量，那么访问这个变量的每个线程都会有这个变量的一个本地副本。但当多个线程操作这个变量时，实际操作的是自己本地内存里面的变量，从而避免了线程安全问题。



## 二、并发编程的其它基础知识

### 1. 并发和并行

* **并发**是指同一个时间段内多个任务同时都在执行，并且都没有执行结束。并发任务强调在一个时间段内同时执行，而一个时间段由多个单位时间积累而成，所以说并发的多个任务在单位时间内不一定同时在执行。
* **并行**是说在单位时间内多个任务同时在执行

### 2. Java的内存模型

Java内存模型规定，将所有的变量都存放在主内存中，当线程使用变量时，会把主内存里面的变量复制到自己的工作空间或者叫做工作内存，线程读写变量时操作的是自己工作内存中的变量。

### 3. synchronized关键字

#### （1） synchronized关键字介绍

synchronized块是Java提供的一种原子性内置锁，Java中的每个对象都可以把它当做一个同步锁来使用，这些Java内置的使用者看不到的锁被称为内部锁，也叫作监视器锁。线程的执行代码在进入synchronized代码块前会自动获取内部锁，这时候其他线程访问改同步代码块时会被阻塞挂起。拿到内部锁的线程会在正常退出同步代码块或者抛出异常后或者在同步块调用了该内置锁资源的wait系列方法时释放该内置锁。内置锁是排它锁，也就是当一个线程获取这个锁后，其他线程必须等待该线程释放锁后才能获取该锁。

#### （2） synchronized的内存语义

* 进入synchronized块的内存语义是把synchronized块内使用到的变量从线程的工作内存中清除，这样在synchronized块内使用到该变量时就不会从线程的工作过内存中获取，而是直接从主内存中获取。退出synchronized块的内存语义是把在synchronized块内对共享变量的修改刷新到主内存。
* 其实这也是加锁和释放锁的语义，当获取锁后会清空锁块内本底内存中将会被用到的共享变量，在使用这些共享变量时从主内存进行加载，在释放锁时将本底内存中修改的共享变量刷新到主内存。
* 除了可以解决共享变量内存可见性问题外，synchronized经常被用来实现原子性操作。
* 注意，synchronized关键字会引起线程上下文切换并带来线程调度开销。

### 4. volatile关键字

#### （1） 与synchronized不同之处

* 这是一种弱形式的同步。

* synchronized太笨重，因为它会带来线程上下文的切换开销。
* volatile关键字可以确保一个变量的更新对其它线程马上可见。
* 当一个变量被声明为volatile时，线程在写入变量时不会把值缓存在寄存器或者其他地方，而是会把值刷新会主内存。当其它线程读取该共享变量时，会从主内存重新获取最新值，而不是使用当前线程的工作内存中的值。
* 不保证操作的原子性

#### （2） 与synchronized相似之处

* 当线程写入volatile变量值时就等价于线程退出synchronized同步块（把写入工作内存的变量值同步到主内存）
* 读取volatile变量值时就相当于进入同步块（先清空本地内存变量值，再从主内存获取最新值）

#### （3） 什么时候使用volatile

* 写入变量值不依赖变量值的当前值时。因为如果依赖当前值，将是获取——计算——写入三步操作，这三步操作不是原子性的，而volatile不保证原子性。
* 读写变量值时没有加锁。因为加锁本身已经保证内存可见性，这时候不需要把变量声明为volatile的。

### 5. Java中的CAS操作

#### （1） CAS概念

* Compare and Swap，其是JDK提供的非阻塞原子性操作
* 它通过硬件保证了比较——更新操作的原子性

#### （2） compareAndSwapLong方法介绍

* `boolean compareAndSwapLong(Object obj, long valueOffset, long expect, long update)`
  * compareAndSwap的意思是比较并交换
  * CAS有四个操作数
    * 对象内存位置
    * 对象中的变量的偏移量
    * 变量预期值
    * 新的值
  * CAS操作含义：如果对象obj中内存偏移量valueOffset的变量值为expect，则使用新的值update替换旧的值expect，然后返回true。这就是处理器提供的一个原子性指令

#### （3） 经典ABA问题

* 假如线程I使用CAS修改初始值为A的变量X，那么线程I会首先去获取当前变量X的值（为A），然后使用CAS操作尝试修改X的值为B，如果使用CAS操作成功了，那么程序运行一定是正确的吗？其实未必，这是因为有可能在线程I获取变量X的值A后，在执行CAS前，线程II使用CAS修改了变量X的值为B，然后又使用了CAS修改了变量X的值为A。所以虽然线程I执行CAS时X的值是A，但是这个A已经不是线程I获取时的A了。
* ABA问题的产生是因为变量的状态值产生了环形转换，就是变量的值可以从A到B，然后再从B到A。如果变量的值只能朝着一个方向转换，比如A到B，B到C，不构成环形，就不会存在问题。JDK中的AtomicStampedReference类给每个变量的状态值都配备了一个时间戳，从而避免了ABA问题的产生。

### 6. Java指令重排序

* Java内存模型允许编译器和处理器对指令重排序以提高运行性能，并且只会对不存在**数据依赖性**的指令重排序。在单线程下重排序可以保证最终执行的结果与线程顺序执行的结果一致，但是多线程下就会存在问题。
* 写volatile变量时，可以确保volatile写之前的操作不会被编译器重排序到volatile写之后。
* 读volatile变量时，可以确保volatile读之后的操作不会被编译器重排序到volatile读之前。

### 7. 伪共享

#### （1） 什么是伪共享

* 为解决计算机系统中主内存与CPU之间运行速度差问题，会在CPU与主内存之间添加一级或者多级高速缓冲存储器（Cache）。这个Cache一般是被集成到CPU内部的，所以也叫CPU Cache。
* 在Cache内部是**按行存储**的，其中每一行称为一个Cache行。Cache行是Cache与主内存进行数据交换的**单位**，Cache行的大小一般为2的幂次数字节。
* 当CPU访问某个变量时i，首先会去看CPU Cache内是否有该变量，如果有则直接从中获取，否则就去主内存里面获取该变量，然后把该变量所在内存区域的一个Cache行大小的内存复制到Cache中。由于存放到Cache行的是内存块而不是单个变量，所以可能会把多个变量存放到一个Cache行中。当多个线程同时修改一个缓存行里面的多个变量时，由于同时只能有一个线程操作缓存行，所以相比将每个变量放到一个缓存行，性能会有所下降，这就是伪共享。

#### （2） 如何避免伪共享

* 在JDK8之前一般都是通过字节填充的方式来避免该问题，也就是创建一个变量时使用填充字段填充该变量所在的缓存行，这样就避免了将多个变量存放在同一个缓存行中。

### 8. 锁的概述

#### （1） 乐观锁与悲观锁

* 悲观锁指对数据被外界修改持保守态度，认为数据很容易就会被其它线程修改，所以在数据被处理前先对数据进行加锁，并在整个数据处理过程中，使数据处于锁定状态。悲观锁的实现往往依靠数据库提供的锁机制，即在数据库中，对数据记录操作前给记录加排它锁。如果获取锁失败，则说明数据正在被其他线程修修改，当前线程则等待或者抛出异常。如果获取成功，则对记录进行操作，然后提交事务后释放排它锁。
* 乐观锁是相对悲观锁来说的，它认为数据在一般情况下不会造成冲突，所以在访问记录前不会加排它锁，而是在进行数据提交更新时，才会正式对数据冲突与否进行检测。乐观锁并不会使用数据库提供的锁机制，一般在表中添加verison字段或者使用业务状态来实现。乐观锁直到提交时才锁定，所以不会产生任何死锁。

#### （2） 公平锁与非公平锁

* 根据线程获取锁的抢占机制，锁可以分为公平锁和非公平锁
* 公平锁表示线程获取锁的顺序是按照线程请求锁的时间早晚来决定的，也就是最早请求锁的线程将最早获取到锁
  * `ReentrantLock pairLock = new ReentrantLock(true)`
* 非公平锁则是在运行时闯入，也就是先来不一定先得
  * `ReentrantLock pairLock = new ReentrantLock(false) `如果构造函数不传递参数，则默认是非公平锁。
* 在没有公平性需求的前提下尽量使用非公平锁，因为公平锁会带来性能开销。

#### （4） 独占锁与共享锁

* 根据锁只能被单个线程持有还是能被多个线程共同持有，锁可以分为独占锁和共享锁
* 独占锁保证任何时候都只有一个线程能得到锁，ReentrantLock就是以独占方式实现的。
  * 独占锁是一种悲观锁，由于每次访问资源都先加上互斥锁，这限制了并发性，因为读操作并不会影响数据的一致性，而独占锁只允许在同一个时间由一个线程读取数据，其它线程必须等待当前线程释放锁才能进行读取。
* 共享锁则可以同时由多个线程持有，例如ReadWriteLock读写锁，它允许一个资源可以被多线程同时进行读操作。
  * 共享锁是一种乐观锁，它放宽了加锁的条件，允许多个线程同时进行读操作。

#### （5） 什么是可重入锁

* 当一个线程要获取一个被其他线程持有的独占锁时，该线程会被阻塞，那么当一个线程再次获取它自己已经获取的锁时是否会被阻塞呢？如果不被阻塞，那么我们说该锁时可重入的，也就是只要该线程获取了该锁，那么可以无限次（严格来说是有限次）地进入被该锁锁住的代码。
* 实际上，synchronized内部锁是可重入锁。可重入锁的原理是在锁内部维护一个线程标示，用来标示该锁目前被哪个线程占用，然后关联一个计数器。一开始计数器值为0，说明该锁没有被任何线程占用。
  * 当一个线程获取了该锁时，计数器的值会变成1，这时其它线程再来获取该锁时会发现锁的所有者不是自己而被阻塞挂起。
  * 但是当获取了该锁的线程再次获取锁时发现锁拥有者是自己，就会把计数器值加+1，当释放锁后计数器值-1.当计数器值为0时，锁里面的线程标示被重置为null，这时候被阻塞的线程会被唤醒来竞争获取该锁。

### 9. 自旋锁

* 由于Java中的线程是与操作系统中的线程一一对应的，所以当一个线程在获取锁（比如独占锁）失败后，会被切换到内核状态而被挂起。当线程获取到锁时有需要将其切换到内核状态而唤醒该线程。而从用户状态切换到内核状态的开销是比较大的，在一定程度上会影响并发性能。
* 自旋锁则是，当前线程在获取锁时，如果发现锁已经被其它线程占有，它不马上阻塞自己，在不放弃CPU使用权的情况下，多次尝试获取（默认次数是10，可以使用-XX:PreBlockSpinsh参数设置该值），很有可能在后面几次尝试中其它线程已经释放了该锁。如果尝试指定的次数后仍没有获取到该锁则当前线程才会被阻塞挂起。由此看来自旋锁是使用CPU时间换取线程阻塞与调度的开销，但是很有可能这些CPU时间白白浪费。

## 三、Java并发包中锁原理剖析

### 1. LockSupport工具类

* JDK中的rt.jar包里面的LockSupport是个工具类，它的主要作用是挂起和唤醒线程，该工具类是创建锁和其它同步类的基础
* LockSupport类与每个使用它的线程都会关联一个许可证，在**默认情况下**调用LockSupport类的方法的线程是**不持有许可证**的。

### 2. 抽象同步队列AQS

#### （1） AQS——锁的底层支持

AbstractQueuedSynchronizer抽象同步队列简称AQS，它是实现同步器的基础组件，并发包中锁的底层就是使用AQS实现的。AQS的类图结构如下所示。

[![aqs类图结构](pic\aqs类图结构.png)]()

由图可以看到，AQS是一个FIFO的双向队列，其内部通过节点head和tail记录队首和队尾元素，队列元素的类型为Node。

* Node中的thread变量用来存放进入AQS队列里面的线程
* Node节点内部的SHARED用来标记该线程是获取共享资源时被阻塞挂起后放入AQS队列的，EXCLUSIVE用来标记线程是获取独占资源时被挂起后放入AQS队列的
* waitStatus记录当前线程等待状态，可以为CANCELLED（线程被取消了）、SIGNAL（线程需要被唤醒）、CONDITION（线程在条件队列里面等待）、PROPAGATE（释放共享资源时需要通知其他节点）
* prev记录当前节点的前驱节点
* next记录当前节点的后继节点

在AQS中维持了一个单一的状态信息state，可以通过getState、setState、compareAndSetState函数修改其值。

* 对于ReentrantLock的实现来说，state可以用来表示当前线程获取锁的可重入次数
* 对于读写锁ReentrantReadWriteLock来说，state的高16位表示读状态，也就是获取该读锁的次数，低16位表示获取到写锁的线程的可重入次数
* 对于semaphore来说，state用来表示当前可用信号的个数
* 对于CountDownlatch来说，state用来表示计数器当前的值

AQS有个内部类ConditionObject，用来结合锁实现线程同步。ConditionObject可以直接访问AQS对象内部的变量，比如state状态值和AQS队列。ConditionObject是条件变量，每个条件变量对应一个条件队列（单项链表队列），其用来存放调用条件变量的await方法后被阻塞的线程，如类图所示，这个条件队列的头、尾元素分别为firstWaiter和lastWaiter。

对于AQS来说，线程同步的关键是对状态值state进行操作。根据state是否属于一个线程，操作state的方式分为独占方式和共享方式。

* 在独占方式下获取和释放资源使用的方法为
  * `void acquire(int arg)`
  * `void acquireInterruptibly(int arg)`
  * `boolean release(int arg)`
* 在共享方式下获取和释放资源的方法为
  * `void acquireShared(int arg)`
  * `void acquireShareInterruptibly(int arg)`
  * `boolean releaseShared(int arg)`
* 这两套函数中都有一个带有Interruptibly关键字的函数
  * 不带Interruptibly关键字的方法获取资源时或者获取资源失败被挂起时，其他线程中断了该线程，那么该线程不会因为被中断而抛出异常，它还是继续获取资源或者被挂起，也就是说不对中断进行响应，忽略中断
  * 而带Interruptibly关键字的方法要对中断进行响应，也就是线程在调用带Interruptibly关键字的方法获取资源时或者获取资源失败被挂起时，其他线程中断了该线程，那么该线程会抛出InterruptedException异常而返回

#### （2） AQS——条件变量的支持

* notify和wait是配合synchronized内置锁实现线程间同步的基础设施，条件变量的signal和await方法也是用来配合锁（使用AQS实现的锁）实现线程间同步的基础设施。
* 它们的不同在于，synchronized同时只能对一个与共享变量的notify或wait方法实现同步，而AQS的一个锁可以对应多个条件变量。

条件变量其实就是一个在AQS内部声明的ConditionObject对象，ConditionObject是AQS的内部类，可以访问AQS内部的变量（例如状态变量state）和方法。在每个条件变量内部都维护了一个条件队列，用来存放调用条件变量的await()方法时被阻塞的线程。**注意**这个条件队列和AQS队列不是一回事。

## 四、 Java并发包中线程池ThreadPoolExecutor原理探究

### 1. 介绍

线程池主要解决两个问题：

* 当执行大量异步任务时线程池能够提供较好的性能，在不使用线程池时，每当需要执行异步任务时直接new一个线程来运行，而线程的创建和销毁是需要开销的。线程池里面的线程是可复用的，不需要每次执行异步任务时都重新创建和销毁线程。
* 线程池提供了一种资源限制和管理手段，比如可以限制线程的个数，动态新增线程等。每个ThreadPoolExecutor也保留了一些基本的统计数据，比如当前线程池完成的任务数目等。

另外，线程池也提供了许多可调参数和可扩展接口，以满足不同情境的需要，程序员可以使用更方便的Executors的工厂方法，比如newCachedThreadPool(线程池线程个数最多可达Integer.MAX_VALUE，线程自动回收)、newFixedThreadPool(固定大小的线程池)和newSingleThreadExecutor(单个线程)等来创建线程池，当然用户还可以自定义。

### 2. 类图介绍

Executor其实是一个工具类，里面提供了好多静态方法，这些方法根据用户选择返回不同的线程池案例。ThreadPoolExecutor继承了AbstractExecutorService，成员变量ctl是一个Integer的原子变量，用来记录线程池状态和线程池中线程个数，类似于ReentrantReadWriteLock使用一个变量来保存两种信息。

![ThreadPoolExecutor类图](pic\ThreadPoolExecutor类图.png)

* 如上类图所示，其中mainLock是独占锁，用来控制新增Worker线程操作的原子性。termination是该锁对应的条件队列，在线程调用awaitTermination时用来存放阻塞的线程
* Worker继承AQS和Runnable接口，是具体承载任务的对象。Worker继承了AQS，自己实现了简单不可重入锁，其中
  * state=0表示锁未被获取状态
  * state=1表示锁已经被获取的状态
  * state=-1是创建时默认的状态，为了避免该线程在运行runWorker()方法前被中断

这里假设Integer是32位二进制表示，则其中高3位用来表示线程池状态，后面29位用来记录线程池线程个数。

线程池状态含义如下

* RUNNING：接收新任务并且处理阻塞队列里的任务
* SHUTDOWN：拒绝新任务但是处理阻塞队列里的任务
* STOP：拒绝新任务并且抛弃阻塞队列里的任务，同时会中断正在处理的任务
* TIDYING：所有任务都执行完（包含阻塞队列里面的任务）后当前线程池活动线程数为0，将要调用terminated方法
* TERMINATED：终止状态。terminated方法调用完成以后的状态

线程池状态转换列举如下

* RUNNING->SHUTDOWN：显式调用shutdown()方法，或者隐式调用了finalize()方法里面的shutdown()方法
* RUNNING或SHUTDOWN->STOP：显式调用shutdownNow()方法时
* SHUTDOWN->TIDYING：当线程池和任务队列都为空时
* STOP->TIDYING：当线程池为空时
* TIDYING->TERMINATED：当terminated() hook方法执行完成时

线程池参数如下

* corePoolSize：线程池核心线程个数
* workQueue：用于保存等待执行的任务的阻塞队列，比如基于数组的有界ArrayBlockingQueue、基于链表的无界LinkedBlockingQueue、最多只有一个元素的同步队列SynchronousQueue及优先队列PriorityBlockingQueue等
* maximunPoolSize：线程池最大线程数量
* ThreadFactory：创建线程的工厂
* RejectedExecutionHandler：饱和策略，当队列满并且线程个数达到maximunPoolSize后采取的策略，比如AbortPolicy（抛出异常）、CallerRunsPolicy（使用调用者所在线程来运行任务）、DiscardOldestPolicy（调用poll丢弃一个任务，执行当前任务）及DiscardPolicy（默默丢弃，不抛出异常）
* keeyAliveTime：存活时间。如果当前线程池中的线程数量比核心线程数量多，并且是闲置状态，则这些闲置的线程能存活的最大时间
* TimeUnit：存活时间的单位时间
* newFixedThreadPool：创建一个核心线程个数和最大线程个数都为nThreads的线程池，并且阻塞队列长度为Integer.MAX_VALUE。keeyAliveTime=0说明只要线程个数比核心线程个数多并且当前空闲则回收
* newSingleThreadExecutor：创建一个核心线程个数和最大线程个数都为1的线程池，并且阻塞队列长度为Integer.MAX_VALUE。keeyAliveTime=0说明只要线程个数比核心线程个数多并且当前空闲则回收
* newCachedThreadPool：创建一个按需创建线程的线程池，初始线程个数为0，最多线程个数为Integer.MAX_VALUE，并且阻塞队列为同步队列。keeyAliveTime=60说明只要当前线程在60s内空闲则回收。这个类型的特殊之处在于，加入同步队列的任务会被马上执行，同步队列里面最多只有一个任务





