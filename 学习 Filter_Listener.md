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

Request对象进行增强：
	设计模式：一些通用的解决固定问题的方式
		装饰模式
		代理模式
			概念：
				真实对象：被代理的对象
				代理对象
				代理模式：代理对象代理真实对象，达到增强真实对象的目的
			静态代理：有一个类文件描述代理模式
			动态代理：在内存中形成代理类
				实现步骤：
					代理对象和真实对象实现相同的接口
					Proxy.newProxyInstance();获取代理对象
					使用代理对象调用方法
					增强方法
				增强方式：
					增强参数列表
					增强返回值类型
					增强方法体
				

```java
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        /*System.out.println("代理执行了");*/
        //使用代理对象调用真实方法，obj为真实方法返回值，通过代理对象return调用真实方法的return
        Object invoke = null;
        if (method.getName().equals("sale")) {
        	args[0] = (double)args[0] * 1.1;
        	Object invoke1 = method.invoke(lenovo, args);

        	return invoke;
        }else {
        	invoke = method.invoke(lenovo, args);
        }
        	return invoke;
        }
    });
```




# Listener

概念：web三大组建之一
	事件监听机制
		事件：一件事情
		事件源：按钮
		监听器：一段代码或一个对象
		注册监听：将事件、事件源、监听器绑定在一起
ServletContextListener 创建及销毁
		contextDestory(ServletContextEvent sce) 销毁前调用
		contextInitialized(ServletContextEvent sce) 创建后调用
	步骤：
		1、定义一个类，实现接口ServletContextListener
		2、覆写方法
		配置




















