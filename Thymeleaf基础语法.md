# Thymeleaf基础语法

## 一、引用命名空间

要使用Thymeleaf，则需要先加入依赖，然后在模板文件中引用命名空间如下：

`<html lang="zh" xmlns:th="http://www.thymeleaf.org">`

## 二、常用th标签

### 1. th:text

`<div th:text="${name}">name</div>`

​	它用于显示控制器传入的name值

​	如果name不存在，要显示默认值，则使用一下代码

`<span th:text="${name}?:'默认值'"></span>`

### 2. th:object

​	它用于接收后台传过来的对象，如以下代码：

`<th:obejct="${user}">`

### 3. th:action

​	它用来指定表单提交地址

`<form th:action="@{/article}+${article.id}" method="post"></form>`

### 4. th:value

​	它用对象将id的值替换为value的属性

`<input type="text" th:value="${article.id}" name="id" />`

### 5. th:field

​	它用来绑定后台对象和表单数据。Thymeleaf里的“th:field”等同于“th:name”和“th:value”，其具体使用方法见以下代码

`<input type="text" id="title" name="title" th:field="${article.title}" />`

`<input type="text" id="title" name="title" th:field="*{title}" />`

## 三、Thymeleaf中的URL写法

​	Thymeleaf是通过语法@{...}来处理URL的，需要使用“th:href”和“th:src”等属性，如以下代码

`<a th:href="@{http://eg.com}/">绝对路径</a>`

`<a th:href="@{/}">相对路径</a>`

`<a th:href="@{css/bootstrap.min.css}/">默认访问static下的css文件夹</a>`

## 四、用Thymeleaf进行条件求值

​	Thymeleaf通过“th:if”和“th:unless”属性进行条件判断。在下面的例子中，`<a>`标签只有在“th:if”中的条件成立时才显示。

`<a th:href="@{/login}" th:if=${session.user == null}>Login</a>`

​	“th:unless”与“th:if”恰好相反——只有当表达式中的条件不成立时才显示其内容。在下方代码中，如果用户session为空，则不显示登录链接

`<a th:href="@{/login}" th:unless=${session.user == null}>Login</a>`

## 五、Switch

​	Thymeleaf支持Switch结构，如以下代码

```html
<div th:switch="${user.role}">
    <p th:case="admin">管理员</p>
    <p th:case="vip">vip会员</p>
    <p th:case="*">普通会员</p>
</div>
```

​	上述代码的意思是：如果用户角色(role)是admin，则显示“管理员”；如果用户角色是vip，则显示“vip会员”；如果都不是，则显示“普通会员”，即使用“*”表示默认情况。

## 六、Thymeleaf中的字符串替换

​	有时需要对文字中的某一处地方进行替换，可以通过字符串拼接操作完成，如以下代码：

`<span th:text="'欢迎您，' + ${name} + '!'"></span>`

​	或，

``<span th:text="|欢迎您，${name}!|"></span>``

​	上面的第2种形式限制比较多，|...|中只能包含变量表达式${...}，不能包含其它常量、条件表达式等

## 七、Thymeleaf的运算符

### 1. 算数运算符

​	如果要在模板中进行算数运算，则可以用下面的写法。以下代码表示求加和取余运算

```html
<span th:text="1+3">1 + 3</span><br/>
<span th:text="9%2">9 % 2</span><br/>
```

### 2. 条件运算符th:if

​	下方代码演示了if判断，表示：如果从控制器传来的role值等于"admin"，则显示”欢迎您，管理员“；如果role值等于”vip“，则显示”欢迎您，vip会员“

```html
<div th:if="${role} eq admin">
    <span>欢迎您，管理员</span>
</div>
<div th:if="${role} eq vip">
    <span>欢迎您，vip会员</span>
</div>
```

​	eq是判断表达式，代表等于。其它的判断表达式如下

* gt：大于
* ge：大于或等于
* eq：等于
* lt：小于
* le：小于或等于
* ne：不等于

### 3. 判断空值

* 判断不为空：

`<sapn th:if="${name} != null">不为空</span>`

* 判断为空

`<sapn th:if="${name} == null">为空</span>`

## 八、Thymeleaf公用对象

​	Thymeleaf还提供了一系列公用（utility）对象，可以通过”#“直接访问，如以下用法：

* 格式化时间

`<td th:text="${#date.format(item.createTime,'yyyy-MM-dd HH:mm:ss')}">格式化时间</td>`

* 判断是不是空字符串

`<span th:if="${#strings.isEmpty(name)}">空的</span>`

* 是否包含（分大小写）

`<span th:if="${#strings.contains(name,'long')}">包含long</span>`