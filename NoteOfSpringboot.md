# SpringBoot实战派
##### 默认大于配置
### 一、MAVEN
#### POM(Project Object Model,项目对象模型)
它是Maven工程的基本工作单元，也是Maven的核心。它是一个XML文件，包含项目的基本信息，用于描述项目如何构建、声明项目依赖等。POM中通常有以下元素:

* ***dependencies***
  在此元素下添加依赖，它可以包含多个<dependency>依赖。
  
* ***dependency***
  <dependency>`与`</dependency>`之间有3个标识，分别如下:
  - groupId:定义隶属的实际项目，坐标元素之一。
  - artifactId:定义项目中的一个模块，坐标元素之一。
  - version:依赖或项目的版本，坐标元素之一。
  
* ***scope***
  - 如果有一个在编译时需要而发布时不需要的JAR包，则可以用scope标签标记该包，并将其值设为provided。
  - scope标签的参数
| 参数 | 描述 |
|:---:|:-------|
| compile | scope的默认值，表示被依赖项目需要参与当前项目的编译、测试、运行阶段，是一个比较强的依赖。打包时也要包含进去 |
| provided | provided表示打包时可以不用打包进去，Web Container会提供。该依赖理论上可以参与编译、测试、运行等周期 |
| runtime | 表示dependency不作用在编译阶段，但会作用在运行和测试阶段，如JDBC驱动适合运行和测试阶段 |
| system | 和provided相似，但是在系统中要以外部JAR包的形式提供，Maven不会在repository中查找它 |
| test | 表示dependency作用在测试阶段，不作用在运行阶段。只在测试阶段使用，用于编译和运行测试代码。不会随项目发布 |

* ***properties***
  如果要使用自定义的变量，则可以在`<properties></properties>`元素中进行变量的定义，然后在其它节点中引用该变量。它的好处是：在依赖配置时引用变量，可以达到统一版本号的目的。
  
* ***plugin***
  在创建Spring Boot项目时，默认提供了spring-boot-maven-plugin插件。它提供打包时需要的信息，将Spring Boot应用打包为可执行的JAR或WAR文件。
  
* ***完整的pom.xml文件***

### 二、Lombok注解简介
* @Data:自动生成Getter/Setter、toString、equals、hashCode方法，以及不带参数的构造方法。
* @NonNull:帮助处理NullPointerException。
* @CleanUp:自动管理资源，不用再在finally中添加资源的close方法。
* @Setter/@Getter:自动生成Getter/Setter方法。
* @ToString:自动生成toString方法。
* @EqualsAndHashcode:从对象的字段中重写hashCode和equals方法。
* @NoArgsConstructor/@RequiredArgsonstructor/@AllArgsConstructor:自动生成构造方法。
* @Value:用于注解final类。
* @Builder:产生复杂的构建器API类。
* @SneakyThrows:用于处理异常。
* @Synchronized:同步方法的转化。
* @Log:支持使用各种日志(logger)对象。只要在使用时，用对应的注解进行标注。比如使用Log4j作为日志库，则在需要加入日志的位置写上注解@Log4j即可。

### 三、MVC模式
MVC是Model(模式)、View(视图)、Controller(控制器)的简写。

#### Model

* 是JAVA的实体Bean，代表存取数据的对象或POJO(Plain Ordinary Java Objects，简单的Java对象)，也可以带有逻辑。其作用是在内存中暂时存储数据，并在数据变化时更新控制器(如果要持久化，则需要把它写入数据库或者磁盘文件中)。

#### View

* 主要用来解析，处理、显示内容，并进行模板渲染。

#### Controller

* 主要用来处理视图中的响应。它决定如何调用Model(模型)的实体Bean、如何调用业务层的数据增加、删除、修改和查询等业务操作，以及如何将结果返给视图进行渲染。建议在控制器中尽量不让业务逻辑代码。

#### 整个工程流程

(1) 客户端(用户)发出的请求有Tomcat(服务器)接收，然后Tomcat将请求转交给DispatcherServlet处理。

(2) DispatcherServlet匹配控制器中配置的映射路径，进行下一步处理。

(3) ViewResolver将ModelAndView或Exception解析成View。然后View会调用render()方法，并根据ModelAndView中的数据渲染出页面。
### 四、三层架构
三层架构，就是将整个应用程序划分为表现层(UI)、业务逻辑层(Service)、数据访问层(DAO/Repository)。

#### 表现层

* 用于展示界面。主要对用户的请求进行接受，以及进行数据的返回。它为客户端(用户)提供应用程序的访问接口(界面)。

#### 业务逻辑层

* 是三层架构的服务层，负责业务逻辑处理，主要是调用DAO层对数据进行增加、删除、修改和查询等操作。

#### 数据访问层

* 与数据库进行交互的持久层，被Service调用。在Spring Data JPA中Hibernate来实现。

### 五、Spring MVC控制器中常使用的注解
#### @Controller

- @Controller标记在类上。使用@Controller标记的类表示是Spring MVC的Controller对象。分发处理器将会扫描使用了该注解的类，并检测其中的方法是否使用了注解@RequestMapping。注解@Controller只是定义了一个控制器类，使用了注解@RequestMapping的方法才是真正处理请求的处理器，完成映射关系。

#### @RestController

- @RestController是Spring 4.0之后才有的注解。它等价于原来的注解@Controller加上注解@ResonseBody的功能，直接返回字符串。用它来标注Rest风格的控制器类。

#### @RequestMapping

- 它用来处理请求地址映射的注解，可用在类或方法上。如果用在类上，则表示类中的所有响应请求的方法都以该地址作为父路径。
- RequestMapping注解有6个属性
  + value:指定请求的地址。
  + method:指定请求的method类型——GET、HEAD、POST、PUT、PATCH、DELETE、OPTIONS、TRACE。
  + consumes:消费消息，指定处理请求的提交内容类型(Content-Type)，例如application/json、text/html。
  + produces:生产消息，指定返回的内容类型。仅当request请求头中的Accept类型中包含该指定类型时才返回。
  + params:指定request中必须包含某些参数值才让该方法处理请求。
  + headers:指定request中必须包含某些指定的header值才能让该方法处理请求。

* @PathVariable
  - 将请求URL中的模板变量映射到功能处理方法的参数上，即获取URL中的变量作为参数。

### 六、处理HTTP请求的方法
RequestMapping的method类型有GET、HEAD、POST、PUT、PATCH、DELETE、OPTIONS、TRACE。可以通过这些method来处理前端用不同方法提交的数据。

#### GET

- 用GET方法可以获取资源。

#### DELETE

- 如果需要删除一个数据，根据Restful风格则需要使用DELETE方法。在使用DELETE方法删除资源时，要注意判断是否成功，因为返回的是VOID方法。一般有以下三个方法进行判断:
  - 使用try catch exception:如果不发生异常，则默认为成功，但是这样并不好。
  - 通过存储过程返回值来判断是否正确执行:如果执行成功，则返回1或大于0的值；如果执行失败，则返回0；
  - 在执行DELETE方法前先查询是否有数据:在执行DELETE方法后返回值是0，所以一般先查询一下是否有数据。

#### POST

- 如果需要添加对象，那一般使用POST方法传递一个Model对象。

tips:GET和POST的区别

* GET在浏览器中可以回退，而POST访问同一个地址时也是再次提交请求。
* GET请求会被浏览器主动缓存，而POST不会。
* GET中的参数会被完整地保留在浏览器历史记录里，而POST中的参数则不会被保留。
* GET只能进行URL编码，而POST支持多种编码方式。
* GET只接收ASCII字符，而POST没有限制。
* GET的安全性相比POST低，因为参数直接暴露在URL上，所以不能用它传递敏感信息。
* GET的参数是通过URL传递的，而POST的参数是放在request body中的。

#### PUT

* 如果对象需要更新，则用PUT方法发送请求。

#### PATCH

* PATCH是一个新引入的方法，是对PUT方法的补充，用来对已知资源进行局部更新。

#### OPTIONS

* 该方法用于获取当前URL。若请求成功，则会在HTTP头中包含一个名为"Allow"的头，其值是所支持的方法，如值为“GET,POST”。它还允许客户端查看服务器的性能。如果遇到“500错误”，则OPTIONS不进行第二次请求。

#### TRACE

* 它显示服务器收到的请求，主要用于测试或诊断。

### 七、Spring AOP
#### 1. 什么是AOP
AOP(Aspect Oriented Program,面向切面编程)把业务功能分为核心、非核心两部分。
* 核心业务功能:用户登录、增加数据、删除数据。
* 非核心业务:性能统计、日志、事务管理。

在Spring的面向切面编程(AOP)思想里，非核心业务功能被定义为切面。核心业务功能和切面功能先被分别进行独立开发，然后把切面功能和核心业务功能”编织“在一起，这就是AOP。

#### 2. AOP中的概念
* 切入点(pointcut):在哪些类、哪些方法上切入。
* 通知(advice):在方法前、方法后、方法前后做什么。
* 切面(aspect):切面 = 切入点 + 通知。即在什么时机、什么地方、做什么。
* 织入(weaving):把切面加入对象，并创建出代理对象的过程。
* 环绕通知:AOP中最强大、灵活的通知，它集成了前置和后置通知，保留了连接点原有的方法。

### 八、IoC容器
* IoC(Inversion of Control)容器，是面向对象编程中的一种设计原则，意为控制反转(也被称为“控制反向”或“控制倒置”)。它将程序中创建的对象的控制权交给Spring框架来管理，以便降低计算机代码之间的耦合度。

  * 控制反转的实质是获得依赖对象的过程被反转了。这个过程由自身管理变为IoC容器主动注入。这正是IoC实现的方式之一:依赖注入(dependency injection, DI),由IoC容器在运行期间动态地将某种依赖关系注入对象之中。
  
* IoC容器的实现方法主要有两种——依赖注入与依赖查找

  - 依赖注入

    IoC容器通过类型或名称等信息将不同对象注入不同属性中。组件不做定位查询，只提供普通的Java方法让容器去决定依赖关系。这是最流行的IoC方法。依赖注入主要有以下几种方法。

    * 设值注入(setter injection): 让IoC容器调用注入所依赖类型的对象。
    * 接口注入(interface injection): 实现特定接口，以供IoC容器注入所依赖类型的对象。
    * 构造注入(constructor): 实现特定参数的构造函数，在创建对象时让Ioc容器注入所依赖类型的对象。
    * 基于注解: 通过Java的注解机制让IoC容器注入所依赖类型的对象，例如，使用@Autowired。
      ***IoC是通过第三方容器来管理并维护这些被依赖对象的，应用程序只需要接收并使用IoC容器注入的对象。***

  - 依赖查找
    依赖查找则通过调用容器提供的回调接口和上下文环境来获取对象，在获取时需要提供相关的配置文件路径、key等信息来确定获取对象的状态。依赖查找通常有两个方法——依赖拖曳(DP)和上下文依赖查找(CDL)。
### 九、Servlet容器
Servlet是在javax.servlet包中定义的一个接口。在开发Spring Boot应用程序时，使用Controller基本能解决大部分的功能需求。但有时也需要使用Servlet，比如实现拦截和监听功能。