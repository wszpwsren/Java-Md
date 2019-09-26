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






















