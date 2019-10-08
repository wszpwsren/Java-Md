# maven

# maven传统web工程

maven依赖管理：
	jar包放置于仓库中，项目中放置jar包坐标
一键构建
	maven集成了tomcat，可以对项目进行各种操作

maven工程导入jar包时，需要解决jar包冲突
	方式1：第一声明优先
		坐标在前，就先声明
			先声明的jar包下的依赖包，优先进入项目
	方式2：
		直接依赖：项目中直接导入的jar包，就是该项目的直接依赖包
		传递依赖：项目中没有直接导入的包，可以通过项目直接依赖的jar包传递到项目中
		直接依赖优先于间接依赖
	方式3：
		排除

```
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.0.2.RELEASE</version>
      <exclusions>
          <exclusion>
              <artifactId>spring-core</artifactId>
              <groupId>org.springframework</groupId>
          </exclusion>
      </exclusions>
    </dependency>
```
	锁定jar包版本：
		<dependencymanagement></dependencymanagement>
		maven工程可以分父子关系
		凡是依赖别的项目后，拿到的别的项目的依赖包，都属于传递依赖
			A被B依赖-》A项目的jar包会传递到B项目中
			A中jar包会被B中同名jar包覆盖
			为了防止覆盖，将A项目中的jar包锁定，无法被覆盖
		
		management中，不提供导入功能，只提供锁定功能

# maven拆分聚合

maven把一个完整的项目分成不同的模块
	每个模块都有自己独立的坐标，哪个地方需要其中的模块，直接引用坐标即可

今后如果开发一个项目，需要先考虑模块是否存在，如果存在直接引用
我们可以把拆分的模块聚合到一起变成一个项目-maven聚合

```
  <!--工程
        工程不等于完整的项目，一个完整的项目看的是代码，如果代码完整，那么就是完整的项目
        工程只能使用自己内部的资源，工程天生是独立的，可以和其他工程或模块建立关联关系
      模块
        模块天生不是独立的，天生是属于父工程的，模块一旦创建，所有父工程的资源都可以使用
      子模块天生集成父工程，可以使用父工程的所有资源
      子模块天生之间没有任何关系
      父子工程不用建立关系
      平级之间的关系叫依赖
    -->
```

传递依赖的包是否能使用，需要参考（默认compile）

![1570483943406](C:\Users\feketerigo\AppData\Roaming\Typora\typora-user-images\1570483943406.png)

实际开发中，如果传递依赖丢失，表现形式是jar包的坐标导不进去

# 私服

--安装第三方jar包到本地仓库

----进入jar包所在目录运行
mvn install:install-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 -Dfile=fastjson-1.1.37.jar -Dpackaging=jar
----打开cmd直接运行
mvn install:install-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 -Dpackaging=jar -Dfile=C:\my_java\授课资料\资料：maven【高级】\安装第三方jar包\fastjson-1.1.37.jar


--安装第三方jar包到私服

--在settings配置文件中添加登录私服第三方登录信息
<server>
<id>thirdparty</id>
<username>admin</username>
<password>admin123</password>
</server>
----进入jar包所在目录运行
mvn deploy:deploy-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 -Dpackaging=jar -Dfile=fastjson-1.1.37.jar -Durl=http://localhost:8081/nexus/content/repositories/thirdparty/ -DrepositoryId=thirdparty
----打开cmd直接运行
mvn deploy:deploy-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37 -Dpackaging=jar -Dfile=C:\my_java\授课资料\资料：maven【高级】\安装第三方jar包\fastjson-1.1.37.jar -Durl=http://localhost:8081/nexus/content/repositories/thirdparty/ -DrepositoryId=thirdparty