Junit单元测试

测试分类：
	黑盒测试、白盒测试
Junit
步骤：
	定义一个测试类
ZY：
测试类名为被测试类名+Test
测试类包名xxx.xxx.xxx.test

	定义测试方法：可以独立运行
ZY：方法名：test+测试的方法名
返回值void
参数列表：空参

	给方法加一个注解@Test
	导入Junit依赖环境
	测试方法
创建对象
调用方法
断言结果（通过断言方法处理操作结果）
[测试结果并不能判断程序是否异常，需要看测试工具的反馈]

注解
@Before
public void init（）{}
用于资源申请，所有测试方法在执行前都会执行该方法

@After
public void close（）{}
在所有测试方法后，用于释放资源

反射：框架设计的灵魂

框架：半成品软件，可以在框架的基础上进行软件开发
反射：将类的各个组成部分封装为其他对象，这就是反射机制
当xxx.class文件在运行前由类加载器加载至JVM中时，由一个对象进行接收（class类对象）
class对象对xxx.class字节码文件进行描述，其中有：成员变量Filed[]，构造方法Constructor[]，成员方法Mehod[]等[[[对象]]]进行描述类

优点：	
	可以在程序运行过程中，操作这些对象（比如调用类的方法）
	解耦，提高程序的可扩展性
class对象的获取方式
	1、Class.forName（”全类名“）静态方法：将字节码文件加载进内存
	多用于配置文件，可以将类名定义在配置文件中。读取文件，加载类
	2、类名.class：类名的class属性
	多用于参数的传递
	3、对象.getClass()：Object类的方法
	多用于对象的获取字节码方式
[[同一个类字节码文件在一次程序运行过程中只会被加载一次（任何方法获取对象为同一个）]]

Class对象的功能：大多数为获取的功能

	获取成员变量 
	Field[] getFields（) 
	获取所有[Public]的成员变量
	Field getField(string)
	获取指定的[Public]的成员变量
	Field[] getDeclaredFields()
	获取所有成员变量
	Field getDeclaredField(String)

通过反射机制，可以对类字节码文件进行修改，不受访问权限限制
如果需要访问，需要忽略访问权限的安全检查
<Field>x.setAccessable(true);//暴力反射

成员变量的可进行操作：
设置值
set（Object obj）
获取值
get（Object obj）

2、获取构造方法
Constructor<?>[] getConstructors()
Constructor<T> getConstructor(Class<?>...parameterTypes)
Constructor<T> getDecleredConstructor(Class<?>... parameterTypes)
Constuctor<?>[] getDeclareedConstructors()
可进行暴力反射

Constructor<Person> constructor = personClass.getConstructor(String.class, int.class);

构造方法：创建对象
T newInstance（Object...initargs）
Person z = constructor.newInstance("z", 10);
ZY:如果使用空参构造方法创建对象，操作需简化


3、获取成员方法
Method[] getMethods()
Mehtod getMethod(String name,Class<?>...parameters)
Method getDecleredMethods()
Mehtod getDecleredMethod(String name,Class<?>...parameters)
可进行暴力反射

获取方法包括继承的方法
调用方法：object invoke（Object obj，Object...args）
获取方法名称：method.getName()

4、获取类名
String getName()
Packege getPackage()

案例
实现：
	1配置文件
	2反射
步骤：
	1、将需要创建的对象的全类名和需要执行的方法定义在配置文件中
	[[pro.properties]]
	className=heima_05.day01.domain.Person
	methodName=setName

	2、在程序中加载读取配置文件
	//new 配置文件对象
	Properties properties = new Properties();
	//new 配置文件（类对象）载入器
	ClassLoader classLoader = Person.class.getClassLoader();
	//将配置文件数据源输入一个流
	InputStream resourceAsStream = classLoader.getResourceAsStream("pro.properties");
	//载入配置文件<T>流
	properties.load(resourceAsStream);
	//加载配置文件
	String className = properties.getProperty("className");
	String methodName = properties.getProperty("methodName");

	3、使用反射加载类文件
	Class aClass = Class.forName(className);

	4、创建对象
	Object obj = aClass.getDeclaredConstructor().newInstance();
	Method methods = aClass.getMethod(methodName);

	5、执行方法
	methods.invoke(obj);

注解
概念：说明程序的
作用分类：
编写文档：生成文档
代码分析：对代码进行分析（反射）
编译检查：实现基本的编译检查

JDK预定义注解

@override 检测重写方法是否正确
@Deprecated 将该注解标注的内容已过时
@SuperessWarning 压制警告（不显示）一般传”all“

自定义注解
public interface heima_05.day01.MyAnno extends java.lang.annotation.Annotation {
}
注解本质是接口，该接口默认继承Annotation接口

格式：


元注解：
用于描述注解的注解
@Target 描述注解能够作用的位置
ElementType[] value()
ElementType:元素类型
Type：用于类
Method：用于方法
Filed：用于成员变量
表示注解只能用于用于x元素类型上

@Retention 描述注解被保留的阶段
RetentionPolicy[] value()
Source Class Runtime
一般使用的为Runtime

@Documented 描述注解是否被抽取到api文档中

@Inherited 描述注解是否被子类继承

public @interface xxxxx{
	属性列表
}
属性：可定义的成员方法
	要求：
	属性的返回值类型必须为
		基本数据类型
		字符串String
		枚举
		注解
		以上类型的数组
	定义的属性在使用时，需要给属性赋值
		@xxx（xx=1）
	如果不使用赋值，那么在定义时，public @interface xxx{int age（） default；}
	如果只有一个属性，且属性名为value，那么在赋值时，value可省略
	赋值方式：12 ，value =12，per=Person.P1（枚举），anno=@Anno，strs = {”a“，”b“}

实例：
注解的解析

注解
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Pro {
    String className();
    String methodName();
}

//解析注解
//获取该类的字节码文件对象（获取注解定位的元素的对象）
Class<ReflectDemo04> reflectDemo04Class = ReflectDemo04.class;
//获取注解对象->在内存中生成了一个注解接口的实现类对象
Pro annotation = reflectDemo04Class.getAnnotation(Pro.class);
//调用注解对象中定义的抽象方法，获取返回值
String className = annotation.className();
String methodName = annotation.methodName();

注解小结：
大多数情况，我们使用注解，而不是自定义注解
注解是给编译器，以及解析程序使用
注解不是程序的一部分，可以理解为一个注释


