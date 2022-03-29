Head First Java
=================
### primitive主数据类型
* `boolean` (java虚拟机决定其位数) true/false
* `char`    16bits              0~65535
* `byte`    8bits               -128~127
* `short`   16bits              -32768~32767
* `int`     32bits              -2147483648~2147483647
* `float`   32bits              范围规模可变
### 伪码
* 实例变量的声明
* 方法的声明
* 方法的逻辑(最重要)
### ArrayList类
#### 常用方法
* `
  add(Object elem) // 向list中加入对象参数
  `
* `
  remove(int index) // 在索引参数中移除对象
  `
* `
  remove(Object elem) // 移除该对象
  `
* `
  contains(Object elem) // 如果和对象参数匹配返回“true”
  `
* `
  isEmpty() // 如果list中没有元素返回“true”
  `
* `
  indexOf(Object elem) // 返回对象参数的索引或-1
  `
* `
  size() // 返回list中元素的一个数
  `
* `
  get(int index) // 返回当前索引参数的对象
  `
#### 操作
###### 创建
```
ArrayList<Egg> myList = new ArrayList<Egg>(); 
//Egg可以为任意数据类型,且不需要指定大小
```
###### 加入元素
```
Egg s = new Egg();
myList.add(s);
Egg b = new Egg();
myList.add(b);
```
###### 查询大小
`
int theSize = myList.size();
`
###### 查询特定元素
`
boolean isIn = myList.contains(s);
`
###### 查询特定元素的位置
`
int idx = myList.indexOf(b);
`
###### 判断集合是否为空
`
boolean empty = myList.isEmpty();
`
###### 删除元素
`
myList.remove(s);
`
### 多态
* 运用多态时，引用类型可以是实际对象类型的父类<br>
例子：<br>
  ```
  Animal[] animals = new Animal[5];
  animals [0] = new Dog();
  animals [1] = new Cat();
  animals [2] = new Wolf();
  animals [3] = new Hippo();
  animals [4] = new Lion();
  for (int i = 0; i < animals.length; i ++) {
  animals[i].eat(); //当i为0时，调用Dog的eat()
  animals[i].roam(); //当i为1时，调用Cat的eat()
  }
  ```
* 参数和返回类型也可以多态
### 三种防止某个类被作出子类的方法
* 存取控制<br>
  就算类不能标记为私有，但是它还是可以不标记为公有。非公有的类只能被同一个包的类做出子类。
* 使用`final`修饰符<br>
  这表示它是继承树的末端，不能被继承。
* 让类只拥有`private`的构造程序 
### Calendar运用
Calendar是一个抽象类，无法取得实例，但可以通过`Calendar.getInstance()`来取得它的具体子类的实例。
* 重要方法
```
   add(int field, int acmount); //加减时间值
   get(int field); //取出指定字段的值
   getInstance(); //返回Calendar，可指定地区
   getTimeInMillis(); //以毫秒返回时间
   roll(int field, boolean up); //加减时间值，不进位
   set(int field, int value); //设定指定字段的值
   set(year, month, day, hour, minute); //设定完整的时间
   setTimeInMiliis(long millis); //以毫秒指定时间
   ...
```
* 关键字段
```
   DATE / DAY_OF_MONTH //每月的几号
   HOUR / HOUR_OF_DAY //小时
   MILLISENCOND //毫秒
   MINUTE //分钟
   MONTH //月份
   YEAR //年份
   ZONE_OFFSET //时区位移
   ...
```
### 异常处理
* 编译器不会注意RuntimeException类型的异常。RuntimeException不需要声明或被包在try/catch的块中（然而你还是可以这么做）。
### 内部类
* 内部类可以使用外部类所有的方法与变量，就算是私用的也一样。
* 内部类把存取外部类的方法和变量当作是开自家冰箱。
### 序列化和文件的输入/输出
* FileOutputStream把字节写入文件;<br>
  ObjectOutputStream把对象转换成可以写入串流的数据;<br>
  当我们调用ObjectOutputStream的writeObject时，对象会被打成串流送到FileOutputStream来写入文件.
* 当对象被序列化时，被该对象引用的实例变量也会被序列化。且所有被引用的对象也会被序列化。
* 如果要让某类能够被序列化，该类就必须实现Serializable，即`implements Serializable`。
* 如果某实例变量不能或不应该被序列化，就把它标记为`transient`（瞬时）的，例如：`transient String currentID`。
### 多线程
* Runnable这个接口只有一个方法:`public void run()`。
* Thread对象不可以重复使用，一旦线程的`run()`方法完成之后，该线程就不能再重新启动了。
* 使用`synchronized`这个关键词来修饰方法使它每次只能被单一的线程存取。可用其来修饰一行或数行的指令而不必整个方法都同步化。
* 使用同步化的程序要小心死锁。死锁会发生是因为两个线程互相持有对方正在等待的东西。
* `Thread.sleep()`让所有的线程都有机会运行。
* 如果两个或以上的线程存取堆上相同的对象可能会出现严重的问题，可能会引发数据的损毁。
* 每个对象都有单一的锁，单一的钥匙。这只会在对象带有同步化方法时才有实际的用途。
* 对象有锁，每个被载入的类也有个锁。当要对静态的方法做同步化时，Java会使用类本身的锁。
### 集合与泛型
* `public <T extends Animal> void takeThing(ArrayList<T> list)`<br>
与<br>
  `public void takeThing(ArrayList<Animal> list)`<br>
是不一样的。第一个表示任何被声明为Animal或Animal的子型的ArrayList是合法的；但第二个只能使用Animal的ArrayList。
  这是不是违背了多态？不不是，详见P571.
* 以泛型的观点来说，`extends`代表`extends`或`implement`。例如：<br>
  `public static <T extends Compare<? super T>> void sort(List<T> list)`<br>
  其中，`Compare`是一个接口，但是用的是`extends`。
* 数组的类型是在运行期间检查的，但集合的类型检查只会发生在编译期间。
* 在使用带有<?>的声明时，编译器不会让你加入任何东西到集合中。
* 在方法参数中使用万用字符时，编译器会阻止任何可能破坏引用参数锁指集合的行为。
### 位操作
定义`int x = -11 // 位组合是11110101`
* 右移运算符:`>>` <br>
以指定量右移字节合，左方用符号位填充，正负号不变。<br>
  `int y = x >> 2 // 位组合是11111101`
* 无符号右移运算符:`>>>` <br>
与`>>`一样，但第一个位也会补0，正负号可能会改变。<br>
  `int y = x >>> 2; // 位组合是00111101`
* 左移运算符:`<<` <br>
与`>>>`一样，但方向相反；右方补0，正负号可能改变。<br>
  `int y = x << 2; // 位组合是11010100`
### StringBuffer和StringBuilder的方法
Java API中最常用到的类包括了String和StringBuffer（因为String是不变的，使用StringBuffer/StringBuilder来操作String比较有效率）。
* `StringBxxxx delete(int start, int end); // 删除部分`
* `StringBxxxx insert(int offset, any primitive or a char[]); // 插入`
* `StringBxxxx replace(int start, int end, String s); // 取代`
* `StringBxxxx reverse(); // 翻转`
* `void setCharAt(int index, char ch); // 替换字符`

### `hashCode()`和`equals()`的相关规定
API文件有对对象的状态指定出必须遵守的规则：
1. 如果两个对象相等，则hashcode必须也是相等的。
2. 如果两个对象相等，对其中一个对象调用`equals()`必须返回true。也就是说，若`a.equals(b)`则`b.equals(a)`。
3. 如果两个对象有相同的hashcode值，他们也不一定是相等的。但若两个对象相等，则hashcode值一定是相等的。
4. 因此若`equals()`被覆盖过，则`hashCode()`也必须被覆盖。
5. `hashCode()`的默认行为是对heap上的对象产生独特的值。如果你没有override过`hashCode()`，则该class的两个对象怎样的都不会被认为是相同的。
6. `equals()`的默认行为是执行`==`的比较。也就是说会去测试两个引用是否对上heap上同一个对象。如果`equals()`没有被覆盖过，两个对象永远都不会被视为相同的，因为不同的对象有不同的字节组合。<br>
`a.equals(b)`必须与`a.hashCode() == b.hashCode()`等值。<br>
 但`a.hashCode() == b.hashCode()`不一定要与`a.equals()`等值。
### 为什么不同对象会有相同的hashcode的可能？
因为`hashCode()`所使用的杂凑算法也许刚好会让多个对象传回相同的杂凑值。越糟糕的杂凑算法越容易碰撞，但这也与数据值域分布的特性有关。

### TIPS
1. 任何有值被引用到的地方，都可以调用方法的方式来取得该类型的值。
2. `null`代表没有从奥做对象的远程控制，它是个引用而不是对象。
3. 使用`==`来比较两个primitive主数据类型，或者判断两个引用是否引用同一个对象；使用`equals()`来判断两个对象是否在意义上相等。
4. `long`类型的`01011101`转化为`short`类型时，左边的字节会被砍掉，变成`1101`
5. 在使用短运算符(例如:`&&`或`||`)时，Java虚拟机若发现作坊表达式不满足条件时不会计算右方的表达式；而长运算符(`&`或`|`)使用在`boolean`表达式时会强制Java虚拟机一定要计算运算符两边的算式。
6. `public`类型的成员会被继承，`private`类型的成员不会被继承。
7. 非抽象类中不能拥有抽象方法。一个具体类必须要实现所有抽象的方法，即被重写。
8. 非primitive的变量只是保存对象的引用而已，而不是对象本身。
9. 构造函数不会被继承。
10. 如果实例对象是个对对象的引用，则引用与对象都是在堆上。
11. 实例变量有默认值，原始的默认值是`0/0.0/false`，引用的默认值是`null`。
12. 对`super()`的调用必须是构造函数的第一个语句。
13. 使用`this()`来从某个构造函数调用同一个类的另一个构造函数；<br>
    `this()`只能用在构造函数中，且必须是第一行语句；<br>
    `super()`和`this()`不能兼得。
14. `final`的变量代表你不能改变它的值；<br>
`final`的method代表你不能覆盖掉该method；
`final`的类代表你不能继承该类(也就是创建它的子类)。
15. 格式化说明：<br>
`%[argument number][flags][width][.precision]type`<br>
    **argument**:如果格式化的参数超过一个以上，可以在这里指定是哪一个;<br>
    **flags**:特定类型的特定选项，例如数字要加逗号或正负号;<br>
    **width**:最小的字符数，注意:这不是总数；输出可以超过此宽度，若不足则会主动补零;<br>
    **.precision**:精确度，注意前面有个圆点符号;<br>
    **type**:一定要指定的类型标识符;
* 完整的日期与时间:`%tc`
* 只有时间:`%tr`
* 周、月、日:`%tA %tB %td`
  <br>
  <br>
    例子:<br>
    `format("%,6.1f",42.000) // 除了没有argument number,其它项目都有。`
16. TCP的端口号是个16位的值。


   
   
   
   
   


