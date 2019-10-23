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