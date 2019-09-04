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
	
自定义mybatis框架

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