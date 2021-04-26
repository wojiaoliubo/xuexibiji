# 今日内容

1. JavaScript：
   1. ECMAScript:
   2. BOM:
   3. DOM:
      1. 事件

## DOM简单学习：为了满足案例需要

* 功能：控制html文档的内容

* 获取页面标签(元素)对象：Element

  * ```javascript
    document.getElementById("id值") // 通过元素的id获取元素对象
    ```

* 操作Element对象：

  1. 修改属性值：
     1. 明确获取的对象是哪一个？
     2. 查看API文档，找其中有哪些属性可以设置
  2. 修改标签体内容：
     1. 属性：innerHTML
     2. 获取元素对象
     3. 使用innerHTML属性修改标签体内容

## 事件简单学习

* 功能：某些组件被执行了某些操作后，触发某些代码的执行。

  * 造句：xxx被xxx，我就xxx
    * 我方水晶被摧毁后，我就责备队友。
    * 敌方水晶被摧毁后，我就夸奖自己。

* 如何让绑定事件

  1. 直接在html标签上，指定事件的属性（操作），属性值就是js代码

     1. 事件：onclick---单击事件

  2. 通过js获取元素对象，指定事件属性，设置一个函数

  3. 代码演示：

     ```html
     <!DOCTYPE html>
     <html>
     	<head>
     		<meta charset="utf-8">
     		<title></title>
     	</head>
     	<body>
     		<img src="../img/off.gif" id="light">
     		
     		
     		<script type="text/javascript">
     			/*
     				分析：
     					1. 获取图片对象
     					2. 绑定单击事件
     					3. 每次点击就来切换图片
     						规则
     						- 如果灯是开着的 on，切换图片为off
     						- 如果灯是开着的 off，切换图片为on
     						使用标记flag来完成
     			*/
     		   
     		//1. 获取图片对象
     		var light = document.getElementById("light");
     		
     		var flag = false;//代表灯是灭的，off图片
     		//2.绑定单击事件
     		 light.onclick = function() {
     			 if(flag) {//判断如果灯是开的，则灭掉
     				 light.src = "../img/off.gif";
     				 flag = false;
     			 }else {
     				//如果灯是灭的则打开
     				light.src = "../img/on.gif";
     				flag = true;
     			 }
     			 
     		 }
     		</script>
     	</body>
     </html>
     ```

# BOM:

1. 概念：Browser Object Model 浏览器对象模型

   - 将浏览器的各个组成部分封装成对象。

2. 组成：

   - Window：窗口对象
   - Navigator：浏览器对象
   - Screen：显示器屏幕对象
   - History：历史记录对象
   - Location：地址栏对象

3. Window：窗口对象

   1. 创建

   2. 方法

      1. 与弹出框有关的方法：

         ```javascript
         alert() // 显示带有一段消息和一个确认按钮的警告框。
         confirm() // 显示带有一段消息以及确认按钮和取消按钮的对话框。
         	- 如果用户点击确定按钮，则方法返回true
         	- 如果用户点击取消按钮，则方法返回false
         prompt() // 显示可提示用户输入的对话框。
         	- 返回值：获取用户输入的值
         ```

         ```html
         // 代码演示
         <!DOCTYPE html>
         <html>
         	<head>
         		<meta charset="utf-8">
         		<title>Window对象</title>
         		<script type="text/javascript">
         			/*
         				Window窗口对象
         			*/
         		   // alert('hello window');
         		   // window.alert('hello windows');
         		   
         		   var flag = confirm('您确定要退出吗');
         		   
         		   if(flag) {
         			   //退出
         			   alert('欢迎再次光临');
         		   } else {
         			   //提示：手别抖
         			   alert('手别抖');
         		   }
         		   
         		   var result = prompt('请输入用户名');
         		   alert(result);
         		</script>
         	</head>
         	<body>
         	</body>
         </html>
         ```

      2. 与打开关闭有关的方法：

         ```html
         close() // 关闭浏览器窗口。
         	- 谁调用我 ，我关谁
         open() // 打开一个新的浏览器窗口
         	- 返回新的Window对象
         ```

         ```html
         <!DOCTYPE html>
         <html>
         	<head>
         		<meta charset="utf-8">
         		<title></title>
         		
         	</head>
         	<body>
         		<input id="openBtn" type="button" value="打开窗口" />
         		<input id="closeBtn" type="button" value="关闭窗口" />
         		
         		
         		<script type="text/javascript">
         			var openBtn = document.getElementById("openBtn");
         			var newWindow;
         			openBtn.onclick = function() {
         				newWindow = open("http://www.baidu.com");
         			}
         			var clsoeBtn = document.getElementById("closeBtn");
         			closeBtn.onclick = function() {
         				newWindow.close();
         			}
         		</script>
         	</body>
         </html>
         ```

      3. 与定时器有关的方式

         ```javascript
         setTimeout() // 在指定的毫秒数后调用函数或计算表达式。
         	- 参数：
         		1. js代码或者方法对象
                 2. 毫秒值
             - 返回值：唯一标识，用于取消定时器
         clearTimeout() // 取消由 setTimeout() 方法设置的 timeout。
         setInterval() // 按照指定的周期（以毫秒计）来调用函数或计算表达式。
         clearInterval() // 取消由 setInterval() 设置的 timeout。
         ```

         ```html
         <!DOCTYPE html>
         <html>
         	<head>
         		<meta charset="utf-8">
         		<title></title>
         	</head>
         	<body>
         		
         		<script type="text/javascript">
         			//一次性定时器
         			//setTimeout("fun();",3000);
         			//var id = setTimeout(fun,3000);
         			//clearTimeout(id);
         			function fun() {
         				alert('boom~~~');
         			}
         			//循环定时器
         			var id = setInterval(fun,2000);
         			clearInterval(id);
         		</script>`	``
         	</body>
         </html>
         ```

      4. 案例2：轮播图

         ```html
         <!DOCTYPE html>
         <html>
         	<head>
         		<meta charset="utf-8">
         		<title>轮播图</title>
         		
         	</head>
         	<body>
         		<img id="img" src="../img/banner_1.jpg" width="100%">
         		
         		
         		
         		
         		
         		<script type="text/javascript">
         			/*
         				分析：
         					1.在页面上使用img标签来展示图片
         					2.定义一个方法，修改图片对象的src属性
         					3.定义一个定时器，每隔三秒调用一次方法
         			*/
         		   var number = 1;
         		   function fun() {
         			   number ++;
         			   if(number > 3) {
         				   number = 1;
         			   }
         			   //获取img对象
         			   var img = document.getElementById("img");
         			   img.src = "../img/banner_"+ number +".jpg";
         		   }
         		   //定义定时器
         		   setInterval(fun,1000);
         		</script>
         	</body>
         </html>
         ```

   3. 属性：

      1. 获取其他BOM对象：

         ```javascript
         history
         location
         Navigator
         Screen
         ```

      2. 获取DOM对象

         ```javascript
         document
         ```

   4. 特点

      - Window对象不需要创建可以直接使用 window使用。 window.方法名();
      - window引用可以省略。  方法名();

4. Location：地址栏对象

   1. 创建(获取)：

      1. window.location
      2. location

   2. 方法：

      - ```javascript
        reload() // 重新加载当前文档。刷新
        ```

      - ```html
        <!DOCTYPE html>
        <html>
        	<head>
        		<meta charset="utf-8">
        		<title></title>
        	</head>
        	<body>
        		
        		<input id="f5" type="button" value="刷新页面" />
        		
        		<script type="text/javascript">
        			//reload方法，定义一个按钮，点击按钮，刷新当前页面
        			alert('sss');
        			var f5 = document.getElementById("f5");
        			f5.onclick = function() {
        				location.reload();
        			}
        		</script>
        	</body>
        </html>
        ```

   3. 属性

      - ```javascript
        href // 设置或返回完整的URL。
        ```

        ```html
        <!DOCTYPE html>
        <html>
        	<head>
        		<meta charset="utf-8">
        		<title></title>
        	</head>
        	<body>
        		
        		<input type="button" id="goItcast" value="去传智" />
        		
        		
        		<script type="text/javascript">
        			//获取href
        			// var href = location.href;
        			//alert(href);
        			//点击按钮，去访问www.itcast.cn官网
        			var goItcast = document.getElementById("goItcast");
        			goItcast.onclick = function() {
        				location.href = "https://www.baidu.com";
        			}
        		</script>
        	</body>
        </html>
        ```

   4. 案例三：自动跳转首页

      ```html
      <!DOCTYPE html>
      <html>
      	<head>
      		<meta charset="utf-8">
      		<title>自动跳转</title>
      		<style type="text/css">
      			p{
      				text-align: center;
      			}
      			span{
      				color: #FF0000;
      			}
      		</style>
      	</head>
      	<body>
      		<p>
      			<span id="time">
      				5
      			</span>秒之后，自动跳转首页
      		</p>
      		
      		
      		
      		
      		<script type="text/javascript">
      			/**
      			 * 分析：
      			 * 	1. 显示页面效果   <p>
      			 *  2. 倒计时读秒的效果
      			 * 			2.1定义一个方法，方法获取span标签，修改span标签体的内容
      			 * 			2.2定义一个定时器，一秒执行一个
      			 *  3. 在方法中判断如果时间<=0,则跳转到首页
      			 */
      			var second = 5;
      			function showTime() {
      				second --;
      				if(second <= 0) {
      					location.href = "https://www.baidu.com";
      				}
      				var time = document.getElementById("time");
      				time.innerHTML = second + "";
      			}
      			
      			//设置定时器
      			setInterval(showTime,1000);
      		</script>
      	</body>
      </html>
      ```

5. History：历史记录对象

   1. 创建(获取)：

      1. ```javascript
         window.history
         ```

      2. ```javascript
         history
         ```

   2. 方法：

      - ```javascript
        back() // 加载 history 列表中的前一个 URL。
        ```

      - ```javascript
        forward() // 加载 history 列表中的下一个 URL。
        ```

      - ```javascript
        go(参数) // 加载 history 列表中的某个具体页面。
        	参数：
            	- 正数：前进几个历史记录
        		- 负数：后退几个历史记录
        ```

   3. 属性：

      - ```javascript
        length // 返回当前窗口历史列表中的 URL数量。
        ```

## DOM

- 概念： Document Object Model 文档对象模型

  - 将标记语言文档的各个组成部分，封装为对象。可以使用这些对象，对标记语言文档进行CRUD的动态操作

- W3C DOM 标准被分为 3 个不同的部分：

  - 核心 DOM - 针对任何结构化文档的标准模型
    - Document：文档对象
    - Element：元素对象
    - Attribute：属性对象
    - Text：文本对象
    - Comment:注释对象
    - Node：节点对象，其他5个的父对象
  - XML DOM - 针对 XML 文档的标准模型
  - HTML DOM - 针对 HTML 文档的标准模型

- 核心DOM模型：

  - Document：文档对象

    1. 创建(获取)：在html dom模型中可以使用window对象来获取

       1. ```javascript
          window.document
          ```

       2. ```javascript
          document
          ```

    2. 方法：

       1. 获取Element对象：

          1. ```javascript
             getElementById() // 根据id属性值获取元素对象。id属性值一般唯一
             ```

          2. ```javascript
             getElementsByTagName() // 根据元素名称获取元素对象们。返回值是一个数组
             ```

          3. ```javascript
             getElementsByClassName() // 根据Class属性值获取元素对象们。返回值是一个数组
             ```

          4. ```javascript
             getElementsByName() // 根据name属性值获取元素对象们。返回值是一个数组
             ```

          5. 代码演示：

             ```html
             <!DOCTYPE html>
             <html>
             	<head>
             		<meta charset="utf-8">
             		<title>Document对象</title>
             	</head>
             	<body>
             		
             		<div id="div1">
             			div1
             		</div>
             		
             		<div id="2">
             			div2
             		</div>
             		
             		<div id="3">
             			div3
             		</div>
             		
             		<div class="cls1">
             			div4
             		</div>
             		
             		<div class="cls1">
             			div5
             		</div>
             		
             		<input type="text" name="username" />
             		
             		
             		
             		<script type="text/javascript">
             			// 2. 根据元素名称获取元素对象们，返回值是一个数组
             			var divs = document.getElementsByTagName("div");
             			// alert(divs.length);
             			// 3.根据Class属性值获取元素对象们，返回值是一个数组
             			var div_cls = document.getElementsByClassName("cls1");
             			//alert(div_cls.length);
             			// 4.根据name属性值获取元素对象们。返回值是一个数组
             			var ele_username = document.getElementsByName("username");
             			//alert(ele_username.length);
             		</script>
             		
             	</body>
             </html>
             ```

       2. 创建其他DOM对象：

          1. ```javascript
             createAttribute(name)
             ```

          2. ```javascript
             createComment()
             ```

          3. ```javascript
             createElement()
             ```

          4. ```javascript
             createTextNode()
             ```

    3. 属性

- Element：元素对象

  1. 获取/创建：通过document来获取和创建

  2. 方法：

     1. ```javascript
        removeAttribute() // 删除属性
        ```

     2. ```javascript
        setAttribute() // 设置属性
        ```

     3. 代码演示
     
        ```html
        <!DOCTYPE html>
        <html>
        	<head>
        		<meta charset="utf-8">
        		<title>Element对象</title>
        	</head>
        	<body>
        		<a>点我试一试</a>
        		<input type="button" id="btn_set" value="设置属性" />
        		<input type="button" id="btn_remove" value="删除属性" />
        		
        		<script type="text/javascript">
        			
        			//获取btn_set
        			var btn_set = document.getElementById("btn_set");
        			//绑定单击事件
        			btn_set.onclick = function() {
        			//1.获取a标签
        			var element_a = document.getElementsByTagName("a")[0];
        			
        			element_a.setAttribute("href","http://www.baidu.com");
        			}
        			
        			var btn_remove = document.getElementById("btn_remove");
        			//绑定单击事件
        			btn_remove.onclick = function() {
        			//1.获取a标签
        			var element_a = document.getElementsByTagName("a")[0];
        			
        			element_a.removeAttribute("href");
        			}
        		</script>
        	</body>
        </html>
        ```

- Node：节点对象，其他5个的父对象

  - 特点：所有dom对象都可以被认为是一个节点

  - 方法：

    - CRUD dom树：

      - ```javascript
        appendChild() // 向节点的子节点列表的结尾添加新的子节点。
        ```

      - ```javascript
        removeChild() // 删除（并返回）当前节点的指定子节点。
        ```

      - ```javascript
        replaceChild() // 用新节点替换一个子节点。
        ```

      - 代码演示

        ```html
        <!DOCTYPE html>
        <html>
        	<head>
        		<meta charset="utf-8">
        		<title>Node对象</title>
        		<style type="text/css">
        			div{
        				border: 1px #FF0000 solid;
        			}
        			
        			#div1{
        				width: 200px;
        				height: 200px;
        			}
        			#div2{
        				width: 100px;
        				height: 100px;
        			}
        			#div3{
        				width: 100px;
        				height: 100px;
        			}
        		</style>
        	</head>
        	<body>
        		<div id="div1">
        			<div id="div2">
        				div2
        			</div>
        			div1
        		</div>
        		<!-- href不设置值时 会跳转本页面 -->
        		<a href="javascript:void(0);" id="del">删除子节点</a>
        		<a href="javascript:void(0);" id="add">添加子节点</a>
        		<!-- <input type="button" id="del" value="删除子节点" /> -->
        		
        		
        		<script type="text/javascript">
        			//1.获取超链接
        			var element_a = document.getElementById("del");
        			//2.绑定单击事件
        			element_a.onclick = function() {
        				var div1 = document.getElementById("div1");
        				var div2 = document.getElementById("div2");
        				div1.removeChild(div2);
        			}
        			
        			//1.获取超链接
        			var element_add = document.getElementById("add");
        			//2.绑定单击事件
        			element_add.onclick = function() {
        				var div1 = document.getElementById("div1");
        				//给div1添加子节点
        				//创建div节点
        				var div3 = document.createElement("div");
        				div3.setAttribute("id","div3");
        				//div1添加div3
        				div1.appendChild(div3);
        			}
        			
        			/**
        			 * 超链接功能：
        			 * 		1.可以被点击，样式
        			 * 		2.点击后跳转到href指定的url
        			 * 	需求：保留1，去除2
        			 *  实现：href="javascript:void(0);"
        			 */
        			
        		</script>
        	</body>
        </html>
        ```

  - 属性：

    - ```javascript
      parentNode // 返回节点的父节点。
      ```

    - ```html
      			var div2 = document.getElementById("div2");
        			var div1 = div2.parentNode;
        			alert(div1);
      ```
      
    
  - 案例4动态表格

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>动态表格</title>
    
        <style>
            table{
                border: 1px solid;
                margin: auto;
                width: 500px;
            }
    
            td,th{
                text-align: center;
                border: 1px solid;
            }
            div{
                text-align: center;
                margin: 50px;
            }
        </style>
    
    </head>
    <body>
    
    <div>
        <input type="text" id="id" placeholder="请输入编号">
        <input type="text" id="name"  placeholder="请输入姓名">
        <input type="text" id="gender"  placeholder="请输入性别">
        <input type="button" value="添加" id="btn_add">
    
    </div>
    
    
        <table id="table">
            <caption>学生信息表</caption>
            <tr>
                <th>编号</th>
                <th>姓名</th>
                <th>性别</th>
                <th>操作</th>
            </tr>
    
            <tr>
                <td>1</td>
                <td>令狐冲</td>
                <td>男</td>
                <td><a href="javascript:void(0);" onclick="delTr(this);">删除</a></td>
            </tr>
    
            <tr>
                <td>2</td>
                <td>任我行</td>
                <td>男</td>
                <td><a href="javascript:void(0);"  onclick="delTr(this);">删除</a></td>
            </tr>
    
            <tr>
                <td>3</td>
                <td>岳不群</td>
                <td>?</td>
                <td><a href="javascript:void(0);"  onclick="delTr(this);">删除</a></td>
            </tr>
    
    
        </table>
    
    
    
    <script>
        
        //1.获取btn
        var btn_add = document.getElementById("btn_add");
        //2.绑定单击事件
        btn_add.onclick = function(){
            //获取每一个输入框内容
            var id = document.getElementById("id").value;
            var name = document.getElementById("name").value;
            var gender = document.getElementById("gender").value;
    
            //获取表格
            var table = document.getElementById("table");
            
            //创建tr
            var tr = document.createElement("tr");
            //创建td
            var td_id = document.createElement("td");
            var text_id = document.createTextNode(id);
            td_id.appendChild(text_id);
            tr.appendChild(td_id);
    
            var td_name = document.createElement("td");
            var text_name = document.createTextNode(name);
            td_name.appendChild(text_name);
            tr.appendChild(td_name);
    
            var td_gender = document.createElement("td");
            var text_gender = document.createTextNode(gender);
            td_gender.appendChild(text_gender);
            tr.appendChild(td_gender);
    
            var td_option = document.createElement("td");
    
            var a = document.createElement("a");
            a.setAttribute("href","javascript:void(0);");
            a.setAttribute("onclick","delTr(this)");
            var text_a = document.createTextNode("删除");
            a.appendChild(text_a);
            td_option.appendChild(a);
            tr.appendChild(td_option);
    
            table.appendChild(tr);
    
        }
    
        function delTr(obj){
            var table = obj.parentNode.parentNode.parentNode;
            var tr = obj.parentNode.parentNode;
            table.removeChild(tr);
        }
    
    </script>
    </body>
    </html>
    ```

* HTML DOM

  1. 标签体的设置和获取：innerHTML

  2. 使用html元素对象的属性

  3. 控制元素样式

     1. 使用元素的style属性来设置

     2. 提前定义好类选择器的样式，通过元素的className属性来设置其class属性值。

     3. 代码演示

        ```html
        <!DOCTYPE html>
        <html>
        	<head>
        		<meta charset="utf-8">
        		<title>控制样式</title>
        		
        		<style type="text/css">
        			.d1{
        				border: 1px solid red;
        				width: 300px;
        				height: 5000px;
        			}
        			.d2{
        				border: 2px solid green;
        				width: 300px;
        				height: 5000px;
        			}
        		</style>
        	</head>
        	<body>
        		<div id="div1">
        			div
        		</div>
        		
        		<div id="div2">
        			div
        		</div>
        		
        		
        		
        		<script type="text/javascript">
        			var div1 = document.getElementById('div1');
        			div1.onclick = function() {
        				//修改样式方式1
        				div1.style.border = "1px solid red";
        				
        				div1.style.width = "300px";
        				
        				div1.style.fontSize = "20px";
        			}
        			
        			var div2 = document.getElementById('div2');
        			div2.onclick = function() {
        				div2.className = "d1";
        			}
        		</script>
        		
        	</body>
        </html>
        ```

  4. 案例4改进

     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <title>动态表格</title>
     
         <style>
             table{
                 border: 1px solid;
                 margin: auto;
                 width: 500px;
             }
     
             td,th{
                 text-align: center;
                 border: 1px solid;
             }
             div{
                 text-align: center;
                 margin: 50px;
             }
         </style>
     
     </head>
     <body>
     
     <div>
         <input type="text" id="id" placeholder="请输入编号">
         <input type="text" id="name"  placeholder="请输入姓名">
         <input type="text" id="gender"  placeholder="请输入性别">
         <input type="button" value="添加" id="btn_add">
     
     </div>
     
     
         <table id="table">
             <caption>学生信息表</caption>
             <tr>
                 <th>编号</th>
                 <th>姓名</th>
                 <th>性别</th>
                 <th>操作</th>
             </tr>
     
             <tr>
                 <td>1</td>
                 <td>令狐冲</td>
                 <td>男</td>
                 <td><a href="javascript:void(0);" onclick="delTr(this);">删除</a></td>
             </tr>
     
             <tr>
                 <td>2</td>
                 <td>任我行</td>
                 <td>男</td>
                 <td><a href="javascript:void(0);"  onclick="delTr(this);">删除</a></td>
             </tr>
     
             <tr>
                 <td>3</td>
                 <td>岳不群</td>
                 <td>?</td>
                 <td><a href="javascript:void(0);"  onclick="delTr(this);">删除</a></td>
             </tr>
     
     
         </table>
     
     
     
     <script>
         
         // //1.获取btn
         // var btn_add = document.getElementById("btn_add");
         // //2.绑定单击事件
         // btn_add.onclick = function(){
         //     //获取每一个输入框内容
         //     var id = document.getElementById("id").value;
         //     var name = document.getElementById("name").value;
         //     var gender = document.getElementById("gender").value;
     
         //     //获取表格
         //     var table = document.getElementById("table");
             
         //     //创建tr
         //     var tr = document.createElement("tr");
         //     //创建td
         //     var td_id = document.createElement("td");
         //     var text_id = document.createTextNode(id);
         //     td_id.appendChild(text_id);
         //     tr.appendChild(td_id);
     
         //     var td_name = document.createElement("td");
         //     var text_name = document.createTextNode(name);
         //     td_name.appendChild(text_name);
         //     tr.appendChild(td_name);
     
         //     var td_gender = document.createElement("td");
         //     var text_gender = document.createTextNode(gender);
         //     td_gender.appendChild(text_gender);
         //     tr.appendChild(td_gender);
     
         //     var td_option = document.createElement("td");
     
         //     var a = document.createElement("a");
         //     a.setAttribute("href","javascript:void(0);");
         //     a.setAttribute("onclick","delTr(this)");
         //     var text_a = document.createTextNode("删除");
         //     a.appendChild(text_a);
         //     td_option.appendChild(a);
         //     tr.appendChild(td_option);
     
         //     table.appendChild(tr);
     
         // }
     	//使用innerHTL添加
     	document.getElementById('btn_add').onclick = function() {
     		//2.获取文本的内容
     		var id = document.getElementById('id').value;
     		var name = document.getElementById('name').value;
     		var gender = document.getElementById('gender').value;
     		
     		//获取table
     		var table = document.getElementsByTagName("table")[0];
     		//追加一行
     		table.innerHTML += "<tr>\n" +
     				"	<td>"+id+"</td>\n" + 
     				"	<td>"+name+"</td>\n" + 
     				"	<td>"+gender+"</td>\n" + 
     				"	<td><a href=\"javascript:void(0);\"  onclick=\"delTr(this);\">删除</a></td>\n" + 
     				"	</tr>";
     	}
     
         function delTr(obj){
             var table = obj.parentNode.parentNode.parentNode;
             var tr = obj.parentNode.parentNode;
             table.removeChild(tr);
         }
     
     </script>
     </body>
     </html>
     ```

## 事件监听机制：

* 概念：某些组件被执行了某些操作后，触发某些代码的执行。

  * 事件：某些操作。如： 单击，双击，键盘按下了，鼠标移动了
  * 事件源：组件。如： 按钮 文本输入框...
  * 监听器：代码。
  * 注册监听：将事件，事件源，监听器结合在一起。 当事件源上发生了某个事件，则触发执行某个监听器代码。

* 常见的事件：

  1. 点击事件：

     1. ```javascript
        onclick // 单击事件
        ```

     2. ```javascript
        ondblclick // 双击事件
        ```

  2. 焦点事件：

     1. ```javascript
        onblur // 失去焦点
        	// 一般用于表单验证
        ```

     2. ```javascript
        onfocus // 元素获得焦点。
        ```

  3. 加载事件：

     1. ```javascript
        onload // 一张页面或一幅图像完成加载。
        ```

  4. 鼠标事件：

     1. ```javascript
        onmousedown // 鼠标按钮被按下。
        	// 定义方法时，定义一个形参，接收event对象
        	// event对象的button属性可以获取鼠标按钮被点击了。
        ```

     2. ```javascript
        onmouseup // 鼠标按键被松开。
        ```

     3. ```javascript
        onmousemove // 鼠标被移动。
        ```

     4. ```javascript
        onmouseover // 鼠标移到某元素之上。
        ```

     5. ```javascript
        onmouseout // 鼠标从某元素移开。
        ```

  5. 键盘事件：

     1. ```javascript
        onkeydown // 某个键盘按键被按下。
        ```

     2. ```javascript
        onkeyup // 某个键盘按键被松开。
        ```

     3. ```javascript
        onkeypress // 某个键盘按键被按下并松开。
        ```

  6. 选择和改变

     1. ```javascript
        onchange // 域的内容被改变。
        ```

     2. ```javascript
        onselect // 文本被选中。
        ```

  7. 表单事件：

     1. ```javascript
        onsubmit // 确认按钮被点击。
        	// 可以阻止表单的提交 返回true时才会被提交
        ```

     2. ```javascript
        onreset // 重置按钮被点击
        ```

  8. 代码演示：

     ```html
     <!DOCTYPE html>
     <html>
     	<head>
     		<meta charset="utf-8">
     		<title>常见事件</title>
     		
     		<script type="text/javascript">
     			
     			//2.加载完成事件
     			window.onload = function() {
     				//1.失去焦点事件
     				document.getElementById("username").onblur = function() {
     					alert("失去焦点了");
     				}
     				//3.绑定鼠标移动到元素之上事件
     				document.getElementById("username").onmouseover = function() {
     					//alert("鼠标来了");
     				}
     				//4.绑定鼠标点击事件
     				document.getElementById("username").onmousedown = function(event) {
     					//alert("鼠标点击了");
     					//alert(event.button);
     				}
     				document.getElementById("username").onkeydown = function(event) {
     					//alert(event.keyCode);
     					if(event.keyCode == 13) {
     						//alert("提交表单");
     					}
     				}
     				document.getElementById("city").onchange = function() {
     					//alert("改变了");
     				}
     				document.getElementById("form").onsubmit = function() {
     					//校验用户名格式是否正确
     					var flag = true;
     					
     					return flag;
     				}
     			}
     		</script>
     		
     	</head>
     	<body>
     			<form action="#" method="get" id="form">
     			<input type="" id="username" />
     			
     			<select id="city">
     				<option>请选择</option>
     				<option>北京</option>
     				<option>上海</option>
     				<option>广州</option>
     				<option>西安</option>
     			</select><br>
     			
     			<input type="submit"/>
     			
     			</form>
     	</body>
     </html>
     ```

## 案例

1. 案例5表格全选

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>表格全选</title>
       <style>
           table{
               border: 1px solid;
               width: 500px;
               margin-left: 30%;
           }
   
           td,th{
               text-align: center;
               border: 1px solid;
           }
           div{
               margin-top: 10px;
               margin-left: 30%;
           }
   		
   		.out{
   			background-color: white;
   		}
   		.over{
   			background-color: pink;
   		}
       </style>
   
   	
   	<script>
   		/**
   		 * 	分析：
   		 * 		1.全选：
   		 * 			- 获取所有的checkbox
   		 * 			- 遍历cb，设置一个cb的属性状态为选中 checked
   		 * 
   		 * 			
   		 */
   		
   		//1.在页面完后绑定事件
   		window.onload = function() {
   			//2.给全选按钮绑定单击事件
   			document.getElementById('selectAll').onclick = function() {
   				//全选功能
   				//1.获取所有的checkbox
   				var cbs = document.getElementsByName("cb");
   				//2.遍历
   				for(var i = 0;i <cbs.length;i++) {
   					//3.设置每一个cb的状态为选中，checked
   					cbs[i].checked = true;
   				}
   			}
   			//2.给全不选按钮绑定单击事件
   			document.getElementById('unSelectAll').onclick = function() {
   				//全不选功能
   				//1.获取所有的checkbox
   				var cbs = document.getElementsByName("cb");
   				//2.遍历
   				for(var i = 0;i <cbs.length;i++) {
   					//3.设置每一个cb的状态为未选中，checked
   					cbs[i].checked = false;
   				}
   			}
   			//3.给反选按钮绑定单击事件
   			document.getElementById('selectRev').onclick = function() {
   				//反选功能
   				//1.获取所有的checkbox
   				var cbs = document.getElementsByName("cb");
   				//2.遍历
   				for(var i = 0;i <cbs.length;i++) {
   					if(cbs[i].checked) {
   						cbs[i].checked = false;
   					} else {
   						cbs[i].checked = true;
   					}
   				}
   			}
   			//4.给first绑定单击事件
   			document.getElementById('firstCb').onclick = function() {
   				//全选功能
   				//1.获取所有的checkbox
   				var cbs = document.getElementsByName("cb");
   				//获取第一个cb
   				
   				//2.遍历
   				for(var i = 0;i <cbs.length;i++) {
   					//设置每一个cb的状态和第一个cb的状态一样
   					cbs[i].checked = this.checked;
   				}
   			}
   			//给所有的tr绑定鼠标移到元素之上和移出元素事件
   			var trs =  document.getElementsByTagName('tr');
   			//2.遍历
   			for(var i = 0;i < trs.length; i++) {
   				//移到元素之上的事件
   				trs[i].onmouseover = function() {
   					this.className = "over";
   				}
   				//移出元素之上的事件
   				trs[i].onmouseout = function() {
   					this.className = "out";
   				}
   			}
   		}
   	</script>
   
       
   
   </head>
   <body>
   
   <table>
       <caption>学生信息表</caption>
       <tr>
           <th><input type="checkbox" name="cb" id="firstCb"></th>
           <th>编号</th>
           <th>姓名</th>
           <th>性别</th>
           <th>操作</th>
       </tr>
   
       <tr>
           <td><input type="checkbox" name="cb"></td>
           <td>1</td>
           <td>令狐冲</td>
           <td>男</td>
           <td><a href="javascript:void(0);">删除</a></td>
       </tr>
   
       <tr>
           <td><input type="checkbox" name="cb"></td>
           <td>2</td>
           <td>任我行</td>
           <td>男</td>
           <td><a href="javascript:void(0);">删除</a></td>
       </tr>
   
       <tr>
           <td><input type="checkbox" name="cb"></td>
           <td>3</td>
           <td>岳不群</td>
           <td>?</td>
           <td><a href="javascript:void(0);">删除</a></td>
       </tr>
   
   </table>
   <div>
       <input type="button" id="selectAll" value="全选">
       <input type="button" id="unSelectAll" value="全不选">
       <input type="button" id="selectRev" value="反选">
   </div>
   </body>
   </html>
   ```

2. 案例6表单验证

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>注册页面</title>
   <style>
       *{
           margin: 0px;
           padding: 0px;
           box-sizing: border-box;
       }
       body{
           background: url(../img/register_bg.png) no-repeat center;
           padding-top: 25px;
       }
   
       .rg_layout{
           width: 900px;
           height: 500px;
           border: 8px solid #EEEEEE;
           background-color: white;
           /*让div水平居中*/
           margin: auto;
       }
   
       .rg_left{
           /*border: 1px solid red;*/
           float: left;
           margin: 15px;
       }
       .rg_left > p:first-child{
           color:#FFD026;
           font-size: 20px;
       }
   
       .rg_left > p:last-child{
           color:#A6A6A6;
           font-size: 20px;
   
       }
   
   
       .rg_center{
           float: left;
          /* border: 1px solid red;*/
   
       }
   
       .rg_right{
           /*border: 1px solid red;*/
           float: right;
           margin: 15px;
       }
   
       .rg_right > p:first-child{
           font-size: 15px;
   
       }
       .rg_right p a {
           color:pink;
       }
   
       .td_left{
           width: 100px;
           text-align: right;
           height: 45px;
       }
       .td_right{
           padding-left: 50px ;
       }
   
       #username,#password,#email,#name,#tel,#birthday,#checkcode{
           width: 251px;
           height: 32px;
           border: 1px solid #A6A6A6 ;
           /*设置边框圆角*/
           border-radius: 5px;
           padding-left: 10px;
       }
       #checkcode{
           width: 110px;
       }
   
       #img_check{
           height: 32px;
           vertical-align: middle;
       }
   
       #btn_sub{
           width: 150px;
           height: 40px;
           background-color: #FFD026;
           border: 1px solid #FFD026 ;
       }
   	.error{
   		color: #FF0000;
   	}
   
   </style>
   <script type="text/javascript">
   	/**
   	 * 	分析:
   	 * 		1.给表单绑定onsubmit事件，监听器中判断每一个方法校验的结果
   	 * 			如果都为true，则监听器方法返回true
   	 * 			如果有一个为false，则监听器方法返回false
   	 * 		2.定义一些方法，分别校验各个表单项。
   	 * 		3.给各个表单项绑定onblur事件
   	 */
   	window.onload = function() {
   		//1.给表单绑定一个onsubmit的事件
   		document.getElementById('form').onsubmit = function() {
   			//调用用户名校验方法 checkUsername();
   			//调用密码校验方法   checkPassword();
   			
   			return checkUserame() && checkPassword();
   		}
   		
   		//给用户名和密码框绑定离开焦点事件
   		document.getElementById("username").onblur = checkUserame;
   		document.getElementById("password").onblur = checkPassword;
   	}
   	//校验用户名的方法
   	function checkUserame() {
   		//1. 获取用户名的值
   		var username =  document.getElementById('username').value;
   		//2. 定义正则表达式
   		var reg_username = /^\w{6,12}$/;
   		//3.判断表达式是否符合正则的规则
   		var flag =  reg_username.test(username);
   		//4.提示信息
   		var s_username =  document.getElementById('s_username');
   		if(flag) {
   			//提示绿色的对勾
   			s_username.innerHTML = "<img width='35px' height='25px' src='../img/gou.png'>";
   		} else {
   			//提示红色的用户名有误
   			s_username.innerHTML = "用户名格式有误";
   		}
   	}
   	//校验密码
   	function checkPassword() {
   		//1. 获取用户名的值
   		var password =  document.getElementById('password').value;
   		//2. 定义正则表达式
   		var reg_password = /^\w{6,12}$/;
   		//3.判断表达式是否符合正则的规则
   		var flag =  reg_password.test(password);
   		//4.提示信息
   		var s_password = document.getElementById('s_password');
   		if(flag) {
   			//提示绿色的对勾
   			s_password.innerHTML = "<img width='35px' height='25px' src='../img/gou.png'>";
   		} else {
   			//提示红色的用户名有误
   			s_password.innerHTML = "密码格式有误";
   		}
   	}
   </script>
   
   </head>
   <body>
   
   <div class="rg_layout">
       <div class="rg_left">
           <p>新用户注册</p>
           <p>USER REGISTER</p>
   
       </div>
   
       <div class="rg_center">
           <div class="rg_form">
               <!--定义表单 form-->
               <form action="#" method="get" id="form">
                   <table>
                       <tr>
                           <td class="td_left"><label for="username">用户名</label></td>
                           <td class="td_right"><input type="text" name="username" id="username" placeholder="请输入用户名">
   						<span id="s_username" class="error"></span>
   						</td>
                       </tr>
   
                       <tr>
                           <td class="td_left"><label for="password">密码</label></td>
                           <td class="td_right"><input type="password" name="password" id="password" placeholder="请输入密码">
                           <span id="s_password" class="error"></span>
   						</td>
                       </tr>
   
                       <tr>
                           <td class="td_left"><label for="email">Email</label></td>
                           <td class="td_right"><input type="email" name="email" id="email" placeholder="请输入邮箱"></td>
                       </tr>
   
                       <tr>
                           <td class="td_left"><label for="name">姓名</label></td>
                           <td class="td_right"><input type="text" name="name" id="name" placeholder="请输入姓名"></td>
                       </tr>
   
                       <tr>
                           <td class="td_left"><label for="tel">手机号</label></td>
                           <td class="td_right"><input type="text" name="tel" id="tel" placeholder="请输入手机号"></td>
                       </tr>
   
                       <tr>
                           <td class="td_left"><label>性别</label></td>
                           <td class="td_right">
                               <input type="radio" name="gender" value="male"> 男
                               <input type="radio" name="gender" value="female"> 女
                           </td>
                       </tr>
   
                       <tr>
                           <td class="td_left"><label for="birthday">出生日期</label></td>
                           <td class="td_right"><input type="date" name="birthday" id="birthday" placeholder="请输入出生日期"></td>
                       </tr>
   
                       <tr>
                           <td class="td_left"><label for="checkcode" >验证码</label></td>
                           <td class="td_right"><input type="text" name="checkcode" id="checkcode" placeholder="请输入验证码">
                               <img id="img_check" src="../img/verify_code.jpg">
                           </td>
                       </tr>
   
   
                       <tr>
                           <td colspan="2" align="center"><input type="submit" id="btn_sub" value="注册"></td>
                       </tr>
                   </table>
   
               </form>
   
   
           </div>
   
       </div>
   
       <div class="rg_right">
           <p>已有账号?<a href="#">立即登录</a></p>
       </div>
   
   
   </div>
   
   
   </body>
   </html>
   ```

   