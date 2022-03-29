# Java核心技术卷1
### 数组
* 在Java中，允许有长度为0的数组。<br>
`new elementType[0]`和`new elementType[] {}`表示长度为0的数组。<br>
  注意，长度为0 的数组与null并不相同。
* 创建一个数组时，所有元素都初始化为0。boolean数组的元素会初始化为false；对象数组的元素则初始化为一个特殊值null，表示这些元素还未存放对象。
#### Array操作
* `static String toString(xxx[] a)`<br>
  返回包含a中元素的一个字符串，这些元素用中括号包围，并用逗号分隔。<br>
* `static xxx[] copyOf(xxx[] a, int end)`  
* `static xxx[] copyOfRange(xxx[] a, int start, int end)`<br>
  返回与a类型相同的一个数组，期长度为length或end-start，数组元素为a的值。如果end大于a.length，结果会填充0或false。<br>
* `static void sort(xxx[] a)`<br>
  使用优化的快速排序算法对数组进行排序。
* `static int binarySearch(xxx[] a, xxx v)`<br>
* `static int binarySearch(xxx[] a, int start, int end, xxx v)`<br>
  使用二分查找算法在有序数组a中查找值v。如果找到v，返回相应的下标；否则，返回一个负数值r。
* `static void fill(xxx[] a, xxx v)`<br>
  将数组的所有数据元素设置为v。
* `static boolean equals(xxx[] a, xxx[] b)`<br>
  如果两个数组大小相同，并且下标相同的元素都对应相等，返回true。
  

### 字符串
* 子串 `substring(int start, int end)`
* char类型转String `String.valueOf(char a)`
* 字符串反转 `StringBuilder(str).reverse().toString`
* String转char[] `str.toCharArray()`
* `==`运算符只能够确定两个字符串是否存放在同一个位置上！当然，如果字符串在同一个位置上，它们必然相等，但完全由可能将内容相同的多个字符串副本放置在不同的位置上。所以千万不要使用`==`来测试字符串的相等性。
* 

### StringBuilder
有些时候，需要由较短的字符串构建字符串，例如，按键或来自文件中的单词。如果采用字符串拼接的方式来达到这个目的，效率会比较低。每次拼接字符串时，都会构建一个新的String对象，既耗时，又浪费空间。使用StringBuilder类就可以避免这个问题的发生。<br>
#### StringBuilder类中的重要方法
* `StringBuilder()` <br>
构造一个空的字符串构建器。
  
* `int length()` <br>
返回构建器或缓冲器中的代码单元数量。
  
* `StringBuilder append(String str)` <br>
追加一个字符串并返回this。
  
* `StringBuilder append(char c)` <br>
追加一个代码单元并返回this。
  
* `StringBuilder appendCodePoint(int cp)` <br>
追加一个码点，并将其转换为一个或两个代码单元并返回this。
  
* `void setCharAt(int i, char c)` <br>
将第i个代码单元设置为c。
  
* `StringBuilder insert(int offset, String str)` <br>
在offset位置插入一个字符串并返回this。
  
* `StringBuilder insert(int offset, char c)` <br>
在offset位置插入一个代码单元并返回this。
  
* `StringBuilder delete(int startIndex, int endIndex)` <br>
删除偏移量从startIndex到endIndex - 1的代码单元并返回this。
  
* `String toString()` <br>
返回一个与构建器或缓冲器内容相同的字符串。
  
### 用于printf的转换符
| 转换符 | 类型 | 示例 |
|:----:|:----:|:---:|
| d | 十进制整数 | 159 |
| x | 十六进制整数 | 9f |
| o | 八进制整数 | 237 |
| f | 定点浮点数 | 15.9 |
| e | 指数浮点数 | 1.59e+01 |
| g | 通用浮点数(e和f中较短的一个) | —— |
| a | 十六进制浮点数 | 0x1.fccdp3 |
| s | 字符串 | Hello |
| c | 字符 | H |
| b | 布尔 | true |
| h | 散列码 | 42628b2 |
| % | 百分号 | % |
| n | 与平台有关的行分隔符 | —— |

### 继承
#### 强制类型转换
* 只能在继承层次内进行强制类型转换。
* 在将父类强制转换成子类之前，应该使用`instanceof`进行检查。

#### 抽象类
* 即使不含抽象方法，也可以将类声明为抽象类。
* 抽象类不能实例化。
* 可以定义一个抽象类的对象变量，但是这样一个变量只能引用非抽象子类的对象。如：
`Person p = new Student("Vince Vu", "Economics");`

#### Java中的四个访问控制修饰符
* 仅对本类可见——private
* 对外部完全可见——public
* 对本包和所有子类可见——protected
* 对本包可见——默认，不需要修饰符

#### hashCode方法
* hashcode（散列码）是由对象导出的一个整型值。散列码是没有规律的。
* 字符串的散列码是由内容导出的，而Object类的默认hashCode方法会从对象的存储地址得出散列码。
* 如果存在数组类型的字段，那么可以使用静态的Arrays.hashCode方法计算一个散列码，这个散列码由数组元素的散列码组成。

#### 枚举类
* `static Enum valueOf(Class enumClass, String name)` <br>
返回给定类中由指定名字的枚举常量。
  
* `String toString()` <br>
返回枚举常量名。
  
* `int ordinal()` <br>
返回枚举常量在enum声明中的位置，位置从0开始计数。
  
* `int compareTo(E other)` <br>
如果枚举常量出现在other之前，返回一个负整数；如果this==other，则返回0；否则返回一个正整数。枚举常量的出现次序在enum声明中给出。
  
#### 反射
能够分析类能力的程序称为反射。反射机制可以用来:<br>
* 在运行时分析类的能力
* 在运行时检查对象，例如，编写一个适用于所有类的toString方法
* 实现泛型整数组操作代码
* 利用Method对象，这个对象很想C++中的函数指针

##### Comparator接口
```
public interface Comparator<T> {
  int compare(T first, T second);
}
```