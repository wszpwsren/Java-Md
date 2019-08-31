# Redis

概念
	redis是一款高性能NOsql数据库
	nosql：
		数据之间没有关联关系
		数据存储在内存中
			//缓存结构
				有数据
					返回数据
				没有数据
					1、从数据库查询数据
					2、将数据放入缓存
					3、返回数据
		NoSQL(NoSQL = Not Only SQL)，意即“不仅仅是SQL”，是一项全新的数据库理念，泛指非关系型的数据库。
		随着互联网web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。

		NOSQL和关系型数据库比较
			优点：
				1）成本：nosql数据库简单易部署，基本都是开源软件，不需要像使用oracle那样花费大量成本购买使用，相比关系型数据库价格便宜。
				2）查询速度：nosql数据库将数据存储于缓存之中，关系型数据库将数据存储在硬盘中，自然查询速度远不及nosql数据库。
				3）存储数据的格式：nosql的存储格式是key,value形式、文档形式、图片形式等等，所以可以存储基础类型以及对象或者是集合等各种格式，而数据库则只支持基础类型。
				4）扩展性：关系型数据库有类似join这样的多表查询机制的限制导致扩展很艰难。
	
			缺点：
				1）维护的工具和资料有限，因为nosql是属于新的技术，不能和关系型数据库10几年的技术同日而语。
				2）不提供对sql的支持，如果不支持sql这样的工业标准，将产生一定用户的学习和使用成本。
				3）不提供关系型数据库对事务的处理。
	
		非关系型数据库的优势：
			1）性能NOSQL是基于键值对的，可以想象成表中的主键和值的对应关系，而且不需要经过SQL层的解析，所以性能非常高。
			2）可扩展性同样也是因为基于键值对，数据之间没有耦合性，所以非常容易水平扩展。
	
		关系型数据库的优势：
			1）复杂查询可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询。
			2）事务支持使得对于安全性能很高的数据访问要求得以实现。对于这两类数据库，对方的优势就是自己的弱势，反之亦然。
	
		总结
			关系型数据库与NoSQL数据库并非对立而是互补的关系，即通常情况下使用关系型数据库，在适合使用NoSQL的时候使用NoSQL数据库，
			让NoSQL数据库对关系型数据库的不足进行弥补。
			一般会将数据存储在关系型数据库中，在nosql数据库中备份存储关系型数据库的数据
	
	1.2.主流的NOSQL产品
		•	键值(Key-Value)存储数据库
				相关产品： Tokyo Cabinet/Tyrant、Redis、Voldemort、Berkeley DB
				典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
				数据模型： 一系列键值对
				优势： 快速查询
				劣势： 存储的数据缺少结构化
		•	列存储数据库
				相关产品：Cassandra, HBase, Riak
				典型应用：分布式的文件系统
				数据模型：以列簇式存储，将同一列数据存在一起
				优势：查找速度快，可扩展性强，更容易进行分布式扩展
				劣势：功能相对局限
		•	文档型数据库
				相关产品：CouchDB、MongoDB
				典型应用：Web应用（与Key-Value类似，Value是结构化的）
				数据模型： 一系列键值对
				优势：数据结构要求不严格
				劣势： 查询性能不高，而且缺乏统一的查询语法
		•	图形(Graph)数据库
				相关数据库：Neo4J、InfoGrid、Infinite Graph
				典型应用：社交网络
				数据模型：图结构
				优势：利用图结构相关算法。
				劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。
	1.3 什么是Redis
		Redis是用C语言开发的一个开源的高性能键值对（key-value）数据库，官方提供测试数据，50个并发执行100000个请求,读的速度是110000次/s,写的速度是81000次/s ，且Redis通过提供多种键值数据类型来适应不同场景下的存储需求，目前为止Redis支持的键值数据类型如下：
			1) 字符串类型 string
			2) 哈希类型 hash
			3) 列表类型 list
			4) 集合类型 set
			5) 有序集合类型 sortedset
		1.3.1 redis的应用场景
			•	缓存（数据查询、短连接、新闻内容、商品内容等等）
			•	聊天室的在线好友列表
			•	任务队列。（秒杀、抢购、12306等等）
			•	应用排行榜
			•	网站访问统计
			•	数据过期处理（可以精确到毫秒
			•	分布式集群架构中的session分离
下载安装
命令操作
	redis的数据结构
		redis存储的是：key：value，key都是字符串，value有5种
			1) 字符串类型 string
			2) 哈希类型 hash	Map格式
			3) 列表类型 list	LinkedList
			4) 集合类型 set		set
			5) 有序集合类型 sortedset	
	操作：
		String类型
			set key value
			get key
			del key
		Hash类型
			hset key field value
			hget key field
			hgetall key
			hdel key field
		List类型
			lpush key value
			rpush key value
			lrange key start end
			lpop key 删除并返回
			rpop key
		Set类型
			sadd key value
			smembers key
			sremove key value
		SortedSet
			//每个元素关联一个double类型的数，通过分数来为集合中的成员从小到大排序
			zadd key score value
			zrange key start end
			zren key value
		key *
        type xxx
		del key
	持久化
		Rdb：默认方式，
			在一定的间隔中，检测key的变化情况，然后持久化数据
			conf->save“”
				save 900 1
					after 900sec if 1key changed
				save 300 10
					after 300sec if 10keys changed
				save 60 10000
					after 60sec if 10000keys changed
		AOF：
			日志记录的方式，可以记录每一条命令的操作。每一次命令操作后，持久化数据
			appendonly no ->appendonly yes
				#appendfsync always
				appendfsync everysec
				#appendfsync no

## 	Jedis

数据结构
持久化操作
使用Java客户端操作Redis数据库的工具

# Maven
































