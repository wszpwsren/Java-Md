# Web回顾

软件架构：
	C/S
	B/S
资源分类
	静态资源
	动态资源
网络通信三要素
	ip
	端口
	协议
		tcp：安全协议
		udp：不安全协议，广播协议

# Tomcat

服务器软件：接收用户的请求，处理请求，做出相应
Web服务器软件：同上
	在Web服务器软件中，可以部署web项目，让用户通过浏览器来访问这些项目
	Web容器
常见的Java相关web服务器软件
	WebLogic：oracle公司，大型JavaEE服务器
	WebSphere：IBM公司，大型JavaEE服务器
	JBoss：JBoss公司，大型JavaEE服务器
	Tomcat：Apache基金会，中小型JavaEE服务器，支持少量JavaEE规范
JavaEE:Java语言在企业级开发中使用的技术规范的总和，共13项

Tomcat
	目录结构：
		bin：可执行文件
		conf：配置文件
		lib：类库
		logs：日志
		temp：临时
		webapps：存放web项目
		work：存放运行时数据
	一般将tomcat的默认端口号修改为80.80端口号为http协议的默认端口号
	关闭：shutdown.bat/ ctrl+c
	配置：
		部署项目的方式：
			1直接将项目放到webapps目录下
				//127.0.01/hello/hello.html
				/hello：项目的访问路径-->虚拟目录
				简化部署：将所有文件压缩为.war，放入根目录下，自动解压缩为虚拟目录
			2配置目录
				Sever.xml中
				<Host>中，<Context docBase=‘url/*真实url*/’ path='/xx/*设置虚拟url*/'/>
			3conf/Catalina/localhost，解决2不安全的问题
				conf/Catalina/localhost创建一个任意名称的xml文件
				文件中编写<Context docBase=‘url/*真实url*/’>
				xml文件名称为虚拟url
				热部署
		静态项目和动态项目
			目录结构区别：
				java动态项目的目录结构：
					/项目的根目录
						/静态项目文件
						/WEB_INF目录（动态项目文件）：
							/web.xml：WEb项目的核心配置文件
							/classes：防止字节码文件的目录
							/lib目录：放置依赖的jar包
		Tomcat集成到IDEA，并创建JAVAEE项目，部署项目
        	RUN-Edit Configuration—Templates-Tomcat Server
        Tomcat热部署（？）
        	RUN-Edit Configuration-Tomcat-On Update action-》Update resource
			
# Servlet

Servlet概念：server applet，运行在服务器端的小程序
		服务器上的JAVA类，依赖于服务器才能运行，tomcat来执行它
		需要遵守一定的规则（接口），才能被tomcat执行
		Servlet就是一个接口，定义了Java类被浏览器访问到/Tomcat识别的规则
		之后自定义一个类，实现servlet接口，覆写方法
	快速入门
		1、创建一个JavaEE项目
		2、定义一个类，实现Servlet，实现方法
		3、配置Servlet
			WEB-INF-》web-app中

```
	<!--  配置Servlet  -->
    <servlet>
        <servlet-name>demo01</servlet-name>
        <servlet-class>web.servlet.ServletDemo01</servlet-class>
    </servlet>
    <!-- 配置Servlet mapping   -->
    <servlet-mapping>
        <servlet-name>demo01</servlet-name>
        <url-pattern>/demo1</url-pattern>
    </servlet-mapping>
```

​	运行过程
​		通过mapping，将url反射为class文件（全类名
​		tomcat将全类名对应的字节码文件加载进内存 class.forName()
​		创建对象 cls.newInstance();
​		调用方法--service
​	Servlet声明周期方法
​		被创建：执行init方法，只执行一次
​			默认情况下，第一次被访问时，Servlet被创建
​			可以配置指定Servlet创建时机（web.xml-<servlet>-<servlet-class>-load-on-startup）
​			init方法只执行一次，说明一个Servlet在内存中只存在一个对象（单例）
​				多个用户同时访问时，可能存在线程安全问题
​				解决：尽量不要在Servlet中定义成员变量，而定义局部变量，即使定义了成员变量，不要对其修改
​		提供服务：执行Service方法，执行多次
​			每次访问Servlet时，Service方法都被调用一次
​		被销毁：执行destory方法，只执行一次
​			Servlet被销毁时执行。服务器关闭时，Servlet被销毁
​			只有服务器正常关闭时，才会执行destory方法
​	Servlet3.0
​		优点：
​			支持注解配置，不需要配置xml
​		步骤：
​			创建JavaEE项目，选择Servlet版本，可以不创建web.xml
​			定义Servlet类，实现接口，覆写方法
​			使用@Webservlet注解进行配置
```
@WebServlet(urlpartten="/demo02")//设置url
	public class ServletDemo implements Servlet {...}
```
​	Servlet体系结构
​			Servlet--接口↓
​				↓
​			GenericServlet--抽象类
​				↓
​			HttpServlet--抽象类
​		简易实现Servlet
​			public class ServletDemo implements GenericServlet {...}
​		HttpServlet：对http协议的一种封装，简化操作
​        	service功能：
​				判断请求方式
​					req.getMethod（）；
​					if（“get“）{doGet（）}if("post"){doPost（）}
​						//封装入HttpServlet
​			步骤：
​				定义类继承HttpServlet
​				覆写doGet/doPost方法

```
	@WebServlet("/demo2")
		public class ServletDemo2 extends HttpServlet {}

```

​	Servlet相关配置
​		urlpartten：Servlet访问路径
​			一个Servlet可以定义多个访问路径
​			路径定义规则：
​				1、/xxx
​				2、/xxx/xxx //  /*任意匹配
​				3、*.xxx  //注意不加/

## HTTP协议
概念：Hyper Text Transfer Protocol
	传输协议：定义了客户端和服务端通信的格式
	特点：
		基于tcp/ip的高级协议
		默认端口号80
		基于请求/响应模型的，一次请求收到一次相应
		无状态的：每次请求之间相互独立
	1.0：每次请求响应都会建立新的连接
	1.1：可以复用连接
	请求消息数据格式：
		请求行
			请求方式 请求url 请求协议/版本
				请求方式
					7种
						GET：
							请求参数在请求行中（url后）
							请求的url长度有限制
						POST：
							请求参数在请求体中
							请求的url长度没有限制
		请求头//键值//客户端将自己信息发给服务器端
			请求头名称：请求头值
				常见的请求头：
					User-Agent：浏览器版本
					Referer：
						告诉服务器，当前请求从哪里来
							防盗链
							统计
					Connection：是否复用
		请求空行
			空行，做分割
		请求体：封装Post请求消息的请求参数
			username=zhangsan
	Request：
		request和response的原理：
			tomcat服务器会根据请求url中的资源路径，创建对应的ServletDemo对象
			tomcat服务器，会创建request和response对象，request对象中封装请求消息数据
			tomcat，会将两个对象传递给service方法，并调用service方法
			定义service方法，通过request对象获取请求消息数据，通过response对象设置响应消息数据
			服务器在给浏览器做出相应之前，会从response对象中那设置的响应消息数据
		Request继承体系结构
			ServletRequest接口
				↓
			HttpServletRequest接口
				↓
			org.apache.catalina.connector.RequestFacade类
		Request：功能
			获取请求消息数据
				获取请求行数据
					GET /day14/demo1？name=zhang HTTP/1.1
					方法
						获取请求方式//GET
							String GetMethod()
						*获取虚拟目录//day14
							String getContextPath（）
						获取Servlet路径/demo1
							String getServletPath（）
						获取get方式请求参数name=zhang
							String getQueryString（）
						*获取请求的URI
							String getRequestURI（）定位符
								/day14/demo
							StringBUffer getRequestURL（）标识符
								http://127.0.0.1/day14/demo1
						获取HTTP协议及版本号
							String getProtocol（）
						获取客户端IP
							String getRemoteAddr（）
				*获取请求头数据
					方法
						String getHeader（String name）
							通过请求头的名称获取请求头的值
						Eunmeration<String> getHeaderNames（）：
							获取所有的请求头名称
								//类型为迭代器
				获取请求体数据
					请求体，只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数
					步骤：
						获取流对象
							BufferedReader getReader（）
							ServletIputStream getInputStream（）
						从流对象中拿出数据
							
			其他功能
				1、获取请求参数通用方式：// get/post通用
					String getParameter（String name）
						根据参数名称获取参数值
					String[] getParametervalues（String name）
						根据参数名获取参数值的数组
					enumeration<String> getParametersNames（）
						获取所有请求的参数名称
					Map<String,String[]> getParameterMap()
						获取所有参数的Map集合
				2、请求转发
				3、共享数据
				4、获取ServletContext对象
		
	响应消息数据格式：
## Request


















