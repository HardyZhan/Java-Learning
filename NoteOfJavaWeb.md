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


		



