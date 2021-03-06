

# Mybatis

# mybatis入门

mabatis概述
	框架：构建-构建之间的交互方法
		//解决方案
		mybatis：持久层框架//dao层
	持久层技术解决方案
		JDBC技术：
			Connection
			PreparedStatement
			ResultSet
		SpringJdbcTemplate
			Spring中对jdbc的简单封装
		Apache的DBUtils：
			Apache对Jdbc的简单封装
		//Jdbc是规范，JdbcTemplate和DBUtils都是工具类
		

		mybatis内部封装了jdbc，只需要配置xml
		ORM：Object Relational Mapping 对象关系映射
			把数据库表和实体类及实体类的属性对应起来

mybatis环境搭建
	创建maven工程并导入坐标
	创建实体类和dao的接口
	创建Mybatis的主配置文件//SqlMapConfig.xml
	创建映射配置文件//UserDao.xml
	ZY：
		创建UserDao.xml 和UserDao.java时名称是为了和之前的知识一致，Mybatis中把数据连接层的操作接口名称和映射文件也叫做：Mapper，即UserDao->UserMapper
		创建resources文件夹时，需要创建包或者单个创建，因为mybatis的映射配置文件位置必须和dao接口的包结构相同
		映射配置文件的mapper标签namespace属性的取值必须是dao接口的全限定类名
		映射配置文件的操作配置，id属性的取值必须是dao接口的方法名
		//无需再写Dao层的实现类
		

		由于使用代理方式创建代理对象，无需创建dao层实现对象
		dao层xml文件mapper执行语句必须填写返回值类型（domain类）
mybatis入门
        读取配置文件
        创建SQLSessionFactory工厂
        创建SqlSession
        创建Dao接口的代理对象
        执行dao中的方法
        释放资源
            //需要在映射配置中告知mybatis要封装到哪个实体类中
            配置的方式：指定实体类的全限定类名
	使用注解
		不使用Dao.xml，在dao接口方法上使用@selecet注解，并指定sql语句
		同时将mapper注解更改为class
		

```
 <!--指定映射配置文件的位置，指的是每个dao独立的配置文件
        如果使用注解配置，那么此处应该使用class属性指定被注解的dao全限定类名-->
    <mappers>
       <!-- <mapper resource="com/k/Dao/UserDao.xml"></mapper>-->
        <mapper class="com.k.Dao.UserDao"></mapper>
    </mappers>
```
	@Test
	public void test(){
	    //读取配置文件，只能读取类路径的配置文件
	    //或者使用ServletContext对象的getRealPath（）
	    InputStream sqlMapConfig = MybatisTest.class.getClassLoader().getResourceAsStream("SqlMapConfig.xml");
	    //创建SqlSessionFactory工厂
	    //使用了构建者模式（builder）
	    SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
	    SqlSessionFactory factory = builder.build(sqlMapConfig);
	    //使用工厂生产SqlSession对象
	    //解耦
	    SqlSession session = factory.openSession();
	    //使用SqlSession创建Dao接口的代理对象
	    UserDao userDao = session.getMapper(UserDao.class);
	    //使用代理对象执行方法
	    List<User> all = userDao.findAll();
	    for (User user : all) {
	        System.out.println(user);
	    }
	    //释放资源
	    session.close();
	}
自定义mybatis分析

​	myvatis在使用代理dao的方式实现crud时
​		1、创建代理对象
​		2、在代理对象中调用selectList方法
​		![1567667902669](C:\Users\feketerigo\AppData\Roaming\Typora\typora-user-images\1567667902669.png)



自定义mybatis框架
	sqlsessionfactorybuilder接收sqlmapconfig.XML，创建sqlsessionfactory
	

# mybatis基本使用

mybatis单表crud操作

Dao.xml

    <select id="findAll" resultType="com.k.domain.User">
        select * from user
    </select>
    <insert id="saveUser" parameterType="com.k.domain.User">
        insert into user(username,address,sex,birthday)values(#{username},#{address},#{sex},#{birthday});
    </insert>
    <update id="updateUser" parameterType="com.k.domain.User">
        update user set username=#{username},address=#{address},sex=#{sex},birthday=#{birthday} where id= #{id};
    </update>
    <delete id="deleteUser" parameterType="java.lang.Integer">
        delete from user where id =#{id};
    </delete>
    <select id="findById" parameterType="java.lang.Integer" resultType="com.k.domain.User">
        select * from user where id = #{id};
    </select>
    <select id="findByName" parameterType="java.lang.String" resultType="com.k.domain.User">
        select * from user where username like #{username}
        /*select * from user where username like '%${value}%'*/
    </select>
    <select id="findTotal" resultType="java.lang.Integer">
        select count(id) from user ;
    </select>

## ↓

Test

```
public class MybatisTest {
    private UserDao userDao;
    private SqlSession session;
    @Before
    public void init(){
        //读取配置文件
        InputStream sqlMapConfig = MybatisTest.class.getClassLoader().getResourceAsStream("SqlMapConfig.xml");
        //创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(sqlMapConfig);
        //使用工厂生产SqlSession对象
        session = factory.openSession();
        //使用SqlSession创建Dao接口的代理对象
        userDao = session.getMapper(UserDao.class);
    }
    @After
    public void destory(){
        session.commit();
        session.close();
    }
    @Test
    public void test(){
```

        //使用代理对象执行方法
        List<User> all = userDao.findAll();
        for (User user : all) {
            System.out.println(user);
        }
    }
    @Test
    public void testsave(){
        User user = new User();
        user.setUsername("name");
        user.setAddress("北京");
        user.setSex("男");
        user.setBirthday(new Date());
        //使用代理对象执行方法
        userDao.saveUser(user);
    }
    @Test
    public void testUpdate(){
        User user = new User();
        user.setId(46);
        user.setUsername("UPDATE");
        user.setAddress("上海");
        user.setSex("男");
        user.setBirthday(new Date());
        userDao.updateUser(user);
    }
    @Test
    public void testDelete(){
        userDao.deleteUser(48);
    }
    @Test
    public void testFind(){
        User user = userDao.findById(49);
        System.out.println(user);
    }
    @Test
    public void testFindByName(){
        List<User> byName = userDao.findByName("%王%");
        for (User user : byName) {
            System.out.println(user);
        }
    }
    @Test
    public void testFindTotal(){
        int total = userDao.findTotal();
        System.out.println(total);
    }
插入时回显id值

```
<insert id="saveUser" parameterType="com.k.domain.User">
        <!--配置插入操作 -->
        <selectKey keyProperty="id" keyColumn="id" resultType="int" order="AFTER">
            select last_insert_id();
        </selectKey>
        insert into user(username,address,sex,birthday)values(#{username},#{address},#{sex},#{birthday});
    </insert>
    
    test中只需要获取user.id即可
```

mybatis参数和返回值
	传递pojo对象（bean对象）
		OGNL表达式（apache）Object Graphic Navigation Language，对象图导航语言
		通过对象的取值方法获取数据。写法上把get省略
		获取对象名称
			类中：uesr.getUsername();
			OGNL:user,username
	结果类型的封装
		输出pojo对象

```
<mapper namespace="com.k.Dao.UserDao">
    <resultMap id="userMap" type="com.k.domain.User">
        <id property="userId" column="id"></id>
    </resultMap>
    <!--配置查询所有-->
    <select id="findAl" resultMap="userMap">
​    select * from user
</select>
```

​	输出pojo列表

​	输出pojo列表
​		字段映射方式（当列名和类属性名不同时）：
​			resultMap：

mybatis的dao编写
	PreparedStatement的执行方法
		execute：CRUD：返回值bool类型，表示是否有结果集
		executeUpdate：CUD，返回值是影响数据库记录的行数
		executeQuery：R，返回值是结果集ResultSet对象

mybatis配置的细节/标签
	可以在标签内部配置连接数据库的信息。也可以通过属性引用外部配置文件信息
        resource属性： 常用的
            用于指定配置文件的位置，是按照类路径的写法来写，并且必须存在于类路径下。
        url属性：
            是要求按照Url的写法来写地址
            URL：Uniform Resource Locator 统一资源定位符。它是可以唯一标识一个资源的位置。
            它的写法：
                http://localhost:8080/mybatisserver/demo1Servlet
                协议      主机     端口       URI

​        URI:Uniform Resource Identifier 统一资源标识符。它是在应用中可以唯一定位一个资源的。
​        
​    

```
file:\\\Users\feketerigo\IdeaProjects\test06_profiles_test\src\main\resources\jdbcC.properties
```


​      

```
  <!--配置别名-->
    <typeAliases>
        <!--type全限定类名。alias属性指定别名，当指定了别名不再区分大小写-->
        <typeAlias type="com.k.domain.User" alias="user"></typeAlias>
         <!--package指定要配置别名的包，当指定之后，该包下的实体类都会注册别名，类名为别名，不区分大小写-->
        <package name="com.k.domain"/>
    </typeAliases>
```




```
<mappers>
        <mapper resource="Dao/UserDao.xml"></mapper>
        <!--指定dao接口所在的包，当指定之后不需要再写mapper-->
        <package name="com.k.Dao"/>
<!--        <mapper class="com.k.Dao.UserDao"></mapper>-->
    </mappers>
```



# mybatis深入和多表

mybatis连接池
	容器是一个集合对象，必须为线程安全的，该集合必须实现队列特性
	配置位置：
		主配置文件datasource标签，type标签表示采用什么连接方式
			type取值：
				pooled
					传统javax.sql.DataSource规范的连接池
					mybatis中有针对规范的实现
				unpooled
					传统的获取连接方式，虽然实现了javax.sql.DataSource接口，但是没有实现池
				jndi
					采用服务器提供的jndi技术实现来获取DataSoruce对象，不同的服务器所能拿到的DataSource是不一样的
					如果不是web或maven的war工程是不能使用的
					//tomcat服务器，采用的连接池是dbcp连接池
mybatis事务控制及设计方法
	基于sqlsession的提交与回滚
	自动提交
		factory生成session时将参数设置为true

mybatis基于xml配置的动态SQL语句的使用
	

​	mapper配置文件中的几个标签
​		<if>
​		<where>
​		<foreach>

<select id="findUserByIds" resultType="User" parameterType="Queryvo">
        select * from user 
        <where>
            <if test="ids != null and ids.size()>0">
                <foreach collection="ids" open="and id in(" close=")" item="id" separator=",">
                    #{id}
                </foreach>
            </if>
        </where>
    </select>
​	<sql>

```
<!--抽取重复sql-->
    <sql id="default">
        select * from USER
    </sql>

<!--配置查询所有-->
```

<select id="findAll" resultType="com.k.domain.User">
    <include refid="default"></include>
</select>



mybatis多表查询[[]]
	一对多
		建立表：用户表，账户表
			使用外键在账户表中添加
		建立实体类：用户类，账户类
			让用户和账户的实体类能体现关系
		建立两个配置文件
			用户配置
			账户配置
		实现配置
			查询用户时，可以同时得到用户下的账户信息
	多对一
		

```
<!--配置查询所有-->
    <select id="findAll" resultMap="account_UserMap">
        select u.*,a.id as aid,a.uid,a.money from account a,user u where u.id = a.uid
    </select>
    <!--定义封装account和user的resultmap-->
    <resultMap id="account_UserMap" type="account">
        <id property="id" column="aid"></id>
        <result property="uid" column="uid"></result>
        <result property="money" column="money"></result>
        <!--一对一的封装映射，配置封装user的内容-->
        <association property="user" column="uid" javaType="user">
            <id property="id" column="id"></id>
            <result column="username" property="username"></result>
            <result column="address" property="address"></result>
            <result column="sex" property="sex"></result>
            <result column="birthday" property="birthday"></result>
        </association>
    </resultMap>
```

​	一对一

 

```
<!--定义user的resultmap-->
    <resultMap id="user_AccountMap" type="user">
        <id property="id" column="id"></id>
        <result property="username" column="username"></result>
        <result property="address" column="address"></result>
        <result property="sex" column="sex"></result>
        <result property="birthday" column="birthday"></result>
        <!--配置user对象中accounts集合的映射-->
        <collection property="accounts" ofType="account">
            <id column="aid" property="id"></id>
            <result column="uid" property="uid"></result>
            <result column="money" property="money"></result>
        </collection>
    </resultMap>
    <!--配置查询所有-->
    <select id="testFindUser" resultMap="user_AccountMap">
        select * from user u left join account a on u.id = a.uid
    </select>
```

​	多对多

```
<resultMap id="roleMap" type="role">
        <id property="roleId" column="rid"></id>
        <result property="roleName" column="ROLE_NAME"></result>
        <result property="roleDesc" column="ROLE_DESC"></result>
        <collection property="users" ofType="user">
            <id column="id" property="id"></id>
            <result column="username" property="username" ></result>
            <result column="address" property="address" ></result>
            <result column="sex" property="sex" ></result>
            <result column="birthday" property="birthday" ></result>
        </collection>
    </resultMap>
    <!--抽取重复sql-->
    <sql id="default">
        select * from role r
    </sql>
    <!--配置查询所有-->
    <select id="findAll" resultMap = "roleMap">
        select * from role
    </select>
    <select id="findR_U" resultMap="roleMap">
        SELECT u.*,
        r.id as rid,
        r.ROLE_NAME,
        r.ROLE_DESC
        FROM role r
        left join user_role ur
        on R.ID = ur.RID
        LEFT JOIN user u
        on u.id = ur.uid
    </select>
```

JNDI ：Java Naming and Directory Interface
	作用为模仿windows系统的注册表
	key：路径（String）
		路径固定，名称指定
	value：jndi中是对象（Object）
		通过配置文件的方式指定



# mybatis缓存和注解开发

mybatis加载时机
	延迟加载
		查询用户时，账户信息按需求加载
		需要配置xml-lazyLoadingEnabled
	立即加载
		查询账户时，用户信息应同时加载
	表关系
		一对多，多对多：延迟加载
		多对一，一对一：立即加载
mybatis一级缓存/二级缓存
	缓存
		存在于内存的临时数据
		减少和数据库的交互次数，提高执行效率
	什么样的数据能使用缓存
		适用于缓存的数据：
			经常查询，并且不常改动
			数据的正确与否不影响最终结果的
		不适用于缓存的数据：
			经常改动的数据
			数据的正确性影响最终结果的
	一级缓存
		MyBatis中SqlSession对象的缓存
			当查询之后，查询的结果会存入SqlSession提供的内存中
				该区域的结构为Map
				当我们再次查询同样的数据，MyBatis会先从SqlSession中查看是否有数据，如果有则拿出使用
				当SqlSession对象消失时，一级缓存消失
				//sqlSession.clearCache
			一级缓存的同步
				调用sqlSession的CUD，commit，close等方法时，会清空一级缓存
				
	二级缓存
		Mybatis的SqlSessionFactory对象的缓存，由同一个Factory创建的SqlSession共享缓存
		使用步骤：
			让Mybatis框架支持二级缓存-》config
			<setting name="cacheEnable" value="true"/>
			让当前的映射文件支持二级缓存-》mapper
			<cache></cache>
			让当前的操作支持二级缓存-》select标签
			<select id="testFindUser" resultType="user" useCache="true">
			//二级缓存中村放的内容是数据
				//{xx:"xx",yy:"yy"}

mybatis注解开发
	//Dao层注解
	环境搭建
		//使用注解开发,当存在xml时，会报错
		//当实体类中名称与数据库名称不同时，使用@results注解
		

```
@Select("select * from user")
    @Results({
            @Result(id=true,column = "id",property = "userId"),
            @Result(column = "username",property = "userName"),
            @Result(column = "address",property = "address"),
            @Result(column = "sex",property = "sex"),
            @Result(column = "birthday",property = "birthday")
    })
    List<User> findAll();
```

​	//当为其他方法使用时

```
 @Select("select * from user")
    @Results(id ="userMap",value={
            @Result(id=true,column = "id",property = "userId"),
            @Result(column = "username",property = "userName"),
            @Result(column = "address",property = "address"),
            @Result(column = "sex",property = "sex"),
            @Result(column = "birthday",property = "birthday")
    })
    List<User> findAll();

@Insert("insert into user(username,address,sex,birthday)values(#{username},#{address},#{sex},#{birthday})")
@ResultMap(value={"userMap"})
void saveUser(User user);
```

​	单表CRUD（代理）
​	多表R

```
@Select("select * from user")
    @Results(id ="userMap",value={
            @Result(id=true,column = "id",property = "id"),
            @Result(column = "username",property = "username"),
            @Result(column = "address",property = "address"),
            @Result(column = "sex",property = "sex"),
            @Result(column = "birthday",property = "birthday"),
            @Result(property = "accounts",column = "id",many=@Many(select = "com.k.dao.AccountDao.findAccountByUid",fetchType = FetchType.LAZY))
    })
    List<User> findAll();
```

```
@Select("select * from account where uid = #{id}")List<Account> findAccountByUid(Integer id);
```

缓存配置

```
@CacheNamespace(blocking = true)
	public interface UserDao {
```

```
<configuration>   
<!--引入外部配置文件-->    
<properties resource="jdbcC.properties"></properties>    <settings>        
	<setting name="cacheEnable" value="true"/>    
</settings>
```

@Configuration
	配置类

@CompenentScan
	用于注解指定spring再创建容器时扫描的包
	属性
		value/basePackages：用于指定创建容器时需要扫描的包
			等同于xml配置的context
@Bean
	用于把当前的方法的返回值作为bean对象存入spring ioc对象
	属性：用于指定bean的id，默认值为方法名

```
//注解配置
@Configuration
@ComponentScan("com.k")
@Scope("prototype")//默认为单例
public class SpringConfiguration {
    @Bean(name = "runner")
    public QueryRunner createQueryRunner(DataSource dataSource) {
        return new QueryRunner(dataSource);
    }
}
```

ZY：
	当配置类作为annotationConfigApplicationContext对象创建的参数时，@Configuration可省略
	@import：用于导入其他的配置类

```
@Import(DruidConfig.class)
```

@propertysource
	用于指定properties文件的位置
	value：用于指定文件的名称及路径




	