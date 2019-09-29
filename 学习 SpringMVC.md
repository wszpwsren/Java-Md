# SpringMVC

## 三层架构

## SpringMVC

## SpringMVC入门

```
	//控制器类
@Controller
public class HelloController {
    @RequestMapping(path = "/hello")
    public String sayHello(){
        System.out.println("HelloSpringMVC");
        return null;
    }
}
```

### springmvc控制器返回值为返回跳转

流程
	启动服务器，加载配置文件
		web.xml——dispatcher--对象创建（load-on-startup）
			--initparam--springmvc.xml加载
				--注解扫描--controller对象创建并加载
				--视图解析器对象创建
				--开启mvc框架（requestmapping↑）
					--执行方法
						--跳转↓
	发送请求，后台处理请求
		<a>发送请求
			执行dispatcherservlet拦截（前端控制器）
				通过注解寻找路径方法
					方法返回路径
						通过视图解析寻找对象（success）
							跳转至jsp
								servlet响应至客户端

![1569395195574](C:\Users\feketerigo\AppData\Roaming\Typora\typora-user-images\1569395195574.png)

SpringMVC基于组件开发方式执行流程（xx器）
	处理器映射器HandlerMapping
		让Controller类中方法执行
			返回Controller类中的SayHello方法
	处理器适配器HandlerAdapter
		将方法执行（执行Handler）
	Handler处理器（ControllerServlet）
		执行方法
			返回ModelAndView
	视图解析器ViewResolver
		视图解析
			返回view
	view解析（DS）

<mvc:annotation>自动加载处理器映射器，处理器适配器，视图解析器（MVC三大组件）

@RequestMapping 
	处理url与方法的映射
	属性：
		value/path
		method：接收什么请求方式
			<a>get
		params：用于指定限制请求参数的条件
        	params=“username” 需要传入该名称的参数
        		<a href="user/x?username=x">
        headers:指定消息头
        	headers={“Accept”}
        		 
## 请求参数的绑定
绑定机制：
	username=x&a=233
		MVC-Controller-
			sayHello（String username，String a）
				会自动赋值
支持的数据类型：
	基本类型/String，JavaBean，集合类型

```
<!--配置中文字符过滤器-->
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```

自定义类型转换器

spring会自动转换数据类型（传输都是string，后台转换）
	

```
	<!--配置类型转换器-->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="com.k.utils.StringToDateConverter"></bean>
            </set>
        </property>
    </bean>
    驱动注入
    <mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>
```

```
public class StringToDateConverter implements Converter<String, Date> {

@Override
public Date convert(String source) {
    if(source==null){
        throw new RuntimeException("空");
    }
    try {
        DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
        Date parse = dateFormat.parse(source);
        return parse;
    } catch (ParseException e) {
        throw new RuntimeException("错误");
    }
}

}
```

获取Servlet API
	

```
/**
     * 获取原生api
     * @return
     */
    @RequestMapping(path = "/testServlet")
    public String test(HttpServletRequest request,HttpServletResponse response){
        System.out.println(request);
        System.out.println(request.getSession());
        System.out.println(request.getSession().getServletContext());
        System.out.println(response);
        return "success";
    }
```

常用注解

RequestParam
	把请求中指定名称的参数给控制器中的形参赋值
RequestBody
	用于获取请求体内容，直接使用得到kv结构数据
	get请求不适用
PathVariable
	用于绑定url的占位符
		/delete/{id}
REST风格URL
	根据请求方式的不同选择controller中的方法
    controller中的方法使用同一地址
HiddentHttpMethodFilter
	WebClient使用静态方法可发送各种请求
RequestHeader
	获取请求消息头
CookieValue
	获取指定Cookie的值
ModelAttribute
	参数
		存入Map后，将存入数据取出
	方法
		表示方法会在控制器的方法执行前执行
			实例：当提交数据不是完整的实体类数据时，会从数据库对象原有数据提取
SessionAttributes
	用于多次执行 控制器方法 间的参数共享
	model.attribute(k,v)添加
	modelMap.get(k)获取
	sessionStatus.setComplete()

## 响应数据和结果视图
	字符串返回值
	无返回值
		自动请求WEB—INF/自己/自己.jsp
	返回值是ModelAndView对象

@ResponseBody响应json数据
	配置前端控制器放行js读取

```
<!--前端控制器放行-->
    <mvc:resources mapping="/js/**" location="/js/"></mvc:resources>
```

Json封装bean需要jackson jar包2.7以上

```
<dependency>            			 		 <groupId>com.fasterxml.jackson.core</groupId>            <artifactId>jackson-databind</artifactId>            <version>2.9.0</version>        
</dependency>
<dependency>            <groupId>com.fasterxml.jackson.core</groupId>            <artifactId>jackson-core</artifactId>            <version>2.9.0</version>        
</dependency>        
<dependency>            <groupId>com.fasterxml.jackson.core</groupId>            <artifactId>jackson-annotations</artifactId>            <version>2.9.0</version>        
</dependency>
```

文件上传

form表单的enctype取值必须为multipart/form-data
	默认值为appliccation/x-www-form-urlencoded
		enctype：是表单请求正文的类型
	method属性的取值为post
	提供一个文件选择域<input type="file">

```
<dependency>            
<groupId>commons-fileupload</groupId>            <artifactId>commons-fileupload</artifactId>            <version>1.3.1</version>        
</dependency>        
<dependency>            
<groupId>commons-io</groupId>            
<artifactId>commons-io</artifactId>            <version>2.4</version>        
</dependency>
```

springmvc可以通过前端控制器调用文件上传
	配置文件解析器request-》upload
		mulltiparresolver
​	执行controller方法（接收multipartfile）
​		//input name必须和multipartfile相同

```
<!--配置文件解析器-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="10485760"></property>
    </bean>
```

跨服务器文件上传
	jersey
	
异常处理器
	由前端控制器控制
	
	编写自定义异常类（提示信息）
	编写异常处理器
	配置异常处理器（调转到页面）

拦截器：类似过滤器，对请求进行预处理，后处理
	过滤器是servlet的，拦截器是springmvc的
	过滤器配置/*时，对所有访问进行过滤，拦截器只会拦截访问的控制器方法，jsp，html等不拦截
		编写拦截器类，实现HandlerInterceptor
		配置拦截器
	

# SSM

配置spring

spring整合springmvc

//能够从controller层调用service层
需要将service注入tomcat ioc
	tomcat加载spring
		ServletContext域对象
			服务器启动的时候创建
			服务器关闭时销毁
		监听器：
			监听ServletContext域对象的创建和销毁
	利用监听器加载spring配置文件，创建web版本工厂，存储servletcontext对象

```
	
​	<!--配置spring监听器,默认只加载WEB-INF目录下的applicationContext.xml配置文件-->

```

```
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <!--设置配置文件路径-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>
```

spring整合mybatis

​	//service能够调用到dao
	将生成的代理对象存入ioc















