# 今日内容

1. JSP：
   1. 指令
   2. 注释
   3. 内置对象
2. MVC开发模式
3. EL表达式
4. JSTL标签
5. 三层架构

## JSP：

1. 指令

   - 作用：用于配置JSP页面，导入资源文件

   - 格式：

     ```jsp
     <%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>
     ```

   - 分类：

     1. ```jsp
        page           配置JSP页面的
        	- contentType：等同于response.setcontentType()
        		1. 设置响应体的mime类型以及字符集
        		2. 设置当前jsp页面的编码（只能是高级的开发工具才能生效，如果使用低级工具，则需要设置pageEncoding属性设置当前页面的字符集）
        	- import：导包
        	- errorPage：错误页面，当前页面发生异常后，会自动跳转到指定的错误页面
        	- iserrorPage：表示当前页面是否是错误页面
        		- true：是。可以使用内置对象exception
        		- false：否。默认值，不可以使用内置对象exception
        ```

     2. ```jsp
        include        页面包含的。导入页面的资源文件
        	- <%@include file="top.jsp"%>
        ```

     3. ```jsp
        taglib         导入资源
        	- <%@ tagilb prefix="c" url="http://java.sun.com/jsp/jstl/core" %>
        ```

2. 注释

   1. html注释：

      ```html
      <!-- -->：只能注释html代码片段
      ```

   2. jsp注释：推荐使用

      ```jsp
      <%-- --%>：可以注释所有
      ```

3. 内置对象

   - 在jsp页面中直接创建，直接使用的对象

   - 一共有9个：

     |   变量名    |      真实类型       |                     作用                     |
     | :---------: | :-----------------: | :------------------------------------------: |
     | pageContext |     PageContext     | 当前页面共享数据，还可以获取其他八个内置对象 |
     |   request   | HttpServletRequest  |        一次请求访问的多个资源（转发）        |
     |   session   |     HttpSession     |             一次会话的多个请求间             |
     | application |   ServletContext    |              所有用户间共享数据              |
     |  response   | HttpServletResponse |                   响应对象                   |
     |    page     |       Object        |          当前页面（Servlet）的对象           |
     |     out     |      JspWriter      |          输出对象，数据输出到页面上          |
     |   config    |    ServletConfig    |              Servlet的配置对象               |
     |  exception  |      Throwable      |                   异常对象                   |

## MVC：开发模式

1. jsp演变历史

   1. 早期只有servlet，只能使用response输出标签，并非常麻烦
   2. 后来有了jsp，简化了Servlet的开发，如果过度使用jsp，在jsp中既写大量的java代码，又写html标签，造成难于维护，难于分工协作。
   3. 再后来，Java的web开发，借鉴MVC开发模式，使得程序的设计更加的合理性

2. MVC：

   1. M：Model，模型。JavaBean
      - 完成具体的业务操作，如：查询数据库，封装对象
   2. V：View，视图。JSP
      - 展示数据
   3. C：Controller，控制器。Servlet
      - 获取用户的输入
      - 调用模型
      - 将数据交给试图进行展示

   * 优缺点：
     1. 优点：
        1. 耦合性低，方便维护，可以利于分工协作
        2. 重用性高
     2. 缺点：
        1. 使得项目架构变得复杂，对开发人员要求高

## EL表达式

1. 概念：Expression Language 表达式语言

2. 作用：替换和简化jsp页面中java代码的编写

3. 语法：${表达式}

4. 注意：

   - jsp默认支持EL表达式。如果要忽略el表达式

     1. ```jsp
        设置jsp中page指令中：isELIgnored="true" 忽略当前jsp页面中所有的EL表达式
        ```

     2. ```jsp
        \${表达式}：忽略当前这个el表达式
        ```

5. 使用：

   1. 运算：
      - 运算符：
        1. 算数运算符：+ - * /(div) %(mod)
        2. 比较运算符：> < >= <= == !=
        3. 逻辑运算符：&&(and)   ||(or)    !(not)
        4. 空运算符：empty
           - 功能：用于判断字符串、集合、数组对象是否为null并且长度是否为0
           - ${empty list}：表示判断字符串、集合、数组对象是否为null或者长度为0
           - ${not empty str}：表示判断字符串、集合、数组对象是否不为null 并且长度>0
   2. 获取值
      1. el表达式只能从域对象中获取值
      
      2. 语法：
         1. ${域名称.键名}：从指定域中获取指定键的值
            - 域名称：
              1. pageScope	             --> pageContext
              2. requestScope            --> request
              3. sessionScope            --> session
              4. applicationScope      --> application(ServletContext)
            - 举例：在request域中存储了name=张三
            - 获取：${requestScope.name}
            
         2. ${键名}：表示依次从最小的域中查找是否有该键对应的值。直到找到为止。
         
            ```jsp
            <%--
              Created by IntelliJ IDEA.
              User: 28943
              Date: 2021/2/20
              Time: 13:00
              To change this template use File | Settings | File Templates.
            --%>
            <%@ page contentType="text/html;charset=UTF-8" language="java" %>
            <html>
            <head>
                <title>获取域中的数据</title>
            </head>
            <body>
                <%
                    //在域中存储数据
                    request.setAttribute("name","zhangsan");
                    session.setAttribute("age","23");
                    session.setAttribute("name","李四");
                %>
            <h3>el获取值</h3>
                ${requestScope.name}
                ${sessionScope.age}
            
                ${name}
            </body>
            </html>
            
            ```
         
         3. 获取对象、List集合、Map集合的值
         
            1. 对象：${域名称.键名.属性名}
         
               - 本质上会去调用对象的getter方法
         
               - 代码演示
         
               - User类
         
                 ```java
                 package cn.itcast.domain;
                 
                 import java.text.SimpleDateFormat;
                 import java.util.Date;
                 
                 public class User {
                     private String name;
                     private int age;
                     private Date birthday;
                 
                     public String getBirStr() {
                         if(birthday != null) {
                             //1.格式化日期对象
                             SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
                             //2.返回字符串即可
                             return sdf.format(birthday);
                         }
                         else {
                             return "";
                         }
                     }
                 
                     public String getName() {
                         return name;
                     }
                 
                     public void setName(String name) {
                         this.name = name;
                     }
                 
                     public int getAge() {
                         return age;
                     }
                 
                     public void setAge(int age) {
                         this.age = age;
                     }
                 
                     public Date getBirthday() {
                         return birthday;
                     }
                 
                     public void setBirthday(Date birthday) {
                         this.birthday = birthday;
                     }
                 }
                 ```
         
               - el3.jsp
         
                 ```jsp
                 <%@ page import="cn.itcast.domain.User" %>
                 <%@ page import="java.util.Date" %><%--
                   Created by IntelliJ IDEA.
                   User: 28943
                   Date: 2021/2/20
                   Time: 13:14
                   To change this template use File | Settings | File Templates.
                 --%>
                 <%@ page contentType="text/html;charset=UTF-8" language="java" %>
                 <html>
                 <head>
                     <title>el获取数据</title>
                 </head>
                 <body>
                     <%
                         User user = new User();
                         user.setName("张三");
                         user.setAge(23);
                         user.setBirthday(new Date());
                 
                 
                         request.setAttribute("user",user);
                     %>
                 <h3>获取对象中的值</h3>
                 ${requestScope.user}<br>
                 <%--
                     通过对象的属性来获取
                         - setter或getter方法，去掉set或get，在剩余部分，首字母变为小写
                         - setName --> Name --> name
                 --%>
                 
                 ${requestScope.user.name}<br>
                 ${requestScope.user.age}<br>
                 ${requestScope.user.birthday}<br>
                 ${requestScope.user.birthday.year}<br>
                 ${requestScope.user.birStr}<br>
                 </body>
                 </html>
                 ```
         
            2. List集合：${域名称.键名[索引]}
         
               ```jsp
               <%@ page import="cn.itcast.domain.User" %>
               <%@ page import="java.util.Date" %>
               <%@ page import="java.util.List" %>
               <%@ page import="java.util.ArrayList" %><%--
                 Created by IntelliJ IDEA.
                 User: 28943
                 Date: 2021/2/20
                 Time: 13:14
                 To change this template use File | Settings | File Templates.
               --%>
               <%@ page contentType="text/html;charset=UTF-8" language="java" %>
               <html>
               <head>
                   <title>el获取数据</title>
               </head>
               <body>
                   <%
                       User user = new User();
                       user.setName("张三");
                       user.setAge(23);
                       user.setBirthday(new Date());
               
               
                       request.setAttribute("user",user);
               
                       List list = new ArrayList();
                       list.add("aaa");
                       list.add("bbb");
                       list.add(user);
               
                       request.setAttribute("list",list);
                   %>
               <h3>获取对象中的值</h3>
               ${requestScope.user}<br>
               <%--
                   通过对象的属性来获取
                       - setter或getter方法，去掉set或get，在剩余部分，首字母变为小写
                       - setName --> Name --> name
               --%>
               
               ${requestScope.user.name}<br>
               ${requestScope.user.age}<br>
               ${requestScope.user.birthday}<br>
               ${requestScope.user.birthday.year}<br>
               ${requestScope.user.birStr}<br>
               
               <h3>获取list的值</h3>
               ${list}<br>
               ${list[0]}<br>
               ${list[1]}<br>
               ${list[10]}<br>
               ${list[2].name}
               </body>
               </html>
               ```
         
            3. Map集合：
         
               - ${域名称.键名.key名称}
               - ${域名称.键名["key名称"]}
         
      3. 隐式对象：
      
         * el表达式中有11个隐式对象
      
         * pageContext：
      
           - 获取jsp其他八个内置对象
      
             1. ```jsp
                <%--
                  Created by IntelliJ IDEA.
                  User: 28943
                  Date: 2021/2/20
                  Time: 14:00
                  To change this template use File | Settings | File Templates.
                --%>
                <%@ page contentType="text/html;charset=UTF-8" language="java" %>
                <html>
                <head>
                    <title>隐式对象</title>
                </head>
                <body>
                    ${pageContext.request}<br>
                    <h4>动态获取虚拟目录</h4>
                    ${pageContext.request.contextPath}<br>
                
                <%
                    
                %>
                </body>
                </html>
                ```

## JSTL

1. 概念：JavaServlet Pages Tag Library   JSP标准标签库
   - 是由Apache组织提供的开源的免费的jsp标签         <标签>
2. 作用：用于简化和替换jsp页面上的java代码
3. 使用步骤：
   1. 导入jstl相关jar包
   2. 引入标签库：taglib指令：<%@ taglib %>
   3. 使用标签
4. 常用的JSTL标签
   1. if           ：相当于java代码的if语句
   2. choose ：相当于java代码的switch语句
   3. foreach：相当于java代码的for语句

