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


















