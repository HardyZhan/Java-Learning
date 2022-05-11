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
 * 参数占位符:
	<br>（1） `#{}`:会将其替换为`?`
    <br>（2） `${}`:拼sql，会存在SQL注入问题
    <br>（3） 使用时机：

    	* 参数传递的时候使用:#{};
    	* 表明或者列名不固定的情况下，使用:${};
   
* 参数类型: parameterType:可以省略
* 特殊字符处理: 
            <br>（1） 使用转义字符:例如 `<` 用 `&lt;`
            <br>（2） CDATA区:
            <br> `<` 用如下代替
```bash
            <![CDATA[
                <
            ]]>
```

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
* `==`和`===`
	- `==`会进行类型转换，`===`不会进行类型转换
* 类型转换
	- 其他类型转换为数字
		+ `string`：将字符串字面值转为数字，如果字面值不是数字，则转为`NaN`，一般使用`parseInt`方法进行转换
		+ `boolean`：`true`转为1，`false`转为0
	- 其他类型转为`boolean`
		+ `number`：0和`NaN`转为`false`，其他的数字转为`true`
		+ `string`：空字符串转为`false`，其他字符串转为`true`
		+ `null`：转为`false`
		+ `undefined`：转为`false`

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

### History对象
* 使用window.history获取，其中window.可以省略
```
	window.history.方法();
	history.方法();
```

* 方法

| 方法 | 描述 |
|:----:|:------:|
| back() | 加载history列表中的前一个URL |
| forward() | 加载history列表中的下一个URL |


## JavaWeb技术栈
### B/S架构
Browser/Server，浏览器/服务器 架构模式，它的特点是，客户端只需要浏览器，应用程序的逻辑和数据都存储在服务器端，浏览器只需要请求服务器，获取Web资源，服务器把Web资源发送给浏览器即可。
### 静态资源
HTML、CSS、JavaScript、图片等。负责页面展现
### 动态资源
Serlet、JSP等。负责逻辑处理
### 数据库
负责存储数据
### HTTP协议
定义通信规则
### Web服务器
负责解析HTTP协议、解析请求数据，并发送响应数据


## HTTP
HyperText Transfer Protocol,超文本传输协议，规定了浏览器和服务器之间数据传输的规则
### HTTP协议特点
* 基于TCP协议：面向连接、安全
* 基于请求-响应模型：一次请求对应一次响应
* HTTP协议是无状态的协议：对于事务处理没有记忆能力。每次请求-响应都是独立的。
	- 缺点：多次请求不能共享数据。Java中使用会话技术(Cookie、Session)来解决这个问题
	- 优点：速度快
### HTTP-请求数据格式
* 请求数据分为3部分
	- 请求行：请求数据的第一行。其中GET表示请求方式，/表示请求资源，HTTP/1.1表示协议版本
	- 请求头：第二行开始，格式为key:value形式
	- 请求体：POST请求的最后一部分，存放请求参数
	
* GET请求和POST请求的区别
	- GET请求请求参数在请求行中，没有请求体。POST请求请求参数在请求体中
	- GET请求请求参数大小有限制，POST没有

### HTTP-响应数据格式
* 响应数据分为3部分
	- 响应行：响应数据的第一行。其中HTTP/1.1表示协议版本，200代表响应状态码，OK表示状态码描述
	- 响应头：第二行开始，格式为key:value形式
	- 响应体：最后一部分。存放响应数据
	
* 状态码说明

| 状态码分类 | 说明 |
|:------:|:--------:|
| 1xx | **响应中**——临时状态码，表示请求已经接收，告诉客户端应该继续请求或者如果它已经完成则忽略它 |
| 2xx | **成功**——表示请求已经被成功接收，处理已完成 |
| 3xx | **重定向**——重定向到其他地方：它让客户端再发起一个请求以完成整个处理 |
| 4xx | **客户端错误**——处理发生错误，责任在客户端，如：客户端的请求一个不存在的资源，客户端未被授权，禁止访问等 |
| 5xx | **服务端错误** ——处理发生错误，责任在服务端，如：服务端抛出异常，路由出错，HTTP版本不支持等 |

## Web服务器
Web服务器是一个应用程序(软件)，对HTTP协议的操作进行封装，使得程序员不必直接对协议进行操作，让Web开发更加便捷。主要功能是"提供网上信息浏览服务"
* Web服务器的作用
	- 封装HTTP协议，简化操作
	- 可以将Web项目部署到服务器中，对外提供网上浏览服务
	
## Servlet
* Servlet是Java提供的一门动态web资源开发技术
* Servlet是JavaEE规范之一，其实就是一个接口，将来我们需要定义Servlet类实现Servlet接口，并由web服务器运行Servlet

### Servlet执行流程
* Servlet由web服务器创建，Servlet方法由web服务器调用

### Servlet生命周期
Servlet运行在Servlet容器(web服务器)中，其生命周期由容器来管理，分为4个阶段：
* 加载和实例化：默认情况下，当Servlet第一次被访问时，由容器创建Servlet对象
	- 通过设置`loadOnStartup = 1`该参数的值可以设置Servlet对象被创建的时间
		+ 负整数：第一次被访问时创建Servlet对象
		+ 0或正整数：服务器启动时创建Servlet对象，数字越小优先级越高
* 初始化：在Servlet实例化之后，容器将调用Servlet的init()初始化这个对象，完成一些如加载配置文件、创建连接等初始化的工作。该方法只**调用一次**
* 请求处理：**每次**请求Servlet时，Servlet容器都会调用Servlet的**service()**方法对请求进行处理
* 服务终止：当需要释放内存或者容器关闭时，容器就会调用Servlet实例的**destroy()**方法完成资源的释放。在destroy()方法调用之后，容器会释放这个Servlet实例，该实例随后会被Java的垃圾收集器所回收

### Servlet urlPattern配置
* urlPattern配置规则
	- 精确匹配
		+ 配置路径`@WebServlet("/user/select")`
		+ 访问路径`localhost:8080/web-demo/user/select`
	- 目录匹配
		+ 配置路径`@WebServlet("/user/*")`
		+ 访问路径`localhost:8080/web-demo/user/aaa`、`localhost:8080/web-demo/user/bbb`
	- 扩展名匹配
		+ 配置路径`@WebServlet("*.do")`
		+ 访问路径`localhost:8080/web-demo/aaa.do`、`localhost:8080/web-demo/bbb.do`
	- 任意匹配(`/*`和'/'的效果一样，但`/*`的优先级更高，会优先拦截请求)(tomcat服务器中有一个默认的servlet，其配置路径是"/",它会访问项目中的静态资源，如果被自己写的Servlet覆盖，就访问不了了，所以请不要覆盖)
		+ 配置路径`@WebServlet("/")`
		+ 访问路径`localhost:8080/web-demo/hehe`、`localhost:8080/web-demo/haha`、`localhost:8080/web-demo/haha/hehe`
* "/"和"/\*"的区别
	- 当我们的项目中的Servlet配置了"/"，会覆盖掉tomcat中的DefaultServlet，当其它的url-pattern都匹配不上时都会走这个Servlet
	- 当我们的项目中配置了"/"，意味着匹配任意访问路径
	
## Request-使用request对象来获取请求数据
* 继承体系
	1. ServletRequest ——> Java提供的请求对象根接口
	2. HttpServletRequest ——> Java提供的对Http协议封装的请求对象接口
	3. RequestFacade ——> Tomcat定义的实现类
* 请求数据
	- 请求行：`GET/request-demo/req1?username=zhangsan&password=123 HTTP/1.1`
		+ `String getMethod()`获取请求方式:`GET`
		+ `String getContextPath()`获取虚拟目录(项目访问路径):`/request-demo`
		+ `StringBuffer getRequestURL()`获取URL(统一资源定位符):`http://localhost:8080/request-demo/req1`
		+ `String getRequestURI()`获取URI(统一资源标识符):`/request-demo/req1`
		+ `String getQueryString()`获取请求参数(GET方式):`username=zhangsan&password=123`
	- 请求头：`User-Agent:Mozilla/5.0 Chrome/91.0.4472.106`
		+ `String getHeader(String name)`根据请求头名称，获取值
	- 请求体：`username=superbaby&password=123`
		+ `ServletputStream getInputStream()`获取字节输入流
		+ `BufferedReader getReader()`获取字符输入流
### Request-通用方式获取请求参数(对get和post方式都一样)
* `Map<String, String[]> getParameterMap()`获取所有参数Map集合
* `String[] getParameterValues(String name)`根据名称获取参数值(数组)
* `String getParameter(String name)`根据名称获取参数值(单个值)

### 解决中文乱码问题
* POST
	- `request.setCharacterEncoding("UTF-8")`
* GET(下面的方法也可以用于POST)
	- 乱码原因：tomcat解码的默认字符集是ISO-8859-1
	- 解决方法
		+ 先对乱码数据进行编码：转为字节数组`byte[] bytes = username.getBytes(StandardCharsets.IOS_8859_1);`
		+ 字节数组解码`username = new String(bytes, StandardCharsets.UTF_8);`
		+ username即还原为中文字符串
### 请求转发-一种在服务器内部的资源跳转方式
* 实现方式:`req.getRequestDispatcher("转发资源路径").forward(req.resp);`
* 请求转发资源间共享数据：使用Request对象
	- `void setAttribute(String name, Object o)`存储数据到request域中
	- `Object getAttribute(String name)`根据key，获取值
	- `void removeAttribute(String name)`根据key，删除该键值对
* 请求转发特点
	- 浏览器地址栏路径不发生变化
	- 只能转发到当前服务器的内部资源
	- 一次请求，可以在转发的资源间使用request共享数据
## Response-使用response对象来设置响应数据
* 继承体系
	1. ServletResponse ——> Java提供的请求对象根接口
	2. HttpServletResponse ——> Java提供的对Http协议封装的请求对象接口
	3. ResponseFacade ——> Tomcat定义的实现类
* 响应数据
	- 响应行：`HTTP/1.1 200 OK`
		+ `void setStatus(int sc);`设置响应状态码
	- 响应头：`Context-Type:text/html`
		+ `void setHeader(String name, String value);`
	- 响应体：`<html><head></head><body></body></html>`
		+ `PrintWriter getWriter()`获取字符输出流
		+ `ServletOutputStream getOutputStream()`获取字节输出流
### Response完成重定向-一种资源跳转方式
* 实现方式:
	- `resp.setStatus(302);`
	- `resp.setHeader("location", "重定向资源路径");`
	- 简化方式完成重定向`resp.sendRedirect("重定向资源路径")`
* 重定向特点
	- 浏览器地址栏路径发生变化
	- 可以重定向到任意位置的资源(服务器内部、外部均可)
	- 两次请求，不能在多个资源使用request共享数据

### Response响应字符数据
* 通过Response对象获取字符输出流`PrintWriter writer = resp.getWriter();`
* 写数据`writer.write("aaa");`

### Response响应字节数据
* 通过Response对象获取字符输出流`ServletOutputStream outputStream = resp.getOutputStream();`
* 写数据`outputStream.write(字节数据);`

## 路径问题
### 明确路径谁用
* 浏览器使用:需要加虚拟目录(项目访问路径)
* 服务端使用:不需要加虚拟目录

## MVC模式和三层架构
### MVC模式-是一种分层开发的模式
* M：Model，业务模型，处理业务
* V：View，视图，界面展示
* C：Controller，控制器，处理请求，调用模型和视图

### 三层架构
* 数据访问层(dao/mapper)：对数据库的CRUD基本操作
* 业务逻辑层(service)：对业务逻辑进行封装，组合数据访问层层中基本功能，形成复杂的业务逻辑功能
* 表现层(web/controller)：接收请求，封装数据，调用业务逻辑层，响应数据

## 会话跟踪技术
* 会话:用户打开浏览器，访问web服务器的资源，会话建立，直到有一方断开连接欸，会话结束。在一次会话中可以包含**多次**请求和响应

* 会话跟踪:一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间**共享数据**H
	- HTTP协议是无状态的，每次浏览器向服务器请求时，服务器都会将该请求视为**新的**请求，因此我们需要会话跟踪技术来实现会话内数据共享

* 实现方式
	- 客户端会话跟踪技术：Cookie
	- 服务端会话跟踪技术：Session

### Cookie
* 客户端会话技术，将数据保存在客户端，以后每次请求都携带Cookie数据进行访问
* Cookie基本使用
	- 创建Cookie对象，设置数据
		+ `Cookie cookie = new Cookie("key", "value");`
	- 发送Cookie到客户端：使用response对象
		+ `response.addCookie(cookie);`
	- 获取客户端携带的所有Cookie，使用request对象
		+ `Cookie[] cookies = request.getCookies();`
	- 遍历数组，获取每一个Cookie对象:for
	- 使用Cookie对象方法获取数据
		+ `cookie.getName();`
		+ `cookie.getValue();`
* Cookie原理
	- Cookie的实现是基于HTTP协议的
		+ 响应头：set-cookie
		+ 请求头：cookie
* Cookie使用细节
	- Cookie存活时间
		+ 默认情况下，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie被销毁
		+ setMaxAge(int seconds)：设置Cookie存活时间
			1. 正数：将Cookie写入浏览器所在电脑的硬盘，持久化存储。到时间自动删除
			2. 负数：默认值，Cookie在当前浏览器内存中，当浏览器关闭，则Cookie被销毁
			3. 零：删除对应Cookie
		+ Cookie存储中文
			* Cookie不能直接存储中文
			* 如需要存储，则需要进行转码：URL编码`URLEncoder.eccode("需要编码的内容", "UTF-8");`
### Session
* 服务端会话跟踪技术：将数据保存到服务端
* JavaEE提供HttpSession接口，来实现一次会话的多次请求间数据共享功能
* Session基本使用
	- 获取Session对象
		+ `HttpSession session = request.getSession();`
	- Session对象功能
		+ `void setAttribute(String name, Object o)`存储数据到session域中
		+ `Object getAttribute(String name)`根据key，获取值
		+ `void removeAttribute(String name)`根据key，删除该键值对
* Session原理
	- Session是基于Cookie实现的
* Session使用细节
	- Session钝化、活化
		+ 服务器重启后，Session中的数据是否还在？
			* 钝化：在服务器正常关闭后，Tomcat会自动将Session数据写入硬盘的文件中
			* 活化：再次启动服务器后，从文件中加载数据到Session中
	- Session销毁
		+ 默认情况下，无操作，30分钟自动销毁(在web.xml中配置)
			* `<session-config>`
			* `	<session-timeout>30</session-timeout>`
			* `</session-config>`
		+ 调用Session对象的invalidate()方法即可自我销毁
		
### Cookie和Session总结
* Cookie和Session都是来完成一次会话内多次请求间数据共享的
* 区别
	- 存储位置：Cookie实践爱那个数据存储在客户端，Session将数据存储在服务端
	- 安全性：Cookie不安全，Session安全
	- 数据大小：Cookie最大3KB，Session无大小限制
	- 存储时间：Cookie可以长期存储，Session默认30分钟
	- 服务器性能：Cookie不占服务器资源，Session占用服务器资源

## Filter
* Filter表示过滤器，是JavaWeb三大组件(Servlet、Filter、Listener)之一
* 过滤器可以把对资源的请求拦截下来，从而实现一些特殊的功能
* 过滤器一般完成一些通用的操作，比如：权限控制、统一编码处理、敏感字符处理等等

### Filter快速入门
* 定义类，实现Filter接，并重写其所有方法
```java
public class FilterDemo implements Filter {
	public void init(FilterConfig filterConfig);
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain);
	public void destroy();
}
```
* 配置Filter拦截资源的路径：在类上定义@WebFilter注解
```java
@WebFilter("/*")
public class FilterDemo implements Filter {
	...
}
```
* 在doFilter方法中输出一句话，并放行
```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
	System.out.println("filter被执行了...");
	//放行
	chain.doFilter(request, response);
}
```

### Filter拦截路径配置
* 拦截具体的资源：/index.jsp：只有访问index.jsp时才会被拦截
* 目录拦截：/user/\*：访问/user下的所有资源，都会被拦截
* 后缀名拦截：\*.jsp：访问后缀名为jsp的资源，都会被拦截
* 拦截所有：/\*：访问所有资源，都会被拦截

### 过滤器链
* 一个Web应用，可以配置多个过滤器，这多个过滤器称为过滤器链。
* 注解配置的Filter，优先级按照过滤器类名(字符串)的自然排序

## Listener
* Listener表示过滤器，是JavaWeb三大组件(Servlet、Filter、Listener)之一
* 监听器可以监听就是在application,session,request三个对象创建、销毁或者往其中添加修改删除属性时自动执行代码的功能组件
* Listener分类：JavaWeb中提供了8个监听器
<table>
	<tr>
		<th>监听器分类</th>
		<th>监听器名称</th>
		<th>作用</th>
	</tr>
	<tr>
		<td rowspan = 2>ServeltConext监听</td>
		<td>ServletContextListener</td>
		<td>用于对ServeltConext对象进行监听(创建、销毁)</td>
	</tr>
	<tr>
		<td>ServletContextAttributeListener</td>
		<td>对ServletContext对象中属性的监听(增删改属性)</td>
	</tr>
	<tr>
		<td rowspan = 4>Session监听</td>
		<td>HttpSessionListener</td>
		<td>对Session对象的整体状态的监听(创建、销毁)</td>
	</tr>
	<tr>
		<td>HttpSessionAttributeListener</td>
		<td>对Session对象中的属性监听(增删改属性)</td>
	</tr>
	<tr>
		<td>HttpSessionBindingListener</td>
		<td>监听对象于Session的绑定和解除</td>
	</tr>
	<tr>
		<td>HttpSessionActiveListener</td>
		<td>对Session数据的钝化和活化的监听</td>
	</tr>
	<tr>
		<td rowspan = 2>Request监听</td>
		<td>ServletRequestListener</td>
		<td>对Request对象进行监听(创建、销毁)</td>
	</tr>
	<tr>
		<td>ServletRequestAttributeListener</td>
		<td>对Request对象中属性的监听(增删改属性)</td>
	</tr>
</table>

## AJAX
* Asynchronous JavaScript And XML -- 异步的JavaScrip和XML
* AJAX作用
	- 与服务器进行数据交换：通过AJAX可以给服务器发送请求，并获取服务器响应的数据
		+ 使用了AJAX和服务器进行通信，就可以使用HTML+AJAX来替换JSP页面了
	- 异步交互：可以在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术，如：搜索联想、用户名是否可用校验，等等...
	

## Axios异步框架
* Axios对原生的AJAX进行封装、简化书写
* 官网:https://www.axios-http.cn

### Axios快速入门
* 引入axios的js文件
	- `<script src="js/axios-0.18.0.js"></script>`
* 使用axios发送请求,并获取响应结果
```JavaScript
axios({
	method:"get",//也可以为post
	url:"http://localhost:8080/brand-case/selectAllServlet"
}).then (function (resp) {
	alert(resp.data);
})
```

## JSON
* JavaScript Object Notation。JavaScript对象表示法
* 由于其语法简单、层次结构鲜明，现多用于数据载体，在网络中进行数据传输
### JSON数据和Java对象转换
* Fastjson是阿里巴巴提供的一个Java语言编写的高性能功能完善的JSON库，是目前Java语言中最快的JSON库，可以实现Java对象和JSON字符串的相互转换
### 使用
* 导入坐标
```xml
<dependency>
	<groupId>com.alibaba</groupId>
	<artfactId>fastjson</artfactId>
	<version>1.2.62</version>
</dependency>
```
* Java对象转JSON
	- `String jsonStr = JSON.toJSONString(obj);`
* JSON字符串转Java对象
	- `User user = JSON.parseObject(jsonStr, User.class);`
	
## Vue
* Vue是一套前端框架，免除原生JavaScript中的DOM操作，简化书写
* 基于MVVM（Model-View-ViewModel）思想，实现数据的双向绑定，将编程的关注点放在数据上
* 官网：https://cn.vuejs.org
### Vue常用指令
* HTML标签上带有v-前缀的特殊属性，不同指令具有不同含义，
* 常用指令
<table>
	<tr>
		<th>指令</th>
		<th>作用</th>
	</tr>
	<tr>
		<td>v-bind</td>
		<td>为HTML标签绑定属性值，如设置href、css样式等</td>
	</tr>
	<tr>
		<td>v-model</td>
		<td>在表单元素上创建双向数据绑定</td>
	</tr>
	<tr>
		<td>v-on</td>
		<td>为HTML标签绑定事件</td>
	</tr>
	<tr>
		<td>v-if</td>
		<td rowspan = 3>条件性的渲染某元素，判定为true时渲染，否则不渲染</td>
	</tr>
	<tr>
		<td>v-else</td>
	</tr>
		<tr>
		<td>v-else-if</td>
	</tr>
	<tr>
		<td>v-show</td>
		<td>根据条件展示某元素，区别在于切换的是display属性的值</td>
	</tr>
	<tr>
		<td>v-for</td>
		<td>列表渲染，遍历容器的元素或者对象的属性</td>
	</tr>
</table>

### Vue生命周期
* 生命周期的八个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法（钩子）

| 状态 | 阶段周期 |
|:-----:|:-------:|
| beforeCreate | 创建前 |
| created | 创建后 |
| beforeMount | 载入前 |
| mounted | 挂在完成 |
| beforeUpdate | 更新前 |
| update | 更新后 |
| beforeDestroy | 销毁前 |
| destroeyed | 销毁后 |

* mounted：挂在完成，Vue初始化成功，HTML页面渲染成功
	- 发送异步请求，加载数据
	

## Element
* Element:是饿了么公司前端开发团队提供的一套基于Vue的网站组件库，用于快速组件网站
* 组件：组成网页的部件，例如超链接、按钮、图片、表格等等
* Element官网：https://element.eleme.cn/#/zh-CNListener

### Element快速入门
* 引入Element的css、js文件和Vue.js
* 创建Vue核心对象
* 官网复制组件代码

### Element布局
* Layout布局：通过基础的24分栏，迅速简便地创建布局
* Container布局：用于布局的容器组件，方便快速搭建页面的基本结构
	




​		



