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
							

​		其他功能
​			1、获取请求参数通用方式：// get/post通用
​				String getParameter（String name）
​					根据参数名称获取参数值
​				String[] getParametervalues（String name）
​					根据参数名获取参数值的数组
​				enumeration<String> getParametersNames（）
​					获取所有请求的参数名称
​				Map<String,String[]> getParameterMap()
​					获取所有参数的Map集合
​				中文乱码问题：
​					get方式：tomcat8 解决了get方式乱码
​					post方式：会乱码
​						解决：设置流的编码
​						request.setCharacterEncoding("utf-8")
​			2、请求转发:一种在服务器内部的资源跳转方式
​				步骤
​					通过方法：RequestDispatcher getResquestDispatcher（String path）获取请求转发器对象
​					使用RequestDispatcher对象来转发：forward（ServletRequest request ServletResponse response）
​					

```
request.getResquestDispatcher（"/Demo2"）.forward(request,response)
```
				特点：
					1、浏览器地址栏路径不变
					2、只能访问内部服务器资源
					3、转发是一次请求
​			3、共享数据
​				域对象：一个有作用范围的对象，可以在范围内共享数据
​				request域：代表一次请求，一般用于请求转发的多个资源中共享
​				方法：
​					1、setAttribute（String name，Object obj）存储数据
​					2、Object getAttibute（String name）：通过键获取值
​					3、removeAttribute（String name）：通过键移除值
​			4、获取ServletContext对象
​				ServletContext getServletContext（）；
​	

## 示例
	1、创建项目，导入html页面，druid配置文件，jar包
	2、创建数据库环境
	3、创建包domain，创建类User
	4、创建包dao，创建类UserDao，创建login方法
	5、编写servlet.LoginServlet类
	6、login.html中form表单的action路径的写法
		虚拟目录+servlet的资源路径
		//servlet使用资源路径
	7、BeanUtils工具类完成数据封装
		JavaBean：标准的Java类
			要求：
				类必须被public修饰
				必须提供空参的构造器
				成员变量必须使用private修饰
				必须提供公共setter和getter方法
				//放在domain下
			功能：封装数据
		概念：
			成员变量
				类内直接定义的变量
			属性//大多数和成员变量一样
				setter和getter方法截取后的产物
					getterUsername（）--》Username--》username
		方法：
			setProperties（）设置属性
			getProperties（）获取属性
			populate（）封装
	
	Map<String, String[]> parameterMap = request.getParameterMap();
	    User loginUser = new User();
	    try {
	        BeanUtils.populate(loginUser,parameterMap);

响应消息数据格式：
	数据格式：
		响应行
			协议/版本 响应状态码 状态码描述
			响应状态码
			分类：
				1xx：服务器接收客户端消息，但没有完成，等待一段时间，发
                2xx：成功发
                3xx：302重定向 304资源缓存
                4xx：客户端错误 
                	404路径没有对应资源 
                	405请求方式没有对应方法
                5xx：服务端错误 500服务器内部异常
		响应头
			格式：头名称：值
			Content-type:服务器告诉客户端本次响应体数据格式及编码格式
			Content-disposition:如何打开响应体数据，
				默认in-line，//在当前页面打开
				attachment:以附件形式打开
		响应空行
		响应体

## Response
功能：设置响应消息
	设置响应行
		格式：HTTP/1.1 200 ok
		设置状态吗 setStatus（int sc）
	设置响应头
		setHeader（String name String value）
	设置响应体
		使用流
		步骤：
			获取输出流
				字符输出流
					PrintWritter getWritter（）
				字节输出流
					ServletOupputStream getOutputStream()
			使用输出流
完成重定向

```
response.setStatus(302);
response.setHeader("location","/test/ServletResponseD2");
//简单的重定向方式
response.senRedirect("/test/ServletResponseD2")
```

	重定向特点：
		转发：地址栏路径不变、
			只能访问当前服务器下、
			一次请求，可以使用request对象共享数据
		重定向：
			地址栏变化，可以访问其他站点资源，两次请求，不能共享数据
	路径写法：
		1、路径分类
			1、相对路径：不确定资源
				以.开头的路径
					使用相对路径，需要找当前资源和目标资源的相对位置关系
					./当前目录 ../上一级目录
				动态获取虚拟目录
				String contextPath = request.getContextPath();
				response.sendRedirect(contextPath+"/demo")
			2、绝对路径：通过绝对路径可以确定唯一资源
				以/开头的路径
					使用绝对路径，
				[[规则]]：判断定义的路径是给谁用的？
					给客户端浏览器使用的（客户端展现）：需要加虚拟目录
						虚拟目录动态获取// .getContextPath
					给服务器使用（服务端展现）：不需要加虚拟目录
						//转发
	传字符
	    获取字符输出流
	    输出数据
	    乱码：
	        设置流的默认编码//可以省略
	        response.setCharacterEncoding("UTF-8")
	        发送编码方式
	        response.setHeader("content-type","text/html;charset=utf-8");
	        简单形式：
	        response.setContentType("text/html;charset=utf-8")
	传字节
	    获取字节输出流
	    ServletOutputStream sos = respose.getOutputStream();
	    输出数据
	    sos.write("heelo".getBytes())
	        //字节流/字符流并不是由我们获取的，所以并不需要刷新流（不考虑输出）
	验证码
	    new date（）.getdate
ServletContext对象
	概念：代表整个Web应用，可以和程序的容器（Tomcat）来通信
	获取：通过request获取
		req.getServletContext
		通过httpServlet获取
		this.getServletContext
	功能：	
		获取MIME类型：通讯过程中的一种文件数据类型
			格式：大类型/小类型 text/html  image/jpeg
				//tomcat下web.xml可以查询类型对应
			获取：String getMimaType（sring file）

​	是一个域对象，用于共享数据
​		setAttribute（String name ，Object value）
​		getAttribute（String name）
​		removeAttribute（String name）
​		ServletContext的范围：可以共享所以用户所有请求的数据
​		获取文件的真实路径（服务器）
​			方法

```
String ServletContext.getRealPath("/path")
			//web目录下资源访问
File file = new 		 File(servletContext.getRealPath("/druid.properties"));
			//不同路径下访问的path也不同
```
	下载实例：
		分析：
			超链接指向的资源如果能被解析，则展示
			任何资源都要下载提示框
			使用响应头设置打开方式
				content-disposition:attachment;filename=xxx
					//以附件形式打开
		步骤：定义页面，编辑超链接，指向Servlet，传递资源名称filename
			定义servlet
				获取文件名称
				使用字节输入流加载文件进内存
				指定response的header设置为附件形式打开
				将数据写出到response输出流
		问题：
			中文文件名：
				获取不同的浏览器版本信息
				获取编码方式

# 会话技术
	会话：一次会话中包含多次请求和响应
		一次会话：浏览器第一次给服务器资源发送请求，会话建立，直到一方断开
		功能：在一次会话中的多次请求中，共享数据
		方式：
			客户端会话技术：Cookie
			服务器端会话技术：Session
Cookie
	客户端会话技术
	入门：
		使用步骤：
			创建Cookie对象，绑定数据
				new Cookie(String naem ,String value)
			发送Cookie对象
				httpServlet
				response.addCookie(cookie ck)
			获取Cookie对象，拿到数据
				Cookie[] request.getCookies()
		实现原理
        	基于响应头set-cookie和请求头cookie实现
        cookie细节
        	持久化保存
        		//浏览器关闭cookie销毁
        		setMaxAge（int seconds）
        			整数：cookie的存活时间
        			负数：默认
        			0：删除cookie信息
        	tomcat 8 之后，cookie支持存储中文数据
			cookie的共享范围
				默认情况下cookie不能共享
                    setPath（String path）：设置cookie共享范围
                        default：当前虚拟目录// /test
                        更大：设置为 / 
				不同的tomcat服务器共享
					setDomain（String path）:
						如果设置一级域名相同，那么多个服务器间共享cookie
							setDomain（“.baidu.com”）
		cookie特点及作用
                cookie数据存贮在客户端
                浏览器对单个cokie大小有限制（4kb），
                同一域名下cookie总数量有限制（20）
			作用：
				cookie一般存储少量不敏感数据
				不登陆的情况下完成用户验证及页面设置
Session
## Jsp入门
概念：Java Server Pages 服务端页面
	一个特殊的页面，既可以直接定义html标签，又可以定义java代码
    	用于简化书写
	jsp原理：
		服务器解析请求消息，寻找jsp资源
		将jsp转换为.java文件
		编译.java文件，生成.class文件
		由.class文件提供访问
			//本质是一个Servlet
	jsp脚本：
		jsp声明java代码的方式
			1、<% %>
				定义的代码，在service方法中，限制等同service
			2、<%! %>
				定义的代码，在jsp转换后的java类的成员位置（成员方法，静态代码块等）
			3、<%= %>
				输出语句，直接输出到页面上
			
	jsp内置对象






