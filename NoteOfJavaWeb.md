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
* 问题:
	- 恒等式
	- &lt;where&gt; 替换 where 关键字