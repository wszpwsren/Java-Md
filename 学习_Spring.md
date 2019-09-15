# Spring

# spring框架概述及spring中基于xml的IOC配置

## spring概述

​	spring
​		方便解耦，简化开发
​		AOP编程支持
​		声明式事务的支持
​		简化API
​	spring的两大核心
​	spring的体系结构
​		DataAccess------Web
​				AOP
​		Core Container（IOC）		
​				Test

## 程序的耦合与解耦
​	耦合问题
​		程序之间的依赖
​			类间依赖
​			方法间依赖
​		开发中
​			需要做到编译器不依赖，运行期依赖
​		解决思路
​			使用反射创建对象，避免使用new
​			通过配置文件获取要创建的对象信息
​				//xml-》key(String)&value(全限定类名)
​	工厂模式解耦	
​		//Bean：可重用组建
​		反射类的单例/多例
			//成员变量的复合变化（单例存在变化）
	IOC
		由工厂提供资源，由工厂寻找资源
		将控制权交给框架，由框架创建对象
	Spring AOP
		基于注解配置
	Spring JCL
		commons-logging
## IOC概念和springIOC
	
​	spring中基于xml的ioc环境搭建
		核心容器对象ApplicationContext
## 依赖注入（Dependency Injection）

# spring基于注解的IOC和IOC的案例

# spring中的AOP和基于XML/注解的AOP配置

# spring中的JDBCTemplate及Spring的事务控制