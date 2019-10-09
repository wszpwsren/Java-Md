# SVN

subversion
	用于控制代码版本
checkout 源码下载到本地
update 本地源码更新至服务器上的最新版本
commit 本地源码更新内容提交
干扰
	复制-修改-合并方案
		服务端进行合并
	锁定-修改-解锁
		单修改模式
服务器：apache/独立
存储
	Berkeley DB 事务安全性表
	FSFS 无数据库存储
	
visualsvn server
tortoisesvn client

	export不受版本控制
	check out 受版本控制

Trunk 主要代码
Breanches 分支代码
	Project+日期+功能点
Tags 发布版本
	Project+版本号

# AdminLTE

wrapper包住了body下的所有代码
.main-header里是网站的logo和导航栏的代码
.main-sidebar里是用户面板和侧边栏菜单的代码
.content-wrapper里是页面的页面和内容区域的代码 .main-footer里是页脚的代码
.control-sidebar里是页面右侧侧边栏区域的代码