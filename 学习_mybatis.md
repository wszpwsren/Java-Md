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

mybatis参数和返回值

mybatis的dao编写

mybatis配置的细节/标签

# mybatis深入和多表

mybatis连接池

mybatis事务控制及设计方法

mybatis多表查询

# mybatis缓存和注解开发

mybatis加载时机

mybatis一级缓存/二级缓存

mybatis注解开发