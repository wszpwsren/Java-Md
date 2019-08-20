
File类
java.io.File类是文件和目录路径名的抽象表示，主要用于文件和目录的增删改查
java把电脑中的文件和目录封装为了一个类，我们可以使用File对其操作
	创建
	删除
	获取
	判断是否存在
	对文件夹遍历
	获取文件大小
File类是一个与系统无关的类，任何的系统都可以使用该类中的方法

File类静态成员变量(共4个，两个string，两个char)
pathSeparator 路径分割符 windows‘;'linux’:‘
separator 文件名称分隔符 windows '\' linux '/'

操作路径：需要使用File.separator（系统兼容）
“c:”+File.separator+"develop"+File.separator+"xxx"+File.separator+"a.txt"

路径
相对路径：简化路径，相对于当前项目的根目录，可以省略项目的根目录
绝对路径：以盘符为开始的文件
zy：
路径不区分大小写
路径中的文件名称分隔符使用反斜杠，反斜杠是转义字符，两个反斜杠代表一个反斜杠
（c:\\xxx\\a.txt）

File构造方法
path路径可以是文件，或文件夹
路径可以是相对、绝对
路径可以存在，也可以不存在
创建File对象，只是把字符串封装为File对象，不考虑路径的真假
File（string parent string child）
父路径子路径可以分开书写
File（File parent string child）
父路径是File类型，可以使用File的方法对路径进行操作

file类常用方法
获取
String getPath
返回路径
String getAbsolutePath
返回绝对路径
string getName
返回路径的结尾部分
long length（）
返回File表示的文件长度（大小），单位为字节
文件夹没有大小（返回0）
如果构造方法中给出的路径不存在，那么length返回0

判断
boolean exists
判断存在
boolean isDirectory
判断是否为目录
boolean isFile
判断是否为文件
boolean endsWith（）
判断是否以xx结尾

创建删除
boolean CreateNewFile() 抛出IOException
当文件不存在时，创建文件，可以使用相对路径，需要确保路径存在，否则异常
我们调用方法时需要处理异常
boolean delete()
boolean mkdir()
文件夹
boolean mkdirs()
多级文件夹
文件夹存在时不会创建，返回false，构造方法中给出的路径不存在返回false
boolean delete()
可删除文件及文件夹
文件夹中有内容返回false，路径不存在返回false
delete方法是直接在硬盘中删除
public boolean createNewFile() throws IOException
当且仅当不存在具有此抽象路径名指定的名称的文件时，原子地创建由此抽象路径名指定的一个新的空文件。


遍历
String[] list()
遍历构造方法中给出的目录，获取所有文件/文件夹名称，获取的名称返回到一个string数组
interface File[] listFiles()
遍历，返回值为路径（相对/绝对）
zy：
遍历的是构造方法中给出的目录
如果构造方法中给出的目录路径不存在，那么空指针异常
如果构造方法中给出的路径不是一个目录，那么空指针异常

File类中有两个和listFiles重载的方法，方法的参数传递过滤器

interface File[] FileFilter
用于抽象（File对象）路径名的过滤器
方法 boolean accept（File pathname）
File pathname使用listFiles遍历得到的对象

interface File[] FileNameFilter （File dir ，String name）
过滤文件名称
dir：被遍历的目录
name:使用ListFiles方法遍历目录，得到的目录与文件的名称
return new File（dir，name）.isDirectory
通过合并为File操作进行比较

accpet方法调用问题
pathname
[listFiles
该方法对构造方法中传递的目录进行遍历，获取目录中的每一个文件/文件夹-》封装为File
该方法会调用参数传递的过滤器中的方法accept
该方法会把遍历得到的每一个File对象传递给accept方法的参数pathname
]

boolean accept（File dir，String name）
如果返回true，就会把传递过去的File对象保存到File数组中

zy：
两个过滤器接口是没有实现类的，需要自己写实现类重写过滤方法，在方法中定义过滤规则

递归
递归要有条件限定
构造方法禁止递归


IO流
分为字节流和字符流
一个字符为两个字节
一个字节为八个bit（二进制）
顶层父类
字节流 InputStream OutputStream
字符流 Reader Writer

一切均为字节
底层永远传输二进制数据

java.io.OutputStream       abstract
close（）
flush（）
wirte（Byte[] byte）
write（Byte[] byte int off int len）
abstract write（int b）

写入数据原理：
java程序->JVM->OS->OS写入方法->把数据写入到文件中

java.io.FileOutputStram extends OutputStream
FileOutputStream（File file，boolean append）
创建一个向指定File对象表示的文件中写入数据的输出文件流
append：追加写入开关

构造方法作用
创建一个FOS对象
会根据构造方法中传递的文件/目录，创建一个空文件
会把FOS对象指向创建好的文件

FileOutputStream（String string boolean append）
创建一个向指定名称的文件中写入数据的输出文件流

[[字节输出流的使用步骤]]
创建一个FOS对象，构造方法中传递写入数据的目的地
调用FOS对象的方法write，把数据写入到文件中
释放资源（流会占用内存）
[三个方法均有异常，需要处理ioException]
[[写数据的时候，会把数据转换为二进制数据]]

任意的文本编辑器，在打开文件的时候，会查询编码表，把字节转换为字符表示
0-127查询ASCII，其余查询系统默认码表，对字节进行转换
如果第一个字节为负数，那么第一个，第二个字节会组成一个中文显示，查询GBK
UTF-8 三个字节为一个汉字  GBK中两个字节为一个汉字

数据的追加写入和换行写
换行windows \r\n   Linux /n mac /r


InputStream
abstract read()
读取一个字节
read（byte[] b ）
读取一定数量的字节，保存在b中

FileInputStream extends InputStream 文件字节输入流
构造方法
FileInputStream（File f）
FileInputStream（String s）
创建一个FIS对象
FIS对象读取要读取的文件

[使用步骤]
创建FIS对象，构造方法中指向要读取的数据源
使用FIS对象中的方法read，读取文件
释放资源

read读取到文件的末尾返回-1
read（）每读取一个字节，那么指针向后移动一位

int read（byte[] b）
String类的构造方法String（byte[] byte）将byte数组转为string
byte：存储每次读取到的多个字节，一般定义长度为1024或其整数倍
返回值为读取的有效数据的字节数


字符流
字节流遇到中文字符时，可能会显示不完整的字符

字符输入流 abstract Reader
int read（）
int read（char[] cbuf）
java.io.FileReader extends InputStreamReader extends Reader
构造方法
FileReader(String filename)
FileReader(File file)
文件的路径或文件
作用
创建一个FR对象
把FR对象指向要读取的文件
使用步骤
创建FR对象，在构造方法中绑定要读取的数据源
使用方法read读取文件
释放资源

字符输出流 abstract Writer

void write（int c）单个字符
void write（char[] cbuf）
abstract void write（char[] cbuf int off int len）
void wirte(String str)写入字符串
void write(String str,int off, int len)
void flush
void close

java.if.FileWriter extends OutputStreamWriter extends Writer
构造方法
FileWriter（File file）
FileWriter（String fileName）根据给定文件名构造一个FielWriter对象
参数：写入数据的目的地
String fileName：文件路径
File file 文件信息
构造方法的作用
创建一个ZFileWriter对象
根据构造方法中传递的文件/文件路径，创建文件
会把FileWriter对象指向创建好的文件

[[[使用步骤]]]：
创建FW对象，构造方法中帮i的那个要写入数据的目的地
使用FileWriter中的方法write，把数据写入到内存缓冲区中（字符转换为字节）
使用FW中的方法flush，把内存缓冲去的数据，刷新到文件中
释放资源（会把内存换种区中的数据刷新到文件中）

关闭和刷新
flush：刷新缓冲区，流对象可继续使用
close：刷新缓冲区，然后释放资源，流对象不可用

字符输出流写数据的其他方法
void write（char[] cbuf）写入字符数组
abstract void write（char[] cbuf int off， int len）
void write（String s）
void write（s，off，len）


IO异常处理
JDK1.7之前 try catch finally
由于FileWriter等类的对象在try里，所以需要在方法体外声明对象fw，并且需要初始化为null，并try catch finally中的close代码

JDK7
在try的后面可以增加一个（），在括号中可以定义流对象
这个流对象的作用域在try中有效
try中的代码执行完毕后，自动释放流对象，不用写finally

JDK9
在try前面可以定义流对象，在try后面的括号中可以直接引入变量名（流对象名称）
在try代码块执行完毕后，流对象也可以释放掉，不用finally

Properties extends HashTable<k,v> implements Map<k,v>
表示一个持久的属性集，Properties可保存在流中或从流中卸载
Properties集合是唯一一个和Io流相结合的集合
属性列表中每个键和值都是字符串
Properties集合中有一些操作字符串的特有方法
Object setProperty(String key , String value) 调用Hashtable中的方法put
String getProperty（String key）key->value
Set stringPropertyName（）调用Map中的方法keyset

[[void load]]
把硬盘中的数据（键值对），读取到集合中使用
void store （InputStream out，String comments）
	（Writer writer ， String comments）
把集合中的临时数据，写入硬盘
使用步骤
1创建Properties对象
2使用Properties集合对象中的方法load方法读取保存键值对的文件
3遍历Properties集合
zy：
1存储键值对的文件中，键值对默认的连接符号为 = 空格 （其他符号）
2存储键值对的文件中，可以使用#进行注释，被注释的键值对不会再被读取 
3文件中，不用加引号

comments：注释，用来解释说明保存的文件是做什么用的，默认Unicode编码，一般使用“”空字符串
使用步骤：
	1、创建Properties集合对象，添加数据
	2、创建字节/字符输出流,构造方法中帮i的那个要输出的地址/文件名
	3、使用Properties集合中的方法store，把集合中的临时数据写入硬盘
	4、释放资源


缓冲流、转换流、序列化流、打印流

缓冲流（高效流）
给基本的字节输入流增加一个缓冲区（数组），提高基本的字节输入流读取效率

BufferedOutputStream
构造方法
BufferedOutputStream（OutputStream out）
传递一个FileOutputStream，缓冲流会给FIleOutputStream增加一个缓冲区，提高效率
BufferedOutputStream（OutpuStrream out ，int size）
size：指定缓冲流内部缓冲区的大小，不指定默认
使用步骤
创建FileOutputStream对象，构造方法中指向输出目的地
创建BufferedOutputStream对象，构造方法传递FileOutputStream对象
使用BufferedOutputStream对象中的方法write，将数据写入到缓冲区中
使用BufferedOutputStream对象中的方法flush，将缓冲区数据刷新到文件中
释放资源

BufferedIntputStream（InputStream in）
构造方法
BufferedInputStream（InputStream out）
传递一个FileInputStream，缓冲流会给FIleInputStream增加一个缓冲区，提高效率
BufferedInputStream（InpuStrream out ，int size）
size：指定缓冲流内部缓冲区的大小，不指定默认
[使用步骤]
1创建FileInputStream对象，构造方法中指向输入数据源
2创建BufferedInputStream对象，构造方法传递FileInputStream对象
3使用BufferedInputStream对象中read方法，读取文件
4释放资源

BufferedWriter extends Writer

构造方法
BufferedWriter（Writer out）
BufferedWriter（Writer out int sz）
参数
传递Writer
sz 指定缓冲区大小
特有的成员方法：
void newlkine（） 写入一个行分隔符。会根据不同的系统获取不同的行分隔符
使用步骤：
创建BufferedWriter对象，构造方法传递Writer
调用BufferedWriter对象的方法wirte，把数据写入到内存缓冲区中
调用BufferedWriter方法flush，把缓存区数据刷新到文件中
释放资源

BufferedReader extends Reader
BufferedReader（Reader in）
BufferedReader（Reader in int sz）
参数
传递Reader
特有的成员方法
public String readline（）读取一个文本行
	行的中止符号：\n 换行\r回车 或\r\n
返回值：包含该行内容的string,不包含行终止符，如已到达流末尾，则返回null
readline并不移动指针

转换流
字符编码
字符集：编码表，对字符进行编码
	ascii编码-ascii字符集
	英语编码，7位表示一个字符，共128字符，扩展集使用8位，共256位
	iso-8859-1，用于显示欧洲使用的语言  ，使用单字节编码，兼容ascii编码
	gb字符集 中文字符集 gb2312 简体中文表，兼容ascii
	gbk 最常用的码表，双字节编码
	Unicode 最多4字节 utf-8、utf-16、utf-32

当utf-8默认读取windows默认gbk的时候，会出现编码
转换流
InputStreamReader
可以把字节转换为字符（与FileReader同功能），并使用指定的charset读取字节，将字节转换为字符
InputStreamWriter(InputStream Input)
InputStreamWriter(InputStream Input, String charNameset)
构造方法参数
inputStream input 字节输入流
使用步骤
创建InputStreamWriter对象，构造方法中传递字节输入流和指定的编码表名称
使用InputStreamWriter对象中的方法read读取文件
释放资源
zy：
构造方法中指定的编码表名称要和文件的编码相同

OutputStreamWriter
OutputStreamWriter(OutputStream out)
OutputStreamWriter(OutputStream out, String charNameset)
out 字节输出流 可以用来写转换之后的字节到文件中
charsetName 指定的编码表名称，不区分大小写
使用步骤
创建OutputStreamWriter对象，构造方法中传递字节输出流和指定的编码表名称
使用OutputStreamWriter对象中的方法write，把字符转换为字节存在存储缓冲区中
使用OutputStreamWriter对象中的方法flush，把内存缓冲区的字节刷新到文件中
释放资源

序列化流
把对象以流的方式，写入到文件中保存，叫写对象，也叫对象的序列化
对象中包含字节，所以使用字节流
把文件中保存的对象，以流的方式读取出来，叫做读对象，也叫对象的反序列化
ObjectOutputStream exten
ds OutputStream：对象的序列化流
构造方法ObjectOutputStream（OutputStream out）
特有的成员方法：
void writeObject（Object obj）
使用步骤：
创建ObjectOutputStream对象，构造方法中传递字节输出流
使用ObjectOutputStream对象中的writeObject方法，吧对象写入到文件中
释放资源

序列化或反序列化需要该对象的类需要实现Serializable接口
Serializable接口也称标记型接口
	要进行序列化或反序列时，会检测该类是否实现了Serializable
ObjectInputStream:对象的反序列化流

反序列化流
ObjectInputStream（InputStream in）
in 字节输入流
特有的成员方法
Object readObject（）从ObjectInputStream读取对象

[[当序列化完成后class文件进行了修改]]，那么再对文件进行反序列化时产生异常InvalidClass
，产生原因为对class文件进行修改后，类的瞬态版本号发生了更改
那么需要在class内自己定义一个瞬态版本号
static final long serialVersionUID = （long xl）；

transient:瞬态关键字
被transient修饰的变量不能被序列化
static：静态关键字
静态优先于非静态加载到内存中
static成员变量不能被序列化


打印流：java.io.PrintStream extends OutputStream
	PrintStream：为其他输出流添加了功能，使他们能方便的打印各种数据
特点：
只负责数据的输出
永远不会抛出IOException
有特有方法print println
构造方法：
	PringtStream（File f）
	PringtStream（String fn）
	PringtStream（OutputStream os）目的地是一个字节输出流
成员方法：
	close（）
	flush（）
	wirte（Byte[] byte）
	write（Byte[] byte int off int len）
	abstract write（int b）
ZY:
如果使用继承自父类的write写数据，那么查看数据的时候会查询编码表并转换
如果使用自己特有的方法print/ln写数据，那么数据不进行转换

PrintStream ps = new PrintStream（”String name“）
System.setOut(ps)
将标准输出流转换至ps



