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
















