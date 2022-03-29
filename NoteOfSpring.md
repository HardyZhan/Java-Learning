### Spring5框架
### IOC容器
* 控制反转，把对象创建和对象之间的调用过程，交给Spring进行管理。
* 使用IOC目的:降低耦合度
#### IOC底层原理
* xml解析
* 工厂模式
* 反射
#### IOC接口(BeanFactory)
Spring提供IOC容器实现两种方式:
* BeanFactory:IOC容器基本实现方式，是Spring内部的使用接口，一般不提供给开发人员使用。
  <br> ***注意***:此方式在加载配置文件时不会创建对象，在获取对象(使用)时才会去创建对象。
* ApplicationContext:BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员进行使用。
  <br> ***注意***:此方式在加载配置文件时就会把在配置文件对象进行创建。
  
ApplicationContext接口有两个实现类:
* FileSystemXmlApplicationContext:此实现类需要输入绝对路径。
* ClassPathXmlApplicationContext:此实现类只需要输入src文件夹下的相对路径即可。
#### IOC操作Bean管理(基于xml)
Bean管理指的是两个操作:
* Spring创建对象(基于xml)。
  
`<bean id="user" class="com.atguigu.spring5.User></bean>`
  <br> (1) 在spring配置文件中，使用bean标签，标签里面添加对应属性，就可以实现对象创建。
  <br> (2) 在bean标签中有很多属性，常用属性有:
  <br> * id属性：唯一标识。
  <br> * class属性：类全路径(包类属性)。
* Spring注入属性(基于xml)。
  <br> (1) DI:依赖注入，就是注入属性。
  <br> * 第一种方式就是使用set方法注入属性。
  ```xml
  <bean id="book" class="com.atguigu.spring5.Book">
  <!--使用property完成属性注入
      name:类里面属性名称
      value:向里面注入的值
  -->
    <property name="bname" value="易筋经"></property>
    <property name="bauthor" value="达摩老祖"></property>
  </bean>
  ```
  <br> * 第二种方式就是通过有参构造注入属性。
  
  ```xml
  <bean id="orders" class="com.atguigu.spring5.Orders">
    <constructor-arg name="oname" value="电脑"></constructor-arg>
    <constructor-arg name="address" value="China"></constructor-arg>
    <!--<constructor-arg index="0" value=""></constructor-arg>
        这种写法index代表构造函数中的第几个参数-->
  </bean>
  ```
  <br> (2) 



##### IOC操作Bean管理(FactoryBean)
###### Spring有两种类型的bean
* 普通bean：在配置文件中定义bean类型就是返回类型
* 工厂bean：在配置文件中定义bean类型可以和返回类型不一样

##### IOC操作Bean管理(bean作用域)
* 在Spring里面，设置创建bean实例是单例还是多实例
* 在Spring里面，默认情况下，bean是单例
* 如何设置是单实例还是多实例？
<br> 在spring配置文件bean标签里面有属性(scope)用于设置单实例还是多实例
  <br> (1) 默认值，singleton，表示是单实例对象
  <br> (2) prototype，表示是多实例对象
  <br> (3) request
  <br> (4) session
  
* singleton和prototype区别
<br>除了分别表示单实例和多实例的区别，在设置scope值是singleton的时候，加载spring配置文件时就会创建单实例对象，而设置为prototype时，则是在调用getBean方法的时候创建多实例对象。

##### IOC操作Bean管理(bean生命周期)
* 生命周期
<br> 从对象创建到对象销毁的过程
  
* bean生命周期
  <br> (1) 通过构造器创建bean实例(无参构造)
  <br> (2) 为bean的属性设置值和对其它bean引用(调用set方法)
  <br> (3) 调用bean的初始化的方法。
  <br> (4) bean可以使用了(对象获取到了)
  <br> (5) 当容器关闭的时候，调用bean的销毁的方法(需要进行配置销毁的方法)
  
* 加上bean的后置处理器后bean的生命周期有七步
  <br> (1) 通过构造器创建bean实例(无参构造)
  <br> (2) 为bean的属性设置值和对其它bean引用(调用set方法)
  <br> ***(3) 把bean实例传递bean后置处理器的方法***
  <br> (4) 调用bean的初始化的方法。
  <br> ***(5) 把bean实例传递bean后置处理器的方法***
  <br> (6) bean可以使用了(对象获取到了)
  <br> (7) 当容器关闭的时候，调用bean的销毁的方法(需要进行配置销毁的方法)

##### IOC操作Bean管理(xml自动装配)
* 什么是自动装配
<br> 根据指定装配原则(属性名称或属性类型)，Spring自动将匹配的属性值注入
  
* bean标签属性autowire属性常用两个值:
  <br> (1)byName根据名称注入，注入值bean的id值和类属性名称要一样
  <br> (2)byType根据属性类型注入

##### IOC操作Bean管理(外部属性文件)
* 直接配置数据库信息
<br> 配置德鲁伊连接池
  
* 引入外部属性文件配置数据库连接池
<br> (1) 创建外部属性文件,properties格式文件，写数据库信息
<br> (2) 把外部properti属性文件引入到spring配置文件中
  <br> 首先引入context名称空间
  <br> 然后在spring配置文件使用标签引入外部属性文件

#### IOC操作Bean管理(基于注解方式)
* 什么是注解？
  <br> (1) 注解是代码中的特殊标记，格式:@注解名称(属性名称=属性值,属性名称=属性值...)
  <br> (2) 使用注解:注解作用在类上面，方法上面，属性上面
  <br> (3) 使用注解目的:简化xml配置
  
* Spring针对Bean管理中创建对象实例提供注解
  <br> (1) @Component
  <br> (2) @Service
  <br> (3) @controller
  <br> (4) @repository
  
* 基于注解方式实现对象创建
  <br> (1) 引入依赖
  <br> (2) 开启组件扫描,如果扫多个包，多个包使用逗号隔开，或者扫描多个包所在的上层目录
  
* 创建类，在类上添加
* 基于注解方式实现属性注入
  <br> (1) @AutoWired:根据属性类型进行自动装配
  <br> (2) @Qualifier:根据属性名称进行注入,需要和@AutoWired一起使用
  <br> (3) @Resource:可以根据类型注入，也可以根据名称注入
  <br> (4) @Value:注入普通属性类型
  
* 完全注解开发
<br> (1) 创建配置类，替代xml配置文件
  <br> (2) 编写测试类
  
### AOP
#### AOP中的基本概念
* 什么是AOP？
<br> (1) 面向切面编程，利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各个部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。
  <br> (2) 通俗描述:不通过修改源代码方式，在主干功能里面添加新功能
  
#### AOP底层原理
* AOP底层使用动态代理
<br> (1) 有接口情况，使用JDK动态代理。创建接口实现代理对象，增强类的实现方法。
  <br> (2) 没有接口的情况，是欧诺个CGLIB动态代理。创建子类的代理对象，增强类的方法。
  
#### AOP(术语)
* 连接点
  <br> 类里面哪些方法可以被增强，这些方法称为连接点。
* 切入点
  <br> 实际被真正增强的方法，称为切入点。
* 通知(增强)
  <br> (1) 实际增强的逻辑部分称为通知(增强)
  <br> (2) 通知有多种类型:前置通知、后置通知、环绕通知、异常通知、最终通知
* 切面
  <br> 是动作，把通知应用到切入点的过程称为切面
  
#### AOP操作(准备)
Spring框架一般都是基于AspectJ实现AOP操作
* AspectJ不是Spring组成部分，独立AOP框架，一般把AspectJ和Spring框架一起使用，进行AOP操作。
* 基于AspectJ实现AOP操作
  <br> (1) 基于xml配置文件实现
  <br> (2) 基于注解方式实现(使用)
  
* 在项目工程里面引入AOP相关依赖
* 切入点表达式
<br> (1) 切入点表达式作用:知道对哪个类里面的哪个方法进行增强
  <br> (2) 语法结构:`execution([权限修饰符][返回类型][类全路径][方法名称]([参数列表]))`
  
#### AOP操作(AspectJ注解)
* 创建类，在类里面定义方法。
* 创建增强类(编写增强逻辑)。
  <br> (1) 在增强类里面，创建方法，让不同方法代表不同通知类型。
  
* 进行通知的配置
  <br> (1) 在spring配置文件中，开启注解扫描
  <br> (2) 使用注解创建User和UserProxy对象
  <br> (3) 在增强类上面添加注解@Aspect
  <br> (4) 在spring配置文件中开启生成代理对象
  
* 配置不同类型的通知
  <br> 在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置
  
* 相同切入点抽取
* 有多个增强类对同一个方法进行增强，设置增强类优先级
  <br> (1) 在增强类上添加注解@Order(数字类型值)，数字类型的值越小优先级越高。
  
* 完全使用注解开发
  <br> (1) 创建配置类，不需要创建xml配置文件
  
#### AOP操作(AspectJ配置文件)
* 创建两个类，增强类和被增强类，创建方法
* 在spring配置文件中创建两个类对象
* 在spring配置文件中配置切入点

### JDBCTemplate(概念和准备)
#### 什么是JdbcTemplate
* Spring框架对JDBC进行封装，使用JdbcTemplate方便实现对数据库操作
#### 准备工作
* 引入相关jar包
* 在spring配置文件中配置数据库连接池
* 配置JdbcTemplate对象，注入DataSource
* 创建service类，创建dao类，在dao注入jdbcTemplate对象

#### JdbcTemplate数据库操作
* 对应数据库创建实体类
* 编写service和dao
  <br> (1) 在dao进行数据库添加操作
  <br> (2) 调用JdbcTemplate对象里面update方法实现添加操作。有两个参数，第一个参数：sql语句；第二个参数：可变参数，设置sql语句的值。
  

### 事务
#### 什么是事务
* 事务是数据库操作最基本的单元，逻辑上的一组操作，要么都成功，如果有一个失败则所有操作都失败。
* 典型场景:银行转账

#### 事务四个特性(ACID)
* 原子性，不可分割
* 一致性，操作之前和操作之后的总量是不变的
* 隔离性，多事务操作时，互相不产生影响
* 持久性，事务提交之后，表中数据真正发生变化

#### 事务操作(搭建事务操作环境)
* Web层(视图层)
* Service层(业务逻辑层)
* DAO层(数据访问层)

#### 事务操作(Spring事务管理介绍)
* 事务添加到JavaEE三层结构里面Service层(业务逻辑层)
* 在Spring进行事务管理操作
  <br> (1) 编程式事务管理
  <br> (2) 声明式事务管理(一般使用这种方式)
  
* 声明式事务管理
  <br> (1) 基于注解方式(一般使用这种方式)
  <br> (2) 基于xml配置文件方式
  
* 在Spring进行声明式事务管理，底层使用AOP

* Spring事务管理API
  <br> (1) 提供一个接口PlatformTransactionManager，代表事务管理器，这个接口针对不同的框架提供不同的实现类
  
#### 事务操作(注解声明式事务管理)
* 在spring配置文件配置事务管理器
* 在spring配置文件，开启事务注解
  <br> (1) 在spring配置文件引入名称空间tx
  <br> (2) 开启事务注解
  
* 在Service类上面(或者获取Service类里面方法的上面)添加事务注解
  <br> (1) @Transactional,这个注解添加到类上面，也可以添加方法上面
  <br> (2) 若添加到类上面，这个类里面所有的方法都添加事务
  <br> (3) 若添加方法上面，则为这个方法添加事务
  
#### 事务操作(声明式事务管理参数配置)
* 在Service类上面添加注解@Transactional，在这个注解里面可以配置事务相关参数
* propagation:事务传播行为
  <br> (1) 概念：多事务方法直接进行调用，这个过程中事务是如何进行管理的
  <br> (2) 事务传播行为有7种
  <br> (a) REQUIRED:如果有事务在运行，当前的方法就在这个事务内运行，否则，就启动一个新的事务，并在自己的事务内运行。
  <br> (b) REQUIRES_NEW:当前的方法必须启动新事务，并在它自己的事务内运行，如果有事务正在运行，应该将它挂起。
  <br> (c) SUPPORTS:如果有事务在运行，当前的方法就在这个事物内运行，否则它可以不运行在事务中。
  <br> (d) NOT_SUPPORTED:当前的方法不应该运行在事务中，如果有运行的事务，将它挂起
  <br> (e) MANDATORY:当前的方法必须运行在事务内部，如果没有正在运行的事务，就抛出异常
  <br> (f) NEVER:当前的方法不应该运行在事务内部，如果有运行的事务，就抛出异常
  <br> (g) NESTED:如果有事务在运行，当前的方法就应该在这个事务的嵌套事务内运行，否则，就启动一个新的事务，并在它自己的事务内运行
* isolation:事务隔离级别
  <br> (1) 事务有特性称为隔离性，即多事务操作之间不会产生影响。
  <br> (2) 有三个读问题:脏读、不可重复读、幻读
  <br> (3) 脏读:一个未提交事务读取到另一个已修改但未提交事务的数据
  <br> (4) 不可重复读:一个未提交事务读取到另一提交事务修改数据，即在另一事务修改提交前读取和修改提交后读取的数据不同
  <br> (5) 幻读:一个未提交事务读取到另一个提交事务添加数据
  <br> (6) 通过设置事务隔离级别，解决读问题
  
| | 脏读 | 不可重复读 | 幻读 | 
|:----:|:------:|:-------:|:-------:|
| READ UNCOMMITTED | 有 | 有 | 有 |
| READ COMMITTED | 无 | 有 | 有 |
| REPEATABLE READ | 无 | 无 | 有 |
| SERIALIZABLE | 无 | 无 | 无 |
  
* timeout:超时时间
  <br> (1) 事务需要在一定时间内进行提交，如果不提交则事务进行回滚
  <br> (2) 默认值是-1，即不超时，设置时间以秒单位进行计算
* readOnly:是否只读
  <br> (1) 读:查询操作， 写:添加修改删除操作
  <br> (2) 默认值false，即表示可以查询，也可以进行添加修改删除操作
  <br> (3) 设置为true后，只能查询
* rollbackFor:回滚
  <br> 设置出现哪些异常进行事务回滚
* noRollbackFor:不回滚
  <br> 设置出现哪些异常不进行事务回滚
  
#### 事务操作(xml声明式事务管理)
* 在spring配置文件中进行参数配置
  <br> (1) 配置事务管理器
  <br> (2) 配置通知(即增强的那部分)
  <br> (3) 配置切入点和切面(即哪个类的哪个方法上)
  
#### 事务操作(完全注解声明式事务管理)
* 创建配置类，使用配置类替代xml配置文件



  

  
