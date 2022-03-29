### SpringMVC
#### 三层架构
* 表述层(表示层)，表述层表示前台页面和后台servlet
* 业务逻辑层
* 数据访问层

#### 转发和重定向区别
* 转发是一次请求，第一次是浏览器请求，第二次是发生在服务器内部，转发可以获取请求域中的数据，不可以跨域，只能访问服务器内部的资源
* 重定向是两次请求，第一次访问servlet，第二次访问重定向所指地址，重定向不可以获取请求域中的请求，可以跨域

#### HttpMessageConverter
* HttpMessageConverter,报文信息转换器，将请求报文转换为Java对象，或将Java对象转换为响应报文
* HttpMessageConverter提供了两个注解和两个类型：@RequestBody，@ResponseBody,RequestEntity,ResponseEntity.

