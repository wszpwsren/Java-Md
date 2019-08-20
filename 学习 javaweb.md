

# Javaweb开发：

​	架构：
​	c/s
​		优点：用户体验好
​		缺点：开发、维护、部署麻烦
​	b/s：通过URL访问不同的客户端程序
​		优点：开发、维护、部署简单
​		缺点：如果应用过大，用户体验差
​			对硬件要求高

​	架构详解：
​	静态资源：
​		使用静态网页开发技术发布的资源
​		HTML，CSS，JAVASCRIPT（三个静态网页开发技术）
​		如果用户请求静态资源，那么服务器会直接将静态资源发送给浏览器，浏览器内置了静态资源的解析引擎，可以展示静态资源
​		
​	动态资源：（java开发
​		使用动态网页开发技术发布的资源
​			jsp/servlet（学习），php，asp
​			如果用户请求动态资源，服务器会执行动态资源，转换为静态资源，再发送给浏览器
​	学习动态资源，必须先学习静态资源：
​		HTML：用于搭建基础网页，展示页面内容
​		CSS：用于美化页面，布局页面
​		JavaScript：控制页面元素，让页面有动态效果

HTML	最基础的网页开发语言
	Hyper Text MarkUp Language 超文本标记语言
	超文本：用超链接的方法，将各种不同空间的文本信息连接起来
	标记语言：由标签构成的语言 <标签名称> 
		标记语言不是编程语言

快速入门：.html/.htm
	标签分为
		围堵标签:<html> </html>
		标签可以嵌套，需要正确嵌套

​		自闭合标签，开始和结束在一起<br/>

​		在开始标签中可以定义属性。属性由键值对构成，值需要用引号（单双都可以）引起来
​		html标签不区分大小写，建议使用小写

标签：
	文件标签：构成HTML最基本的标签
		html：html文档的根标签
		head：头标签。用于指定html文档的一些属性。引入外部的资源
		title：定义标题
		body：体标签
		< !DOCTYPE html>? : html5中定义该文档是html文档
		< meta charset="UTF-8">指定字符集
		

​	文本标签：和文本相关的标签
​		注释：<!-- 注释内容 -->
​		< h1> to < h6> 标题标签，字体大小递减
​		< p>	段落标签，围堵标签
​		< br/>	换行标签
​		< hr/>	显示一条水平线
​		<b> 字体加粗
​		<i> 斜体
​		<font>字体标签 //已过时
​		特殊字符  &copy；等

​	图片标签：
​		<img>  展示图片 需要在模组下
​		./当前目录
​		../上级目录

​	列表标签：
​		有序列表：
​			ol:
​			li:

```html
<ol> 
    <li>2</li>   
    <li>3</li>
</ol>
```

​		无序列表：
​			ul:
​			li:

```
<ul type="2">    
	<li>1</li>
</ul>
```

​	连接标签：
​		a:超链接
​			herf：指定访问资源的URL（统一资源定位符）
​				target="_ blank"新选项卡跳转
​				target="_self"本页面跳转
​				./本地目录
​				mailto:xxx@xxx.com

```html
<a herf="1_Helloworld.html"<img src="1_HelloWorld.html"></a>
```

​	div/span标签：
​		<span>将文本包裹起来，进行样式控制<span/>
​			文本信息在一行展示，也称为行内标签，内联标签
​		< div>同上<div/ >
​			每个div占满一行。块级标签

语义化：
html4 < div id=xxx><div/>	
html5 <xxx><div></div></xxx>

	<table border="1" width="30%" style="stroke-dasharray: 300px">
	    <thead>
	        <caption>信息</caption>
	        <tr>
	            <td>编号</td>
	            <td>成绩</td>
	        </tr>
	    </thead>
	    <tbody>
	        <tr>
	            <td>x</td>
	            <td>122</td>
	        </tr>
	    </tbody>
	    <tfoot>
	        <tr>
	            <td>0</td>
	        </tr>
	    </tfoot>
	</table>

form：用于定义表单的。可以定义一个范围，范围代表采集用户数据的范围
表单项中的数据要想被提交，必须指定其name属性
	属性：
		action：指定提交数据的URL
		method：指定提交方式
			分类：一共7种
			get：
				1、请求参数会在地址栏中显示，会封装在请求行中
				2、url长度/请求长度是有限制的
				3、不太安全
​			post：
​				1、请求参数不会在地址栏中显示，会封装在请求体中
​				2、请求大小没有限制
				3、较为安全
				
< form action="#" method="get">
​    用户名：<input name="username"><br/>
​    密码： <input name="password"><br/>
​        <input type="submit" value="登陆">
​    </form >

表单项标签：
	input：可以通过ytpe属性值，改变元素展示的样式
		type属性：
			text：文本
				placeholder：指定输入框的提示信息
				label标签：指定输入项的文字描述信息
					label的 for 属性一般会和input的 id 属性值对应
					如果对应了，则点击label区域，会获取焦点，下同
			password：密码
			radio：单选框
				要想让多个单选框实现单选的效果，则多个name的属性值必须相同
				一般会给每一个单选框提供value属性，指定其被选中时提供的值
			checkbox：复选框
				checked 默认选中，上同
			file：文件选择框
			hidden：隐藏域，用于提交一些信息
			按钮：
				submit：提交
				button：按钮
				image：图片提交按钮，src指定图片路径
	select：下拉列表
		name：元素名称
		子元素option，指定列表项
	textarea：文本域
​		cols：指定列数，每行多少字符
		rows：指定行数
​			

<form action="#" method="get">
        <label for="username">用户名</label>:
            <input type="text" name="username" placeholder="请" id="username"><br/>
        密码： <input type="password" name="password"><br/>
        性别：
            <input type="radio" name="gender" value="male" checked>男
            <input type="radio" name="gender" value="female">女<br/>
        爱好：
            <input type="checkbox" name="bobby" value="篮球">篮球
            <input type="checkbox" name="bobby" value="JAVA">JAVA<br/>
        图片：
            <input type="file" name="file"><br/>
        隐藏域:
            <input type="hidden" name="id" value="url"><br/>
        按钮
            <input type="button" value="登陆1"><br/>
        登陆
            <input type="submit" value="登陆"><br/>
        图片按钮
            <input type="image" src="HTML.iml"><br/>
        省份
            <select name="province">
                <option value="">--请--</option>
                <option value="1">北京</option>
                <option value="2" selected>上海</option>
            </select><br/>
        自我描述
            <textarea cols="40" rows="5" name="describition"></textarea>
    </form>
##### 详见p588

​			

### CSS
Cascading Style Sheets 层叠样式表
	层叠：多个样式可以作用于一个html的元素（标签）上，同时生效
优点：
	1、功能强大
	2、内容展示和样式控制分离
		降低耦合度
		让分工协作更容易
		提高开发效率

CSS的使用：CSS与html的结合：
	内联样式
		在标签内使用style属性指定css代码

	<div style="color:red;">hello css</div>
​	内部样式
​		在head标签内，定义style标签，style标签的标签体内容就是css代码

<style>
   	div{
        color: brown;
    }
</style>

仅当前页面可用，想要对整个模组使用，使用↓
	外部样式
		定义css资源文件
			a.css

​		在head标签内，定义link标签，引入外部的资源文件，两种写法：
```css
<link rel="stylesheet" href="css/a.css">
<!-- crtl+j -->
```

```
<style>    
	@import "css/a.css";
</style>
```

ZY:
	1、2、3种方式，css作用范围越来越大
	1方式不常用，2、3方式较常用

css语法
	格式：
		选择器：{
			属性名：属性值；
			属性名2：属性值；
		}
	ZY：
		每一对属性需要使用；隔开，最后一段可以不使用；
	

选择器：筛选具有相似特征的元素	
	分类：
		基础选择器
			id选择器：选择具体的id属性值的元素，建议一个html页面种id值唯一
				语法：#id属性值{}
			元素选择器：选择具有相同标签名称的元素
				语法：标签名称{}
			类选择器：选择具有相同的class属性值的元素
				语法：.class属性值{}
			优先级：id选择器优先级》类选择器》元素选择器
		扩展选择器
			选择所有元素：
				语法：*{}
			并集选择器：选择多个子选择器的情况
				语法：选择器1，选择器2{}
			子选择器：筛选选择器1元素下的选择器2元素//中间为空格
				语法：选择器1 选择器2{}
			父选择器：筛选选择器2上的父元素（选择器1）
				语法：选择器1>选择器2{}
			属性选择器：选择元素名称，属性名=属性值的元素
				语法：元素名称[属性名=“属性值”]{}
			伪类选择器：选择一些元素所具有的状态
				语法：元素：状态{}
					如<a>
						link:初始化的状态
						visited：被访问过的状态
						active：正在访问的状态
						hover：鼠标悬浮的状态

属性：
	1、字体、文本
		font-size:字体大小
		color：文本颜色
		text-align：对齐方式
		line-height：行高
	2、背景
		background：（复合属性）
	3、边框
		border：设置边框（复合属性）
	4、尺寸
		width：宽度
		height：高度
	5、盒子模型
		margin：外边距
		padding：内边框
			默认情况下内边距会影响整个盒子的大小
			box-sizing：border-box；设置盒子的属性，让width和height是最终盒子大小
			float：浮动
				left/right
	

<style>
        *{
            margin: 0px;
            padding: 0px;
            box_sizing:border-box;
        }
        body{
            background: url("33e3874df5fd4df0a6fabfbfdaebd222.gif");
        }
        .reg_layout{
            width: 900px;
            height: 500px;
            border: 8px solid #EEEEEE;
            background: white;
            padding: 15px;
            margin: 50px auto auto;
        }
        .reg_left{
            /*border: 1px solid red;*/
            float: left;
            margin: 15px;
        }
        .reg_right{
            /*border: 1px solid red;*/
            float: right;
            margin: 15px;
        }
        .reg_right p{
            color: #ffd026;

        }
        .reg_right p a{
            color: pink;
        }
        .center{
            border 1px solid red;
            float: left;
            width: 450px;
        }
        .reg_left>p:first-child{
            color: #ffd026;
            font-size: 20px;
        }
        .reg_left>p:last-child{
            font-size: 20px;
            color: aliceblue;
        }
        div div form table tr td{
            width: 100px;
            text-align: right;
            height: 40px;
            padding-left: 50px;
        }
        #username,#checkc,#password,#birth,#em{
            width: 251px;
            height: 35px;
            border 1px solid gray;
            border-radius: 5px;
            align-content: center;
        }
        #checkc{
            width: 110px;
        }
        #img_c{
            height: 32px;
            vertical-align: middle;
        }
        #butt{
            width:150px;
            height: 40px;
            background: darkgoldenrod;
            border: 1px solid darkgray;
        }
    </style>


    <div class="reg_layout">
        <div class="reg_left">
            <p>新用户注册</p>
            <p>User Register</p>
        </div>
        <div class="reg_right">
            <p>已有账号？<a href="1_HelloWorld.html">立即登陆</a></p>
        </div>
        <div class="center">
            <div class="re_form">
                <form action="#" method="post">
                    <table>
                        <tr>
                            <td><label for="username">用户名</label></td>
                            <td><input type="text" name="username" id="username" placeholder="用户名"></td>
                        </tr>
                        <tr>
                            <td><label for="password">密码</label></td>
                            <td><input type="password" name="password" id="password"></td>
                        </tr>
                        <tr>
                            <td><label for="em">邮箱</label></td>
                            <td><input type="email" name="em" id="em"></td>
                        </tr>
                        <tr>
                            <td><label>性别</label></td>
                            <td><input type="radio" name="gender" value="male">男
                                <input type="radio" name="gender" value="female">女</td>
                        </tr>
                        <tr>
                            <td><label for="birth">出生日期</label></td>
                            <td><input type="date" name="birth" id="birth"></td>
                        </tr>
                        <tr>
                            <td><label for="checkc">验证码</label></td>
                            <td><input type="text" name="checkc" id="checkc">
                                <img id="img_c" src ="33e3874df5fd4df0a6fabfbfdaebd222.gif">
                            </td>
                        </tr>
                        <tr>
                            <td colspan="2" align="centet"><input type="submit" id="butt" value="注册"></td>
                        </tr>
                    </table>
                </form>
            </div>
        </div>
    </div>
</body>
</html>

## JS

##### JavaScript

​	概念：一门客户端脚本语言
​		运行在客户端浏览器中，每个浏览器都有JavaScript解析引擎
​		脚本语言：不需要编译，直接就可以在浏览器中解析执行
​	功能：
​		可以来增强用户和html页面的交互过程
​		可以控制html元素，让页面元素有一些动态效果，增强用户体验
​	发展史：
​		1992，Nombase，开发第一门客户端脚本语言，专门用以表单校验：c-- 
​			后更名ScriptEase
​		1995，Netscape（网景）开发了一门客户端脚本语言 LiveScript
​			sun与ns修改了LiveScript,命名为JavaScript
​		1996，mircosoft抄袭js开发了JScript
​		1997，ECMA（欧洲计算机制造商协会）统一了js语言标准，为ECMAScript
​	JavaScript = ECMAScript+JavaScript(BOM'对象'+DOM'对象')
​	

##### ECMAScript

​	基本语法
​		1与html的结合方式
​			内部JS
​				定义script标签，标签体就是js代码
​			外部JS
​				定义<script>，通过src属性引入外部的js文件
​			ZY:
​				<script>可定义在任何位置，js代码运行顺序由标签位置决定
​				<script>标签可定义多个
​		2注释
​			单行注释//
​			多行注释/* */
​		3数据类型
			原始数据类型（基本数据类型）
				1 number	整数、小数、NAN（not a number）
				2 string	字符、字符串
				3 boolean	
				4 null		一个对象为空的占位符
				5 undefined	未定义。如果一个变量没有初始化，则默认赋值undefined
			引用数据类型：对象
				
​		4变量：一块存储数据的内存空间
			Java语言是强类型语言，JS是弱类型的语言
				强类型：申请空间时规定变量类型，只能存储固定类型的数据
				弱类型：申请空间时不规定变量类型，可以存放任意类型的数据
			语法：var 变量名 = 初始化值；
				//输出到页面上 document.write();
				//typeof();
​		5运算符
			一元运算符：只有一个运算数：
				++ --,+
					//++ --同c
					//在js中，如果运算数不是运算符所要求的类型，那么js会自动的将运算数按照运算类型进行类型转换
						string转number：
							string数字：按照字面值转换
							string非数字：则转为NaN
								NaN做任何运算均为NaN
								var b = +（abc）->b= NaN
						boolean转number：
							true，1；false，2；
						
			算术运算符
				+ - * / % 
					//除法保留16位小数
			赋值运算符
				= += -=
			比较运算符
				< > <= >= = == ===（全等于）
					比较方式：
						类型相同，直接比较
							字符串：按照字典顺序比较，按位逐一比较
						类型不同，先进行类型转换，在比较
							===在比较之前，先判断类型，如果类型不同，则返回false
							true>false
			逻辑运算符
				&& || ！
					其他类型转boolean
						number：0或NaN为假，其余为真
						string：除了空字符串“”，其余都是true
						null、undefined 都是false
						对象	所有对象都是true
			三元运算符
				? :
​		6流程控制语句
			if else
			switch
				java中，switch语句可以接收的数据类型：byte int short char，enum，string
					switch（变量）{
					case
					}
				js中，switch语句可以接收任意的数据类型
			while
			do while
			for
		7特殊语法：
			js内，如果一行只有一条语句，可以省略“；”
			[变量可以使用var关键字，也可以不使用]
				用var：定义的变量是局部变量
				不用var：定义的变量是全局变量
​	基本对象
		Function对象：函数对象
			1、创建
				function fun1(形参){方法体}
					参数列表：（a，b，c...）
				var fun1 = function () {}
			2、方法
			3、属性
				length：代表形参的个数
			4、特点
				方法定义时，形参不写类型
				方法是一个对象，如果定义名称相同的方法，那么后者覆盖前者
				[js中，方法的调用只与方法的名称有关，和参数列表无关]
					不传则为undefined，多传则传入参数对象数组中
				在方法声明中，有一个隐藏的内置对象（数组）：arguments，封装所有的实际参数
					alert(argument[0]);
				
			5、调用
		Array
		Boolean
		Date
		Math
		Number
		String
		RegExp
		Global


































