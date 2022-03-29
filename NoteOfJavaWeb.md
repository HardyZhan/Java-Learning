# JavaWeb
## 数据库
### 关系型数据库
* 关系型数据库是建立在关系模型基础上的数据库，简单说，关系型数据库是由多张能互相连接的二维表组成的数据库
#### SQL分类
* DDL(Data Definition Language) 数据定义语言，用来定义数据库对象：数据库，表，列等
* DML(Data Manipulation Language) 数据库操作语言，用来对数据库中的表进行增删改
* DQL(Data Quert Language) 数据查询语言，用来查询数据库中表的记录
* DCL(Data Control Language) 数据控制语言，用来定义数据库的访问权限和安全级别，及创建用户
#### DDL操作数据库语句
* 查询：`SHOW DATABASES;`
* 创建：`CREATE DATABASE 数据库名称;`或者(判断，如果不存在则创建)`CREATE DATABASE IF NOT EXISTS 数据库名称;`
* 删除：`DROP DATABASES 数据库名称;`或者(判断，如果存在则删除)`DROP DATABASE IF EXISTS 数据库名称;`
* 使用数据库:(查看当前使用的数据库)`SELECT DATABASE();`或者(使用数据库)`USE 数据库名称;`

## JDBC
* JDBC就是使用Java语言操作关系型数据库的一套API。
### 步骤
* 创建工程，导入驱动jar包
* 注册驱动`Class.forName("com.mysql.jdbc.Driver");`
* 获取连接`Connection conn = DriverManager.getConnection(url, username, password);`
* 定义SQL语句`String sql = "";`
* 获取执行SQL对象`Statement stmt = conn.createStatement();`
* 执行SQL`stmt.executeUpdate(sql);`

## MyBatis
### 什么是MyBatis？
* MyBatis是一款优秀的**持久层**框架，用于简化JDBC开发
* 官网:https://mybatis.org/mybatis-3/zh/index.html
### 持久层
* 负责将数据到保存到数据库的那一层代码
* JavaEE三层架构:表现层、业务层、持久层
### 框架
* 框架是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
* 在框架的基础之上构建软件编写更加高效、规范、通用、可扩展

### xml配置时注意
 * 参数占位符:<br>
	<br>（1） #{}:会将其替换为?
    <br>（2） ${}:拼sql，会存在SQL注入问题
    <br>（3） 使用时机：
       <br> * 参数传递的时候使用:#{};
       <br> * 表明或者列名不固定的情况下，使用:${};
                
* 参数类型: parameterType:可以省略
* 特殊字符处理: 
            <br>（1） 使用转义字符:例如 < 用 &+l+t+;
            <br>（2） CDATA区:
            <br>< 用如下代替
            <br><![CDATA[
                <
            ]]>


### 动态SQL
* if: 条件判断
	- test:逻辑表达式
* 问题:第一个条件不需要逻辑运算符
* 解决方案：
	- 恒等式
	- &lt;where&gt; 替换 where 关键字
	


### MyBatis事务
* openSession():默认开启事务，进行增删改查操作后需要使用sqlSession.commit();手动提交事务
* openSession(true):可以设置为自动提交事务(关闭事务)

## HTML
* HyperText Markup Language：超文本标记语言
* W3C标准：网页主要由三部分组成
	- 结构：HTML
	- 表现：CSS
	- 行为：JavaScript
* 前端技术参考手册：w3cschools
### 基础标签
| 标签 | 描述 |
|:----:|:---------:|
| &lt;h1&gt; - &lt;h6&gt; | 定义标题大小，h1最大，h6最小 |
| &lt;font&gt; | 定义文本的字体、字体尺寸、字体颜色 |
| &lt;b&gt; | 定义粗体文字 |
| &lt;i&gt; | 定义斜体文字 |
| &lt;u&gt; | 定义文本下划线 |
| &lt;center&gt; | 定义文本居中 |
| &lt;p&gt; | 定义段落 |
| &lt;br&gt; | 定义折行 |
| &lt;hr&gt; | 定义水平线 | 

### 图片、音频、视频标签
| 标签 | 描述 |
|:----:|:-----:|
| &lt;img&gt; | 定义图片 |
| &lt;audio&gt; | 定义音频 |
| &lt;video&gt; | 定义视频 |

* img：定义图片
	- src：规定显示图像的URL(统一资源定位符)
	- height：定义图像的高度
	- width：定义图像的宽度
* audio：定义音频。支持的音频格式：MP3、WAV、OGG
	- src：规定音频的URL
	- controls：显示播放控件
* video：定义视频。支持的视频格式：MP4、WebM、OGG
	- src：规定视频的URL
	- controls：显示播放软件

### 超链接标签
| 标签 | 描述 |
|:----:|:-----:|
| &lt;a&gt; | 定义超链接，用于链接到另一个资源 |

* href：指定访问资源的URL
* target：指定打开资源的方式
	- &#95;self:默认值，在当前页面打开
	- &#95;blank:在空白页面打开
	

### 列表标签
| 标签 | 描述 |
|:----:|:-----:|
| &lt;ol&gt; | 定义有序列表 |
| &lt;ul&gt; | 定义无序列表 |
| &lt;li&gt; | 定义列表项 |

### 表格标签
| 标签 | 描述 |
|:----:|:-----:|
| &lt;table&gt; | 定义表格 |
| &lt;tr&gt; | 定义行 |
| &lt;td&gt; | 定义单元格 |
| &lt;th&gt; | 定义表头单元格 |

<table>
	<tr>
		<th>标签</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>&lt;table&gt;</td>
		<td>定义表格</td>
	</tr>
	<tr>
		<td>&lt;tr&gt;</td>
		<td>定义行</td>
	</tr>
	<tr>
		<td>&lt;td&gt;</td>
		<td>定义单元格</td>
	</tr>
	<tr>
		<td>&lt;th&gt;</td>
		<td>定义表头单元格</td>
	</tr>
</table>

* table:定义表格
	- border:规定表格边框的宽度
	- width:规定表格的宽度
	- cellspacing:规定单元格之间的空白
* tr:定义行
	- align:定义表格行的内容对齐方式
* td:定义单元格
	- rowspan:规定单元格可横跨的行数
	- colspan:规定单元格可横跨的列数
### 布局标签
| 标签 | 描述 |
|:----:|:-----:|
| &lt;div&gt; | 定义HTML文档中的一个区域部分，经常与CSS一起使用，用来布局网页 |
| &lt;span&gt; | 用于组合行内元素 |

### 表单标签
| 标签 | 描述 |
|:----:|:-----:|
| &lt;form&gt; | 定义表单 |
| &lt;input&gt; | 定义表单项，通过type属性控制输入形式 |
| &lt;label&gt; | 为表单项定义标注 |
| &lt;select&gt; | 定义下拉列表 |
| &lt;option&gt; | 定义下拉列表的列表项 |
| &lt;textarea&gt; | 定义文本域 |

* form：定义表单
	- action：规定当提交表单时向何处发送表单数据，URL
	- method：规定用于发送表单数据的方式
		+ get：浏览器会将数据直接附在表单的action URL之后。大小有限制
		+ post：浏览器会将数据放到http请求消息体中。大小无限制
		
* type属性

| type取值 | 描述 |
|:----:|:-----:|
| &lt;text&gt; | 默认值，定义单行的输入字段 |
| &lt;password&gt; | 定义密码字段 |
| &lt;radio&gt; | 定义单选按钮 |
| &lt;checkbox&gt; | 定义复选框 |
| &lt;file&gt; | 定义文件上传按钮 |
| &lt;hidden&gt; | 定义隐藏的输入字段 |
| &lt;submit&gt; | 定义提交按钮，提交按钮会把表单数据发送到服务器 |
| &lt;reset&gt; | 定义重置按钮，重置按钮会清楚表单中的所有数据 |
| &lt;button&gt; | 定义可点击按钮 |

## CSS
* CSS(Cascading Style Sheet,层叠样式表)是一门语言，用于控制网页表现

### CSS导入方式
* 内联样式：在标签内使用style属性，属性值是css属性键值对
	- `<div style="color:red">Hello CSS~</div>`

* 内部样式：定义&lt;style&gt;标签，在标签内部定义css样式
```css
	<style type="text/css">
		div{
			color: red;
		}
	</style>
```

* 外部样式：定义link标签，导入外部的css文件
	- `<link rel="stylesheet" href="demo.css">`
	- 其中，demo.css内容如下
```css
	div{
		color: red;
	}
```

### CSS选择器
* 概念：选择器是选取需设置样式的元素(标签)
* 分类
	- 元素选择器。`元素名称{color:red;}`
	- id选择器。`#id属性值{color:red;}`
	- 类选择器。`.class属性值{color:red;}`


## JavaScript
* JavaScript是一门跨平台、面向对象的脚本语言，来控制网页行为的，它能使网页可交互

### JavaScript引入方式
* 内部脚本：将JS代码定义在HTML页面中
	- 在HTML中，JavaScript代码必须位于&lt;script&gt;与&lt;/script&gt;之间
	- 在HTML文档中可以在任意地方，放置任意数量的&lt;script&gt;。
	- 一般把脚本置于&lt;body&gt;元素的底部，可改善显示速度，因为脚本执行会拖慢显示
* 外部脚本：将JS代码定义在外部JS文件中，然后引入HTML页面中。
	- 外部脚本不能包含&lt;script&gt;标签
	- &lt;script&gt;标签不能自闭合
	
### 输出语句
* window.alert() 写入警告框
* document.write() 写入HTML输出
* 使用console.log() 写入浏览器控制台

### 变量
* 使用**var**关键字来声明变量
* JavaScript是一门弱类型语言，变量**可以存放不同类型的值**
* 变量名需要遵守如下规则
	- 组成字符可以是任何字母、数字、下划线( _ )或美元符号( $ )
	- 数字不能开头
	- 建议使用驼峰命名
* ECMAScript6新增了**let**关键字来定义变量，它的用法类似于**var**，但是所声明的变量，只在**let**关键字所在的代码块内有效，且不允许重复声明
* ECMAScript6新增了**const**关键字，用来声明一个只读的常量，一旦声明，常量的值就不能改变

### 数据类型
* 分两类
	- 原始类型
		+ number:数字(整数、小数和NaN)
		+ string:字符、字符串、单双引号皆可
		+ boolean:布尔，true,false
		+ null:空对象
		+ undefined:当声明的变量未初始化，该变量的默认值是undefined
	- 引用类型
* 使用typeof运算符可以获取数据类型`alert(typeof age);`

### 运算符
* ==和===
	- ==会进行类型转换，===不会进行类型转换
* 类型转换
	- 其他类型转换为数字
		+ string：将字符串字面值转为数字，如果字面值不是数字，则转为NaN，一般使用parseInt方法进行转换
		+ boolean：true转为1，false转为0
	- 其他类型转为boolean
		+ number：0和NaN转为false，其他的数字转为true
		+ string：空字符串转为false，其他字符串转为true
		+ null：转为false
		+ undefined：转为false

### 流程控制语句

### 函数
* JavaScript函数通过function关键字进行定义(类似于matlab)

### JavaScript对象
* Array对象用于定义数组，是变长的，长度类型都可变

### window对象
* window：浏览器窗口对象
* 属性:获取其他BOM对象

| BOM对象 | 描述 |
|:---:|:-------:|
| history | 对History对象的只读引用 |
| Navigator | 对Navigator对象的只读引用 |
| Screen | 对Screen对象的只读引用 |
| location | 对location对象的只读引用 |

* 方法

| 方法 | 描述 |
|:----:|:------:|
| alert() | 显示带有一段消息和一个确认按钮的警告框 |
| comfirm() | 显示带有一段消息以及确认按钮和取消按钮的对话框 |
| setInterval() | 按照指定的周期(以毫秒计)来调用函数或计算表达式 |
| setTimeout() | 在指定的毫秒数后调用函数或计算表达式 |