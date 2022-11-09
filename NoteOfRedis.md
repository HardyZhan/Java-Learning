# 一、 Redis6

## 1. NoSQL数据库

### （1） 简介

NoSQL（NoSQL=Not only SQL）,意为“不仅仅是SQL”，泛指非关系型的数据库。

NoSQL不依赖业务逻辑方式存储，而以简单的**key-value**模式存储，因此大大增加了数据库的扩展能力

* 不遵循SQL标准
* 不支持ACID
* 远超于SQL的性能

### （2） 适用场景

* 对数据高并发的读写
* 海量数据的读写
* 对数据的高可扩展性

### （3） 不适用场景

* 需要事务支持
* 基于sql的结构化查询存储，处理复杂的关系，需要**即席**查询
* （用不着sql的和用了sql也不行的情况，请考虑用NoSQL）

### （4） 常见的NoSQL数据库

* Memcache
  * 很**早**出现的NoSQL数据库
  * 数据都在内存中，一般**不持久化**
  * 支持简单的key-value模式，支持**类型单一**
  * 一般是作为**缓存数据库**辅助持久化的数据库
* Redis
  * 几乎覆盖了Memcached的绝大部分功能
  * 数据都在内存中，**支持持久化**，主要用作备份恢复
  * 除了支持简单的key-value模式，**还支持多种数据结构的存储**，比如**list、set、hash、zset**等
  * 一般是作为**缓存数据库**辅助持久化的数据库
* MongoDB
  * 高性能、开源、模式自由（schema free）的**文档型数据库**
  * 数据都在内存中，如果内存不足，把不常用的数据保存到硬盘
  * 虽然是key-value模式，但是对value（尤其是**json**）提供了丰富的查询功能
  * 支持**二进制数据及大型对象**
  * 可以根据数据的特点**替代RDBMS**，成为独立的数据库，或者配合RDBMS，存储特定的数据

## 2. Redis6安装与启动关闭

### （1） 安装过程

[Redis官方网站](http://redis.io)

基于linux-centos系统，下载压缩包后放入/opt目录下

由于安装前所在环境必须安装gcc

* `yum install gcc` 安装gcc环境
* `tar -zxvf 压缩包名称` 解压redis压缩包
* `cd 文件夹名称` 进入解压后的文件夹
* `make` 编译所有文件
* `make install` 安装redis
* 安装目录默认为/usr/local/bin，其中有如下文件
  * redis-benchmark:性能测试工具，可以在自己本子运行，看看自己本子性能如何
  * redis-check-aof:修复有问题的AOF文件
  * redis-check-dump:修复有问题的dump.rdb文件
  * redis-sentinel:Redis集群使用
  * **redis-server**:Redis服务器启动命令
  * **redis-cli**:客户端，操作入口

### （2） 启动及关闭

* 前台启动（不推荐）：`redis-server`
* 后台启动（推荐）：
  * 修改redis目录中的redis.conf文件，将其中的daemonize no改成yes
  * 进入/usr/local/bin默认安装目录启动redis：`redis-server redis.conf的路径`
  * 执行ps命令查看redis运行情况：`ps -ef | grep redis`
  * 执行`redis-cli`命令通过客户端连接redis
  * 执行`ping`命令测试连通状态
* 关闭redis
  * 单实例关闭：`redis-cli shutdown`
  * 进入终端后再关闭：`shutdown`
  * 多实例关闭：`redis-cli -p 端口号 shutdown`

### （3） Redis相关知识

* 默认16个数据库，类似数组下标从0开始，初始默认使用0号库
* 使用命令select &lt;dbid&gt; 来切换数据库。如：`select 8`
* 统一密码管理，所有库同样密码
* `dbsize` 查看当前数据库的key的数量
* `flushdb` 清空当前库
* `flushall` 通杀全部库

Redis是单线程+多路IO复用技术

多路复用是指使用一个线程来检查多个文件描述符（Socket）的就绪状态，比如调用select和poll函数，传入多个文件描述符，如果有一个文件描述符就绪，则返回，否则阻塞直到超时。得到就绪状态后进行真正的操作可以在同一个线程里执行，也可以启动线程执行（比如使用线程池）

## 3. 常用五大数据类型

### （1） Redis键（key）

* `key *` 查看当前库所有key
* `exists key` 判断某个key是否存在
* `type key` 查看你的key是什么类型
* `del key` 删除指定的key数据
* `unlink key` 根据value选择非阻塞删除
  * 仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作
* `expire key 10` 10秒钟：为给定的key设置过期时间
* `ttl key` 查看还有多少秒过期，-1表示永不过期，-2表示已过期
* `select` 命令切换数据库
* `dbsize` 查看当前数据库的key的数量
* `flushdb` 清空当前库
* `flushall` 通杀全部库

### （2） Redis字符串（String）

* 简介

  * String是Redis最基本的类型，一个key对应一个value
  * String类型是二进制安全的。意味着Redis的string可以包含任何数据，比如jpg图片或者序列化的对象
  * String类型是Redis最基本的类型，一个Redis中字符串value最多可以是512M

* 常用命令

  * `set <key> <value>` 添加键值对

    ![redis_T1](E:\mymenu\Java-Learning\pic\redis_T1.png)

    * NX：当数据库中key不存在时，可以将key-value添加数据库
    * XX：当数据库中key存在时，可以将key-value添加数据库，与NX参数互斥
    * EX：key的超时秒数
    * PX：key 的超市毫秒数，与EX互斥

  * `get <key>` 查询对应键值

  * `append <key> <value>` 将给定的&lt;value&gt;追加到原值的末尾

  * `strlen <key>` 获得值的长度

  * `setnx <key> <value>` 只有在key不存在时，设置key的值

  * `incr <key>` 将key中储存的数字值增1，只能对数字值操作，如果为空，新增值为1

  * `decr <key>` 将key中储存的数字值减1，只能对数字值操作，如果为空，新增值为-1

  * `incrby/decrby <key> <步长>` 将key中储存的数字值增减。自定义步长

  * `mset <key> <value> <key> <value> <key> <value>...` 同时设置一个或多个键值对

  * `mget <key> <key> <key>...` 同时查询一个或多个键

  * `msetnx <key> <value> <key> <value> <key> <value>...` 同时设置一个或多个key-value对，当且仅当所有给定key都不存在。**原子性，有一个失败则都失败**

  * `getrange <key> <起始位置> <结束位置>` 获得值的范围，类似java中的substring，左闭右闭

  * `setrange <key> <起始位置> <value>`  用&lt;value&gt;设置覆写&lt;key&gt;所储存的字符串值，从起始位置开始(索引从0开始)

  * `setex <key> <过期时间> <value>` 设置键值的同时，设置过期时间，单位秒

  * `getset <key> <value>` 以新换旧，设置了新值的同时获得旧值

* 原子性

  * 所谓**原子**操作是指不会被线程调度机制打断的操作
  * 这种操作一旦开始，就一直运行到结束，中间不会有任何context switch（切换到另一个线程）
  * 在单线程中，能够在单挑指令中完成的操作都可以认为是“原子性操作”，因为中断只能发生于指令之间
  * 在多线程中，不能被其它进程（线程）打断的操作就叫原子操作
  * Redis单命令的原子性主要得益于Redis的单线程

* 数据结构

  * String的数据结构为简单动态字符串（Simple Dynamic String，缩写SDS）。是可以修改的字符串，内部结构实现上类似于java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配。注意的是字符串最大长度为512M。

### （3） Redis列表（List）

* 简介
  * 单键多值
  * Redis列表是简单的字符串列表，按照插入顺序排序，可以添加一个元素到列表的头部（左边）或者尾部（右边）
  * 它的底层实际是一个**双向链表**，对两端的操作性很高，通过索引下标去操作中间的节点性能会较差
* 常用命令
  * `lpush/rpush <key> <value1> <value2>...` 从左边/右边插入一个或多个值
  * `lpop/rpop <key>` 从左边/右边吐出一个值。**值在键在，值光键亡**
  * `rpoplpush <key1> <key2>` 从&lt;key1&gt;列表右边吐出一个值，插到&lt;key2&gt;列表左边
  * `lrange <key> <start> <stop>` 按照索引下标获得元素（从左到右）
    * `lrange mylist 0 -1  0左边第一个，-1右边第一个（0-1表示获取所有）
  * `lindex <key> <index>` 按照索引下标获得元素（从左到右）
  * `llen <key>` 获得列表长度
  * `linsert <key> before <value> <newvalue>` 在&lt;value&gt;的后面插入&lt;newvlaue&gt;插入值
  * `lrem <key> <n> <value>` 从左边删除n个value（从左到右）
  * `lset <key> <index> <value>` 将列表key下标为index的值替换成value
* 数据结构
  * List的数据结构为快速链表quickList
  * 首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是ziplist，即压缩列表
    * 它将所有的元素紧挨着一起存储，分配的是一块连续的内存
  * 当数据量比较多的时候才会改成quicklist
  * 因为普通的链表需要的附加指针空间太大，会比较浪费空间。比如这个列表里存的只是int类型的数据，结构上还需要两个额外的指针prev和next
  * Redis将链表和ziplist结合起来组成了quicklist，也就是将多个ziplist使用双向指针串起来使用。这样既满足了快速的插入删除性能，又不会出现太大的空间冗余

### （4） Redis集合（Set）

* 简介
  * Redis Set对外提供的功能与list类似是一个列表的功能，特殊之处在于set是可以**自动排重**的，当你需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的。
  * Redis的set时String类型的**无序集合**，它底层其实是一个value为null的hash表，所以添加，删除，查找的复杂度都是O(1)
* 常用命令
  * `sadd <key> <value1> <value2>...` 将一个或多个member元素加入到集合key中，已经存在的member元素将被忽略
  * `smembers <key>` 取出该集合的所有值
  * `sismember <key> <value>` 判断集合 &lt;key&gt;是否为含有该&lt;value&gt;值，有1，没有0
  * `scard <key>` 返回该集合的元素个数
  * `srem <key> <value1> <value2>...` 删除集合中的某个元素
  * `spop <key>` **随机从该集合中吐出一个值**
  * `srandmember <key> <n>` 随机从该集合中取出n个值，不会从集合中删除
  * `smove <source> <destination> value` 把一个集合中一个值从一个集合移动到另一个集合
  * `sinter <key1> <key2>` 返回两个集合的**交集**元素
  * `sunion <key1> <key2>` 返回两个集合的**并集**元素
  * `sdiff <key1> <key2>` 返回两个集合的**差集**元素（key1中的，不包含key2中的）
* 数据结构
  * Set数据结构是dict字典，字典是用哈希表实现的
  * Java中HashSet的内部实现使用的是HashMap，只不过所有的value都指向同一个对象。
  * Redis的Set结构也一样，它的内部也是用hash结构，所有的value都指向同一个内部值

### （5） Redis哈希（Hash）

* 简介
  * Redis hash是一个键值对集合
  * Redis hash是一个string类型的**field**和**value**的映射表，hash表特别适合用于存储对象，类似Java里面的`Map<String, Object>`
* 常用命令
  * `hset <key> <field> <value>` 给&lt;key&gt;集合中的&lt;field&gt;键赋值&lt;value&gt;
  * `hget <key1> <field>` 从&lt;key1&gt;集合中取出value
  * `hmset <key1> <field1> <value1> <field2> <value2>...` 批量设置hash的值
  * `hexists <key1> <field>` 查看哈希表key中，给定field是否存在
  * `hkeys <key>` 列出该hash集合的所有field
  * `hvals <key>` 列出该hash集合的所有value
  * `hincrby <key> <field> <increment>` 为哈希表key中的域field的值加上增量 1 -1
  * `hsetnx <key> <field> <value>` 将哈希表中的域field的值设置为value，当且仅当域field不存在
* 数据结构
  * Hash类型对应的数据结构是两种：ziplist（压缩列表），hashtable（哈希表）
  * 当field-value长度较短且个数较少时，使用ziplist，否则使用hashtable

### （6） Redis有序集合（sorted set）

* 简介
  * Redis有序集合zset与普通集合set非常相似，是一个没有重复元素的字符串集合
  * 不同之处是有序集合的每个成员都关联了一个**评分（score）**，这个评分（score）被用来按照从最低分到最高分的方式排序集合中的成员。**集合的成员是唯一的，但是评分可以是重复的**。
  * 因为元素是有序的，所以也可以很快的根据评分（score）或者次序（position）来获取一个范围的元素
  * 访问有序集合的中间元素也是非常快的，因此能够使用有序集合作为一个没有重复成员的智能列表
* 常用命令
  * `zadd <key> <score1> <value1> <score2> <value2>...` 将一个或多个member元素及其score值加入到有序集合key中
  * `zrange <key> <start> <stop> [WITHSCORES]` 返回有序集合key中，下标在&lt;start&gt;&lt;stop&gt;之间的元素，带WITHSCORES，可以让分数一起和值返回到结果集
  * `zrangebyscore <key> <min> <max> [WITHSCORES] [LIMIT OFFSET COUNT]` 返回有序集合key中，所有score值介于min和max之间（包括min或max）的成员。有序集合成员按照score值递增（从小到大）次序排列
  * `zrevrangebyscore <key> <max> <min> [WITHSCORES] [LIMIT OFFSET COUNT]` 返回有序集合key中，所有score值介于min和max之间（包括min或max）的成员。有序集合成员按照score值递减（从大到小）次序排列
  * `zincrby <key> <increment> <value>` 为元素的score加上增量
  * `zrem <key> <value>` 删除该集合下，指定值的元素
  * `zcount <key> <min> <max>` 统计该集合，分数区间内的元素个数
  * `zrank <key> <value>` 返回该值在集合中的排名，从0开始
* 数据结构
  * SortedSet（zset）是Redis提供的一个非常特别的数据结构，一方面它等价于Java的数据结构`Map<String, Double>` ，可以给每个元素value赋予一个权重score，另一方面它有类似于TreeSet，内部的元素会按照权重score进行排序，可以得到每个元素的名词，还可以通过score的范围来获取元素的列表
  * zset底层使用了两个数据结构
    * hash，hash的作用就是关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值
    * 跳跃表，跳跃表的目的在于给元素value排序，根据score的范围获取元素列表

## 4. Redis6新数据类型

### （1） Bitmaps

* 简介
  * Bitmaps本身不是一种数据类型，实际上它就是字符串（key-value），但是它可以对字符串的位进行操作
  * Bitmaps单独提供了一套命令，所以在Redis中使用Bitmaps和使用字符串的方法不太相同。可以把Bitmaps想象成一个以位为单位的数组，数组的每个单元只能存储0和1，数组的下标在Bitmaps种叫做偏移量
* 命令
  * `setbit <key> <offset> <value>` 设置Bitmaps种某个偏移量的值（0或1）
    * offset：偏移量从0开始 
  * `getbit <key> <offset>` 获取Bitmaps中某个偏移量的值
    * 获取键的第offset位的值（从0开始算）
  * `bitcount <key> [start end]` 统计字符串从start字节到end字节比特值位1的数量。
    * start和end参数的设置都可以使用负数值：-1表示最后一个位，-2表示倒数第二个位
    * start和end是指bit组的字节的下标数，二者皆包含
  * `bitop and(or/not/xor) <destkey> [key...]` bitop是一个复合操作，它可以做多个Bitmaps的and（交集）、or（并集）、not（非）、xor（异或）操作并将结果保存在destkey中

### （2） HyperLogLog

* 简介
  * Redis HyperLogLog是用来做基数统计的算法，HyperLogLog的优点是在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的
  * 在Redis里面，每个HyperLogLog键只需要花费12KB内存，就可以计算接近2^62个不同元素的基数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比
  * 但是因为HyperLogLog只会根据输入元素来计算基数，而不会储存输入元素本身，所以HyperLogLog不能像集合那样，返回输入的各个元素
* 命令
  * `pfadd <key> <element> [element...]` 添加指定元素到HyperLogLog中
  * `pfcount <key> [key...]` 计算HLL的**近似基数**，可以计算多个HLL，比如用HLL存储每天的UV，计算一周的UV使用7天的UV合并计算即可
  * `pfmerge <destkey> <sourcekey> [sourcekey...]` 将一个或多个HLL合并后的结果存储在另一个HLL中，比如每月活跃用户可以使用每天的活跃用户来合并计算可得 

### （3） Geospatial

* 简介
  * Redis3.2中增加了对GEO类型的支持。GEO，Geographic。地理信息的缩写。
  * 该类型，就是元素的2维坐标，在地图上就是经纬度
  * redis基于该类型提供了经纬度设置，查询，范围查询，距离查询，经纬度Hash等常见操作
* 命令
  * `geoadd <key> <longitude> <latitude> <member> [longitude latitude member...]` 添加地理位置（经度、纬度、名称）
    * 两极无法添加，一般会下载城市数据，直接通过Java程序一次性导入
    * 有效的经度从-180°到180°。
    * 有效的纬度从-85.05112878°到85.05112878°
    * 当坐标位置超出指定范围时，该命令将会返回一个错误
    * 已经添加的数据是无法再次往里面添加的
  * `geopos <key> <member> [member...]` 获得指定地区的坐标值
  * `geodist <key> <member1> <member2> [m|km|ft|mi]` 获取两位置之间的直线距离
    * m表示米（默认值）
    * km表示千米
    * mi表示英里
    * ft表示英尺
  * `georadius <key> <longitude> <latitude> radius m|km|ft|mi` 以给定的经纬度为中心，找出某一radius半径内的元素

## 5. Redis事务、锁机制

### （1） Redis的事务定义

* Redis事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其它客户端发来的命令请求所打断
* Redis事务的主要作用就是**串联多个命令**防止别的命令插队

### （2） 三个基本命令

* multi
* exec
* discard

从输入multi命令开始，输入的命令都会一次进入命令队列中，但不会执行，直到输入exec后，Redis会将之前的命令队列中依次执行。组队的过程中可以通过discard来放弃组队。

### （3） 事务的错误处理

* 在组队中某个命令出现了报告错误，执行的时候整个的所有队列都会被取消
* 执行的时候某个命令出现了错误，那么只有错误的命令被取消，其余命令正常执行

### （4） WATCH key [key...]

​	在执行multi之前，先执行watch key1 [key2] 可以监视一个或多个key，如果在事务执行之前这个或这些key被其它命令所改动，那么事务将被打断。

### （5） UNWATCH

取消watch命令对所有key的监视

如果在执行watch命令之后，exec命令或discard命令先被执行了的话，那么就不需要再执行unwatch了

### （6） Redis事务三特性

* 单独的隔离操作
  * 事务中的所有命令都会被序列化、按顺序地执行。事务在执行的过程中，不会被其它客户端发送来的命令请求打断
* 没有隔离级别的概念
  * 队列中的命令没有提交之前都不会实际被执行，因为事务提交前任何指令都不会被实际执行
* 不保证原子性
  * 事务中如果有一条命令执行失败，其后的命令仍然会被执行，没有回滚

## 6. 并发模拟工具ab

* `yum install httpd-tools`

## 7. Redis持久化之RDB

### （1） Redis持久化定义

* 在指定的**时间间隔**内将内存中的数据集**快照**写入磁盘，也就是行话将的Snapshot快照，它恢复时是将快照文件直接读到内存里

### （2） 备份是如何执行的

* Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，在用这个**临时文件替换上一次持久化好**的文件。整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能，如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加高效。
* RDB的缺点是**最后一次持久化后的数据可能丢失**。

### （3） Fork

* Fork的作用是复制一个与当前进程**一样**的进程。新进程的所有数据（变量、环境变量、程序计数器等）数值都和原进程一致，但是是一个全新的进程，并作为**原进程的子进程**。
* 在Linux程序中，fork()会产生一个和父进程完全相同的子进程，但子进程在此后多会exec系统调用，处于效率考虑，Linux中引入了**“写时复制技术”**。
* 一般情况父进程和子进程会**共用**同一段物理内存，只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。

### （4） RDB的优势

* 适合大规模的数据恢复
* 对数据完整性和一致性要求不高更适合使用
* 节省磁盘空间
* 恢复速度快

### （5） RDB的劣势

* Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑
* 虽然Redis在fork时使用了写时拷贝技术，但是如果数据庞大时还是比较消耗性能
* 在备份周期在一定间隔时间做一次备份，所以如果Redis意外down的话，就会丢失最后一次快照后的所有修改

## 8. Redis持久化之AOF（Append Only File）

### （1） AOF定义

以**日志**的形式来记录每个**写操作**（增量保存），将Redis执行过的所有写指令记录下来（**读操作不记录**），只许**追加**文件但不可以**改写**文件，reids启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

### （2） AOF持久化流程

* 客户端的请求写命令会被append追加到AOF缓冲区
* AOF缓冲区根据AOF持久化策略[always,everysec,no]将操作sync同步到磁盘的AOF文件中
* AOF文件大小超过重写策略或手动重写时，会对AOF文件爱你rewrite重写，压缩AOF文件容量
* Redis服务重启时，会重新load加载AOF文件中的写操作达到数据恢复的目的

### （3） AOF默认不开启

* 可以在redis.conf中配置文件名称，默认为appendonly.aof
* AOF文件的保存路径同RDB的路径一致

### （4） AOF和RDB同时开启，redis听谁的

* 同时开启，系统默认取AOF的数据（数据不存在）

### （5） AOF启动/修复/恢复

* AOF的备份机制和性能虽然和RDB不同，但是备份和恢复的操作同RDB一样，都是拷贝备份文件，需要恢复时在拷贝到Redis工作目录下，启动系统即加载
* 正常恢复
  * 修改默认的appendonly no，改为yes
  * 将有数据的aof文件复制一份保存到对应目录（查看目录：config get dir）
  * 恢复：重启redis然后重新加载
* 异常恢复
  * 修改默认的appendonly no，改为yes
  * 如遇到AOF文件损坏，通过`/usr/local/bin/redis-check-aof --fix appendonly.aof`进行恢复
  * 备份被写坏的AOF文件
  * 恢复：重启redis然后重新加载

#### （6） AOF同步频率设置

* `appendfsync always` 始终同步，每次Redis的写入都会立刻记入日志
  * 性能较差但数据完整性比较好
* `appendfsync everysec` 每秒同步，每秒记入日志一次
  * 如果宕机，本秒的数据可能丢失
* `appendfsync no` 不主动进行同步，把同步时机交给操作系统

### （7） AOF优势

* 备份机制更稳健，丢失数据概率更低
* 可读的日志文本，通过操作AOF稳健，可以处理误操作

### （8） AOF劣势

* 比起RDB占用更多的磁盘空间
* 恢复备份速度要慢
* 每次读写都同步的话，有一定的性能压力
* 存在个别Bug，造成不能恢复

## 9. Redis主从复制原理

* 当从连接上主服务器之后，从服务器向主服务器发送进行数据同步消息
* 主服务器接到从服务器发送过来的同步消息，把主服务器数据进行持久化，rdb文件，把rdb文件发送从服务器，从服务器拿到rdb进行读取
* 每次主服务器进行写操作之后，和从服务器进行数据同步

## 10. 缓存穿透

### （1） 描述

* key对应的数据在数据源并不存在，每次针对此key的请求从缓存获取不到，请求都会压倒数据源，从而干扰可能压垮数据源。
* 比如用一个不存在的用户id获取用户信息，不论缓存还是数据库都没有，若黑客利用此漏洞进行攻击可能压垮数据库

### （2） 解决方案

* 对空值缓存
  * 如果一个查询返回的数据为空（不管数据是否存在），我们仍然把这个空结果（null）进行缓存，设置空结果的过期时间会很短，最长不超过五分钟
* 设置可访问的名单（白名单）
  * 使用bitmaps类型定义一个可以访问的名单，名单id作为bitmaps的偏移量，每次访问和bitmaps里面的id进行比较，如果访问id不在bitmaps里面，进行拦截，不允许访问
* 采用布隆过滤器
  * 布隆过滤器（Bloom Filter）是1970年由布隆提出的，它实际上是一个很长的二进制向量（位图）和一系列随机映射函数（哈希函数）
  * 布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是空间效率和查询时间都远远超过一般的算法，缺点是有一定的误识别率和删除困难
  * 将所有可能存在数据哈希到一个足够大的bitmaps中，一个一定不存在的数据会被这个bitmaps拦截掉，从而避免了对底层存储系统的查询压力
* 进行实时监控
  * 当发现Redis的命中率开始急速降低，需要排查访问对象和访问数据，和运维人员配合，可以设置黑名单限制服务

## 11. 缓存击穿

### （1） 描述

* key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮

### （2） 解决方案

* 预先设置热门数据
  * 在redis高峰访问之前，把一些热门数据提前存入到redis里面，加大这些热门数据key的时长
* 实时调整
  * 现场监控哪些数据热门，实时调整key 的过期时长
* 使用锁（缺点是效率低）
  * 就是在缓存失效的时候（判断拿出来的值为空），不是立即去load db
  * 先使用缓存工具的某些带成功操作返回的操作（比如Redis的SETNX）去set一个mutex key
  * 当操作返回成功时，再进行load db的操作，并回设缓存，最后删除mutex key
  * 当操作返回失败，证明有线程在load db，当前线程睡眠一段时间再重试整个get缓存的方法

## 12. 缓存雪崩

### （1） 描述

* key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮
* 缓存雪崩与缓存击穿的区别在于这里针对很多key缓存，前者则是某一个key

### （2） 解决方案

* 构建多级缓存架构
  * nginx缓存+redis缓存+其他缓存（ehcache等）
* 使用锁或队列
  * 用加锁或者队列的方式保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。不适用高并发情况
* 设置过期标志更新缓存
  * 记录缓存数据是否过期（设置提前量），如果过期会触发通知另外的线程在后台去更新实际key的缓存
* 将缓存失效时间分散开
  * 比如我们可以在原有的失效时间基础上增加一个随机值，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件





