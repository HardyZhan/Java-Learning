# 深入理解Java虚拟机
### Java技术体系
![Java技术体系所包含的内容](Java技术体系.jpg)
[来源](https://docs.oracle.com/javase/6/docs/)
* JDK(Java Develpoment Kit)是用于支持Java程序开发的最小环境，它包括<br>
  (1) Java程序设计语言<br>
  (2) Java虚拟机<br>
  (3) Java API类库<br>
  
* Java API类库中的Java SE API子集和Java虚拟机这两部分统称为JRE(Java Runtime Environment),JRE是支持Java程序运行的标准环境。

### 运行时数据区域
#### 程序计数器
* 程序计数器是一块较小的内存空间，它的作用可以看作是当前线程所执行的字节码的行号指示器。在虚拟机的概念模型里，字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。
* 如果线程正在执行的是一个java方法，这个计数器记录的是正在执行的虚拟机字节码指令的地址；如果正在执行的是Native方法，这个计数器值则为空(Undefined)。此内存区域是唯一一个Java虚拟机规范中没有规定任何OutOfMemoryError情况的区域。
#### Java虚拟机栈
* **虚拟机栈**描述的是Java方法执行的内存模型：每个方法被执行的时候都会同时创建一个栈帧(Stack Frame)用于存储局部变量表、操作栈、动态链接、方法出口等信息。每一个方法被调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中从入栈道出栈的过程。
* **局部变量表**存放了编译期可知的各种基本数据类型(boolean、byte、char、short、int、float、long、double)，对象引用(reference类型，它不等同于对象本身，根据不同的虚拟机实现，它可能是一个指向对象起始地址的引用指针，也可能指向一个代表对象的句柄或者其它与此对象相关的位置)和returnAddress类型(指向了一条字节码指令的地址)。
* 局部变量表所需的内存空间在编译期间完成分配，当进入一个方法时，这个方法需要在帧中分配多大的局部变量时完全确定的，在方法运行期间不会改变局部变量表的大小。
* 在Java虚拟机规范中，对这个区域规定了**两种异常状况**:<br>
  (1) 如果线程请求的栈深度大于虚拟机所允许的深度，将抛出**StackOverflowError**异常；<br>
  (2) 如果虚拟机栈可以动态扩展(当前大部分的Java虚拟机都可动态扩展，只不过Java虚拟机规范中也允许固定长度的虚拟机栈)，当扩展时无法申请到足够的内存时会抛出**OutOfMemoryError**异常。
  
#### 本地方法栈
* 其与虚拟机栈所发挥的作用时非常相似的，只不过虚拟机栈为虚拟机执行Java方法(也就是字节码)服务，而本地方法栈则是为虚拟机使用到的Native方法服务。

#### Java堆
* 对于大多数应用来说，Java堆(Java Heap)是Java虚拟机所管理的内存中最大的一块。Java堆是被所有线程共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是存放对象实例，几乎所有的对象示例都在这里分配内存。
* Java堆是垃圾收集器管理的主要区域，因此很多时候也被称做”GC堆“(Garbage Collected Heap)。
* 根据Java虚拟机规范的规定，Java堆可以处于物理上不连续的内存空间，只要逻辑上是连续的即可，就像我们的磁盘空间一样。在实现时，既可以实现成固定大小的，也可以是可扩展的，不过当前主流的虚拟机都是按照可扩展来实现的(通过-Xmx和-Xms控制)。
* 如果在堆中没有内存完成实力分配，并且堆也无法再扩展时，将会抛出OutOfMemoryError异常。

#### 方法区
* 方法区(Method Area)与Java堆一样，是各个线程**共享**的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。虽然Java虚拟机规范把方法去描述为堆的一个逻辑部分，但是它却有一个别名叫做NonHeap(非堆)，目的应该是与Java堆区分开来。
* Java虚拟机规范堆这个区域的限制非常宽松，除了和Java堆一样不需要连续的内存和可以选择固定大小或者可扩展外，还可以选择不实现垃圾收集。
* 这个区域的内存回收目标主要是针对常量池的回收和对类型的卸载。

#### 运行时常量池
* 运行时常量池(Runtime Constant Pool)是方法区的一部分。Class文件中，除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池(Constant Pool Table),用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加载后存放到方法区的运行时常量池中。
* 运行时常量池相对于Class文件常量池的另外一个重要特征是具备**动态性**，Java语言并不要求常量一定只能在编译器产生，也就是并非预置入Class文件中常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中，这种特性被开发人员利用的比较多的便是**String类的intern()方法**。

#### 直接内存
* 直接内存(Direct Memory)并不是虚拟机运行时数据区的一部分，也不是Java虚拟机规范中定义的内存区域，但是这部分内存也被频繁地使用，而且也可能导致OutOfMemoryError异常出现。

### 对象访问
 `Object obj = new Object();`
若这句代码出现在方法体中，那么<br>
* ”Object obj“这部分的语义将会反映到Java栈的本地变量表中，作为一个reference类型数据出现。
* ”new Object()“这部分的语义将会反映到Java堆中，形成一块存储了Object类型所有实例数据值(Instance Data,对象中各个实例字段的数据)的结构化内存，根据具体类型以及虚拟机实现的对象内存布局(Object Memory Layout)的不同，这块内存的长度是不固定的。
* 另外，在Java堆中还必须包含能查找到此对象类型数据(如对象类型、父类、实现的接口、方法等)的地址信息，这些类型数据则存储在方法区中。 
* 由于reference类型在Java虚拟机规范里面只规定了一个指向对象的引用，并没有定义这个引用应该通过哪种方式区定位，以及访问到Java堆中的对象的具体信息，因此不同虚拟机实现的对象访问方式会有所不同，主流的访问方式有两种：使用句柄和直接指针。<br>
  (1) 如果使用句柄访问方式，Java堆中将会划分出一块内存来作为句柄池，referen中存储的就是对象的句柄地址，而句柄中包含了对象实例数据类型和类型数据各自的具体信息地址。如图<br>
  ![通过句柄访问对象](句柄.jpg)
  使用句柄访问方式的最大好处就是reference中存储的是稳定的句柄地址，在对象被移动(垃圾收集时移动对象是非常普遍的行为)时只会改变句柄中的实例数据指针，而reference本身不需要被修改。<br>
  (2) 如果使用直接指针访问方式，Java堆对象的布局就必须考虑如何放置访问类型数据䣌相关信息，reference中直接存储的就是对象地址，如图<br>
  ![通过直接指针访问对象](直接指针.jpg)
  使用直接指针访问方式的最大好处就是速度更快，它节省了一次指针定位的时间开销，由于对象的访问在Java中非常频繁，因此这类开销积少成多后也是一项非常可观的执行成本。
  
### Java虚拟机规范中描述的两种异常
#### StackOverflowError异常
* 如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出StackOverflowError异常。
* 在单个线程下，无论是由于栈帧太大，还是虚拟机栈容量太小，当内存无法分配的时候，虚拟机抛出的都是StackOverflowError异常。
#### OutOfMemoryError异常
* 如果虚拟机再扩展栈时无法申请到足够的内存空间，则抛出OutOfMemoryError异常。

### 垃圾收集器与内存分配策略
* GC需要完成的三件事<br>
  (1) 哪些内存需要回收<br>
  (2) 什么时候回收<br>
  (3) 如何回收<br>
  
#### 根搜索算法
* 通过一系列的名为"GC Roots"的对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径称为引用链(Reference Chain)，当一个对象到GC Roots没有任何引用链相连时，则证明此对象是不可用的。 
* 在根搜索算法中不可达的对象，也并非是”非死不可“的，这时候它们暂时处于“缓刑”阶段，要真正宣告一个对象死亡，至少要经历两次标记过程：如果对象在进行根搜索后发现没有与GC Roots相连接的引用链，那它将会被第一次标记并且进行一次筛选，筛选的条件是此对象是否有必要执行finalize()方法。当对象没有覆盖finalize()方法或者finalize()方法已经被虚拟机调用过，虚拟机将这两种情况都视为“没有必要执行”。


#### Java中的引用
* 强引用(Strong Reference)，指在程序代码之中普遍存在，类似"Object obj = new Object()"这类的引用，只要强引用还存在，垃圾收集器永远不会回收掉被引用的对象。
* 软引用(Soft Reference)，软引用用来描述一些还有用，但并非必需的对象。对于软引用关联着的对象，在系统将要发生内存溢出异常之前，将会把这些对象列进回收范围之中并进行第二次回收。如果这次回收还是没有足够的内存，才会抛出内存溢出异常。
* 弱引用(Weak Reference)，弱引用也是用来描述非必需对象的，但是它的强度比软引用更弱一点，被弱引用关联的对象只能生存到下一次垃圾收集发生之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。
* 虚引用(Phantom Reference)，虚引用也称为幽灵引用或者幻影引用，它是最弱的一种引用关系。一个对象是否由虚引用的存在，完全不会对其生存时间构成影响，也无法通过虚引用来取得一个实例对象。为一个对象设置虚引用关联的唯一目的就是希望能在这个对象被收集器回收时收到一个系统通知。

#### 回收方法区
* 方法区的垃圾收集主要回收两部分内容：废弃常量和无用的类。
* 无用的类定义：<br>
  (1) 该类所有的实例都已经被回收，也就是Java堆种不存在该类的任何实例。<br>
  (2) 加载该类的ClassLoader已经被回收。<br>
  (3) 该类对应的java.lang.Class对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。<br>
  
#### 垃圾收集算法
* 标记-清除算法
* 复制算法
* 标记-整理算法
* 分代收集算法

#### 垃圾收集器
* Serial收集器(新生代收集器)
* ParNew收集器(新生代收集器)。可用`-XX:ParallelGCThreads`来限制垃圾收集的线程数。
* Parallel Scavenge收集器(新生代收集器)。目标是达到一个可控制的吞吐量(Throughput)，即CPU用于运行用户代码的时间与CPU总消耗时间的比值。它无法与CMS收集器配合工作。<br>
  (1) 可用`-XX:MaxGCPauseMillis`来控制最大垃圾收集停顿时间。<br>
  (2) 可用`-XX:GCTimeRatio`直接设置吞吐量大小。<br>
  (3) 可用`-XX:+UseAdaptiveSizePolicy`作为一个开关，打开后，不需要手工指定新生代的大小(-Xmn)、Eden与Survivor区的比例(-XX:SurvivorRatio)、晋升老年代对象年龄(-XX:PretenureSizeThreshold)等细节参数虚拟机会根据当前系统的运行情况收集性能监控信息，动态调整这些参数以提供最合适的停顿时间或最大的吞吐量，这种调节方式成为GC自适应的调节策略(GC Ergonomics)。<br>
* Serial Old收集器(Serial收集器的老年代版本)
* Parallel Old收集器(Parallel Scavenge收集器的老年代版本)
* CMS(Concurrent Mark Sweep)收集器。目标是获取最短回收停顿时间。基于“标记-清除”算法。其运作过程分为四个步骤:<br>
  (1) 初始标记(CMS initial mark)<br>
  (2) 并发标记(CMS concurrent mark)<br>
  (3) 重新标记(CMS remark)<br>
  (4) 并发清除(CMS concurrent sweep)<br>
  
* G1收集器。基于“标记-整理”算法。

##### 垃圾收集器参数总结
| 参数 | 描述 |
|:---:|:---:|
| UseSerialGC | 虚拟机运行在Client模式下的默认值，打开此开关后，使用Serial+Serial Old的收集器组合进行内存回收 |
| UseParNewGC | 打开此开关后，使用ParNew+Serial Old的收集器组合进行内存回收 |
| UseConcMarkSweepGC | 打开此开关后，使用ParNew+CMS+Serial Old的收集器组合进行内存回收。Serial Old收集器将作为CMS收集器出现Concurrent Mode Failue失败后的后备收集器使用 |
| UseParallelGC | 虚拟机运行在Server模式下的默认值，打开此开关后，使用Parallel Scavenge+Serial Old（PS MarkSweep）的收集器组合进行内存回收 |
| UseParallelOldGC | 打开此开关后，使用Parallel Scavenge+Parallel Old的收集器组合进行内存回收 |
| SurvivorRatio | 新生代中Eden区域与Survivor区域的容量比值，默认为8，代表Eden:Survivor=8:1 |
| PretenureSizeThreshold | 直接晋升到老年代的对象大小，设置这个参数后，大于这个参数的对象将直接在老年代分配 |
| MaxTenuringThreshold | 晋升到老年代的对象年龄。每个对象在坚持过一次Minor GC之后，年龄就加1，当超过这个参数值时就进入老年代 |
| UseAdaptiveSizePolicy | 动态地调整Java堆中各个区域的大小以及进入老年代的年龄 |
| HandlePromotionFailure | 是否允许分配担保失败，即老年代的剩余空间不足以应付整个新生代的整个Eden和Survivor区的所有对象都存活的极端情况 |
| ParallelGCThreads | 设置并行GC时进行内存回收的线程数 |
| GCTimeRatio | GC时间占总时间的比率，默认值为99，即允许1%的GC时间。尽在使用Parallel Scavenge收集器时生效 |
| MaxGCPauseMillis | 设置GC的最大停顿时间。尽在使用Parallel Scavenge收集器时生效 |
| CMSInitiatingOccupancyFraction | 设置CMS收集器在老年代空间被使用多少后出发垃圾收集。默认值为68%，仅在使用CMS收集器时生效 |
| UseCMSCompactAtFullCollection | 设置CMS收集器在完成垃圾收集后是否要进行一次内存碎片整理。仅在使用CMS收集器时生效 |
| CMSFullGCsBeforeCompaction | 设置CMS收集器在进行若干次垃圾收集后再启动一次内存碎片整理。仅在使用CMS收集器时生效 |

#### Minor GC 和 Full GC
* 新生代GC(Minor GC):指发生在新生代的垃圾收集动作，因为Java对象大多具备朝生夕灭的特性，所以Minor GC非常频繁，一般回收速度也比较快。
* 老年代GC(Major GC/Full GC):指发生在老年代的GC，出现了Major GC，经常会伴随至少一次的Minor GC(但非绝对的，在ParallelScavenge收集器的收集策略里就有直接进行Major GC的策略选择过程)。MajorGC的速度一般会比MinorGC慢10倍以上。

#### 内存分配与回收策略
* 对象优先在Eden分配<br>
  大多数情况下，对象在新生代Eden区中分配。当Eden区没有足够的空间进行分配时，虚拟机将发起一次Minor GC。
* 大对象直接进入老年代
* 长期存活的对象将进入老年代
* 动态对象年龄判定<br>
  为了更好地适应不同程序的内存状况，虚拟机并不总是要求对象的年龄必须达到MaxTenuringThreshold才能晋升老年代，如果在Survivor空间中相同年龄所有对象大小的总和大于Survivor空间的一半，年龄大于或等于该年龄的对象就可以直接进入老年代，无须等到MaxTenuringThreshold中要求的年龄。
  
* 空间分配担保<br>
  在发生Minor GC时，虚拟机会检测之前每次晋升到老年代的平均大小是否大于老年代的剩余空间大小，如果大于，则改为直接进行一次Full GC。如果小于，则查看HandlePromotionFailure设置是否允许担保失败；如果允许，那只会进行Minor GC；如果不允许，则也要改为进行一次Full GC。
  
### 虚拟机性能监控与故障处理工具
Sun JDK监控和故障处理工具

| 名称 | 主要作用 |
|:---:|:-------:|
| jps | JVM Process Status Tool,显示指定系统内所有的HotSpot虚拟机进程 |
| jstat | JVM Statistics Monitoring Tool,用于收集HotSpot虚拟机各方面的运行数据 |
| jinfo | Configuration Info for Java,显示虚拟机配置信息 |
| jmap | Memory Mao for Java,显示虚拟机配置信息 |
| jhat | JVM Heap Dump Browser,用于分析heapdump文件，它会建立一个HTTP/HTML服务器，让用户可以再浏览器上查看分析结果 |
| jstack | Stack Trace for Java,显示虚拟机的线程快照 |

#### jps：虚拟机进程状况工具
* 功能:可以列出正在运行的虚拟机进程，并显示虚拟机执行主类(Main Class, main()函数所在的类)的名称，以及这些进程的本地虚拟机的唯一ID(LVMID, Local Virtual Machine Identifier)。
* jps命令格式: `jps [options] [hostid]`。
* jps可以通过RMI协议查询开启了RMI服务的远程虚拟机进程状态，hostid为RMI注册表中注册的主机名。
* jps工具主要选项

| 选项 | 作用 |
|:---:|:----:|
| -q | 只输出LVMID,省略主类的名称 |
| -m | 输出虚拟机进程启动时传递给主类main()函数的参数 |
| -l | 输出主类的全名，如果进程执行的是Jar包，输出Jar路径 |
| -v | 输出虚拟机进程启动时JVM参数 |

#### jstat:虚拟机统计信息监视工具
* jstat(JVM Statistics Monitoring Tool)是用于监视虚拟机各种运行状态信息的命令行工具。它可以显示本地或远程虚拟机进程中的类装载、内存、垃圾收集、JIT编译等运行数据。
* jstat命令格式: `jstat [ option vmid [interval [s|ms] [count]] ]`。对于命令格式中的VMID与LVMID:如果是本地虚拟机进程，VMID与LVMID是一致的，如果是远程虚拟机进程，那VMID的格式应当是`[protocol:][//]lvmid[@hostname[:port]/servername]`<br>
  (1) 其中，参数interval和count代表查询间隔和次数，如果省略这两个参数，说明只查询一次。<br>
  (2) 假设需要每250毫秒查询一次进程2764垃圾收集的状况，一共 查询20次，那命令应当是`jstat -gc 2764 250 20`。<br>
  (3) 选项option代表着用户希望查询的虚拟机信息，主要分三类:类装载、垃圾收集和运行期编译状况。<br>
  
* jstat工具的主要选项

| 选项 | 作用 |
|:---:|:-----:|
| -class | 监视类装载、卸载数量、总空间及类装载所耗费的时间 |
| -gc | 监视Java堆状况，包括Eden区、2个survior区、老年代、永久代等的容量、已用空间、GC时间合计等信息 |
| -gccapacity | 监视内容与-gc基本相同，但输出主要关注Java堆各个区域使用到的最大和最小空间 |
| -gcutil | 监视内容与-gc基本相同，但输出主要关注已使用空间占总空间的百分比 |
| -gccause | 与-gcutil功能一样，但是会额外输出导致上一次GC产生的原因 |
| -gcnew | 监视新生代GC的状况 |
| -gcnewcapacity | 监视内容与-gcnew基本相同，输出主要关注使用到的最大和最小空间 |
| -gcold | 监视老年代GC的状况 |
| -gcoldcapacity | 监视内容与-gcold基本相同，输出主要关注使用到的最大和最小空间 |
| -gcpermcapacity | 输出永久代使用到的最大和最小空间 |
| -compiler | 输出JIT编译器编译过的方法、耗时等信息 |
| -printcompilation | 输出已经被JIT编译的方法 |
  