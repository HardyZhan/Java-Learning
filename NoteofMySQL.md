# MySQL必知必会
### 数据库的组成
* 表:某种特定类型数据的结构化清单
* 列:表中的一个字段。所有的表都是由一个或多个列组成的
* 数据类型:所容许的数据的类型。每个表列都有相应的数据类型，它限制(或容许)该列中存储的数据
* 行:表中的一个记录
* 主键:一列(或一组列),其值能够唯一区分表中每个行。<br>
表中的任何列都可以作为主键，只要它满足一下条件:<br>
  (1) 任意两行都不具有相同的主键值。<br>
  (2) 每个行都必须具有一个主键值(主键列不允许NULL)。
  
### 选择数据库
* USE——使用USE关键字来选择数据库，在数据库选择成功后会显示Database changed消息。
  
### 了解数据库和表
SHOW命令:数据库、表、列、用户、权限等的信息被存储在数据库和表中，不过内部的表一般不直接访问，可以用SHOW命令来显示这些信息。<br>
* SHOW DATABASES;——返回可用数据库的一个列表。
* SHOW TABLES;——返回当前选择的数据库内可用表的列表。
* SHOW COLUMNS FROM sutdent;——SHOW COLUMNS要求给出一个表名(本例中的student)，它对每个字段返回一行，行中包含字段名、数据类型、是否允许NULL、键信息、默认值以及其他信息。
* SHOW STATUS;——用于显示广泛的服务器状态信息。
* SHOW CREATE DATABASE 和 SHOW CREATE TABLE，分别用来显示创建特定数据库或表的MySQL语句。
* SHOW GRANTS;——用来显示授予用户(所有用户或特定用户)的安全消息。
* SHOW ERRORS 和 SHOW WARNINGS，用来显示服务器错误或警告消息。
* 更多命令执行HELP SHOW查看

DESCRIBE语句:MySQL支持用DESCRIBE作为SHOW COLUMNS FROM的一种快捷方式。

### SELECT语句
* 检索单个列:SELECT 列名 FROM 表名;
* 检索多个列:SELECT 列名,列名,列名 FROM 表名;
* 检索所有列:SELECT * FROM 表名;
* 检索不同的行:使用DISTINCT关键字，指示MySQL只返回不同的值——SELECT DISTINCT vend_id FROM products;<br>
注意：不能部分使用DISTINCT.DISTINCT关键字应用于所有列而不仅是前置它的列。如果给出SELECT DISTINCT vend_id,prod_price，除非指定的两个列都不同，否则所有行都将被检索出来。
  
* 限制结果:使用LIMIT——`SELECT prod_name FROM products LIMIT 5`;`LIMIT 5`指示MySQL返回不多于5行。<br>
而为了得出下一个5行，可指定要检索的**开始行**和**行数**——`SELECT prod_name FROM products LIMIT 5,5`，`LIMIT 5,5`指示MySQL返回从行5开始的5行。<br>
  注意:
  (1) 检索出来的第一行为行0而不是行1.因此，LIMIT 1，将检索出第二行而不是第一行。
  (2) 在行数不够时——LIMIT中指定要检索的行数为检索的最大行数。如果没有足够的行，MySQL将只返回它能反回的那么多行。

* SQL语句由子句构成，有些子句是必需的，而有的是可选的。一个子句通常由一个关键字和所提供的数据组成。子句的例子有SELECT语句的FROM子句。
  
#### ORDER BY子句
* ORDER BY子句取一个或多个列的名字，据此对输出进行排序。
* 对prod_name列以字母顺序排序数据: `SELECT prod_name FROM products ORDER BY prod_name;`
* 按多个列排序: `SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price,prod_name;`(先按prod_price排序，相同时再按prod_name排序)
* 指定排序方向,数据排序不限于升序排序(从A到Z)。这只是默认的排序顺序，使用DESC关键字进行降序(从Z到A)排序: `SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC;`
* 若打算用多个列排序以降序排序其中一列: `SELECT prod_id,prod_price,prod_name FROM products ORDER BY pro_price DESC,prod_name;`
* DESC关键字只应用到直接位于其前面的列名
* 如果想在多个列上进行降序排序，必须对每个列指定DESC关键字
* ASC是升序(默认)
* ORDER BY子句的位置：应该保证它位于FROM子句之后；如果使用LIMIT，它必须位于ORDER BY之后。使用子句的次序不对将产生错误消息。
  
#### WHERE子句
* 在SELECT语句中，数据根据WHERE子句中指定的搜索条件进行过滤。
* WHERE子句在表名(FROM子句)之后给出, `SELECT prod_name,prod_price FROM products WHERE prod_price = 2.50;`,这条语句从products表中检索两个列，但不返回所有行，只返回prod_price值为2.50的行。
* 在同时使用ORDER BY和WHERE子句时，应该让ORDER BY位于WHERE之后。
* WHERE子句操作符处理常用的=、<>(不等于)、!=、<、<=、>、>=，还有BETWEEN () AND ()。
* 如果将值与串类型列进行比较，则需要线定单引号，如: `WHERE prod_name='fuses`，单引号用来限定字符串。
* 一个特殊的WHERE子句可以用来检查具有NULL值的列。 `SELECT prod_name FROM products WHERE prod_price IS NULL`。<br>
注意：**NULL与不匹配**，在通过过滤选择出不具有特定值的行时，你可能希望返回具有NULL值的行。但是，不行，因为未知具有特殊的含义，数据库不知道它们是否匹配，所以在匹配过滤或不匹配过滤时不返回它们。
  
##### 组合WHERE子句
* 为了进行更强的过滤控制，MySQL允许给出多个WHERE子句。这些子句可以用两种方式使用:以AND子句的方式或OR子句的方式使用。
* AND操作符: `WHERE vend_id = 1003 AND prod_price <= 10;`上述例子中使用了一个关键字AND，还可以添加多个过滤条件，每添加一条就要使用一个AND。
* OR操作符: `WHERE vend_id = 1002 OR vend_id = 1003;`
* AND在计算次序中优先级高于OR。任何时候使用具有AND和OR操作符的WHERE子句，都应该使用圆括号明确地分组操作符。
* IN操作符:用来指定条件范围，范围中的每个条件都可以进行匹配，功能与OR相当。IN取合法值的由逗号分隔的清单，全都括在圆括号中。 `WHERE vend_id IN (1002,1003) ORDER BY prod_name;`
* IN操作符一般比OR操作符清单执行更快。其最大的优点是可以包含其他SELECT语句，使得能够更动态地建立WHERE子句。
* NOT操作符:WHERE子句中的NOT操作符有且只有一个功能，那就是否定它之后所跟的任何条件。 `WHERE vend_id NOT IN (1002,1003)`
* MySQL支持使用NOT对IN、BETWEEN和EXISIS子句取反。

### 通配符
* 通配符:用来匹配值的一部分的特殊字符。
* 搜索模式:由字面值、通配符或两者组合构成的搜索条件。根据MySQL的配置方式，搜索可以是区分大小写的。
* 为了在搜索子句中使用通配符，必须使用LIKE操作符。
* 操作符在它作为谓词时不是操作符。
#### LIKE操作符
* LIKE指示MySQL，后跟的搜索模式利用通配符匹配而不是直接相等匹配进行比较。
* 百分号(%)通配符。%表示任何字符出现任意次数。<br>
  (1) `WHERE prod_name LIKE 'jet%'`此例子使用了搜索模式'jet%'。在执行这条子句时，将检索任意jet起头的词。%告诉MySQL接受jet之后的任意字符，不管它有多少字符。<br>
  (2) `WHERE prod_name LIKE '%anvi1%'`通配符可在搜索模式中任意位置使用，并且可以使用多个通配符。
  (3) 除了一个或多个字符外，%还能匹配0个字符。
  (4) 虽然似乎%通配符可以匹配任何东西，但有一个例外，即NULL。
  
* 下划线(_)通配符。下划线的用途与%一样，但下划线只匹配单个字符而不是多个字符。
#### 通配符的使用技巧
通配符搜索的处理一般要比前面讨论的其他搜索所花时间更长，所以要记住使用通配符的一些技巧。
* 不要过度使用通配符。如果其他操作符能达到相同的目的，应该使用其他操作符。
* 在确实需要使用通配符时，除非绝对有必要，否则不要把它们用在搜索模式的开始处。把通配符置于搜索模式的开始处，搜索起来是最慢的。

### 正则表达式
正则表达式是用来匹配文本的特殊的串(字符集合)。
* 基本字符匹配。 `WHERE prod_name REGEXP ‘1000’`除关键字LIKE被REGEXP替代外，这条语句看上去非常像LIKE的语句，它告诉MySQL：REGEXP后所跟的东西作为正则表达式(与文字正文1000匹配的一个正则表达式)处理。
* REGEXP与LIKE的重要差别：<br>
`WHERE prod_name LIKE '1000';`与`WHERE prod_name REGEXP '1000'`如果被匹配的文本在列值中出现，LIKE将不会找到它，相应的行也不被返回(除非使用通配符)。而REGEXP在列值内进行匹配，如果被匹配的文本在列值出现，REGEXP将会找到它，相应的行将被返回。
  
* MySQL中的正则表达式匹配不区分大小写。为区分大小写，可使用BINARY关键字，如 `WHERE prod_name REGEXP BINARY 'JetPack .000'`
* 进行OR匹配。为搜索两个串之一，使用|，如： `WHERE prod_name REGEXP '1000|2000';`当然也可以给出两个以上的OR条件
* 匹配几个字符之一。 `WHERE prod_name REGEXP '[123] Ton';`[123]定义一组字符，它的意思是匹配1或2或3，因此如果有，1 ton，2 ton和3 ton都匹配且返回。<br>
字符集合也可以被否定，即，它们将匹配除指定字符外的任何东西。为否定一个字符集，在集合的开始处放置一个^即可，即[^123]。
  
* 匹配范围。集合可用来定义要匹配的一个或多个字符。如下面的集合将匹配数字0到9：[0-9]。此外，范围不一定只是数值，[a-z]匹配任意字母字符。
* 匹配特殊字符。.匹配任意字符，如果要找出包含.字符的值，需要用\\为前导。\\-表示查找-，\\.表示查找.。(为什么用两个反斜杠？多数正则表达式实现使用单个反斜杠转移特殊字符，以便能使用这些字符本身。但MySQL要求两个反斜杠，MySQL自己解释一个，正则表达式库解释另一个)

* 空白元字符
  
| 元字符 | 说明 |
|:-----:|:----:|
| \\f | 换页 |
| \\n | 换行 |
| \\r | 回车 |
| \\t | 制表 |
| \\v | 纵向制表 |

* 匹配字符类

| 类 | 说明 |
|:---:|:----:|
| [:alnum:] | 任意字母和数字(同[a-zA-Z0-9]) |
| [:aplha:] | 任意字符(同[a-zA-Z]) |
| [:blank:] | 空格和制表(同[\\t]) |
| [:cntrl:] | ASCII控制字符(ASCII 0到31和127) |
| [:digit:] | 任意数字(同[0-9]) |
| [:graph:] | 与[:print:]相同，但不包括空格 |
| [:lower:] | 任意小写字母(同[a-z]) |
| [:print:] | 任意可打印字符 |
| [:punct:] | 既不在[:alnum:]又不在[:cntrl:]中的任意字符 |
| [:space:] | 包括空格在内的任意空白字符(同[\\f\\n\\r\\t\\v]) |
| [:upper:] | 任意大写字母(同[A-Z]) |
| [:xdight:] | 任意十六进制数字(同[a-fA-F0-9]) |

* 重复元字符

| 元字符 | 说明 |
|:---:|:-----:|
| * | 0个或多个匹配 |
| + | 1个或多个匹配(等于{1,}) |
| ? | 0个或1个匹配(等于{0,1}) |
| \{n\} | 指定数目的匹配 |
| \{n,\} | 不少于指定数目的匹配 |
| \{n,m\} | 匹配数目的范围(m不超过255) |

例子：

`WHERE prod_name REGEXP '\\([0-9] sticks?\\)'`

其中，\\(匹配(，[0-9]匹配任意数字，sticks？匹配stick和sticks(s后的？使s可选，因为？匹配它前面的任何字符的0次或1次出现)，\\)匹配)。

`WHERE prod_name REGEXP '[[:digit:]]{4}'`

其中，[:digit:]匹配任意数字，因而它为数字的一个集合。{4}确切地要求它前面的字符(任意数字)出现4次。

* 定位符

| 元字符 | 说明 |
|:---:|:----:|
| ^ | 文本的开始 |
| $ | 文本的结尾 |
| [[:<:]] | 词的开始 |
| [[:>:]] | 词的结尾 |

例子：

`WHERE prod_name REGEXP '^[0-9\\.]'`

其中，^匹配串的开始。因此，^[0-9\\.]只在.或任意数字为串中第一个字符时才匹配它们。没有^，则还要多检索出4个别的行(哪些中间有数字的行)。

* ^的双重用途 ^有两种用法。在集合中(用[和]定义)，用来否定该集合，否则，用来指串的开始处。

### 创建计算字段
#### 拼接字段
* 在MySQL的SELECT语句种，可使用Concat()函数来拼接两个列:<br>
  `SELECT Concat(vend_name, ' (', vend_country, ')') FROM vendors ORDER BY vend_name;`<br>
  输出: `ACME (USA)`

* 多数DBMX使用+或||来实现拼接，MySQL则使用Concat()函数来实现。
* Concat()拼接串，即把多个串连接起来形成一个较长的串。Concat()需要一个或多个指定的串，各个串之间用逗号分隔。

#### 删除数据多余空格
* 使用MySQL种的RTrim()函数可以去掉值右边的所有空格。如: `SELECT Concat(RTrim(vend_name), ' (',RTrim(vend_country), ')') FROM vendors ORDER BY vend_name;`
* MySQL除了支持RTrim()，还支持LTrim()(去掉串左边的空格)以及Trim()(去掉串左右两边的空格)。

#### 使用别名
一个未命名的列不能用于客户机应用种，因为客户机没有办法引用它。所以需要给SELECT语句拼接好的字段一个列别名。
* 别名(alias)是一个字段或值的替换名。别名用AS关键字赋予，如: `SELECT Concat(RTrim(vend_name), ' (',RTrim(vend_country), ')') AS vend_title FROM vendors ORDER BY vend_name;`<br>
它指示SQL创建一个包含指定计算的名为vend_title的计算字段。任何客户机应用都可以按名引用这个列，就像它是一个实际的表列一样。
  
* 别名还有其它用途。常见的用途包括在实际的表列名不符合规定的字符(如空格)时重新命名它，在原来的名字含混或容易误解时扩充它，等等。
* 别名有时也称为导出列(derived column)。

#### 执行算术字段
* `SELECT prod_id,quantity,item_prie,quantity*item_price AS expanded_price FROM orderitems WHERE order_num = 20005;`
* MySQL算术操作符

| 操作符 | 说明 |
|:---:|:---:|
| + | 加 |
| - | 减 |
| * | 乘 |
| / | 除 |

### 使用数据处理函数
#### 常用的文本处理函数
| 函数 | 说明 |
|:---:|:----:|
| Left() | 返回串左边的字符 |
| Length() | 返回串的长度 |
| Locate() | 找出串的一个子串 |
| Lower() | 将串转换为小写 |
| LTrim() | 去掉串左边的空格 |
| Right() | 返回串右边的字符 |
| RTrim() | 去掉串右边的空格 |
| Soundex() | 返回串的SOUNDEX值 |
| SubString() | 返回子串的字符 |
| Upper() | 将串转换为大写 |
* SOUNDEX是一个将任何文本串转换为描述其语音表示的字母数字模式的算法。SOUNDEX考虑了类似的发音字符和音节，使得能对串进行发音比较而不是字母比较。

#### 常用日期和时间处理函数
| 函数 | 说明 |
|:---:|:------:|
| AddDate() | 增加一个日期(天、周) |
| AddTime() | 增加一个时间(时、分) |
| CurDate() | 返回当前日期 |
| CurTime() | 返回当前时间 |
| Date() | 返回日期时间的日期部分 |
| DateDiff() | 计算两个日期之差 |
| Date_Add() | 高度灵活的日期运算函数 |
| Date_Format() | 返回一个格式化的日期或时间串 |
| Day() | 返回一个日期的天数部分 |
| DayOfWeek() | 对于一个日期，返回对应的星期几 |
| Hour() | 返回一个时间的小时部分 |
| Minute() | 返回一个时间的分钟部分 |
| Month() | 返回一个日期的月份部分 |
| Now() | 返回当前日期和时间 |
| Second() | 返回一个时间的秒部分 |
| Time() | 返回一个日期时间的时间部分 |
| Year() | 返回一个日期的年份部分 |
* 注意MySQL使用的日期格式。无论你什么时候指定一个日期，不管是插入或更新表值还是用WHERE子句进行过滤，日期必须为格式yyyy-mm-dd。
* 如果你想要的仅是日期，则使用Date()是一个良好的习惯。
* 使用BETWEEN ADN可以匹配一个时间范围。

#### 常用的数值处理函数
| 函数 | 说明 |
|:---:|:---:|
| Abs() | 返回一个数的绝对值 |
| Cos() | 返回一个角度的余弦 |
| Exp() | 返回一个数的指数值 |
| Mod() | 返回除操作的余数 |
| Pi() | 返回圆周率 |
| Rand() | 返回一个随机数 |
| Sin() | 返回一个角度的正弦 |
| Sqrt() | 返回一个数的平方根 |
| Tan() | 返回一个角度的正切 |

### 汇总数据
#### SQL聚集函数
| 函数 | 说明 |
|:---:|:----:|
| AVG() | 返回某列的平均值 |
| COUNT() | 返回某列的行数 |
| MAX() | 返回某列的最大值 |
| MIN() | 返回某列的最小值 |
| SUM() | 返回某列值之和 |

* AVG()函数<br>
`SELECT AVG(prod_price) AS avg_price FROM products;`<br>
  AVG()只用于单个列，只能用来确定特定数值列的平均值，而且列名必须作为函数参数给出。为了获得多个列的平均值，必须使用多个AVG()函数。<br>
  AVG()函数忽略列值为NULL的行。
  
* COUNT()函数<br>
`SELECT COUNT(*) AS num_cust FROM customers;`<br>
  COUNT()函数有两种使用方式：<br>
  (1) 使用COUNT(*)对表中行的数目进行计数，不管表列中包含的是空值(NULL)还是非空值。<br>
  (2) 使用COUNT(column)对特定列中具有值的行进行计数，忽略NULL值。
  
* MAX()函数<br>
`SELECT MAX(prod_price) AS max_price FROM products;`<br>
  对非数值数据使用MAX()的时候，虽然MAX()一般用来找出最大的数值或日期值，但MySQL允许将它用来返回任意列中的最大值，包括返回文本列中的最大值。在用于文本数据时，如果数据按相应的列排序，则MAX()返回最后一行。<br>
  MAX()函数忽略列值为NULL的行。
  
* MIN()函数<br>
`SELECT MIN(prod_price) AS min_price FROM products;`<br>
  对非数值数据使用MIN()的时候，MIN()函数与MAX()函数类似，MySQL允许将它用来返回任意列中的最小值，包括返回文本列中的最小值。在用于文本数据时，如果数据按相应的列排序，则MIN()返回最前面的行。<br>
  MIN()函数忽略列值为NULL的行。
  
* SUM()函数<br>
`SELECT SUM(quantity) AS items_ordered FROM orderitems WHERE order_num = 20005;`<br>
  在多个列上进行计算: `SELECT SUM(item_price*quantity) AS total_price FROM orderitems WHERE order_num = 20005;`<br>
  SUM()函数忽略列值为NULL的行。
  
* **以上五个聚集函数都可以如下使用**<br>
  (1) 对所有的行执行计算，指定ALL参数或不给参数(因为ALL时默认参数行为);<br>
  (2) 只包含不同的值，指定DISTINCT参数。例如： `SELECT AVG(DISTINT prod_price) AS avg_price FROM products WHERE vend_id = 1003;`其中使用了DISTINCT参数，因此平均值只考虑各个不同的价格。<br>
  **注意**:如果指定列名，则DISTINCT只能用于COUNT()。DISTINCT只能用于COUNT(*),因此不允许使用COUNT(DISTINCT)，否则会产生错误。类似的，DISTINCT必须使用列名，不能用于计算或表达式。
  
### 分组数据
#### GROUP BY
* 例句: `SELECT vend_id, COUNT(*) AS num__prods FROM products GROUP BY vend_id;`
#### GROUP BY的使用规则
* GROUP BY子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。
* 如果在GROUP BY子句中嵌套了分组，数据将在最后规定的分组进行汇总。换句话说，在建立分组时，指定的所有列都是一起计算(所以不能从个别的列取回数据)。
* GROUP BY子句中列出的每个列都必须是检索列或有效的表达式(但不能是聚集函数)。如果在SELECT中使用表达式，则必须在GROUP BY子句中指定相同的表达式。不能使用别名。
* 除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子句中给出。
* 如果分组列中具有NULL值，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。
* GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。<br>
**使用ROLLUP**:使用WITH ROLLUP关键字，可以得到每个分组以及每个分组汇总级别(针对每个分组)的值，如下: `SELECT vend_id, COUNT(*) AS num_prods FROM products GROUP BY vend_id WITH ROLLUP;`
  
#### HAVING子句(过滤分组)
目前为止所学过的所有类型的WHERE子句都可以用HAVING来替代。唯一的差别是WHERE过滤行，而HAVING过滤分组。
* HAVING支持所有WHERE操作符。例句： `SELECT cust_id, COUNT(*) AS orders FROM orders GROUP BY cust_id HAVING COUNT(*) >= 2;`
* HAVING和WHERE的差别：这里有一种理解方法，WHERE在数据分组前进行过滤，HAVING在数据分组后进行过滤。这是一个重要的区别，WHERE排除的行不包括在分组中。这可能会改变计算值，从而影响HAVING子句中基于这些值过滤掉的分组。

#### ORDER BY(分组和排序)
虽然GROUP BY和ORDER BY经常完成相同的工作，但是它们是非常不同的。
* ORDER BY与GROUP BY

| ORDER BY | GROUP BY |
|:------:|:----------:|
| 排序产生的输出 | 分组行。但输出可能不是分组的顺序 |
| 任意列都可以使用(甚至非选择的列也可以使用) | 只可能使用选择列或者表达式，而且必须使用每个选择列表达式 |
| 不一定需要 | 如果与聚集函数一起使用列(或表达式)，则必须使用 |

#### SELECT子句顺序
| 子句 | 说明 | 是否必须使用 |
|:---:|:---:|:----------:|
| SELECT | 要返回的列或者表达式 | 是 |
| FROM | 从中检索数据的表 | 仅在从表选择数据时使用 |
| WHERE | 行级过滤 | 否 |
| GROUP BY | 分组说明 | 仅在按组计算聚集时使用 |
| HAVING | 组级过滤 | 否 |
| ORDER BY | 输出排序顺序 | 否 |
| LIMIT | 要检索的行数 | 否 |

### 使用子查询
#### 利用子查询进行过滤
可以把一条SELECT语句返回的结果用于另一条SELECT语句的WHERE子句。 `SELECT cust_id FROM orders WHERE order_num IN (SELECT order_num FROM orderitems WHERE prod_id = 'TNT2');`
* 在WHERE子句中使用子查询，应该保证SELECT语句具有与WHERE子句中相同数目的列。通常，子查询将返回单个列并且与单个列匹配，但如果需要也可以使用多个列。
* 虽然子查询一般与IN操作符结合使用，但也可以用于测试等于(=)、不等于(<>)等。

#### 作为计算字段使用子查询
* `SELECT cust_name, cust_state, (SELECT COUNT(*) FROM orders WHERE orders.cust_id = customers.cust_id) AS orders FROM customers ORDER BY cust_name;`

### 联结表
#### 外键
外键作为某个表中的一列，它包含另一个表的主键值，定义了两个表之间的关系。
#### 创建联结
```
SELECT vend_name, prod_name, prod_price
FROM vendors, products
WHERE vendors.vend_id = products.vend_id
ORDER BY vend_name, prod_name;
```
* 这里最大的差别就是所指定的两个列(prod_name和prod_price)在一个表中，而另一个列(vend_name)在另一个表中。

* vendors和products两个表用WHERE子句正确联结，WHERE子句指示MySQL匹配vendors表中的vend_id和products表中的vend_id。  

* 可以看到要匹配的两个列以vendors.vend_id和products.vend_id指定。这里需要这种完全限定列名，因为如果只给出vend_id，则MySQL不知道指的是哪一个。

* **注意**:应该保证所有的联结都有WHERE子句，否则MySQL将返回比想要的数据多得多的数据。同理，应该保证WHERE子句的正确性。不正确的过滤条件将导致MySQL返回不正确的数据。

* 完全限定列名:在引用的列可能出现二义性时，必须使用完全限定列名(用一个点分隔的表名和列名)。如果引用一个没有用表名限制的具有二义性的列名，MySQL将返回错误。

* 笛卡尔积:由没有联结条件的表关系返回的结果为笛卡尔积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。
#### 内部联结
目前为止所用的联结称为等值联结，它基于两个表之间的相等测试。这种联结也称为内部联结。对于这种联结可以使用稍微不同的语法来明确指定联结的类型:

```
SELECT vend_name, prod_name, prod_price
FROM vendors INNER JOIN products
ON vendors.vend_id = products.vend_id;
```
两个表之间的关系是FROM子句的组成部分，以INNER JOIN指定。在使用这种语法时，联结条件用特定的ON子句而不是WHERE子句给出。传递给ON的实际条件与传递给WHERE的相同。
#### 联结多个表
SQL对一条SELECT语句中可以联结的表的数目没有限制，通过AND连接。
```
SELECT prod_name, vend_name, prod_price, quantity
FROM orderitems, products, vendors
WHERE products.vend_id = vendors.vend_id
  AND orderitems.prod_id = products.prod_id
  AND order_num = 20005;
```
这里的FROM子句列出了3个表，而WHERE子句定义了这两个联结的条件，第三个联结条件用来过滤出订单20005中的物品。
* MySQL在运行时关联指定的每个表以处理联结。这种处理可能是非常耗费资源的，因此应该仔细，不要联结不必要的表。联结的表越多，性能下降越厉害。

### 创建高级联结
#### 使用表别名
SQL允许给表起别名
```
SELECT cust_name, cust_contact
FROM customers AS c, orders AS o, orderitems AS oi
WHERE c.cust_id = 0.cust_id
  AND oi.order_num = o.order_num
  AND prod_id = 'TNT2';
```
**注意**:表别名只在查询执行中使用。与列别名不一样，表别名不返回到客户机。
#### 自联结
```
SELECT p1.prod_id, p1.prod_name
FROM products AS p1, products AS p2
WHERE p1.vend_id = p2.vend_id
  AND p2.prod_id = 'DTNTR';
```
此查询中需要的两个表实际上是相同的表。
* 自联结通常作为外部语句用来替代从相同表中检索数据时使用的子查询语句。虽然最终结果是相同的，但有时候处理联结远比处理子查询快得多。
### 自然联结
无论何时对表进行联结，应该至少有一个列出现在不止一个表中(被联结的列)。标准的联结(即前一章中介绍的内部联结)返回所有数据，甚至相同的列多次出现。自然联结排除多次出现，使每个列只返回一次。
```
SELECT c.*, o.order_num, o.order_date,
       oi.prod_id, oi.quantity, oi.item_price
FROM customers AS c, orders AS o, orderitems AS oi
WHERE c.cust_id = o.cust_id
  AND oi.order_num = o.order_num
  AND prod_id = 'FB';
```
事实上，迄今为止我们建立的每个内部联结都是自然联结，很可能我们永远不会用到不是自然联结的内部联结。
#### 外部联结
许多联结将一个表中的行与另一个表中的行相关联。但有时候会需要包含没有关联行的那些行。联结包含了那些相关表中没有关联行的行，这种类型的联结成为外部联结。
```
SELECT customers.cust_id, order.order_num
FROM customers LEFT OUTER JOIN orders
  ON customers.cust_id = order.cust_id;
```
* 这条语句是用来关键字OUTER JOIN来指定联结的类型(而不是在WHERE子句中指定)。
* 在使用OUTER JOIN语法时，必须使用RIGHT或LEFT关键字指定包括其所有行的表(RIGHT指出的是OUTER JOIN右边的表，而LEFT指出的是OUTER JOIN左边的表)。
* 上面的例子使用LEFT OUTER JOIN从FROM子句的左边表(customers表)中选择所有行。

#### 使用带聚集函数的联结
```
SELECT customers.cust_name,
       customers.cust_id,
       COUNT(orders.order_num) AS num_ord
FROM customers INNER JOIN orders
ON customers.cust_id = orders.cust_id
GROUP BY customers.cust_id;
```

### 组合查询
多数SQL查询都只包含从一个或多个表中返回数据的单条SELECT语句。MySQL也允许执行多个查询(多条SELECT语句)，并将结果作为单个查询结果集返回。这些组合查询通常称为并或复合查询

有两种基本情况，其中需要使用组合查询
* 在单个查询中从不同的表返回类似结构的数据
* 对单个表执行多个查询，按单个查询返回数据

#### 创建组合查询
可用UNION操作符来组合数条SQL查询。

#### 使用UNION
UNION的使用很简单，所需的只是给出每条SELECT语句，在各条语句之间放上关键字UNION。
```
SELECT vend_id, prod_id, prod_price
FROM products
WHERE prod_price <= 5
UNION
SELECT vend_id, prod_id, prod_price
FROM products
WHERE vend_id IN (10001,10002);
```
* 使用UNION的组合查询可以应用不同的表
#### UNION规则
* UNION必须由两条或两条以上的SELECT语句组成，语句之间用关键字UNION分隔
* UNION中的每个查询必须包含相同的列、表达式或聚集函数(不过各个列不需要以相同的次序给出)
* 列数据类型必须兼容:类型不必完全相同，但必须是DBMS可以隐含地转换的类型

#### 包含或取消重复行
* UNION从查询结果集中自动去除了重复的行
* 使用UNION ALL可以返回所有匹配行

#### 对组合查询结果排序
* 在用UNION组合查询时，只能使用一条ORDER BY子句，它必须出现在最后一条SELECT语句之后。

### 全文本搜索
* 并非所有引擎都支持全文本搜索。两个最常使用的引擎为MyISAM和InnoDB，前者支持全文本搜索，而后者不支持。
#### 理解全文本搜索
在使用全文本搜索时，MySQL不需要分别查看每个行，不需要分别分析和处理每个词。MySQL创建指定列中各词的一个索引，搜索可以针对这些词进行。这样MySQL可以快速有效地决定哪些词匹配(哪些行包含它们)，哪些词不匹配，它们的匹配频率等等。

#### 使用全文本搜索
为了进行全文本搜索，必须索引被搜索的列，而且要随着数据的改变不断地重新索引。在对表列进行适当设计后，MySQL会自动进行所有的索引和重新索引。
#### 启动全文本搜索支持
在创建表时启用全文本搜索。CREATE TABLE语句接受FULLTEXT子句，它给出被索引列的一个逗号分隔的列表。
```
CREATE TABLE productnotes
(
  note_id int NOT NULL AUTO_INCREMENT,
  prod_id char(10) NOT NULL,
  note_date datetime NOT NULL,
  note_text text NULL,
  PRIMARY KEY(note_id),
  FULLTEXT(note_text)
) ENGINE=MyISAM;
```
* 不要再导入数据时使用FULLTEXT。应该先导入所有数据，然后再修改表，定义FULLTEXT。

#### 进行全文本搜索
在索引之后，使用Match()和Against()执行全文本搜索，其中Match()指定被搜索的列，Against()指定要使用的搜搜表达式。
```
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('rabbit');
```
* 传递给Match()的值必须与FULLTEXT()定义中的相同。如果指定多个列，则必须列出他们(而且次序正确)。
* 除非使用BINARY方式，否则全文本搜索不区分大小写。

#### 使用查询扩展
查询扩展用来设法放宽所返回的全文本搜索结果的范围。

查询扩展使用方法:`Against(‘xxxx’ WITH QUERY EXPANSION)`

在使用查询扩展时，MySQL对数据和索引进行两遍扫描来完成搜索
* 首先，进行一个基本的全文本搜索，找出与搜索条件匹配的所有行
* 其次，MySQL检查这些匹配行并选择所有有用的词
* 再其次，MySQL再次进行全文本搜索，这次不仅使用原来的条件，而且还使用所有有用的词

表中的行越多，使用查询扩展返回的结果就越好

#### 布尔文本搜索
以布尔方式，可以提供关于如下内容的细节
* 要匹配的词
* 要排斥的词(如果某行包含这个词，则不返回该行，即使它包含其他指定的词也是如此)
* 排列提示(指定某些词比其他词更重要，更重要的词等级更高)
* 表达式分组
* 另外一些内容
**即使没有FULLTEXT索引也可以使用**
  
使用方法:`Against('xxxx' IN BOOLEAN MODE)`

全文本布尔操作符

| 布尔操作符 | 说明 |
|:-----:|:---------:|
| + | 包含，词必须存在 |
| - | 排除，词必须不出现 |
| > | 包含，而且增加等级值 |
| < | 包含，且减少等级值 |
| () | 把词组成子表达式(允许这些子表达式作为一个组被包含、排除、排列等) |
| ~ | 取消一个词的排序值 |
| * | 词尾的通配符 |
| “” | 定义一个短语(与单个词的列表不一样，它匹配整个短语以便包含或排除这个词语) |

#### 全文本搜索的使用说明
* 在索引全文本数据时，短词被忽略且从索引中排除。短词定义为那些具有3个或3个以下字符的词(如果需要，这个数目可以更改)。
* MySQL带有一个内建的非用词(stopword)列表，这些词在索引全文本数据时总是被忽略。如果需要，可以覆盖这个列表
* 许多词出现的频率很高，搜索它们没有用处(返回太多的结果)。因此MySQL规定了一条50%规则，如果一个词出现在50%以上的行中，则将它作为一个非用词忽略。50%规则不用于IN BOOLEAN MODE。
* 如果表中的行数少于3行，则全文本搜索不返回结果(因为每个词或者不出现，或者至少出现在50%的行中)。
* 忽略词中的单引号。例如，don't索引为dont。
* 不具有词分隔符(包括日语和汉语)的语言不能恰当地返回全文本搜索结果。
* 仅MyISAM数据库引擎支持全文本搜索。

### 插入数据之INSERT
#### 插入完整的行
```
INSERT INTO customers(cust_name,
  cust_address,
  cust_city,
  cust_state,
  cust_zip,
  cust_county,
  cust_contact,
  cust_email)
VALUES('Pep E. LaPew',
  '100 Main Street',
  'Los Angeles',
  'CA',
  '90046',
  'USA',
  NULL,
  NULL);
```
此例子中在表名后的括号里明确地给出了列名。在插入行时，MySQL将用VALUES列表中的相应值填入列表中的对应项。即使表的结构改变，此INSERT语句仍然能够正确地工作。

#### 插入多个行
```
INSERT INTO customers(cust_name,
  cust_address,
  cust_city,
  cust_state,
  cust_zip,
  cust_county)
VALUES('Pep E. LaPew',
  '100 Main Street',
  'Los Angeles',
  'CA',
  '90046',
  'USA'),
  (
    'M. Martian',
    '42 Galaxy Way',
    'New York',
    'NY',
    '11213',
    'USA'
);
```
其中单挑INSERT语句有多组值，每组值用一对圆括号括起来，用逗号分隔。

#### 插入检索出的数据
```
INSERT INTO customers(cust_id,
  cust_contact,
  cust_email,
  cust_name,
  cust_address,
  cust_city,
  cust_state,
  cust_zip,
  cust_county)
SELECT cust_id,
  cust_contact,
  cust_email,
  cust_name,
  cust_address,
  cust_city,
  cust_state,
  cust_zip,
  cust_county
FROM custnew;
```
* 这个例子使用INSERT SELECT从custnew中将所有数据导入customers。
* 事实上，INSERT和SELECT语句中的列名不要求匹配，MySQL甚至不关心SELECT返回的列名，它使用的是列的位置。

### 更新和删除数据
#### 更新数据-UPDATE语句
基本的UPDATE语句由3部分组成，分别是
* 要更新的表
* 列名和它们的新值
* 确定要更新行的过滤条件
```
UPDATE customers
SET cust_name = 'The Fudds',
    cust_email = 'elmer@fudd.com'
WHERE cust_id = 10005;
```
在更新多个列时，只需要使用单个SET命令，每个‘列=值’对之间用逗号分隔(最后一列之后不用逗号)。
#### INGORE关键字
如果用UPDATE语句更新多行，并且在更新这些行中的一行或多行时出现一个错误，则整个UPDATE操作被取消(错误发生前的所有行被恢复到它们原来的值)。即使时发生错误，也继续进行更新，可使用IGNORE关键字，如`UPDATE IGNORE customers...`

#### 删除数据
```
DELETE FROM customers
WHERE cust_id = 10006;
```
* DELETE不需要列名或通配符。DELETE删除整行而不是删除列。为了删除指定的列，请使用UPDATE语句。
* 如果想从表中删除所有行，不要使用DELETE。可使用TRUNCATE TABLE语句，它完成相同的工作，但速度更快(TRUNCATE实际是删除原来的表并重新创建一个表，而不是逐行删除表中的数据)。

### oooooooooooooooooooo-