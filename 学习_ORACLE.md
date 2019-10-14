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
	
oracle专用条件表达式
	select e.name.
		decode(e.name,
			'smith','1'
				'alen','2'
					'ss')中文名//"中文名"
	from emp e;
	
多行函数（聚合函数）
select count(1) from emp;//count主键
//sum（sal）
//max（sal）
//min（sal）
//avg（sal）

分组查询
	//分组查询中，出现在group by 后面的原始列，才能出现在select后
	//（只能使用聚合函数）
	// 聚合函数有一个特性，会将多行记录转为单行
select e.deptno,avg(e.sal) --e.ename
from emp e
group by e.deptno
	
selcet e.deptno,avg(e.sal)  asal
from emp e
group by e.deptno
having avg(e.sal)//asal>2000
	//所有条件都不能使用别名来判断
	//条件的优先级大于select
	
笛卡尔积
	select * from emp e,dept d;
内连接
	select * from emp e innner join dept d on e.deptno = d.deptno;
外连接
	select * from emp e right join dept d on e.deptno=d.deptno;
oracle专用外连接
	select * from emp e, dept d where e.deptno(+) = d.deptno;右外
自链接	
	select e1.ename,e2.ename from emp e1，emp e2 where e1.mgr = e2.empno;
子查询
	子查询返回一个值
	select * from emp where sal in //=
		(selcet sal from emp where ename='scott')
	子查询返回一个集合
	select * from emp where sal in
		(select sal from emp where deptno=10)
	子查询返回一张表
	select  t.deptno,t.msal,e.name,d.dname
	from (
	select deptno,min(sal) msal
	from emp
	group by deptno;
	) t,emp e,dept d
	where t.deptno = e.deptno
	and t.msal = e.sal
	and e.deptno =d.deptno;
	
oracle中的分页
	rownum行号：select操作每查询一条记录，就会在该行上加一个行号，
	行号从1开始递增
	排序操作会影响rownum的顺序
	select * from(
		select rownum rn,t.* from（
			select rownum,e.* from emp e  order by e.sal desc） t
	where rownum<11) tt where rn>5
	
视图
	视图是提供一个查询的窗口，所有数据来自原表
	create view v_emp as select ename,job from emp with read only
	
	视图可以屏蔽掉一些敏感信息
	视图可以保证总部和分部的数据及时统一
	
索引
	索引就是在表的列上构建一个二叉树
	达到提高查询效率的目的，会影响增删改的效率
	
	单列索引
		创建
		create index idx_ename on emp(ename)
		单列索引触发条件
			查询条件必须是索引列中的原始值
			//单行函数，模糊查询会影响索引的触发
	复合索引
		create index idx_ename_job on emp(ename,job)
		复合索引触发条件
		复合索引中第一列为优先检索列
		如果需要触发复合索引，必须包含有优先检索列中的原始值
		如果只含优先检索列，触发单列索引
		//or关键字不触发索引（查询个数）
	
	