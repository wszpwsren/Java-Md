

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
​			原始数据类型（基本数据类型）
​				1 number	整数、小数、NAN（not a number）
​				2 string	字符、字符串
​				3 boolean	
​				4 null		一个对象为空的占位符
​				5 undefined	未定义。如果一个变量没有初始化，则默认赋值undefined
​			引用数据类型：对象
​				
​		4变量：一块存储数据的内存空间
​			Java语言是强类型语言，JS是弱类型的语言
​				强类型：申请空间时规定变量类型，只能存储固定类型的数据
​				弱类型：申请空间时不规定变量类型，可以存放任意类型的数据
​			语法：var 变量名 = 初始化值；
​				//输出到页面上 document.write();
​				//typeof();
​		5运算符
​			一元运算符：只有一个运算数：
​				++ --,+
​					//++ --同c
​					//在js中，如果运算数不是运算符所要求的类型，那么js会自动的将运算数按照运算类型进行类型转换
​						string转number：
​							string数字：按照字面值转换
​							string非数字：则转为NaN
​								NaN做任何运算均为NaN
​								var b = +（abc）->b= NaN
​						boolean转number：
​							true，1；false，2；
​						
​			算术运算符
​				+ - * / % 
​					//除法保留16位小数
​			赋值运算符
​				= += -=
​			比较运算符
​				< > <= >= = == ===（全等于）
​					比较方式：
​						类型相同，直接比较
​							字符串：按照字典顺序比较，按位逐一比较
​						类型不同，先进行类型转换，在比较
​							===在比较之前，先判断类型，如果类型不同，则返回false
​							true>false
​			逻辑运算符
​				&& || ！
​					其他类型转boolean
​						number：0或NaN为假，其余为真
​						string：除了空字符串“”，其余都是true
​						null、undefined 都是false
​						对象	所有对象都是true
​			三元运算符
​				? :
​		6流程控制语句
​			if else
​			switch
​				java中，switch语句可以接收的数据类型：byte int short char，enum，string
​					switch（变量）{
​					case
​					}
​				js中，switch语句可以接收任意的数据类型
​			while
​			do while
​			for
​		7特殊语法：
​			js内，如果一行只有一条语句，可以省略“；”
​			[变量可以使用var关键字，也可以不使用]
​				用var：定义的变量是局部变量
​				不用var：定义的变量是全局变量
​	基本对象
​		Function对象：函数对象
​			1、创建
​				function fun1(形参){方法体}
​					参数列表：（a，b，c...）
​				var fun1 = function () {}
​			2、方法
​			3、属性
​				length：代表形参的个数
​			4、特点
​				方法定义时，形参不写类型
​				方法是一个对象，如果定义名称相同的方法，那么后者覆盖前者
​				[js中，方法的调用只与方法的名称有关，和参数列表无关]
​					不传则为undefined，多传则传入参数对象数组中
​				在方法声明中，有一个隐藏的内置对象（数组）：arguments，封装所有的实际参数
​					//alert(argument[0]);
​				
​			5、调用
​				Array数组对象
​					1创建
​						var arr= new Array（元素列表）；
​						var arr = new Array(默认长度)；
​						var arr = [元素列表]；
​					2方法
​						join（参数）；将数组中的元素按照指定的分隔符拼接为字符串
​							//默认为“，”分割
​						push（参数）：向数组的尾部添加一个元素（参数）
​					3属性
​						length：长度
​					4特点
​						js中，数组的元素类型可变（不限制类型）
​						js中，数组的长度可变，未赋值元素未undefined
​				Boolean
​				Date
​					1、创建：
​						var date = new date（）；
​					2、方法
​						toLocalString（）；返回当前date对象对应的时间本地字符串
​						getTime（）；获取毫秒值，返回1970/1/1至描述时间的毫秒
​				Math
​					1、创建：
​						不用创建，使用静态方法
​					2、方法：
​						random（）；返回0-1之间的数，含0不含1
​						ceil（）上取整
​						floor（）下取整
​						round（）四舍五入
​				Number
​				String
​				RegExp 正则表达式对象
​					1、单个字符匹配[参数]://多个参数为或连接
​						特殊定义
​							\d：单个数字字符
​							\w：单个单词字符[a-zA-Z0-9_]
​					2、量词符号：
​						* 表示出现0次或多次
​							// \w*
​						? 表示出现0次或1次
​						+ 表示出现1次或多次
​						{m,n} 表示出现次数>=m,<=n
​							// \w{6,12}
​							// m，n可缺省
​					3、对象：
​						创建：
​							var reg = new RegExp（“表达式”）；
​								new RegExp（“^\\w{6,23}$”）;
​								//注意反斜杠（转译符号）
​							var reg = /表达式/；
​						方法：
​							test（参数）：验证指定的字符串是否符合正则定义的规范
​							reg2.test（“xxx”）；
​					4、开始结束服号：
​						^:表示以此起始
​						$：表示以此结尾
​		​				Global
​					特点：全局对象，这个Global中封装的方法不需要对象就可以直接调用
​					URL编码：
​						步骤：将汉字转为二进制位，再转为16进制位，在16进制位前加%分割，gbk单个汉字2字节，utf8单个汉字3个字节
​					方法：
​						1、encodeURI（）：	URL编码
​						2、decondeURI（）：	URL解码
​						3、encodeURIComponent（）URL编码，编码的字符更多（：/等）
​						4、decodeURIComponent（）URL解码
​						5、parseInt（）将字符串转为数字
​							遍历字符是否是数字直到非数字为止，将数字转为number
​								//如果第一个为字符，返回NaN
​						6、isNaN（）：判断一个值是不是NaN
​							NaN：参与的==均为false
​						7、eval（）：将js的字符串转为脚本执行
​							document.write(eval("alert(123)"))
​							//弹出123，打印undefined
​							

#### JavaScript:

DOM简单学习：
	功能：控制html文档的内容
	代码：获取页面标签（元素）对象 （Element）
		document.getElementById("id值")
			通过元素的id获取元素对象
			//一般放在body结束符前
	操作element对象：
		修改属性值：
			明确获取的对象
			明确对象的属性值
		修改标签体内容：
			标签体属性：innerHTML
事件简单学习：
	概念：某些组件被执行了某些操作后，触发某些代码的执行
	绑定事件：
		第一种：直接在html标签上，指定事件的属性，属性值为js代码
			缺点：代码耦合
		第二种：通过js获取元素对象，指定事件属性，设置一个函数

	<script>
	    function f() {
	        alert('miao');
	    }
	    var elementById1 = document.getElementById("cat");
	    elementById1.onclick = f;
	</script>
​	事件：onclick

​	ECMAScript:
​	BOM:
​		概念：Browser Object Model 浏览器对象模型
​			浏览器窗口对象：Window
​				//包含DOM对象
​				1、创建
​				2、方法
​					1、与弹出框有关方法
​						alert（）；弹出一个警告框
​						comfirm（）；显示带有一段消息及确认按钮和取消按钮的对话框
​							如果用户点击确认，则方法返回true；
​							如果点击取消，则返回false
​						prompt（）；显示用户输入的对话框
​							返回值获取用户输入的值
​					2、与打开关闭有关的方法
​						close();关闭调用浏览器窗口
​						open();打开一个新的窗口，返回该窗口对象
​					3、与定时器有关的方法
​						setTimeout（，）； 在指定的毫秒数后调用方法
​							参数1：js代码或方法对象
​							参数2：毫秒值
​						clearTimeout（）；取消settimeout
​							参数：定时器对象
​						setInterval（）；按照指定周期调用方法
​						clearIntercal（）；取消循环定时器
​							参数：循环定时器对象
​				3、属性
​					1、获取其他BOM对象
​						history
​						location
​						navigater
​						screen
​					2、获取DOM对象
​						document（DOM）
​							window.document.getElementById();
​				4、特点
​					Window对象不需要创建，可以直接使用window引用
​					window引用可以省略。//alert（）；
​			浏览器对象:Navigator
​			地址栏对象：Location对象
​				1、创建（获取）
​					location
​					window.location
​				2、方法
​					reload（）重新加载当前文档（刷新）
​				3、属性
​					href（）设置或返回完整的URL
​			浏览器历史记录对象：History
​				包含用户在当前页面访问过的历史URL
​				创建：window.history（）
​				方法：
​					back（） 加载history列表的前一个URL
​					forward（） 加载history列表的后一个URL
​					go（） 加载history列表的具体页面
​						参数：
​							正数：前进几个历史记录
​							负数：后退几个历史记录
​				属性：
​					length 返回历史URL数量
​					
​			显示器屏幕对象：Screen

​	DOM:
​		概念：Document Object Moel 文档对象模型
​			将标记语言文档的各个组成部分，封装为对象，可以使用这些对象，对标记语言文档进行CRUD的动态操作
​			DOM是W3C的标准，定义了访问HTML和XML文档的标准
​				核心DOM	针对任何结构化文档的标准模型
​					Document对象
​					Element对象
​					Attribute对象
​					Text对象
​					Comment对象
​					Node对象，其他五个的父对象
​				XML DOM	
​				HTML DOM 针对HTML文档的标准模型
​				DOM将HTML文档表达为树结构
​		

核心DOM
	Document对象:
		获取：
			在html dom模型中可以使用window对象来获取
				window.document
		方法：
			获取Element对象
				getElementById：根据Id属性值获取元素对象
				getElementsByTagName:根据标签获取元素对象，返回一个数组<数组>
				getElementsByClassName：根据class属性获取元素对象
				getElementsByName：根据name属性值获取元素对象
			创建其他DOM对象
				createAttribute（name）
				createComment（）
				createElement（）
		
	Element对象
		获取：
			通过document来获取和创建
		方法:
			removeAttribute（）删除属性
			setAttribute（）设置属性
		属性：
		
	Node对象，顶层父对象
		所有的DOM对象都可以认为是节点
		方法：
			CRUD DOM树：
				appendChild（）；向节点的子节点列表的结尾添加新的子节点
					
				removeChiled();删除（并返回）当前节点的指定子节点
					//必须指定
				replaceChild（）；用新节点替换指定子节点
		//超链接功能：
			1、可以被点击，存在样式
			2、点击后跳转到href指定的url
			需求：保留1去掉2
			实现：href设置=“javascript：void（0）；”
			实现：我使用#
HTML DOM：
	标签体的设置和获取：innerHTML
		xxx.innerHTML+="<input type='text'>"
	使用HTML元素对象的属性
	控制元素样式
		使用元素的style属性来设置
			div.style.border ="1px solid red";
		提前定义好类选择器的样式，通过对元素的class属性赋值调用定义的样式
事件：（监听机制）
	概念：某些组建被执行了某些操作后，除法某些代码的执行
		事件：某些操作，比如：点击，双击，鼠标移动
		事件源：组建： 如按钮 文本输入框
		监听器：代码
		注册监听：将事件、事件源、监听器结合在一起。
			当事件源发生了事件，则触发执行某个监听器代码
	常见事件：
		点击事件：
			onclick：单击
			onblclick：双击
		焦点事件：
			onblur：失去焦点
				//用于表单校验
			onfocus：获得焦点
		加载事件：
			onload：页面或图像加载完成
				//用于保证页面加载完成（script标签的位置相关）
		鼠标事件：
			onmousemove 移动
			onmouseup	松开
			onmousedown 按下
				定义方法时，定义一个形参来接收event对象
					event.button可以获取被点击的鼠标键，//左0中1右2
			onmouseover 上
			onmouseout	移开
		键盘事件：
			onkeyup
			onkeydown
				event.keyCode获取被点击的key，回车13
			onkeypress
		选中/改变事件：
			onselcet 被选中
			onchange 域内容被改变
				//作用于下拉列表
		表单事件
			oncommit 确认按钮被点击
				方法返回false；则表单被阻止
	

```
1、document.getElementById("form").onsubmit = function (ev) {
            var flag = false;
            return flag;
        }
2、<form action="#"  id="form" onclick="return check（）；">
```

​			onreset 重置按钮被点击

//表单提交
	给表单绑定onsubmit事件，监听其中判断每一个方法校验的结果
		如果都为true，监听器返回true
	定义一些方法分别校验各个表单项
	给各个表单绑定onblur事件
	

#### bootstrap
	概念：一个前端开发框架，来自twitter，基于HTML，CSS，JavaScript
		框架：一个半成品软件
		好处：
			定义了很多css样式和js插件，可以直接使用这些样式和插件
			响应式布局
	快速入门
		在项目中将三个文件夹复制到项目中
		创建html页面，引入必要的资源文件

bootstrap基础文件


<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- Bootstrap -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" rel="stylesheet">
    
    <!-- HTML5 shim 和 Respond.js 是为了让 IE8 支持 HTML5 元素和媒体查询（media queries）功能 -->
    <!-- 警告：通过 file:// 协议（就是直接将 html 页面拖拽到浏览器中）访问页面时 Respond.js 不起作用 -->
    <!--[if lt IE 9]>
    <script src="https://cdn.jsdelivr.net/npm/html5shiv@3.7.3/dist/html5shiv.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/respond.js@1.4.2/dest/respond.min.js"></script>
    <![endif]-->
</head>
<body>

<h1>你好，世界！</h1>
<!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->

<script src="https://cdn.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"></script>
<!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/js/bootstrap.min.js"></script>
</body>
</html>


###BootStrap
	响应式布局
		同一套页面可以兼容不同分辨率的设备
		实现：依赖于栅格系统
			栅格系统：将一行平均分为12个格子，可以指定元素占几个格子
		步骤：
			1、定义容器，相当于table 
				容器分类：
					container 每个栅格宽度固定，两边留白
					container——fluid 100%宽度占比
			2、定义行，相当于tr	样式：row
			3、定义元素，相当于td，指定该元素在不同的设备上占多少个格子	样式 col-设备代号-格子数目
				设备代号
					 .col-xs- 手机 <768px
					 .col-sm- PAD >=768px
					 .col-md- 中等屏幕 >=992px
					 .col-lg- 大型屏幕 >=1200px
			4、注意事项
				一行中如果格子数目超过12，则超出部分自动换行
				栅格类属性可以相上兼容 ，适用于与屏幕宽度大于或等于分界点大小的设备
				如果设备分辨率小于了栅格类属性的分辨率最小值，会一个元素占满一行（栅格类并不向下兼容）
			
	CSS样式和插件
		全局CSS样式：//文档内查看
			按钮：class=  “btn btn-default”
			图片
				<img src="1az.jpg" class="img-responsive" alt="Responsive image">
				//1、响应式
			表格
				
			表单
				给表单项添加form-control类标签，或标签添加form-group类标签
				
		组件：
			导航条
				
			分页条
				
		插件：
			Carowsel 轮播图

###XML
	概念 Extersible Markup Language 可扩展标记语言
		可扩展：标签都是自定义的
	功能
		存储数据
			1、作为配置文件
			2、在网络中传输
	Xml与Html的区别
		xml标签自定义，html标签预定义
		xml语法严格，html语法宽松
		xml存储数据，html展示数据
		//xml-properties
		//w3c 万维网联盟 world wide web consortium
	语法结构
		基本语法
			xml文档的后缀名
			xml的第一行必须是文档声明
			xml有且只有一个根标签
			属性值必须使用引号包含
			必须有开始/结束标签，或者为自闭合标签
			xml标签名称区分大小写
		组成部分
			文档声明
				格式：<?xml version='1.0' ?>
				属性列表：
					version 常用1.0，必须属性
					encoding 编码方式 告知解析引擎当前文档使用的字符集，默认ISO-8859-1
					standalone //yes\no
					//依赖于其他文件
			指令：结合css控制标签样式
			标签
				命名规则：
					包含字母数字其他字符
					不能以数字或标点开始
					不能以xml开头
					不能包含空格
			标签属性
				id属性值唯一
			文本内容
				代码需要转译
					<  &lt
					>  &gt
					&  &amp
						CDATA区：该区域内数据会原样展示
							<![CDATA[
							//code
							]]>
		约束：说明文档，规定xml文档的书写规则
			xml编写：软件使用者
			xml解析：软件
			约束编写：框架开发者
			分类：
				DTD：一种简单约束技术,无法限定内容
					引入DTD文档到XML文档中
						内部DTD
							将约束规则定义在XML文档中
							

```
<!DOCTYPE students[
		//dtd代码
		]>
```

​					外部DTD
​						将约束的规则定义在外部DTD文件中
​							本地

```
<!DOCTYPE students/*根标签名*/ SYSTEM "student.dtd"/*src*/>
```

​							网络

```
	<!DOCTYPE students PUBLIC "student.dtd" "src">  
```

​				Schema：一种复杂约束技术

```
<?xml version="1.0"?>
<xsd:schema xmlns="http://www.itcast.cn/xml"
        xmlns:xsd="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://www.itcast.cn/xml" elementFormDefault="qualified">
    <xsd:element name="students" type="studentsType"/>
    <xsd:complexType name="studentsType">
        <xsd:sequence>
            <xsd:element name="student" type="studentType" minOccurs="0" maxOccurs="unbounded"/>
        </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="studentType">
        <xsd:sequence>
            <xsd:element name="name" type="xsd:string"/>
            <xsd:element name="age" type="ageType" />
            <xsd:element name="sex" type="sexType" />
        </xsd:sequence>
        <xsd:attribute name="number" type="numberType" use="required"/>
    </xsd:complexType>
    <xsd:simpleType name="sexType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="male"/>
            <xsd:enumeration value="female"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="ageType">
        <xsd:restriction base="xsd:integer">
            <xsd:minInclusive value="0"/>
            <xsd:maxInclusive value="256"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="numberType">
        <xsd:restriction base="xsd:string">
            <xsd:pattern value="heima_\d{4}"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema> 
```

​	

解析： 操作XML文档，将文档中的数据读取到内存中
	操作XML文档
		解析：读取
		写入：保存
	解析方式：
		DOM：DOM树
		SAX：逐行读取
	XML常见解析器
		JAXP：sun公司，支持DOM，SAX
		DOM4J：解析器
		JSOUP：Java的html解析去，可解析XML，DOM
		PULL：Android内置，SAX
	Jsoup
		入门：
			导入jar包
			获取Document对象
			获取对应标签Element对象
			获取数据

```
public static void main(String[] args) throws IOException {
        //根据xml文档获取document
        String path = JsoupDemo.class.getClassLoader().getResource("schema/student.xml").getPath();
        //解析xml文档，加载进内存，获取dom树->Document
        Document doc = Jsoup.parse(new File(path), "utf-8");
        Elements ele = doc.getElementsByTag("name");
        Element element = ele.get(0);
        String text = element.text();
        System.out.println(text);
    }
```

​		常见对象：
​			Jsoup：工具类，可以解析HTML或XML文档，返回DOcument
​				parse：解析html或xml文档，返回document
​					parse（File in STring charset charsetname）
​					parse（string html）
​					parse（Url url ，int timeoutmillis）
​			Document对象：文档对象，DOM树
​					获取element对象
​						getElemenetByid(String idvalue)根据id value
​						getElementsByTag（String Tagname）
​						getElementsByAttribute（String key）
​						getElementsByAttributeValue(String key string v)
​			Elements对象：元素Element的集合，可当作Arraylist<Element>使用
​			Element对象：元素对象
​				获取子对象
​					getElemenetByid(String idvalue)根据id value
​						getElementsByTag（String Tagname）
​						getElementsByAttribute（String key）
​						getElementsByAttributeValue(String key string v)
​				获取属性值
​					String attr（String key）根据属性的名称获取值
​				获取文本
​					string text（）：获取子标签的纯文本内容
​					string html（）：获取inner html 标签体的所有内容，包括子标签的的标签内容及文本内容
​			Node对象：节点对象
​				父类对象
​				方法
​					childnode
​					

​	快捷查询方式:
​		selector：选择器
​			使用的方法：
​				elements select（String sccQuery）
​					语法：a[href] 参考Select类中定义的语法

```
Elements select1 = doc.select("student[number='heima_0001']");
```
```
Elements select1 = doc.select("student[number='heima_0001']>age");
```

​		xPath：

​			使用Jsoup的Xpath需要额外导入jar
​			














