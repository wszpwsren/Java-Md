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
# Servlet




















