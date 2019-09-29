Spring

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
​			//成员变量的复合变化（单例存在变化）
​	IOC
​		由工厂提供资源，由工厂寻找资源
​		将控制权交给框架，由框架创建对象
​	Spring AOP
​		基于注解配置
​	Spring JCL
​		commons-logging
## IOC概念和springIOC

​	spring中基于xml的ioc环境搭建
​		核心容器对象ApplicationContext
​	核心容器的两个接口引发出的问题：
​    	ApplicationContext:     单例对象适用              采用此接口
​          它在构建核心容器时，创建对象采取的策略是采用立即加载的方式。也就是说，只要一读取完配置文件马上就创建配置文件中配置的对象。
​          BeanFactory:            多例对象使用
​          它在构建核心容器时，创建对象采取的策略是采用延迟加载的方式。也就是说，什么时候根据id获取对象了，什么时候才真正的创建对象。

Spring bean对象的创建方式
	使用默认构造函数创建
		<bean id="accountService" class="com.k.service.impl.AccountServiceImpl">
		在Spring配置文件中使用bean标签，配id和class属性后，为采用默认构造函数创建bean对象，没有默认构造函数则无法构造
	使用类中的方法创建对象，并存入Spring容器
		<bean id="accout" factory-bean="environment" factory-method="getProperty"></bean>
	使用类中的静态方法创建对象
		<bean id="accout" class="netscape.javascript.JSException" factory-method="aspectOf"></bean>
	Bean的作用范围调整
		//默认单例，通过scope调整
	<bean id="accout" class="netscape.javascript.JSException" factory-method="aspectOf" scope="prototype"></bean>
		singleton 单例
		protype 多例
		request	作用于web应用的请求范围
		session	作用于web应用的会话范围
		global-session	作用于集群环境的会话范围（全局会话范围）
	bean对象的生命周期
		单例对象
			单例对象的生命周期和容器相同
		多例对象
			生成：使用对象时对象生成
			使用：对象使用过程中一直存在
			销毁：对象被GC销毁

## 依赖注入（Dependency Injection）
	Spring依赖注入 Dependency Injection
	    依赖关系的管理交由Spring维护
	        称为依赖注入
	    在当前类需要用到其他类的对象，由Soring为我们提供，我们只需要在配置文件中说明
	        依赖注入
	            能注入的数据：有三类
	                基本类型及String
	                其他bean类型（配置文件或注解配置的bean）
	                复杂类型/集合类型
	            注入的方式：
	                构造函数提供
	                set方法提供
	                注解提供
​	//经常变化的数据，并不适用于注入的方式
​	
​	

```
构造函数注入
​	    标签:constructor-arg
​	    标签位置：bean标签内部
​	    标签属性：
​	        type:用于指定要注入的数据的数据类型，该数据类型也是构造函数中某个或某些参数的类型
​	        index:用于指定要注入的数据给构造函数中指定索引位置的参数赋值。从0开始
​	        name:用于指定给构造函数中指定名称的参数赋值
​	        ——————————————以上用于指定给哪个参数赋值——————————————
​	        value:用于提供基本类型和String类型的数据
​	        ref:用于引用其他bean对象（Spring中的ioc核心容器中出现过的bean对象）
​	    -->
​	    <bean id="accountService" class="com.k.service.impl.AccountServiceImpl">
​	        <constructor-arg name="age" value="18"></constructor-arg>
​	        <constructor-arg name="name" value="test"></constructor-arg>
​	        <constructor-arg name="birthday" ref="date"></constructor-arg>
​	    </bean>
​	    <!--配置一个日期对象-->
​	    <bean id="date" class="java.util.Date"></bean>
​	    
	    //在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功
	    //改变了bean对象的实例化方式，如果创建对象时，不需要使用这些数据，也必须提供
```




```
<!--set方法注入
        标签 propertiy
        标签位置：bean内部
        标签属性：
            name:用于指定注入时调用的[方法名称]
            ——————————————以上用于指定给哪个参数赋值——————————————
            value:用于提供基本类型和String类型的数据
            ref:用于引用其他bean对象（Spring中的ioc核心容器中出现过的bean对象）
        -->
        <bean id="accountService2" class="com.k.service.impl.AccountServiceImpl2">
            <property name="name" value="张三"></property>
            <property name="age" value="21"></property>
            <property name="birthday" ref="date"></property>
        </bean>
        //创建对象时，没有明确限制，可以直接使用默认构造函数
        //如果有某个成员必须有值，则获取对象时set方法无法保证一定注入
```

```
<!--注入集合类型
        用于给Collection结构集合注入的标签
            list array set
        用于给map结构集合注入的标签
            map props
        结构相同，标签可互换
    -->
        <bean id="accountService3" class="com.k.service.impl.AccountServiceImpl3">
            <property name="myStrs">
                <array>
                    <value>AAA</value>
                    <value>AAB</value>
                </array>
            </property>
            <property name="myList">
                <list>
                    <value>AAA</value>
                    <value>AAB</value>
                </list>
            </property>
            <property name="mySet">
                <set>
                    <value>AAA</value>
                    <value>AAB</value>
                </set>
            </property>
            <property name="myProp">
                <props>
                    <prop key="testC">ccc</prop>
                    <prop key="sd">222</prop>
                </props>
            </property>
            <property name="myMap">
                <props>
                    <prop key="testC">ccc</prop>
                    <prop key="sd">222</prop>
                </props>
            </property>
        </bean>
```

```
   账户的业务层实现类
     <bean id="" CLASS="" scope="" init-method="" destory-method = "">
         <property name="" value=""></property>
         </bean>
     其中
     用于创建对象的
          <bean></bean>
          @Component
          把当前类对象存入Spring容器中
          属性：
              value：用于指定bean的id，默认类名，首字母小写
          @Controller：表现层
          @Service：业务层
          @Repository：持久层
              作用与属性和component一样
              是Spring框架提供的明确的三层使用，区分三层对象
     用于注入数据的
          <property></property>
          @Autowired:
              自动按照类型注入，只要容器中有唯一的一个bean对象类型和要注入的类型匹配，就可以注入
              如果ioc容器中没有任何bean类型和要注入的变量类型匹配，则报错
              如果ioc容器中有多个bean类型和要注入的变量类型匹配，则通过变量名称与ioc中key相对比，如果都不相同，则报错
                  位置：
                      可以是变量上，也可以是方法上
              //注解注入时，set方法可以不使用
          @Qualifier
              作用：在按照类中注入的基础上，再按照名称注入。
              它给类成员注入时不能单独使用，但给方法注入时可以单独使用
                  属性
                      value：用于指定注入bean的id
          @Resource
              作用：直接按照bean的id注入，可以独立使用
                  属性：
                      name：用于指定bean的id
              以上三个注入都只能注入其他bean类型的数据，基本类型和String类型无法注入
              集合类型只能通过xml实现
          @Value
              用于注入基本类型及String类型
                  属性：value：用于指定数据的值。可以使用Spring中的SpEl(spring的el表达式)
                      //SpEL-》${表达式}
                      //是谁的el从谁找数据
     用于改变作用范围的
          <scope></scope>
          @Scope
              作用：用于指定bean的作用范围
              属性：
                  value：指定范围的取值，singleton，prototype
     和生命周期相关的
          xxmethod xxmethod
          @preDestory
              用于指定销毁方法
          @preConstruct
              用于指定初始化方法
```

   

# spring基于注解的IOC和IOC的案例

Spring中ioc的常用注解

案例使用xml方式和注解方式实现单表crud操作
	dao层 dbutils

使用注解方式实现ioc
	Spring的一些新注解使用

Spring和Junit的整合

# spring中的AOP和基于XML/注解的AOP配置

# spring中的JDBCTemplate及Spring的事务控制

# Junit spring

junit不会判断我们是否使用了spring框架，并且没有生成ioc容器

解决：
	导入spring整合junit

```
//使用Spring提供的junit
@RunWith(SpringJUnit4ClassRunner.class)
//告知spring运行器，spring和ioc的创建方式
@ContextConfiguration(classes = SpringConfiguration.class)
public class AppTest 
{
    /**
     * Rigorous Test :-)
     */
    @Autowired
    ApplicationContext ac;
    @Test
    public void testFindAll() {}
```

# AOP

## 完善account案例

ThreadLocal
	把Connection和当前线程绑定，从而使一个线程中只有一个连接
	事务控制都应该存在于service层
	

```
private ThreadLocal<Connection> tl = new ThreadLocal<Connection>();
```

```
//由于线程和连接池绑定，当连接关闭后，线程与连接的绑定没有取消，需要手动将其解绑
public void removeConnection(){
    tl.remove();
}
connectionUtils.removeConnection();
```



## 分析问题

## 动态代理

特点：字节码随用随创建，随用随加载
作用：不修改源码的基础上对方法增强
分类：
	1、基于接口的动态代理
		Proxy（JDK）
			Proxy类的newProxyInstance方法创建
			//要求被代理类最少实现要给接口
			newProxyInstance
				参数：
					Classloader 
						用于加载代理对象字节码的，和被代理对象共用类加载器
					CLass<?>(interface)
						让代理对象和被代理对象有相同的方法
						xx.getClass().getiInterfaces()
					InvocationHandler
						如何代理
						一般写一个该接口的实现类，通常为匿名内部类
						此接口的实现类是谁用谁写
						

```
				/**
                 * 作用：执行被代理对象的任何接口方法都会经过该方法（拦截？）
                 * proxy-》代理对象的引用
                 * method-》当前执行的方法
                 * args-》当前执行方法所需的参数
                 * return-》和被代理对象有相同的返回值
                 */
                (proxy, method, args) -> {
                    
​                return method.invoke(accountService, args);
​            }
```

​	2、基于子类的动态代理
​	Enhancer(cglib)
​	创建代理对象的要求：
​		被代理类不能为最终类
​		create方法的参数：
​			class：字节码
​				用于指定被带离对象的字节码
​			callback：用于提供增强的代码
​				一般写该方法的子接口实现类：methodInterceptor

## 动态代理的实现

```
<!--配置代理的service-->
    <bean id="proxyAccountService" factory-bean="beanFactory" factory-method="getAccountService"></bean>

<!--配置beanfactory-->
<bean id="beanFactory" class="com.itheima.factory.BeanFactory1">
    <!-- 注入service -->
    <property name="accountService" ref="accountService"></property>
    <!-- 注入事务管理器 -->
    <property name="txManager" ref="txManager"></property>
</bean>
```

## 解决案例问题

## AOP

面向切面编程
	基于动态代理
	//在程序运行中，在不修改源码的情况下增强方法

## AOP相关术语

Joinpoint
	指被拦截到的点（方法），spring只支持方法类型的连接点
Pointcut
	指我们要对哪些Joinpoint进行拦截的定义
Advice
	拦截/通知
	拦截到Joinpoint后要做的事
	通知类型：前置，后置，环绕，异常，最终
		在环绕通知中有明确的切入点方法调用
Introduction
	引介 
		在不修改类代码的前提下，可以在运行器为类动态添加方法/field
Target
	代理目标对象
Weaving
	织入
	指把增强应用到目标对象的过程
Proxy
	代理
	被织入
Aspect
	切面
	指切入点和通知的结合
	

## 事件

```
需要编写业务代码
把共用代码抽取出来，制作成通知
在配置文件中，声明切入点和通知间的关系（切面）
spring：监控切入点方法的执行，监控到切入点使用代理，根据通知类别进行织入
```

## AOP基于XML和注解的配置

```
<!--配置spring ioc-->
<!--    <bean id="accountService" class="com.k.service.impl.AccountImpl"></bean>-->
    <context:component-scan base-package="com.k"></context:component-scan>
    <!--Spring AOP配置-->
    <!--把通知的bean加入ioc-->
    <bean id="logger" class="com.k.utils.Logger"></bean>
    <!--使用aop:config标签标明开始aop的配置-->
    <aop:config >
        <!--使用aspect配置切面
            id:给切面提供一个唯一表示
            ref：是指定通知类bean 的id
            -->
        <aop:aspect id="logAdvice" ref="logger">
            <!--配置通知类型,织入过程
            method:指定哪个方法
            pointcut:用于指定切入点表达式
                //指对业务层哪些方法进行织入
                表达式：
                    execution
                    访问修饰符 返回值 包名.包名.类名.方法名（参数列表）
                    public void com.k.service.impl.AccountServiceImpl.saveAccount()
                    //访问修饰符可以省略
                    //包名可以使用..表示当前包及其子包
                    //类名方法名可以使用*通配
                    参数列表：
                        可以写数据类型：
                            基本类型写名称
                            引用类型写包名.类名 java.lang.String
                        可以使用*通配
                        可以使用..表示全匹配
                    全统配写法
                    * *..*.*(..)
                    实际写法
                    * com.k.service.impl.*.*(..)
            -->
            <aop:before method="printLog" pointcut="execution(public void com.k.service.impl.AccountImpl.saveAccount())"></aop:before>
        </aop:aspect>
    </aop:config>
```

​	配置前置before
​	配置后置after-returning
​	配置异常after-throwing
​	配置最终after
​	配置环绕 around
​		//环绕通知有明确的切入点方法调用
​		spring框架提供了一个接口：proceedingJoinPoint
​			存在方法proceed（）；
​				此方法就相当于明确调用切入点方法
​				该接口可以作为环绕通知的方法参数，程序执行时，spring框架会为我们体哦给你该接口的实现类

```
<!--配置切入点表达式
                id:用于指定表达式的唯一标识
                //此标签写在aspect标签内，只能当前切面可用
                //或将pointcut放置在aspect前，可供其他切面使用
            -->
            <aop:pointcut id="pt" expression="execution(public void com.k.service.impl.AccountImpl.saveAccount())"/>
            <aop:after method="printLog" pointcut-ref="pt"></aop:after>>
```

```
 /**
     * 环绕通知：是spring框架为我们提供的可以在代码中手动控制增强方法何时执行的方式
     * @param pjp
     * @return
     * @throws Throwable
     */
    public Object aroundPrinting(ProceedingJoinPoint pjp) throws Throwable {
        Object rtValue =null;
        try {
            Object[] args = pjp.getArgs();//获取方法执行所需的参数
            System.out.println("around日志/前置");
            rtValue = pjp.proceed(args);//明确调用业务层方法（切入点方法）
            System.out.println("around日志/后置");
            return rtValue;
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            System.out.println("around日志/异常");
        }finally {
            System.out.println("around日志/最终");
        }
    }
```

## 注解

​	@Aspect
​		表示当前类是一个切面类
​	//配置切入点
​	@Pointcut（"execution(public void com.k.service.impl.AccountImpl.saveAccount())"）
​	private void pt（）{}
​	@Before("pt()")
​	@After-returning("pt()")
​	@AfterThrowing("pt()")
​	@After("pt()")
​	@Around

```
xml配置
//开启注解aop支持
<aop:aspectj-sutoproxy>
```

SPringJdbcTemplate
	JdbcTemplate作用
		与数据库交互，实现crud
	如何创建该对象
	常用方法
	

```
//用于代替注解控制（bug），通过环绕通知解决
@Around("pt()")
public Object aroundAdvice(ProceedingJoinPoint pjp) {
    Object rtValue = null;
    try {
        Object[] args = pjp.getArgs();
        this.beginTransaction();
        rtValue = pjp.proceed(args);
        this.commit();
        return rtValue;
    } catch (Throwable throwable) {
        this.rollback();
        throw new RuntimeException(throwable);
    } finally {
        this.release();
    }
}
```

Spring的声明式事务控制
	事务存在于业务层
	spring提供了事务控制的接口
	spring事务控制基于aop
	

PlatformTransactionManager接口
实现类Datasource/hibernate
XML
注解