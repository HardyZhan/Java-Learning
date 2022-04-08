# SpringMVC
## 三层架构
* 表述层(表示层)，表述层表示前台页面和后台servlet
* 业务逻辑层
* 数据访问层

## 转发和重定向区别
* 转发是一次请求，第一次是浏览器请求，第二次是发生在服务器内部，转发可以获取请求域中的数据，不可以跨域，只能访问服务器内部的资源
* 重定向是两次请求，第一次访问servlet，第二次访问重定向所指地址，重定向不可以获取请求域中的请求，可以跨域

## HttpMessageConverter
* HttpMessageConverter,报文信息转换器，将请求报文转换为Java对象，或将Java对象转换为响应报文
* HttpMessageConverter提供了两个注解和两个类型：@RequestBody，@ResponseBody,RequestEntity,ResponseEntity.

### @RequestBody
* @RequestBody可以获取请求体，需要在控制器方法设置一个形参，使用@RequestBody进行标识，当前请求的请求体就会为当前注解所标识的形参赋值

### @RespnseBody
* 用于标识一个控制器方法，可以将该方法的返回值直接作为响应报文的响应体响应到浏览器
#### springMVC处理json

@ResponseBody处理json的步骤

* 导入jackson的依赖
```xml
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
	<version>2.12.1</version>
</dependency>
```

* 在SpringMVC的核心配置文件中开启mvc的注解驱动，此时在HandlerAdapter中会自动装配一个消息转换器：MappingJackson2HttpMessageConverter，可以将响应到浏览器的java对象转换为json格式的字符串
	- `<mvc:annotation-driven />`

* 在处理器方法上使用@ResponseBody注解进行标识
* 将Java对象直接作为控制器方法的返回值返回，就会自动转换为json格式的字符串

### @RestController
* 是springMVC提供的一个复合注解，标识在控制器的类上，就相当于为类添加了@Controller注解，并且为其中的每个方法添加了@ResponseBody注解

## 文件的上传和下载
### 文件下载
* 使用ResponseEntity实现下载文件的功能
```java
@RequestMapping("/testDown")
public ResponseEntity<byte[]> testResonseEntity(HttpSession session) throws IOException {
	// 获取ServletContext对象
	ServletContext servletContext = session.getServletContext();
	// 获取服务器中文件的真实路径
	String realPath = servletContext.getRealPath("/static/img/1.jpg");
	// 创建输入流
	InputStream is = new FileInputStream(realPath);
	// 创建字节数组
	byte[] bytes = new byte[is.available()];
	// 将流读到字节数组中
	is.read(bytes);
	// 创建HttpHeaders对象设置响应头信息
	MultivalueMap<String, String> headers = new HttpHeaders();
	// 设置要下载方式以及下载文件的名字
	headers.add("Content-Disposition", "attachment;filename=1.jpg");
	// 设置响应状态码
	HttpStatus statusCode = HttpStatus.OK;
	// 创建ResponseEntity对象
	ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, statusCode);
	// 关闭输入流
	is.close();
	return responseEntity;
}
```
### 文件上传
* 文件上传要求form表单的请求方式必须为post，并且添加属性`enctype="multipart/form-data"`
* SpringMVC中将上传的文件封装到MultipartFile对象中，通过此对象可以获取文件相关信息
#### 上传步骤
1. 添加依赖
```xml
<dependency>
	<groupId>commons-fileupload</groupId>
	<artifactId>commons-fileupload</artifactId>
	<version>1.3.1</version>
</dependency>
```
2. 在SpringMVC的配置文件中添加配置
```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
```
3. 编写控制器代码
```java
public String testUp(MultipartFile photo, HttpSession session) {
	// 获取上传的文件的文件名
	String fileName = photo.getOriginalFilename();
	// 获取上传文件的后缀名
	String suffixName = fileName.substring(fileName.lastIndexOf("."));
	// 将UUID作为文件名
	String uuid = UUID.randomUUID().toString();
	// 将uuid和后缀名拼接后的结果作为最终的文件名
	fileName = uuid + suffixName;
	// 通过ServletContext获取服务器中photo目录的路径
	ServletContext servletContext = session.getServletContext();
	String photoPath = servletContext.getRealPath("photo");
	File file = new File(photoPath);
	// 判断photoPath所对应路径是否存在
	if (!file.exits()) {
		// 若不存在，则创建目录
		file.mkdir();
	}
	String finalPath = photoPath + File.separator + fileName;
	photo.transferTo(new File(finalPath));
	return "success";
}
```

## 拦截器
### 拦截器的配置
* SpringMVC中的拦截器用于拦截控制器方法的执行
* SpringMVC中的拦截器需要实现HandlerInterceptor或者继承HandlerInterceptorAdapter类
* SpringMVC的拦截器必须在SpringMVC的配置文件中进行配置
```xml
    <mvc:interceptors>
        <!--<bean class="com.example.mvc.interceptors.FirstInterceptor"></bean>-->
        <!--<ref bean="firstInterceptor"></ref>-->
        <!--以上两种配置方式都是对DispatcherServlet所处理的所有请求进行拦截-->
        <mvc:interceptor>
            <mvc:mapping path="/*"/> <!--这里的/*代表上下文的一层目录，如果想要拦截所有目录，需要设置为/**-->
            <mvc:exclude-mapping path="/"/>
            <ref bean="firstInterceptor"></ref>
        </mvc:interceptor>
    </mvc:interceptors>
```

### 拦截器的三个抽象方法
SpringMVC中的拦截器有三个抽象方法

* preHandle：控制器方法执行之前执行preHandle()，其boolean类型的返回值标识是否拦截或放行，返回true为放行，即调用控制器方法；返回false表示拦截，即不调用控制器方法
* postHandle：控制器方法执行之后执行postHandle()
* afterHandle：处理完视图和模型数据，渲染视图完毕之后执行afterComplation()

### 多个拦截器的执行顺序
* 若每个拦截器的preHandle()都返回true
	- 此时多个拦截器的执行顺序和拦截器在SpringMVC的配置文件的配置顺序有关：
		+ preHandle()会按照配置顺序的顺序执行，而postHandle()和afterComplation()会按照配置的反序执行
* 若某个拦截器的preHandle()返回了false
	- preHandle()返回false和它之前的拦截器的preHandle()都会执行，postHandle()都不执行，返回false的拦截器之前的拦截器的afterComplation()会执行
	
## 异常处理器
### 基于配置的异常处理
* SpringMVC提供了一个处理控制器方法执行过程中所出现的异常的接口：HandlerExceptionResolver
* HandlerExceptionResolver接口的实现类有
	- DefaultHandlerExceptionResolve
	- SimpleMappingExceptionResolver
* SpringMVC提供了自定义的异常处理器SimpleMappingExceptionResolver，使用方式
```xml
    <!--配置异常处理-->
    <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <property name="exceptionMappings">
            <props>
                <!--
                    properties的键表示处理器方法执行过程中出现的异常
                    properties的值表示若出现指定异常时，设置一个新的视图名称，跳转到指定页面
                -->
                <prop key="java.lang.ArithmeticException">error</prop>
            </props>
        </property>
        <!--设置将异常信息共享在请求域中的键-->
        <property name="exceptionAttribute" value="ex"></property>
    </bean>
```
### 基于注解的异常处理
```java
@ControllerAdvice
public class ExceptionController {
	// @ExceptionHandler用于设置所标识方法处理的异常
	@ExceptionHandler(value = {ArithmeticException.class, NullPointerException.class})
	// ex表示当前请求处理中出现的异常对象
	public String handleArithmeticException(Exception ex, Model model) {
		model.addAttribute("ex", ex);
		return "error";
	}
}
```

## 注解配置SpringMVC
* 使用配置类和注解代替web.xml和SpringMVC配置文件的功能
### 创建初始化类，代替web.xml
* 在Servlet3.0环境中，容器会在类路径中查找实现javax.servlet.ServletContainerInitializer接口的类，如果找到的话就用它来配置Servlet容器
* Spring提供了这个接口的实现，名为SpringServletContainerInitializer，这个类反过来又会查找实现WebApplicationInitializer的类并将配置的任务交给它们来完成。Spring3.2引入了一个便利的WebApplicationInitializer基础实现，名为AbstractAnnotationConfigDispatcherServletInitializer，当我们的类扩展了AbstractAnnotationConfigDispatcherServletInitializer并将其部署到Servlet3.0容器的时候，容器会自动发现它，并用它来配置Serlet上下文。
### 创建SpringConfig配置类，代替spring的配置文件
### 创建WebConfig配置类，代替SpringMVC的配置文件

## SpringMVC常用组件
* DispatcherServlet：前端控制器，不需要工程师开发，由框架提供
	- 作用：统一处理请求和响应，整个流程控制的中心，由它调用其他组件处理用户的请求
* HandlerMapping：处理器映射器，不需要工程师开发，由框架提供
	- 作用：根据请求的url、method等信息查找Handler，即控制器方法
* Handler：处理器，需要工程师开发
	- 作用：在DispatcherServlet的控制下Handler对具体的用户请求进行处理
* HandlerAdapter：处理器适配器，不需要工程师开发，由框架提供
	- 作用：通过HandlerAdapter对处理器(控制器方法)进行执行
* ViewResolver：视图解析器，不需要工程师开发，由框架提供
	- 作用：进行视图解析，得到相应的视图，例如：ThymeleafView、InternalResourceView、RedirectView
* View：视图，不需要工程师开发，由框架提供
	- 作用：将模型数据通过页面展示给用户
	
## SpringMVC的执行流程
* 用户向服务器发送请求，请求被SpringMVC前端控制器DispatcherServlet捕获
* DispatcherServlet对请求URL进行解析，得到请求资源标识符(URI)，判断请求URI对应的映射
	- 不存在
		+ 再判断是否配置了mvc:default-servlet-handler
		+ 如果没配置，则控制台报映射查找不到，客户端展示404错误
		+ 如果有配置，则访问目标资源(一般为静态资源，如:JS,CSS,HTML)，找不到客户端也会展示404错误
	- 存在则继续执行下面的流程
* 根据该URI，调用HandlerMapping获得该Handler配置的所有相关对象(包括Handler对象以及Handler对象对应的拦截器)，最后一个HandlerExecutionChain执行链对象的形式返回
* DispatcherServlet根据获得的Handler，选择一个合适的HandlerAdapter
* 如果成功获得HandlerAdapter，此时将开始执行拦截器的preHandler(...)方法【正向】
* 提取Request中的模型数据，填充Handler入参，开始执行Handler(Controller)方法，处理请求。在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作:
	- HttpMessageConveter：将请求信息(如Json、xml等数据)转换成一个对象，将对象转换为指定的响应信息
	- 数据转换：对请求消息进行数据转换。如String转换成Integer、Double等
	- 数据格式化：对请求信息进行数据格式化。如将字符串转换成格式化数字或格式化日期等
	- 数据验证：验证数据的有效性(长度、格式等)，验证结果存储到BindingResult或Error中
* Handler执行完成后，向DispatcherServlet返回一个ModelAndView对象
* 此时将开始执行拦截器的postHandle(...)方法【逆向】
* 根据返回的ModelAndView(此时会判断是否存在异常：如果存在异常，则执行HandlerExeceptionResolver进行异常处理)选择一个适合的ViewResolver进行视图解析，根据Model和View，来渲染视图
* 渲染视图完毕执行拦截器的afterCompletion(...)方法【逆向】
* 将渲染结果返回给客户端

