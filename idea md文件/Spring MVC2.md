# 第一章 响应数据和结果视图

1. 返回值分类 

   1. 字符串

      * UserController.java

        ```java
            @RequestMapping("/testString")
            public String testString(Model model) {
                System.out.println("testString方法执行了");
                //模拟从数据库中查询出User对象
                User user = new User();
                user.setUsername("张三");
                user.setPassword("123456");
                user.setAge(16);
                model.addAttribute("user",user);
                return "success";
            }
        ```

      * success.jsp

        ```jsp
            <h3>执行成功</h3>
        
            ${user.username}
            ${user.password}
            ${user.age}
        ```

   2. void

      * UserController.java

        ```java
            /**
             * 返回值类型为void
             */
            @RequestMapping("/testVoid")
            public void testVoid(HttpServletRequest request, HttpServletResponse response) throws Exception {
                System.out.println("testVoid方法执行了");
                //方式1：编写请求转发的程序
        //        request.getRequestDispatcher("/WEB-INF/pages/success.jsp").forward(request,response);
                //方式2：重定向
        //        response.sendRedirect(request.getContextPath() + "/index.jsp");
                //方式3：直接进行响应
                //首先要解决中文乱码问题
                response.setCharacterEncoding("UTF-8");
                response.setContentType("text/html;charset=UTF-8");
                //直接会进行响应
                response.getWriter().print("大傻逼");
                return;
            }
        ```

   3. ModelAndView

      * UserController.java

        ```java
            @RequestMapping("/testModelAndView")
            public ModelAndView testModelAndView() {
                ModelAndView mv = new ModelAndView();
                System.out.println("ModelAndView方法执行了");
                //模拟从数据库中获取对象
                User user = new User();
                user.setUsername("吴奇刚");
                user.setPassword("123456");
                user.setAge(56);
        
                //把user对象存储到nv对象中，也会把user对象存入到request对象
                mv.addObject("user",user);
                //跳转到哪个页面
                mv.setViewName("success");
        
                return mv;
            }
        ```

2. 转发和重定向

   1. UserController.java

      ```java
          @RequestMapping("/testForwardOrRedirct")
          public String testForwardOrRedirect() {
              System.out.println("testForwardOrRedirect方法执行了");
      
              //请求的转发
      //        return "forward:/WEB-INF/pages/success.jsp";
              //重定向
              return "redirect:/index.jsp";
          }
      ```

3. ResrponseBody响应json数据

   1. response.jsp

      ```jsp
      <%--
        Created by IntelliJ IDEA.
        User: 28943
        Date: 2021/3/18
        Time: 17:12
        To change this template use File | Settings | File Templates.
      --%>
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      
          <script src="/js/jquery.min.js"></script>
      
          <script>
              //页面加载，绑定单击事件
              $(function () {
                  $("#btn").click(function () {
                      alert("hello btn");
                  });
              });
          </script>
      </head>
      <body>
          <a href="/user/testString">testString</a><br>
      
          <a href="/user/testVoid">testVoid</a><br>
      
          <a href="/user/testModelAndView">testModelAndView</a><br>
      
          <a href="/user/testForwardOrRedirct">testForwardOrRedirct</a><br>
      
          <button id="btn">发送ajax的请求</button>
      </body>
      </html>
      ```

   2. springmvc.xml

      ```xml
          <!--开启控制器，哪些静态资源不拦截-->
          <mvc:resources mapping="/js/" location="/js/**"/>
      ```

## 第二章 SpringMVC实现文件上传

1. 文件上传的回顾

   1. 文件上传的必要前提

      ```
      A form 表单的enctype取值必须是：multipart/form-data
      					默认值是：application/x-www-form-urlencoded
      				enctype：是表单请求正文的类型
      B method 属性取值必须是Post
      C 提供一个文件选择域<input type="file" />
      ```

   2. 文件上传的原理分析

      ```
      当form表单的enctype取值不是默认值后，request.getParameter()将生效。
      enctype="appication/x-www-form-urlencode"时，form表单的正文内容是：
      		key=value&key=value&key=value
      当form表单的enctype取值为Mutilpart/form-data时，请求正文的内容就变成：
      每一部分都是MIME类型描述的正文
      -----------------------------------7dala433602ac            分界符
      Content-Disposition: form-data;name="userName"              协议头
      aaa                                                         协议的正文
      -----------------------------------7dala433602ac
      Content-Disposition: form-data;name="file";filename="C:\Users\zhy\Desktop\fileupload_demofile\b.txt"
      Content-Type: text/plain                                    协议的类型(MIME类型)
      
      bbbbbbbbb
      -----------------------------------7dala433602ac
      ```

2. springmvc传统方式的文件上传

   1. 说明

      ```
      传统方式的文件上传，指的是我们上传的文件和访问的应用存在同一台服务器上。
      并且上传完成之后，浏览器可能跳转。
      ```

   2. 实现步骤

      1. 第一步：拷贝文件上传的jar包到工程的lib目录

      2. 第二步：编写jsp页面

         ```jsp
             <form action="/user/fileupload1" method="post" enctype="multipart/form-data">
                 选择文件<input type="file" name="upload" /><br>
                 <input type="submit" value="上传">
             </form>
         ```

      3. 第三步：编写控制器

         ```java
         package cn.itcast.controller;
         
         import org.apache.commons.fileupload.FileItem;
         import org.apache.commons.fileupload.disk.DiskFileItemFactory;
         import org.apache.commons.fileupload.servlet.ServletFileUpload;
         import org.springframework.stereotype.Controller;
         import org.springframework.web.bind.annotation.RequestMapping;
         
         import javax.servlet.http.HttpServletRequest;
         import java.io.File;
         import java.util.List;
         import java.util.UUID;
         
         /**
          *
          */
         @Controller
         @RequestMapping("/user")
         public class UserController {
             /**
              * 文件上传
              * @return
              */
             @RequestMapping("/fileupload1")
             public String fileupload1(HttpServletRequest request) throws Exception{
                 System.out.println("文件上传");
         
                 //使用fileupload组件完成文件上传
                 //上传的位置
                 String path = request.getSession().getServletContext().getRealPath("/uploads/");
                 //判断该路径是否存在
                 File file = new File(path);
                 if (!file.exists()) {
                     //创建该文件夹
                     file.mkdirs();
                 }
         
                 //解析request对象，获取上传文件项
                 DiskFileItemFactory factory = new DiskFileItemFactory();
                 ServletFileUpload upload = new ServletFileUpload(factory);
                 //解析request
                 List<FileItem> items = upload.parseRequest(request);
         
                 //遍历
                 for (FileItem item : items) {
                     //进行判断，判断当前的item是否为上传文件项
                     if(item.isFormField()) {
                         //普通表单项
                     } else {
                         //上传文件项
                         //获取上传文件的名称
                         String fileName = item.getName();
                         //把文件的名称设置唯一值，uuid
                         String uuid = UUID.randomUUID().toString().replace("-", "");
                         fileName = uuid + "_" + fileName;
                         //完成文件上传
                         item.write(new File(path,fileName));
                         //删除临时文件
                         item.delete();
                     }
                 }
                 return "success";
             }
         }
         ```

      4. 第四步：springmvc上传文件

         1. 配置文件解析器

            ```xml
                <!--配置文件解析器对象-->
                <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
                    <property name="maxUploadSize" value="10485760"></property>
                </bean>
            <!-- 注意：
            		文件上传的解析器id是固定的，不能起别的名称，否则无法实现请求参数的绑定。（不光是文件，其他字段也将无法绑定）
             -->
            ```

         2. 控制器

            ```java
            /**
                 * springmvc文件上传
                 * @return
                 */
                @RequestMapping("/fileupload2")
                public String fileupload2(HttpServletRequest request, MultipartFile upload) throws Exception{
                    System.out.println("springmvc文件上传");
            
                    //使用fileupload组件完成文件上传
                    //上传的位置
                    String path = request.getSession().getServletContext().getRealPath("/uploads/");
                    //判断该路径是否存在
                    File file = new File(path);
                    if (!file.exists()) {
                        //创建该文件夹
                        file.mkdirs();
                    }
            
                    //说明上传文件项
                    //获取上传文件的名称
                    String fileName = upload.getOriginalFilename();
                    //把文件的名称设置唯一值，uuid
                    String uuid = UUID.randomUUID().toString().replace("-", "");
                    fileName = uuid + "_" + fileName;
                    //完成文件上传
                    upload.transferTo(new File(path,fileName));
            
                    return "success";
                }
            ```

3. springmvc跨服务器方式的文件上传

   1. 分服务器的目的

      ```
      在实际开发中，我们会有很多处理不同功能的服务器。例如：
      	应用服务器：负责部署我们的应用
      	数据库服务器：运行我们的数据库
      	缓存和消息服务器：负责处理大并发访问的缓存和消息
      	文件服务器：负责存储用户上传文件的服务器
      (注意：此处说的不是服务器集群)
      分服务器处理的目的是让服务器各司其职，从而提高我们项目的运行效率。
      ```

   2. 准备两个tomcat服务器，并创建一个用于存放图片的web工程

   3. 拷贝jar包

   4. 编写控制器实现上传文件

      ```java
          /**
           * springmvc跨服务器文件上传
           * @return
           */
          @RequestMapping("/fileupload3")
          public String fileupload3(MultipartFile upload) throws Exception{
              System.out.println("springmvc跨服务器文件上传");
              //1.定义上传服务器的路径
              String path = "http://localhost:9090/uploads/";
      
              //说明上传文件项
              //获取上传文件的名称
              String fileName = upload.getOriginalFilename();
              //把文件的名称设置唯一值，uuid
              String uuid = UUID.randomUUID().toString().replace("-", "");
              fileName = uuid + "_" + fileName;
              //完成文件上传，跨服务器上传
              //1.创建客户端对象
              Client client = Client.create();
              //2.和图片服务器进行连接
              WebResource webResource = client.resource(path + fileName);
              //3.上传文件
              webResource.put(upload.getBytes());
              return "success";
          }
      ```

   5. 编写jsp页面

      ```jsp
          <h3>springmvc跨服务器文件上传</h3>
      
          <form action="/user/fileupload3" method="post" enctype="multipart/form-data">
              选择文件<input type="file" name="upload" /><br>
              <input type="submit" value="上传">
          </form>
      ```

   6. 编辑配置器

      ```xml
          <!--配置文件解析器对象-->
          <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
              <property name="maxUploadSize" value="10485760"></property>
          </bean>
      ```

# 第三章SpringMVC中的异常处理

1. 异常处理思路

   ```
   	系统中的异常包括两类：预期异常和运行时异常RuntimeException，前者通过捕获异常从而获取异常信息，后者主要是通过规范代码开发，测试通过手段减少运行时异常的发生。
   	系统的dao、service、controller出现都通过throws Exception向上抛出，最后由springmvc前端控制器交由异常处理器进行异常处理，如下图：	
   ```

   ![1616127330216](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616127330216.png)

2. 实现步骤

   1. 编写异常类和错误页面

      * 自定义异常类

        ```java
        package cn.itcast.exception;
        
        /**
         *  自定义的异常类
         */
        public class SysException extends Exception{
        
            //存储提示信息的
            private String message;
        
            @Override
            public String getMessage() {
                return message;
            }
        
            public void setMessage(String message) {
                this.message = message;
            }
        
            public SysException(String message) {
                this.message = message;
            }
        }
        ```

      * 错误页面

        ```jsp
        <%--
          Created by IntelliJ IDEA.
          User: 28943
          Date: 2021/3/19
          Time: 16:29
          To change this template use File | Settings | File Templates.
        --%>
        <%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
        <html>
        <head>
            <title>Title</title>
        </head>
        <body>
            ${errorMsg}
        </body>
        </html>
        ```

   2. 自定义异常处理器

      ```java
      package cn.itcast.exception;
      
      import org.springframework.web.servlet.HandlerExceptionResolver;
      import org.springframework.web.servlet.ModelAndView;
      
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      
      /**
       *  异常处理器对象
       */
      public class SysExceptionResolver implements HandlerExceptionResolver {
          /**
           * 处理异常业务逻辑
           * @param httpServletRequest
           * @param httpServletResponse
           * @param
           * @param
           * @return
           */
          @Override
          public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object handler, Exception ex) {
              //获取到异常对象
              SysException e = null;
              //如果抛出的是系统自定义异常，则直接转换
              if(ex instanceof SysException) {
                  e = (SysException) ex;
              }else{
                  //如果抛出的不是系统自定义异常则重新构造一个系统错误异常
                  e = new SysException("系统正在维护");
              }
              //创建ModelAndView对象
              ModelAndView mv = new ModelAndView();
              mv.addObject("errorMsg",e.getMessage());
              mv.setViewName("error");
              return mv;
          }
      }
      ```

   3. 配置异常处理器

      ```xml
          <!--配置异常处理器对象-->
          <bean id="sysExceptionResolver" class="cn.itcast.exception.SysExceptionResolver"></bean>
      
      ```

# 第四章 SpringMVC中的拦截器

1. 拦截器的作用

   ```
   	Spring MVC的处理器拦截器类似于Servlet开发中的过滤器Filter，用于对处理器进行预处理和后处理。
   用户可以自定义一些拦截器开始先特定的功能。
   	谈到拦截器，还要向大家提一个词——拦截器链（Interceptor Chain）。拦截器链就是将拦截器按一定的顺序 联结成一条链。在访问被拦截的方法或字段时，拦截器链中的拦截器就会按其之前定义的顺序被调用。
   	说到这里，可能大家脑海中有了一个疑问，这不是我们之前学的过滤器吗？是的，它和过滤器是有几分相似，但是也有区别，接下来我们就来说说它们的区别：
   		过滤器是servlet规范中的一部分，任何java web工程都可以使用。
   		拦截器是SpringMVC框架自己的，只有使用了SpringMVC的工程才可以使用。
   		过滤器在url-pattern中配置了/*之后，可以对所有要访问的资源进行拦截。
   		拦截器它是只会拦截访问控制器的方法，如果访问的是jsp，html，css，imge或者js是不会进行拦截的。
   	它也是AOP思想的具体应用。
   	我们要想自定义拦截器，要求必须实现：HandlerInterceptor接口。
   ```

2. 自定义拦截器的步骤

   1. 第一步：编写一个普通类实现HandlerInterceptor接口

      ```java
      package cn.itcast.interceptor;
      
      import org.springframework.web.servlet.HandlerInterceptor;
      import org.springframework.web.servlet.ModelAndView;
      
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      
      /**
       *  自定义拦截器实现HandlerInterceptor接口
       */
      public class MyInterceptor1 implements HandlerInterceptor {
          /**
           * controller方法执行前执行
           * return true 放行，执行下一个拦截器
           * @param request
           * @param response
           * @param handler
           * @return
           * @throws Exception
           */
          @Override
          public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
              System.out.println("MyInterceptor执行了");
      
              return true;
          }
      
          @Override
          public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
      
          }
      
          @Override
          public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
      
          }
      }
      ```

   2. 第二步：配置拦截器

      ```xml
          <!--配置拦截器-->
          <mvc:interceptors>
              <!--配置拦截器-->
              <mvc:interceptor>
                  <!--你要拦截的具体方法-->
                  <mvc:mapping path="/user/**"/>
                  <!--不要拦截的方法-->
                  <!--<mvc:exclude-mapping path=""/>-->
                  <!--配置拦截器对象-->
                  <bean class="cn.itcast.interceptor.MyInterceptor1"></bean>
              </mvc:interceptor>
          </mvc:interceptors>
      ```

      

