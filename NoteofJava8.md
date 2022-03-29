# Java8 实战
### 行为参数化
* 行为参数化就是一个方法接受多个不同的行为作为参数，并在内部使用它们，完成不同行为的能力。
### Lambda表达式
可以把Lambda表达式理解为一种简洁的可传递匿名函数：它没有名称，但它有参数列表、函数主体、返回类型，可能还有一个可以抛出的异常列表。

只有在接受函数式接口的地方才可以使用Lambda表达式。
#### 匿名
* 说它是匿名的，因为它不像普通的方法那样有一个明确的名称：写的少想得多。
#### 函数
* 说它是一种函数，是因为Lambda函数不像方法那样属于某个特定的类。，但和方法一样，Lambda有参数列表、函数主体、返回类型，可能还有一个可以抛出的异常列表。
#### 传递
* Lambda表达式可以作为参数传递给方法或存储在变量中。
#### 简介
* 你无须像匿名类那样写很多模板代码。
#### 例子，利用Lambda表达式可以更为简介地自定义一个Comparator对象：<br>
先前：<br>
```
Comparator<Apple> byWeight = new Comparator<Apple>() {
    public int compare(Apple a1, Apple a2) {
        return a1.getWeight().compareTo(a2.getWeight());    
    }
}
```
之后(用了Lambda表达式)：<br>
```
Comparator<Apple> byWeight = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```
在Lambda表达式中，有三个部分，对于上面的例子来说：
* 参数 (Apple a1, Apple a2) 这里采用了Comparator中的compare方法的参数，两个Apple。
* 箭头 -> 箭头->把参数列表与Lambda主体分隔开。
* 主体 a1.getWeight().compareTo(a2.getWeight()) 比较两个Apple的重量。表达式就是Lambda的返回值。

四个有效的Lambda表达式的例子：
* `(String s) -> s.length()` 具有一个String类型的参数并分会一个int。Lambda没有return语句，因为隐含了return。
* `(Apple a) -> a.getWeight() > 150` 有一个Apple类型的参数并返回一个boolean（苹果的重量是否超过150克）。
* 第三个表达式具有两个int类型的参数而没有返回值（void返回）。
```
(int x, int y) -> {
System.out.println("Result:");
System.out.println(x + y);
}
```
* `() -> 42` 第四个表达式没有参数，返回一个int。

### 函数式接口
只定义**一个**抽象的方法的接口。
* Lambda表达式允许你直接以内联的形式为函数式接口的抽象方法提供实现，并把整个表达式作为函数式接口的实例。例如：<br>
Runnable是只定义了一个抽象方法run（）的函数式接口。然后利用直接传递的Lambda打印“Hello World 3”。 <br>
```
Runnable r1 = () -> System.out.println("Hello World");
public static void process(Runnable r) {
    r.run();
}
process(() -> System.out.println("Hello World 3"));
```
* 函数式接口的抽象方法的签名基本上就是Lambda表达式的签名。这种抽象方法叫做函数描述符。
#### 常用函数式接口
|    函数式接口    | Predicate< T > |
|:--------------:|:------------:|
| Predicate< T > | T -> boolean |
| Consumer< T > | T -> void |
| Function< T, R > | T -> R |
| Supplier< T > | () -> T |
| UnaryOperator< T > | T -> T |
| BinaryOperator< T > | (T, T) -> T |
| BiPredicate< T, U > | (T, U) -> boolean |
| BiConsumer< T, U > | (T, U) -> void |
| BiFunction< T, U, R > | (T, U) -> R |
### 基本类型特化
* Java类型要么是引用类型，如Byte, Integer, Object, List；要么是基本类型，如:int, double, byte, char;<br>
但是泛型只能绑定到引用类型。这是由泛型内部的实现方式造成的。
  
* Java有一个**自动装箱**机制来帮助程序员执行装箱和拆箱操作。但这在性能方面是要付出代价的。装箱后的值本质上就是把基本类型包裹起来，并保存在堆里。因此，装箱后的值需要更多的内存，并需要额外的内存搜索来获取被包裹的基本值。
### 类型检查
* 特殊的void兼容规则

　　如果一个Lambda的主体是一个语句表达式，它就和一个返回void的函数描述符兼容（当然需要参数列表也兼容）。

　　例如，以下两行都是合法的，尽管List的add方法返回了一个boolean，而不是Consumer上下文(T -> void)所要求的void:<br>
```
// Predicate返回了一个boolean
Predicate<String> p = (String s) -> list.add(s);
// Consumer返回了一个void
Consumer<String> b = (String s) -> list.add(s);
```
* 使用局部变量<br>
　　Lambda表达式也允许使用自由变量（不是参数，而是在外层作用域中定义的变量），就像匿名类一样。它们被称作**捕获Lambda**。
  
　　例如，下面的Lambda捕获了portNumber变量:
```
int portNumber = 1337;
Runnable r = () -> System.out.println(portNumber);
```
　　注意：Lambda可以没有限制地捕获（也就是在其主体中引用）实例变量和静态变量。但局部变量必须声明为final，或事实上是final。

　　例如，下面的代码无法编译，因为portNumber变量被赋值两次:
```
int portNumer = 1337;
Runnable r = () -> System.out.println(portNumber);
portNumber = 31337;
```
### 方法引用
* 如何构建方法引用<br>
方法引用主要有三类。
  
  (1) 指向**静态方法**的方法引用（例如Integer的parseInt方法，写作`Integer::parseInt`）。
  
  (2) 指向任意类型实力方法的方法引用（例如String的length方法，写作`String::length`）。
  
  (3) 指向现存对象或表达式实例方法的方法引用（假设你有一个局部变量`expensive Transaction`保存了Transaction类型的对象，它提供了实例方法getValue，那你就可以这么写`expensive Transaction::getValue`）。
* 构造函数引用
  
  对于一个现有构造函数，可以利用它的名称和关键字new来创建它的一个引用：ClassName::new。
  
　　例如，使用Map来将构造函数映射到字符串值。

```
static Map<String, Function<Integer, Fruit>> map = new HashMap<>();
static {
    map.put("apple", Apple::new);
    map.put("orange", Orange::new);
    //etc...
}
public static Fruit giveMeFruit(String fruit, Integer weight) {
    return map.get(fruit.toLowerCase()).apply(weight);
}
```
### 复合Lambda表达式的有用方法
* 谓词复合。
  <br>　　谓词接口包括三个方法:negate、and和or，让你可以重用已有的Predicate来创建更复杂的谓词。
  <br>　　注意:and和or方法是按照在表达式链中的位置，从左向右确定优先级的。因此`a.or(b).and(c)`可以看作`(a || b) && c`。同样，`a.and(b).or(c)`可以看作`(a && b) || c`。



### 流
* 定义：从支持数据处理操作的源生成的元素序列。<br>
  (1) 元素序列：就像集合一样，流也提供了一个接口，可以访问特定元素类型的一组有序值。
  
  (2) 源：流会使用一个提供数据的源，比如集合、数组或I/O资源。请注意：从有序集合生成流时会保留原有的顺序。由列表生成的流，其元素顺序与列表一致。
  
  (3) 数据处理操作：流的数据处理功能支持类似于数据库的操作，以及函数式编程语言中的常用操作，例如：filter、map、reduce、find、match、sort等。流操作可以顺序执行，也可以并行执行。
  
  (4) 流水线：很多流操作本身会返回一个流，这样多个操作就可以链接起来，构成一个更大的流水线。
  
  (5) 内部迭代：与集合使用迭代器进行显式迭代不同，流的迭代操作是在后台进行的。
  

* 使用流一般包括三件事：<br>
  (1) 一个**数据源**(如集合)来执行一个查询；
  
  (2) 一个**中间操作**链，形成一条流的流水线；
  
  (3) 一个**终端操作**， 执行流水线，并能生成结果。


* 中间操作

| 操作 | 类型 | 返回类型 | 操作参数 | 函数描述符 |
|:---:|:---:|:-------:|:------:|:--------:|
| filter | 中间 | Stream< T > | Predicate< T > | T -> boolean |
| map | 中间 | Stream< R > | Function< T, R > | T -> R |
| limit | 中间 | Stream< T > |
| sorted | 中间 | Stream< T > | Comparator< T > | (T, T) -> int |
| distinct | 中间 | Stream< T > |

* 终端操作

| 操作 | 类型 | 返回类型 | 目的 |
|:---:|:---:|:-------:|:---:|
| forEach | 终端 | void | 消费流中的每个元素并对其应用Lambda |
| count | 终端 | long | 返回流中元素的个数 |
| collect | 终端 | (generic) | 把流归约成一个集合，比如List、Map，甚至是Integer。|

* filter和map等中间操作会返回一个流，并可以链接在一起。可以用它们来设置一条流水线，但并不会生成任何结果。
* forEach和count等终端操作会返回一个非流的值，并处理流水线以返回结果。

### 使用流
#### 筛选
* 用谓词筛选。<br>
　　Stream接口支持filter方法。该操作会接受一个谓词（一个返回boolean的函数）作为参数，并返回一个包括所有符合谓词的元素的流。例如：
```
List<Dish> vegetarianMenu = menu.stream()
                                .filter(Dish::isVegetarian)
                                .collect(toList());
```

* 筛选各异的元素。<br>
　　流还支持一个叫做dictinct的方法，它会返回一个元素各异（根据流所生成的hashCode和equals方法实现）的流。例如，以下代码会筛选出列表中所有的偶数，并确保没有重复（使用equals方法进行比较）
```
List<Integer> numbers = Arrays.adList(1, 2, 1, 3, 3, 2, 4);
number.stream()
      .filter(i -> i % 2 == 0)
      .distinct()
      .forEach(System.out.println);
```

#### 流的切片
* 使用谓词对流进行切片<br>
  (1) **使用takeWhile**。<br>
  　　它可以帮助你利用谓词对流进行分片（即使你要处理的流失无限流也毫无困难）。更妙的是，它会在遭遇第一个不符合要求的元素时停止处理。<br>
  演示：<br>
  ```
  List<Dish> slicedMenu1
    = specialMenu.stream()
                 .takeWhile(dish -> dish.getCalories() < 320)
                 .collect(toList());
  ```
  　　与之相比，filter需要遍历整个流中的数据，对其中每一个元素执行谓词操作，性能很差。

  (2) **使用dropWhile**。<br>
  　　dropWhile操作是对takeWhile操作的补充。它会从头开始，丢弃所有谓词结果为false的元素。一旦遭遇谓词计算的结果为true，它就停止处理，并返回所有剩余的元素，即便要处理的对象是一个有无限数量元素构成的流，它也能工作的很好。<br>
  演示：<br>
  ```
  List<Dish> slicedMenu1
    = specialMenu.stream()
                 .dropWhile(dish -> dish.getCalories() < 320)
                 .collect(toList());
  ```
  
* 截短流<br>
　　流支持limit(n)方法，该方法会返回另一个不超过给定长度的流。
  
* 跳过元素<br>
　　流还支持skip(n)方法，返回一个扔掉了前n个元素的流。如果流中元素不足n个，则返回一个空流。请注意：limit(n)和skip(n)是互补的！
  
#### 映射
* 对流中每一个元素应用函数<br>
  　　流支持map方法，它会接受一个函数作为参数。这个函数会被应用到每个元素上，并将其映射成一个新的元素（使用**映射**一词，是因为它和**转换**类似，但其中的细微差别在于它是“创建一个新版本”而不是“修改”）。<br>
演示：下面的代码把方法引用`Dish::getName`传给了map方法，来**提取**流中菜肴的名称。<br>
```
List<String> dishNames = menu.stream()
                             . map(Dish::getName)
                             .collect(toList());
```  
* 流的扁平化<br>
  　　使用flatMap方法，将各个数组映射成流的内容而不是分别映射成一个流。所有使用`flatMap(Arrays::stream)`时生成的单个流都被合并起来，即扁平化为一个流。<br>
  　　简而言之，flatMap方法让你把一个流中的每个值都换成另一个流，然后把所有的流链接起来成为一个流。
  
#### 查找和匹配
　　另一个常见的数据处理套路是看看数据集中的哪些元素是否匹配一个给定的属性。Stream API通过allMatch、anyMatch、noneMatch、findFirst和findAny方法提供了这样的工具。
* 检查谓词是否至少匹配一个元素。<br>
　　anyMatch方法可以回答“流中是否有一个元素能匹配给定的谓词”。
```
if (menu.stream().anyMatch(Dish::isVegetarian)) {
  System.out.println("The menu is (somewhat) vegetarian friendly!!");
}
```  
　　　　anyMatch方法返回一个boolean，因此是一个终端操作。
* 检查谓词是否匹配所有元素<br>
  (1) allMatch方法的工作原理和anyMatch类似，但它会看看流中的元素是否都能匹配给定的谓词。<br>
  (2) noneMatch方法。它可以确保流中没有任何元素与给定的谓词匹配。<br>
  　　**注意**：anyMatch、allMatch和noneMatch这三个操作都用到了所谓的**短路**，这就是大家熟悉的Java中&&和||运算符短路在流中的版本。
  
* 查找元素。<br>
　　findAny方法将返回当前流中的任意元素值。它可以与其他流操作结合使用。比如你想找到一到素食菜肴：
```
Optional<Dish> dish = 
  menu.stream()
      .filter(Dish::isVegetarian)
      .findAny();
```
　　代码中的Optional是什么？<br>
　　Optional<T>类(java.util.Optional)是一个容器类，代表一个值存在或不存在。在上面的代码中，findAny可能什么元素都没找到。Java8的库设计人员引入了Oprional<T>,这样就不用返回众所周知容易出现问题的null了。<br>
　　Optional中的几种可以迫使你显式地检查值是否存在或处理值不存在的方法：<br>
　　(1) isPresent()将在Optional包含值的时候返回true，否则返回false。<br>
　　(2) ifPresent(Consumer<T> block)会在值存在的时候执行给定的代码块。Consumer是一个函数式接口，它让你传递一个接受T类型参数，并返回void的Lambda表达式。<br>
　　(3) T get()会在值存在时返回值，否则抛出一个NoSuchElement异常。<br>
　　(4) T orElse(T other)会在值存在时返回值，否则返回一个默认值。<br>

* 查找第一个元素。<br>
　　有些流有一个出现顺序来指定流中项目出现的逻辑顺序（比如有List或排序好的数据列生成的流）。对于这种流，你可能想要找到第一个元素。为此，有一个findFirst方法，它的工作类似于findAny。<br>
  　　演示：
  
```
List<Integer> someNumbers = Arrays.asList(1, 2, 3, 4, 5);
Optional<Integer> firstSquareDivisibleByThree = 
  someNumbers.stream()
             .map(n -> n * n)
             .filter(n -> n % 3 == 0)
             .findFirst();
```

#### 归约
* 定义：将流中所有元素反复结合起来得到一个值的这类查询叫做**归约操作**。用函数式编程语言的术语来说，这称为**折叠**(fold)。

#### 构建流
* 由值创建流<br>
使用静态方法 `Stream.of` 通过显式值创建一个流。它可以接受任意数量的参数。<br>
  例如:
  ```
  Stream<String> stream = Stream.of("Modern", "Java", "In", "Action");
  stream.map(String::toUpperCase).forEach(System.out::println);
  ```
  也可以使用empty得到一个空流: `Stream<String> emptyStream = Stream.empty();`

* 由可空对象创建流
* 由数组创建流 `Arrays.stream(XXXX);`
* 由文件生成流
* 由函数生成流：创建无限流

#### 汇总相关的工厂方法
* `summingInt`:求和。如:`menu,stream().collect(summingInt(Dish::getCalories));` (summingLong,summingDouble同理)
* `averagingInt`:求平均。如:`menu.stream().collect(averagingInt(Dish::getCalories));` (averagingLong,averagingDouble同理)
* `summarizingInt`:求元素个数，总和，平均值，最大值，最小值。如:`menu.stream().collect(summarizingInt(Dish::getCalories));` 使用getter方法来访问结果。(summarizingLong,summarizingDouble同理)

#### 广义的归约汇总
reduce工厂方法<br>
`int totalCalories = menu.stream().collect(reducing(0, Dish::getCalories, (i, j) -> i + j));`<br>
它需要三个参数：
* 第一个参数是归约操作的起始值，也就是流中没有元素时的返回值，所以很显然对于数值和而言0是一个合适的值。
* 第二个参数是使用的函数，即方法引用，它称为转换函数。
* 第三个参数是一个BinaryOperator，将两个项目累积成一个同类型的值。它称为累积函数。上面的例子就是对两个int求和。

从逻辑上说，归约操作的工作原理：利用累积函数，把一个初始化为起始值的累加器，和把转换函数应用到流中的每个元素上得到的结果不断迭代合并起来。
###### 单参数reducing工厂方法创建的收集器（是三参数方法的特殊情况），如：<br>
`Optional<Dish> mostCalorieDish = menu.stream().collect(reducing((d1, d2) -> d1.getCalories() > d2.getCalories() ? d1 : d2));` <br>
　　它把流中的第一个项目作为起点，把**恒等函数**（即一个函数仅仅是返回其输入参数）作为一个转换函数。这也意味着，要是把单参数reducing收集器传递给空流的collect方法，收集器就没有起点，因此将返回一个Optional<Dish>对象。

#### Collectors类的静态工厂方法
| 工厂方法 | 返回类型 | 用于 |
|:------:|:-------:|:---:|
| toList | List< T > | 把流中所有项目收集到一个List |
| 使用示例:`List<Dish> dishes = menuStream.collect(toList());` |
| toSet | Set< T > | 把流中所有项目收集到一个Set，删除重复项 |
| 使用示例:`Set<Dish> dishes = menuStream.collect(toSet());` |
| toCollection | Collection< T > | 把流中所有项目收集到给定的供应源创建的集合 |
| 使用示例:`Collection<Dish> dishes = menuStream.collect(toCollection(), ArrayList::new);` |
| counting | Long | 计算流中元素的个数 |
| 使用示例:`long howManyDishes = menuStream.collect(counting());` |
| summingInt | Integer | 对流中项目的一个整数属性求和 |
| 使用示例:`int totalCalories = menuStream.collect(summingInt(Dish::getCalories));` |
| averagingInt | Double | 计算流中项目Integer属性的平均值 |
| 使用示例:`double avgCalories = menuStream.collect(averagingInt(Dish::getCaloreis));` |
| summarizingInt | IntSummaryStatistics | 收集关于流中项目Integer属性的统计值，例如最大、最小、总和与平均值 |
| 使用示例:`IntSummaryStatistics menuStatistics = menuStream.collect(summarizingInt(Dish::getCalories));` |
| joining | String | 连接对流中每个项目调用toString方法所生成的字符串 |
| 使用示例:`String shortMenu = menuStream.map(Dish::getName).collect(joining(", "));` |
| maxBy | Optional< T > | 一个包裹了流中按照给定比较器选出的最大元素的Optional,或如果流为空则为Optional.empty() |
| 使用示例:`Optional<Dish> fattest = menuStream.collect(maxBy(comparingInt(Dish::getCalories)));` |
| minBy | Optional< T > | 一个包裹了流中按照给定比较器选出的最小元素的Optional,或如果流为空则为Optional.empty() |
| 使用示例:`Optional<Dish> lightest = menuStream.collect(minBy(comparingInt(Dish::getCalories)));` |
| reducing | 归约操作产生的类型 | 从一个作为累加器的初始值开始，利用BinaryOperator与流中的元素逐个结合，从而将流归约为单个值 |
| 使用示例:`int totalCalories = menuStream.collect(reducing(0, Dish::getCalories, Integer::sum));` |
| collectingAndThen | 转换函数返回的类型 | 包裹另一个收集器，对其结果应用转换函数 |
| 使用示例:`int howManyDishes = menuStream.collect(collectingAndThen(toList(), List::size));` |
| groupingBy | Map<K, List< T >> | 根据项目的一个属性的值对流中的项目作分组，并将属性值作为结果Map的键 |
| 使用示例:`Map<Dish.Type, List<Dish>> dishesByType = menuStream.collect(groupingBy(Dish::getType));` |
| partitioningBy | Map<Boolean, List< T >> | 根据对流中每个项目应用谓词的结果来对项目进行分区 |
| 使用示例:`Map<Boolean, List<Dish>> vegetarianDishes = menuStream.collect(paritioningBy(Dish::isVegetarian));` |

### Collector接口
#### 定义
```java
public interface Collector<T, A, R> {
  Supplier<A> supplier();
  BiConsumer<A, T> accumulator();
  Function<A, R> finisher();
  BinaryOperator<A> combiner();
  Set<Characteristics> characteristics();
}
```
* T是流中要收集的项目的泛型。
* A是累加器的类型，累加器是在收集过程中用于累积部分结果的对象。
* R是收集操作得到的对象（通常但并不一定是集合）的类型。

##### 理解Collector接口声明的方法
前四个方法都会返回一个呗collect方法调用的函数，第五个方法characteristics则提供了一系列特征，也就是一个提示列表，告诉collect方法在执行归约操作的时候可以应用哪些优化（如并行化）。

1. 建立新的结果容器：supplier方法<br>
supplier方法必须返回一个结果为空的Supplier，也就是一个无参数函数，在调用时它会创建一个空的累加器实例，供数据收集过程使用。
   
2. 将元素添加到结果容器：accumulator方法<br>
accumulator方法会返回执行归约操作的函数。
   
3. 对结果容器应用最终转换：finisher方法<br>
在遍历完流后，finisher方法必须返回在累积过程的最后要调用的一个函数，以便将累加器对象转换为整个集合操作的最终结果。
   
4. 合并两个结果容器：combiner方法<br>
它会返回一个供归约操作使用的函数，它定义了对流的各个子部分进行并行处理时，各个子部分归约所得的累加器要如何合并。
   
5. characteristics方法<br>
它会返回一个不可变的Characteristics集合，它定义了收集器的行为——尤其是关于流是否可以并行归约，以及可以使用哪些优化的提示。Characteristics是一个包含三个项目的枚举：<br>
   (1) UNORDERED--归约结果不受流中项目的遍历和累积顺序的影响。
   
   (2) CONCURRENT--accumulator函数可以从多个线程同时调用，且该收集器可以并行归约流。如果收集器没有标为UNORDERED，那它仅在用于无序数据源时才可以进行并行归约。
   
   (3) IDENTITY_FINISH--这表明完成器方法返回的函数是一个恒等函数，可以跳过。这种情况下，累加器对象将会直接用作归约过程的最终结果。这也意味着，将累加器A不加检查地转换为结果R是安全的。

### 并行数据处理
* parallel方法 <br>
对顺序流调用parallel方法可以将流转换成并行流，并行流内部使用了默认的ForkJoinPool，它默认的线程数量就是你的处理器数量。