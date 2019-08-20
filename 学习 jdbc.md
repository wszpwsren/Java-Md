# JDBC

Java DataBase Connectivity 
JDBC：官方定义了操作所有关系型数据库的规则——接口
JDBC实现类：数据库驱动
[执行代码的是驱动包（jar）中的实现类]

入门：
	将jar包导入工程
		复制jar包到目录下
		右键add as library
	
	注册驱动
步骤：	
	获取数据库连接对象Connection
	定义sql
	获取sql语句的对象Statement
	执行sql，接收返回结果
	处理数据
	释放资源

```java
Class.forName("com.mysql.cj.jdbc.Driver");
Connection root = DriverManager.getConnection("jdbc:mysql://localhost:3306/world", "root", "683512");
String sql = "update country set indepyear = -3000 where code = 'chn'";
Statement statement = root.createStatement();
int i = statement.executeUpdate(sql);
System.out.println(i);
statement.close();
root.close();
```

详解对象：
	DriverManager：驱动管理器对象
		功能
		注册驱动//可省略：Class.forName("com.mysql.cj.jdbc.Driver");
		告诉驱动使用哪个jar
		

```java
static {
	try {
		DriverManager.registerDriver(new Driver());
	} catch (SQLException var1) {
		throw new RuntimeException("Can't register driver!");
	}
}
```

获取数据库连接：
​Connection root = DriverManager.getConnection("jdbc:mysql://localhost:3306/world", "root", "683512");
​	url：指定连接的路径
​	mysql调用：jdbc:mysql://ip(域名)：端口号/数据库名称
​	Connection：数据库连接对象
​		功能：
​		获取statement
​	 	获取preparedstatement

​	管理事务：
​		开启事务：setAutoCommit：调用该方法，设置参数为false即开启事务
​		提交事务：commit（）；
​		回滚事务：rollback（）；

​	Statement：执行sql对象

​	执行sql：execute（String sql）：执行任意sql
​		int executeUpdate（String sql）：执行DML语句（insert/update/delete）、DDL语句（create/alter/drop）

​	返回值为影响的行数
​		ResultSet executeQuery（String sql）：执行DDL语句	

​	ResultSet：结果集对象，封装查询结果
​		next（）；光标从当前位置向前移动一行
​		getxxx（）；获取数据//xxx代表数据类型
​			参数：
​				int：代表列的编号（从1开始）
​				string：代表列的名称  
​			使用步骤
​				游标向下移动，判断是否是最后一行末尾，取出数据
​	PreparedStatement：执行sql对象

​	SQL注入问题
​		sql。。。。or 'a'='a';
·		在拼接sql时，有一些sql的特殊关键字参与字符串的拼接，会造成安全性问题
​
​		解决办法：使用PreparedStatement：预编译sql语句对象
​			特点：参数使用？占位符替代			
​				步骤：
​					1获取数据库连接对象Connection
​					2定义sql
​						zy：参数使用占位符‘？’
​						如select * from stu where id=？ and name = ？；
​					3获取sql语句的对象PreparedStatement，参数传递sql语句
​					Connection.prepareStatement(String sql)
​					4给？赋值
​						setxxx（参数1，参数2）
​							参数1-》？的位置，从1开始
​							参数2-》？的值
​					5执行sql，接收返回结果，不需要传递sql语句
​					6处理数据
​					7释放资源

		后续将会使用PreparedStatement来完成CRUD操作

抽取JDBC工具类：JDBCUtils
	目的：简化书写
	分析：
		抽取一个方法注册驱动（）
		抽取一个方法获取连接对象
			//不希望传参，需要保证工具类的通用性
			//使用配置文件
		抽取一个方法释放资源
			//释放资源需要分别释放
	详见\src\heima_06\day01\JDBCDemoUtils.java
	\src\jdbc.properties
	\src\heima_06\day01\util\JDBCUtils.java

JDBC控制事务
	事务：一个包含多个步骤的业务操作
	操作：开启事务，回滚事务，提交事务
		开启事务：setAutoCommit：调用该方法，设置参数为false即开启事务
			执行sql前开启事务
		提交事务：commit（）；
			sql执行完成后提交事务
		回滚事务：rollback（）；
			catch中回滚

	当异常出现就rollback，那么catch设置为exception
	connection可能为null，需要判断connection是否为null

详见\src\heima_06\day01\JDBCDemoTran.java


数据库连接池
	概念：是一个容器：存放数据库连接的容器
		当系统初始化好后，容器会申请一些连接对象，当用户访问数据库时，从容器中获取连接对象，用户访问完后，会将连接对象归还给容器
	好处：节约资源、高效
	实现：由数据库厂家实现：
		基本实现：生成标准connection对象
			获取连接：getconnection（）
			归还连接：调用connection.close（）归还连接
			c3p0：数据库连接池技术//比较老
			Druid：数据库连接池技术，由阿里巴巴实现
		连接池实现：申请自动参与连接池的connection对象
	
c3p0：实现
		导入c3p0-0.9.5.2、mchange-commons-java-0.2.11
		定义配置文件：
			c3p0.properties或c3p0-config.xml
			路径：类路径（将文件放入src目录下）
			config可以配置多个，如果不设置，那么使用默认config
			配置方法DataSource ds  = new ComboPooledDataSource("otherc3p0");
		创建核心对象 ComboPooledDataSource
		获取连接：getconnnection
		
Druid：
	步骤：
		导入jar包
		定义配置文件：
			特点：
			是properties形式的
			可以放在任意目录下
		加载配置文件：
			Properties
		获取数据库连接池：
			通过工厂类DuridDataSourceFactory获取
		获取连接：
			getconnection	

```
//导包
//定义配置文件
//加载配置文件
Properties properties = new Properties();
InputStream resourceAsStream = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
properties.load(resourceAsStream);
//获取连接池
DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
//获取连接
Connection connection = dataSource.getConnection();
System.out.println(connection);
```



​	简化操作：
​		定义工具类：
​			定义类：JDBCUTILS
​			提供静态代码块加载配置文件，初始化连接池对象
​				提供方法
​					获取连接方法：通过数据库连接池获取连接
​					释放资源
​					获取连接池的方法

详见\src\datasource\utils\JDBCUtils.java


Spring JDBC：Spring框架提供的工具类，用于封装JDBC
	提供了一个JDBCTemplate 对象简化JDBC的开发
	步骤：
		导入jar包
		创建JdbcTemplate对象。依赖于数据源DtaSource
			JdbcTemplate template = new JdbcTemplate（ds）；
		调用JdbcTemplate的方法来完成CRUD的操作
			update（）；执行DML语句
			queryForMap();执行DQL语句，将结果集封装为map集合
				//查询的结果集长度只能为1，列名为key，值作为value
			queryForList（）；执行DQL语句，将结果集封装为List集合
				//将每一条记录封装为map集合，再将多个map集合装在到list集合中
			query（）；，将结果封装为JavaBean对象
				//query的参数RowMapper，
				一般我们使用BeanPropertyRowMapper实现类，实现自动向JavaBean的自动封装
			queryForObject：将结果封装为对象
				//一般用于聚合函数
				Long aLong = template.queryForObject(sql, long.class);
