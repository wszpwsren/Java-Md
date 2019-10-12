# ORACLE

数据库
	物理存储
实例oracle instance
	进程与内存结构
用户
	管理表的基本单位
	用户是再实例下建立的
表空间
	数据库较大的情况下，将数据分块为表空间
	是数据文件的逻辑映射
	存储于dbf中
	ora（变量文件？）
数据文件
	数据存储在表空间中

![1570761177316](C:\Users\feketerigo\AppData\Roaming\Typora\typora-user-images\1570761177316.png)

创建表空间

```
create tablespace kr
datafile '/home/oracle/oracle_db.dbf'
size 100m
autoextend on
next 10m;
```

delete from person;
	删除全部数据
drop table person ;
	删除表
truncate table person ;
	删除全部数据（较快）（带有suo）
	
	
	
	
	
	
	
	
	
	
	
	
	
	