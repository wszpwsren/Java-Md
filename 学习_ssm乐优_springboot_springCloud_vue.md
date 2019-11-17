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

	Thymeleaf模板结合vuejs,储于seo优化的考虑，不采用单页应用
	SEO
		搜索引擎优化
	前台后台共享相同的微服务集群
		商品、搜索、订单、购物车、用户中心、eureka、zuul等

js es6
fastdfs 文件管理
mycat 数据库中间件
## ECMAScript6 es6
	可以用来编写复杂的大型程序
	var：变量越界
		可以作用于作用域以外
	let：es6变量
		存在作用域
	const；常量		
	includes（）
	startwith（）
	endswith（）
	const ss= `
		hello
		es6
		hello
	`
		`（左上角）,可以定义多行字符串
	解构表达式：
		const arr = [10,20,30]
		const [x,y,z]=arr
		x
		y
		let person1 = {name="1",age=1};
		const{name:a,age:b} = person1
		a
		b
	函数优化
		function f1(a,b = 2/*es6*/)
	箭头函数（lambda）
		()=>{}
	解构加箭头
		const p = {name:"1",age:3}
		const func3 = ({name})=>console.log(name);
		func3(p);
	map()函数
		接收一个函数，将原数组的所有元素用这个函数处理后返回新数组
		let arr = [10,20,30];
		let arr2 = arr.map(a=>a*10);
	reduce
		接收一个函数和一个初始值
		函数参数接收两个参数：
			第一个参数是上一次reduce处理的结果
			第二个参数是数组中要处理的下一个元素
			//遍历
	对象扩展
		keys(obj)：获取对象的所有key形成的数组
			Object.keys(person)
		values（obj）
		entries（obj）：获取key和value形成的二维数组
		assign（dest,....src）；将多个src对象的值拷贝到dest中
	数组扩展
		let arr = [10,20,2,30,3]
		arr.find(a=>a==30)
			find参数为一个函数
		arr.findIndex(a=>a==30)
		arr.includes(元素)

## Vue
Node.js
	基于事件循环的异步IO框架
	单线程运行
	js可以编写后台代码
	
vue
	npm init -y
		生成package.json
			配置文件信息
	npm install vue --save
		生成node_modules文件夹
	el：“#app”
		代表对应的html模板
	双向渲染绑定
		<input type="text" v-model="num">
		双向渲染绑定方法
		methods:{
            incr(){
                this.num++;
                }
        <input type="button" value="点我" v-on:click = "incr">
	每个vue应用都是通过new vue创建的
		存在各种属性
			el：element 选择器
			data：数据模型
				vue实例创建时，会尝试获取data中定义的所有属性，并进行视图渲染，并进行监视（响应式系统）
			methods
	

钩子函数 hf
	vue实例的生命周期
		创建实例、初始化、装载模板、渲染模板等

​		new vue（）
​		init event&lifecycle

		##### 				hf beforeCreate
​		init injections & reactivity
​		##### 				hf created
​		has el？
​		has template？
​		complie template and render

		##### 				hf beforeMount
​		create vm.$el
​		##### 		hf mounted(渲染？)
​		##### 		cycle hf beforeUpdated hf updatred
​		##### hf beforeDestory

​		Teardown watchers,child,componets,listeners

​		#### hf destoryed

		created() {
	        //加载数据,ajax到后台微服务拿取数据
	        
	    }
指令directives
	指令是带有v- 前缀的特性，预期值是单个js表达式（v-on）
	当表达式的值改变时，将其产生的连带影响，响应于dom
插值表达式
	{{xxx}}
	支持js，支持js内置函数（必须有返回值）
	可以直接获取vue中定义的数据或函数
	使用函数时需要使函数表达（sum（））
	可以使用v-text和x-html替代
v-text
	<span v-text = "name">undefined</span>

v-model
	只能在双向绑定中使用（表单）
	input、select、checkbox、radio、components
	
	<input type="checkbox" value="ios" v-model="language">ios
	{{language.join(",")}}
	多checkbox对应一个model时为数组
	单checkbox对应一个model时为bool
	radio对应input的value
	text、textarea对应model为字符串
	select单选对应字符串，多选对应数组

v-on
	v-on：事件名=“js或函数名”
	可以简写为@
		@click=“add”
	事件修饰符
		@contextMenu="incr"  右键点击
			 incr(ev){
             	ev.preventDefault();
             	//防止默认事件触发
		但是vue提倡不使用dom操作
		 	@contextMenu.prevent="incr($event)"
			//防止默认事件触发
		.stop:阻止事件冒泡到父元素
        .prevent:阻止默认事件发生
        .capture：使用事件捕获模式
        .self：仅限元素自身触发事件
        .once：单次
    按键修饰符
    	@keyup 键弹起事件
    	@keyup.13 回车弹起事件
    	//@keyup.enter
    	@keyup.65 a弹起事件
    	@keyup.65.13 组合按键
v-for
	v-for=“item in items”
	<li v-for="(user,index) in users":key="index">{{user.name}}-{{index}}</li>
	
v-if
    v-if=“bool表达式”
    如果false则不渲染
    	v-else-if
    	v-else

<span v-if="random>0.75">\>0.75</span>
<span v-else-if="random<0.5">\>0.5</span>
<span v-else>else</span>
<input type="button" v-model="random" @click="random=Math.random(1)">
v-if、v-else-if、v-else之间不能有[任何]标签

v-if，v-for间存在优先级
<ul>
    <li v-if="index>=1" v-for="(user,index) in users" :key="index">{{user.name}}-{{index}}</li>
</ul>

v-show
	v-show = “bool表达式”
    如果false则将style调为display：none
    	
v-bind
	为元素绑定属性值
	<input type="button" v-bind:value="random" @click="random=Math.random(1)">
    通常在样式中使用
    <input type="button" v-bind:class="{active1:store<1}" @click="store--">
    简写-：	
    <input type="button" :class="{active1:store<1}" @click="store--">
    
计算属性
	<div>{{new Date(birthday).getFullYear()+"年"+new Date(birthday).getMonth()+"月"+new Date(birthday).getDay()+"日"}}</div>
​	简化
	<div>{{date1}}</div>
​	computed:{
​            date1(b){
​                return new Date(this.birthday).toLocaleDateString();
​            }
​        }
​    	//computed在methods之外
​    	//可以直接使用计算属性，该属性为一个属性（数据模型），而不是一个方法
​    

    我们可以将同一函数定义为要给方法而不是计算属性，计算属性只有在它的相关依赖发生改变时才改变（然而缓存）

watch监听
	监听方法名要和被监听的model同名
	watch:{
            search(newval,oldval){ 
            }
        }
组件
	//vue中，所有的vue实例都是组件
	全局组件
		会有一个专用目录存储vue全局组件
		任何vue实例都可以使用全局组件
		Vue.component("组件名",{实例对象})
		

		<div id="app">//父组件
	    <counter></counter>//子组件
		</div>
	
		Vue.component("counter",{
	    template: "<button @click='num++'>点击。{{num}}</button>",
	    data(){
	        return{
	            num:0
	        }
	    }
	})
	局部组件
		<div id="app">
	    <component1></component1>
		</div>
		
		const component = {
	    template: "<div>???{{name}}</div>",
	    data() {
	        return {
	            name: "zhang"
	        }
	    }
	}
		const app = new Vue({
	    el:"#app",
	    components:{
	        component1 : component
	    }
	})
		//局部组件不能直接调用（不存在于vue域内），需要通过components：{实例名：组件名}进行调用

组件通讯
	父向子通讯 props//单向通讯
		父组件使用子组件时，自定义属性（属性名任意，属性值为要传递的数据）
		子组件通过props接收父组件的数据，通过自定义属性的属性名进行调用
	<div id="app">
        <counter :num1="num"></counter><!--num1为自定义属性名-->
    </div>
​    

Vue.component("counter",{
    template: "<button @click='num1++'>点击。{{num1}}</button>",
    props: ["num1"]
})

​	props验证
​		//没有约束力
​	props：{
​		num1:{
​			type:Number
​		}
​	}
​	
​		动静态传递
​			:num1="num"动态
​			:num1="0"动态
​			num2="num"静态（永远获取字符串）
​	
​	子向父传递


​	
​	
​	
​	
​	
​	
​	
​	
​	
​	