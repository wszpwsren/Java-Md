# AJAX

概念：ASynchronous JavaScript And XML
	异步同步：客户端服务端相互通信的基础上
		同步：客户端必须等待服务器端的响应，等待期间不能做其他操作
		异步：服务端处理请求的同时客户端不等待
	Ajax 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。 
		Ajax 可以使网页实现异步更新。
	## 实现方式：
		## 原生的JS实现方式（了解）
				 //1.创建核心对象
	            var xmlhttp;
	            if (window.XMLHttpRequest)
	            {// code for IE7+, Firefox, Chrome, Opera, Safari
	                xmlhttp=new XMLHttpRequest();
	            }
	            else
	            {// code for IE6, IE5
	                xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
	            }
	
	            //2. 建立连接
	            /*
	                参数：
	                    1. 请求方式：GET、POST
	                        * get方式，请求参数在URL后边拼接。send方法为空参
	                        * post方式，请求参数在send方法中定义
	                    2. 请求的URL：
	                    3. 同步或异步请求：true（异步）或 false（同步）
	
	             */
	            xmlhttp.open("GET","ajaxServlet?username=tom",true);
	
	            //3.发送请求
	            xmlhttp.send();
	
	            //4.接受并处理来自服务器的响应结果
	            //获取方式 ：xmlhttp.responseText
	            //什么时候获取？当服务器响应成功后再获取
	
	            //当xmlhttp对象的就绪状态改变时，触发事件onreadystatechange。
	            xmlhttp.onreadystatechange=function()
	            {
	                //判断readyState就绪状态是否为4，判断status响应状态码是否为200
	                if (xmlhttp.readyState==4 && xmlhttp.status==200)
	                {
	                   //获取服务器的响应结果
	                    var responseText = xmlhttp.responseText;
	                    alert(responseText);
	                }
	            }
	## JQeury实现方式
		1. $.ajax()
			* 语法：$.ajax({键值对});
			 //使用$.ajax()发送异步请求
	            $.ajax({
	                url:"ajaxServlet1111" , // 请求路径
	                type:"POST" , //请求方式
	                //data: "username=jack&age=23",//请求参数
	                data:{"username":"jack","age":23},
	                success:function (data) {
	                    alert(data);
	                },//响应成功后的回调函数
	                error:function () {
	                    alert("出错啦...")
	                },//表示如果请求响应出现错误，会执行的回调函数
	
	                dataType:"text"//设置接受到的响应数据的格式
	            });
		2. $.get()：发送get请求
			* 语法：$.get(url, [data], [callback], [type])
				* 参数：
					* url：请求路径
					* data：请求参数
					* callback：回调函数
					* type：响应结果的类型
	
		3. $.post()：发送post请求
			* 语法：$.post(url, [data], [callback], [type])
				* 参数：
					* url：请求路径
					* data：请求参数
					* callback：回调函数
					* type：响应结果的类型

# JSON
概念： JavaScript Object Notation		JavaScript对象表示法
	Person p = new Person();
	p.setName("张三");
	p.setAge(23);
	p.setGender("男");

	var p = {"name":"张三","age":23,"gender":"男"};
	
	* json现在多用于存储和交换文本信息的语法
	* 进行数据的传输
	* JSON 比 XML 更小、更快，更易解析。

语法：
	1. 基本规则
		* 数据在名称/值对中：json数据是由键值对构成的
			* 键用引号(单双都行)引起来，也可以不使用引号
			* 值得取值类型：
				1. 数字（整数或浮点数）
				2. 字符串（在双引号中）
				3. 逻辑值（true 或 false）
				4. 数组（在方括号中）	{"persons":[{},{}]}
				5. 对象（在花括号中） {"address":{"province"："陕西"....}}
				6. null
		* 数据由逗号分隔：多个键值对由逗号分隔
		* 花括号保存对象：使用{}定义json 格式
		* 方括号保存数组：[] 
	2. 获取数据:
		1. json对象.键名
		2. json对象["键名"]
		3. 数组对象[索引]
		4. 遍历
			//不能这样获取值
		 	for (var personKey in person) {
				alert(personKey+":"+person)
			}
			//这样遍历获取
			for (var personKey in person) {
				alert(personKey+person[personKey])
			}
				 //1.定义基本格式
		        var person = {"name": "张三", age: 23, 'gender': true};
		
		        var ps = [{"name": "张三", "age": 23, "gender": true},
		            {"name": "李四", "age": 24, "gender": true},
		            {"name": "王五", "age": 25, "gender": false}];
	3、json数据和java对象的转换
        Json解析器
        	常见解析器Jsonlib、Gson、fastJson、jackson
        Json转Java
			1、导入jackson包
			2、创建Jackson核心对象ObjectMapper
			3、调用ObjectMapper的方法进行转换
				readValue(Json数据字符串，Class)
				
    	Java转Json
			使用步骤
				1、导入jackson包
				2、创建Jackson核心对象ObjectMapper
				3、调用ObjectMapper的方法进行转换
					Map s = oM.readValue(json,Map.class);
					/**
                     * 转换方法
                     *  writeValue（参数1，参数2）
                     *      参数1
                     *          FIle    obj转为json字符串，并保存到指定文件中
                     *          Writer  obj转为字符输出流
                     *          Outputstream obj转为字节输出流
                     *      参数2 obj
                     *  writeValueAsString(obj):将对象转为json字符串
                     */
					注解：
						@JsonIgnore：排除属性
							@JsonIgnore
    						private String password;
                    	@JsonFormat:属性格式化
                    		@JsonFormat(pattern = "yyyy-MM-dd")
							//专门处理时间数据
					复杂Java对象转换
						List
							[{},{},{}]
						Map		
							{x:{y:y},z:{u:i}}
							{"third":{"id":1,"username":"zhang","password":"111111"},"first":{"id":1,"username":"zhang","password":"111111"},"second":{"id":1,"username":"zhang","password":"1111121"}}
		
		1. 服务器响应的数据，在客户端使用时，要想当做json数据格式使用。有两种解决方案：
		1. $.get(type):将最后一个参数type指定为"json"
		2. 在服务器端设置MIME类型
		response.setContextType("text/html;charset=utf-8")
		
		
		
		
		
		
		
		
		
		
		
		
		