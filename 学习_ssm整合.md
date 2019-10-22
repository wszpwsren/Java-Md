# ssm整合案例

![1571044941272](C:\Users\feketerigo\AppData\Roaming\Typora\typora-user-images\1571044941272.png)

${pageContext.request.contextPath}

<%@taglib prexfix="c" uri = "xx/jsp/jtsl/core"

添加操作
	添加完成后，应当调用查询操作

```
@Select("select * from orders")
    @Results({
            @Result(id=true,property = "id",column = "id"),
            @Result(property = "orderNum",column = "orderNum"),
            @Result(property = "orderTime",column = "orderTime"),
            @Result(property = "orderStatus",column = "orderStatus"),
            @Result(property = "peopleCount",column = "peopleCount"),
            @Result(property = "payType",column = "payType"),
            @Result(property = "orderDesc",column = "orderDesc"),
            @Result(property = "product",column = "productId",javaType = Product.class,one = @One(select = "com.k.dao.OrdersDao.findById"))
    })
    public List<Orders> findAll();
```

# 订单分页

##### PageHelper

## 插件配置（选一个）

​	mybatis配置
​	<!--     
​	plugins在配置文件中的位置必须符合要求，否则会报错，顺序如下:    properties?, settings?,     typeAliases?, typeHandlers?,     objectFactory?,objectWrapperFactory?,     plugins?,     environments?, databaseIdProvider?, mappers? 
​	--> 
​	<plugins>    
​		<!-- com.github.pagehelper为PageHelper类所在包名 -->  		<plugin interceptor="com.github.pagehelper.PageInterceptor">        		<!-- 使用下面的方式配置参数，后面会有所有的参数介绍 -->    		<property name="param1" value="value1"/>    			</plugin> 
​	</plugins>
​	spring配置
​	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
​        <property name="dataSource" ref="dataSource" />
​        <!-- 传入PageHelper的插件 -->
​        <property name="plugins">
​            <array>
​                <!-- 传入插件的对象 -->
​                <bean class="com.github.pagehelper.PageInterceptor">
​                    <property name="properties">
​                        <props>
​                            <prop key="helperDialect">mysql</prop>
​                            <prop key="reasonable">true</prop>
​                        </props>
​                    </property>
​                </bean>
​            </array>
​        </property>
​    </bean>

## Service层配置

@Override
    public List<Orders> findAll() throws Exception {
        //pagenum：起始页码
        //pagesize：每页个数
        //该段代码必须写在查询代码前
        PageHelper.startPage(1,5);
        return ordersDao.findAll();
    }

## aside页面（请求）进行页面代码配置

```
<li id="system-setting"><a
						href="${pageContext.request.contextPath}/orders/findAll.do"?page="1&size=4"> 
<i class="fa fa-circle-o"></i> 订单管理
					</a></li>
```

## controller层代码

```
@RequestMapping("/findAll.do")
    public ModelAndView findAll(@RequestParam(name = "page",required = true,defaultValue = "1")int page,
                                @RequestParam(name = "size",required = true,defaultValue = "4")int size) throws Exception {
        List<Orders> all = ordersService.findAll(page,size);
        //pageinfo是一个分页Bean
        PageInfo ordersPageInfo = new PageInfo(all);
        ModelAndView mav = new ModelAndView();
        mav.addObject("pageInfo",ordersPageInfo);
        mav.setViewName("orders-page-list");
        return mav;
    }
```



## page页面（响应）进行返回数据处理（遍历）

```
<c:forEach items="${pageInfo.list}" var="orders">
```

pagehelper 将数据封装进.list对象中

## 页码处理

<div class="box-tools pull-right">
                        <ul class="pagination">
                            <li>
                                <a href="${pageContext.request.contextPath}/orders/findAll.do?page=1&size=${pageInfo.pageSize}" aria-label="Previous">首页</a>
                            </li>
                            <li><a href="${pageContext.request.contextPath}/orders/findAll.do?page=${pageInfo.pageNum-1}&size=${pageInfo.pageSize}">上一页</a></li>
                            <c:forEach begin="1" end="${pageInfo.pages}" var="pageNum">
								<li><a href="${pageContext.request.contextPath}/orders/findAll.do?page=${pageNum}&size=${pageInfo.pageSize}">${pageNum}</a></li>
							</c:forEach>
                            <li><a href="${pageContext.request.contextPath}/orders/findAll.do?page=${pageInfo.pageNum-1}&size=${pageInfo.pageSize}">下一页</a></li>
                            <li>
                                <a href="${pageContext.request.contextPath}/orders/findAll.do?page=${pageInfo.pages}&size=${pageInfo.pageSize}" aria-label="Next">尾页</a>
                            </li>
                        </ul>
                    </div>

## 改变页码数量处理

​	页面change函数

				+ ```
	function changePageSize() {
				//获取下拉框的值
				var pageSize = $("#changePageSize").val();
	
	​		//向服务器发送请求，改变没页显示条数
	​		location.href = "${pageContext.request.contextPath}/orders/findAll.do?page=1&size="+pageSize;
	  }
	```


​	

# Spring Security

认证：建立主体

授权：授权之前建立用户验证主体

## 使用

导入依赖

web.xml配置filter

spring security核心配置（配置主体）

userdetails
	用户封装当前认证的用户信息
	实现类user
userdeailsservice
	规范化了认证方法

spring security
	不需要controller，由ss控制
	自己写的service必须实现userdetailsservice
	
	授权：
		user创建的第三个参数
		需要一个list集合
		该集合的泛型为simpleGrantedAuthority
		（ROLE_USER）

FilterChainProxy

结果集封装（bean/@result）

选择查询 sql语句/@result

查询逻辑



# 权限控制

## 方法级
JSR250注解
	spring security 中
		<security:global-method-security jsr250-annotations="enabled"/>
	Controller方法上指定注解
		@RolesAllowed
	Pom中导入javax.annotation/jsr250
	
@secured注解
    spring security 中
        <security:global-method-security secured-annotations="enabled"/>
    Controller方法上指定注解
    	@Secured("ROLE_ADMIN")
    		//springsecurity中需要添加role_前缀
    		//security提供的注解
支持表达式注解
	spring security 中
		<security:global-method-security pre-post-annotations="enabled"/>
	Controller方法上指定注解
		
		@PreAuthorize
			方法调用前，基于表达式结果限制方法访问
		@PostAuthorize
			允许调用方法，但是表达式结果为false，则抛出安全性异常
		@PostFilter
			允许方法调用，但按照表达式来过滤方法的结果
		@PreFilter
			允许方法调用，但必须在进入方法之前过滤输入值
		
​    
​     Spring Security允许我们在定义URL访问或方法访问所应有的权限时使用Spring EL表达式，在定义所需的访问权限时如果对应的表达式返回结果为true则表示拥有对应的权限，反之则无。Spring Security可用表达式对象的基类是SecurityExpressionRoot，其为我们提供了如下在使用Spring EL表达式对URL或方法进行权限控制时通用的内置表达式。

表达式					描述
hasRole([role])	当前用户是否拥有指定角色。
hasAnyRole([role1,role2])	多个角色是一个以逗号进行分隔的字符串。如果当前用户拥有指定角色中的任意一个则返回true。
hasAuthority([auth])	等同于hasRole
hasAnyAuthority([auth1,auth2])	等同于hasAnyRole
Principle	代表当前用户的principle对象
authentication	直接从SecurityContext获取的当前Authentication对象
permitAll	总是返回true，表示允许所有的
denyAll	总是返回false，表示拒绝所有的
isAnonymous()	当前用户是否是一个匿名用户
isRememberMe()	表示当前用户是否是通过Remember-Me自动登录的
isAuthenticated()	表示当前用户是否已经登录认证成功了。
isFullyAuthenticated()	如果当前用户既不是一个匿名用户，同时又不是通过Remember-Me自动登录的，则返回true。





## 页面标签级
	pom导包
		spring-security-taglibs
	页面导入
		<%@taglib prefix="security" uri="http://www.springframework.org/security/tags" %>
		
	使用
	authentication
		用于获取认证对象（操作的用户信息）
	authorize
		用户控制页面上标签的显示
	accesscontrollist
		用于鉴定acl权限
	
	<security:authentication property="principal.username">
	用户获取用户名，注意principal
	
	<security:authorize access="hasRole('ADMIN')">
	用于指定查看权限
	需要将审查元素框在选择范围内
	
	<!-- 配置监听器，监听request域对象的创建和销毁的 -->
	获取ip地址（获取request对象）
  <listener>
    <listener-class>org.springframework.web.context.request.RequestContextListener</listener-class>
  </listener>
	
	String ip = request.getRemoteAddr();
	
	maven中javax.servlet的scope需要修改？
	compile/provided？
	
	//获取用户
        User user = (User) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
        String username = user.getUsername();
	
	
	
	
	
	