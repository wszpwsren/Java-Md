# SpringBoot

<!--所有的springboot应用都要以该工程为父工程-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.9.RELEASE</version>
    </parent>

 <!--启动器：每一个启动器背后都是一堆依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
不需要配置web.xml

整合springmvc
	server.port
	静态资源
		classpath:META-INF,classpath:resources,classpath:static,classpath:public
	拦截器
		自定义拦截器，implements HandlerIntercepter
		配置拦截器：自定义java配置类，@Configuration，实现webmvcconfigurer接口
整合数据源
	引入jdbc启动器，引入mysql驱动，配置参数
    添加配置spring.datasource.xxx
整合mybatis
	引入启动器
	覆盖默认配置 
		mybatis.type-alias-package
		mybatis.mapper-loaction=classpath:myubatis/mappers/**/*.xml
	定义一个接口，在接口上添加@Mapper注解
整合通用mapper
	引入启动器
	接口继承Mapper<User>
整合事务
	添加Transactional注解

ConfigurationProperties：属性读取类

EnableConfigurationProperties (属性读取类.class)



## thymeleaf

jsp-》servlet-》tomcatservlet容器-》解析





# SpringCloud

## 架构
垂直拆分
水平拆分
分布式
	方便水平扩展
	方便单独优化
	解耦
	提高并发量
	
	增加维护成本
	重复开发
		SOA面向服务架构
			由注册中心及组件组成
			微服务架构
				微服务每一个服务对应唯一的业务，单一职责
				拆分粒度小
				面向服务：对外暴漏Rest风格的接口（url路径式接口），不限定服务的语言及技术			

微服务框架
	springcloud
	dubbo
	
RPC/RMI
	Rrmote Produce Call
	自定义数据格式
	传输效率高
HTTP
	规定了数据传输的格式

Spring RestTemplate
对http客户端进行了封装，实现了json的序列化和饭序列化

## Eureka

注册中心
	
	@EnableEurekaServer//启用eureka服务器
	yml：
		spring:
  application:
    name: k-eureka #将来会作为微服务的名称注入到eureka容器中
eureka:
  client:
    service-url:
      defaultZone: http://localhost:${server.port}/eureka

心跳机制
	renew服务续约
		由服务提供者维持
		间隔时间30s，过期时间90s
		lease-renewal-interval-in-seconds
		lease-expiration-duration-in-seconds
	客户端拉取服务列表
		拉取至本地
		每30s拉取一次
		registry-fetch-interval-seconds
    	DiscoveryClient对象中
		拉取服务
	服务正常关闭操作时
		触发一个服务下线的REST请求给server
	失效剔除
		每60秒扫描一次
		eviction-interval-timer-in-ms
		过期时间90s
	eureka自我保护机制
		关闭
			enable-self-preservation: false
高可用的eureka
	服务器相互注册（1->2,2->3,3->1)
	
注册双重map
	map<serviceId,map<serviceInstancename,InstanceObject>>

## Zuul

网关，提供路由，访问过滤
	所有请求通过zuul-》consumer，进行拦截/权限控制
	【服务无状态】为了保证对外服务的安全性，我们需要实现对服务访问的权限控制，而开放服务的权限控制机制会贯穿（全部服务实现权限控制）并污染开放服务的业务逻辑
		需要考虑接口访问的权限控制
	通过服务网关统一向外提供rest api时，提供服务路由、负载均衡、权限控制
	配置zuul时，通过zuul.prefix： /api 保证通过网关来访问
zuul过滤器
	izuulfilter 祖接口
		shouldFilter
			true/false
		run
			业务逻辑
	zuulfilter 抽象实现
		filterType
			pre	路由前执行过滤器
			route 路由过程中执行过滤器
				完成后进行路由分发
			post 路由完成后执行过滤器
			error 路由错误执行过滤器
		![1573383418355](C:\Users\feketerigo\AppData\Roaming\Typora\typora-user-images\1573383418355.png)
		filterOrder
			int越小优先级越高
	

zuul:
  routes:
    service-provider: #路由名称,习惯上为服务名
      path: /service-provider/**
      // url: http://localhost:8082 通过路径路由，不常用
      serviceId: service-provider 通过服务名路由

​	//service-provider: /service-provider/** #第三种路由方式，通过直接写路径路由

第四种，默认路由方式，路由名称为服务名

## Ribbon
负载均衡

配置服务提供者地址列表，自动进行负载均衡
eureka集成了ribbon

启动：

```
	@Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
```

springboot 测试
	@SpringBootTest
    @RunWith（SpringRunner.class）
默认访问策略
	轮询
	

```

service-provider:
  ribbon: NFLoadBalancerRuleClassName:com.netflix.loadbalancer.RandomRule
```

## Feign

服务调用，集成Ribbon
	引入openFeign启动器
	feign.hystrix.enable=true，开启feign熔断
	引导类上@EnableFeignClients（client）
	创建接口，在接口添加@FeignClient（value=“服务id”,fallback=实现类.class）
	接口中定义一些方法，书写方式与controller类似
	创建熔断类，实现feign接口，实现熔断方法


## Hystrix

容错管理组件，实现断路器模式

## 版本

rail/sr/小版本

Finchley基于sb2.0.x

## 使用步骤

​	1、引入组件启动器
​	2、覆盖默认配置
​	3、在引导类上添加注解，开启相关组件


SpringCloud-在springboot基础上构建的微服务框架
	eureka
		引入服务端启动器：eureka-server
		添加配置
			spring.application.name服务名
			eureka.client.server-url.defaultZone http://localhost:10086/eureka
			eureka.server.eviction-internal-timer-in-ms 剔除无效连接的间隔时间
			eureka.server.enable-self-preservation 关闭自我保护
			@EnableEurekaServer 开启eureka服务端
			

​	引入客户端启动器：eureka-client
​	添加配置
​		spring.application.name 服务名
​		eureka.client.server-url.defaultZone http://localhost:10086/eureka
​		eureka.instance.lease-renewal-internal-in-seconds 心跳时间
​		eureka.instance.lease-expiration-duraion-in-seconds 过期时间
​		eureka.client.register-with-eureka 是否注册，默认
​		eureka.client.fetch-register 是否拉取服务列表，默认
​		eureka.client.registry-fetch-interval-seconds 拉取服务的间隔时间
​	@EnableDiscoveryClient
​	
​	Ribbon
​	eureka\feign\zuul已集成
​	配置负载均衡策略 <服务名>.ribbon.NFloadBalancerRuleClassName 
​	
​	hystrix
​		降级
​			引入启动器
​			添加配置，超时时间
​			@EnableCircuitBreaker
​			代码
​				返回值和参数列表与被熔断方法一致,被熔断方法@HystrixCommand（局部）
​				返回值和被熔断方法（全部）一致，无参数,类上@DefaultProperties（defaultFallback="全局熔断方法名"）,被熔断方法@HystrixCommand
​		熔断
​			多次降级
​	feign
​		集成hibbon、hystrix
​		引入feign启动器
​		feign.hystrix.enable=true
​		@EnableFeignClients
​		代码	
​			定义一个接口
​            @FeignClients（value="服务名",fallback=熔断类.class）
​            方法上使用的注解都是springMVC的注解
​	Zuul
​		@EnableZuulProxy
​		自定义过滤器


​	
# 电商

传统项目：
	需求方为公司内部
互联网项目：
	需求方为外部
技术特点
	技术范围广，高并发（分布式、静态化、缓存、异步并发、池、队列），高可用（集群、负载均衡、限流、降级、熔断），数据量大，业务复杂，数据安全

saas software as a service
	提供商为企业搭建信息化所需的所有网络设施及软件	
soa
	面向服务架构
RPC remote procedure call
	远程过程调用
RMI remote method invocation
	远程方法调用	

## 开发过程

​	产品经理-需求设计书》技术主管-评审》开发评估》api文档》ui开发、后端开发》测试》上线
​	![1525703759035](F:\BaiduYunDownload\乐优商城-11月版\leyou\day04-项目搭建及es6语法\笔记\assets\1525703759035.png)
​	

## 后台管理系统

​	商品管理、销售管理、用户管理、权限管理、统计
​	前后端分离开发，后台系统通过Vuejs搭建出单页应用

## 前台门户系统
	

​	