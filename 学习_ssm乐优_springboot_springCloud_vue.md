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



## Hystrix

容错管理组件，实现断路器模式

## 版本

rail/sr/小版本

Finchley基于sb2.0.x

## 使用步骤

​	1、引入组件启动器
​	2、覆盖默认配置
​	3、在引导类上添加注解，开启相关组件

