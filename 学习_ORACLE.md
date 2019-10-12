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
	删除全部数据（较快）（带有索引的情况）
	索引可以提高查询效率，但是影响增删改效率
	
序列
	默认从1开始递增	
	select s_person.nextval from dual;
		//查看下一个序列值
		//dual为虚表，为补全语法
	使用：
	insert into person (pid,pname) values (s_person.nextval,'li');

scott，密码tiger
	
字符函数
	单行
	upper/lower
数值函数
	单行
	round（m，n）四舍五入
		m保留n位小数，n可以为负
	trunc（m，n）直接截取
	mod（m，n）求余
日期函数
	查询入职至今的天数
	select sysdate-e.hiredate from emp e;
	加一天
	select sysdate+1 from dual；
	查询入职至今的月数
	select months_between(sysdate-e.hiredate) from emp e;
转换函数
	select to_char(sysdate,'yyyy-mm-dd hh24:mi:ss')from dual
通用函数
	计算年薪
	select e.sal*12+nvl(e.comm,0) from emp e;
	//nvl
		如果null，则转换为xx
条件表达式
	select e.name,
		case e.ename
			when 'smith'then'史'
				when 'allen'then'艾'
					else'瓦'
						end
	from emp e;
	//如果不加逗号，那么查询都为一列
	
	select e.sal,
		case 
			when e.sal>3000 then 'high'
				when e.sal>1500 then 'middle'
					else 'low'
						end
	from emp e;
	
	
	
	
	
	
	
	
	
	
	
	