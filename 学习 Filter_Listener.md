# Filter

三大组件：Servlet，Filter，Listener
Filter：当访问服务器资源时，过滤器将请求拦截下来，完成一些功能
	用于完成通过的操作：验证登陆、统一编码处理、敏感字符过滤
入门：
	步骤
		定义一个类，实现接口Filter
		覆写方法
			//放行
        	filterChain.doFilter(servletRequest,servletResponse);
		配置拦截路径
	配置方式：
		web.xml
		注解配置
			@WebFilter
	过滤器细节：
	Web.xml配置
	过滤器执行流程
	过滤器生命周期方法
		init 服务器启动后，创建Filter对象，调用init方法
		destory 服务器关闭后，Filter对象被销毁（正常关闭 ）
		do Filter 每一次请求被拦截，执行一次
	过滤器配置详解
		拦截路径配置
			具体资源路径：/index.jsp
			目录拦截：/user/*
			后缀名拦截 *.jsp
			拦截所有 /*
		拦截方式配置：资源被访问的方式
			注解配置
				设置dispatcherTypes
					REQUEST:default
						浏览器直接请求
					FORWARD
						转发访问资源
					INCLUDE
						包含访问资源
					ERROR
						错误跳转
					ASYNC
						异步访问资源
			Web.xml
			
	过滤器链（多个）
		执行顺序：1、2
			过滤器1执行
			过滤器2执行
			资源执行
			过滤器2执行
			过滤器1执行
		过滤器先后顺序问题：
			注解配置：按类名的字符串比较规则比较，值小的先执行
				
			Web.xml：谁在上谁先执行
# Listener
























