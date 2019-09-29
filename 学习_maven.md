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

# 私服