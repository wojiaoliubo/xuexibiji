# 第一章：Spring MVC的基本概念

1. 关于三层架构和MVC

   1. 三层架构

      > ​	我们的开发架构一般都是基于两种形式，一种是C/S架构，也就是客户端/服务器，另一种是B/S架构，也就是浏览器/服务器。在JavaEE开发中，几乎全都是基于B/S架构的开发。那么在B/S架构中，系统标准的三层架构包括：表现层、业务层、持久层。三层架构在我们的实际开发中使用的非常多，所以我们课程中的案例也都是基于三层架构设计的。
      >
      > ​	三层架构中，每一层各司其职，接下类我们就说说每层都负责哪些方面：
      >
      > ​	**表现层**：
      >
      > ​			也就是我们常说的web层，它负责接受客户端请求，向客户端响应结果，通常客户端使用http协议请求web层，web需要接受http请求，完成http响应。
      >
      > ​			表现层包括展示层和控制层：控制层负责接受请求，展示层负责结果的展示。
      >
      > ​			表现层依赖业务层，接收到客户端请求一般会调用业务层进行业务处理，并将处理对象响应给客户端。
      >
      > ​			表现层的设计一般都是用MVC模型。（MVC是表现层的设计模型，和其他层没有关系）
      >
      > ​	**业务层**：
      >
      > ​			也就是我们常说的service层。它负责业务逻辑处理，和我们开发项目的需求息息相关。web层依赖业务层，但是业务层不依赖web层。
      >
      > ​	**持久层**：
      >
      > ​			也就是我们常说的dao层。负责数据持久化，包括数据层即数据库和数据访问层，数据库就是对数据进行持久化的载体，数据访问层是业务层和持久层交互的接口，业务层需要通过数据访问层将数据持久化到数据库中。通俗的讲，持久层就是和数据库交互，对数据库表进行增删改查的。

   2. MVC模型

      > ​	MVC 全名是 Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，
      > 是一种用于设计创建 Web 应用程序表现层的模式。MVC 中每个部分各司其职：
      >
      > ​	Model（模型）：
      >
      > ​		通常指的就是我们的数据模型。作用一般情况下用于封装数据。
      >
      > ​	View（视图）：
      >
      > ​		通常指的就是我们的 jsp 或者 html。作用一般就是展示数据的。
      >
      > ​		通常视图是依据模型数据创建的。
      >
      > ​	Controller（控制器）：
      >
      > ​		是应用程序中处理用户交互的部分。作用一般就是处理程序逻辑的。 
      >
      > ​		它相对于前两个不是很好理解，这里举个例子： 
      >
      > ​		例如：
      >
      > ​			我们要保存一个用户的信息，该用户信息中包含了姓名，性别，年龄等等。
      >
      > ​			这时候表单输入要求年龄必须是1~100之间的整数。姓名和性别不能为空。并且把数据填充进模型之中。
      >
      > ​			此时除了js的校验之外，服务器端也应该有数据准确性的校验，那么校验就是控制器该做的。
      >
      > ​			当校验失败后，由控制器负责把错误页面展示给使用者。
      >
      > ​			如果校验和成功，也就是控制器负责把数据填充到模型，并且调用业务层实现完整的业务需求。

2. SpringMVC概述

   1. SpringMVC是什么

      > ​	SpringMVC 是一种基于 Java 的实现 MVC 设计模型的请求驱动类型的轻量级 Web 框架，属于SpringFrameWork的的后续产品，已经融合在 Spring Web Flow 里面。Spring 框架提供了构建 Web 应用程序的全功能 MVC 模块。使用 Spring 可插入的 MVC 架构，从而在使用 Spring 进行 WEB 开发时，可以选择使用 Spring的 Spring MVC 框架或集成其他 MVC 开发框架，如 Struts1(现在一般不用)，Struts2 等。
      >
      > ​	SpringMVC 已经成为目前最主流的 MVC 框架之一，并且随着 Spring3.0 的发布，全面超越 Struts2，成为最优秀的MVC框架。
      >
      > ​	它通过一套注解，让一个简单的 Java 类成为处理请求的控制器，而无须实现任何接口。同时它还支持RESTful编程风格的请求。

   2. SpringMVC在三层架构的位置

      ![1615978739137](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1615978739137.png)

   3. SpringMVC的优势

      ```
      1.清晰的角色划分：
      	前端控制器(DispatcherServlet)
      	请求到处理器映射（HandlerMapping）
      	处理器适配器（HandlerAdapter）
      	视图解析器（ViewResolver）
      	处理器或页面控制器（Controller）	
      	验证器（ Validator）
      	命令对象（Command 请求参数绑定到的对象就叫命令对象）
      	表单对象（Form Object 提供给表单展示和提交到的对象就叫表单对象）
      2.分工明确，而且扩展点相当灵活，可以很容易扩展，虽然几乎不需要。
      3.由于命令对象就是一个 POJO，无需继承框架特定 API，可以使用命令对象直接作为业务对象。
      4.和 Spring 其他框架无缝集成，是其它 Web 框架所不具备的。
      5.可适配，通过 HandlerAdapter 可以支持任意的类作为处理器。
      6.可定制性，HandlerMapping、ViewResolver 等能够非常简单的定制。
      7.功能强大的数据验证、格式化、绑定机制。
      8.利用 Spring 提供的 Mock 对象能够非常简单的进行 Web 层单元测试。
      9.本地化、主题的解析的支持，使我们更容易进行国际化和主题的切换。
      10.强大的 JSP 标签库，使 JSP 编写更容易。
      ………………还有比如RESTful风格的支持、简单的文件上传、约定大于配置的契约式编程支持、基于注解的零配置支持等等。
      ```

   4. SpringMVC和Struts2的优分析

      ```
      共同点：
      	它们都是表现层框架，都是基于 MVC 模型编写的。
      	它们的底层都离不开原始 ServletAPI。
      	它们处理请求的机制都是一个核心控制器。
      区别：
      	Spring MVC 的入口是 Servlet, 而 Struts2 是 Filter
      	Spring MVC 是基于方法设计的，而 Struts2 是基于类，Struts2 每次执行都会创建一个动作类。所以Spring MVC会稍微比Struts2快些。
      	Spring MVC 使用更加简洁,同时还支持 JSR303, 处理 ajax 的请求更方便
      	(JSR303 是一套 JavaBean 参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注解加在我们JavaBean的属性上面，就可以在需要校验的时候进行校验了。)
      	Struts2 的 OGNL 表达式使页面的开发效率相比 Spring MVC 更高些，但执行效率并没有比 JSTL 提升，尤其是struts2的表单标签，远没有html执行效率高。
      ```

# 第二章 SpringMVC的入门

1. SpringMVC的入门案例

   1. 前期准备

   2. 导入jar包

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      
      <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
      
        <groupId>cn.itcast</groupId>
        <artifactId>springmvc_day01_01_start</artifactId>
        <version>1.0-SNAPSHOT</version>
        <packaging>war</packaging>
      
        <name>springmvc_day01_01_start Maven Webapp</name>
        <!-- FIXME change it to the project's website -->
        <url>http://www.example.com</url>
      
        <properties>
          <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
          <maven.compiler.source>1.8</maven.compiler.source>
          <maven.compiler.target>1.8</maven.compiler.target>
          <spring.version>5.0.2.RELEASE</spring.version>
        </properties>
      
        <dependencies>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
          </dependency>
      
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
          </dependency>
      
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
          </dependency>
      
          <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
            <scope>provided</scope>
          </dependency>
      
          <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.0</version>
            <scope>provided</scope>
          </dependency>
      
        </dependencies>
      
        <build>
          <finalName>springmvc_day01_01_start</finalName>
          <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
            <plugins>
              <plugin>
                <artifactId>maven-clean-plugin</artifactId>
                <version>3.0.0</version>
              </plugin>
              <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
              <plugin>
                <artifactId>maven-resources-plugin</artifactId>
                <version>3.0.2</version>
              </plugin>
              <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.7.0</version>
              </plugin>
              <plugin>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.20.1</version>
              </plugin>
              <plugin>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.2.0</version>
              </plugin>
              <plugin>
                <artifactId>maven-install-plugin</artifactId>
                <version>2.5.2</version>
              </plugin>
              <plugin>
                <artifactId>maven-deploy-plugin</artifactId>
                <version>2.8.2</version>
              </plugin>
            </plugins>
          </pluginManagement>
        </build>
      </project>
      ```

   3. 配置核心控制器-一个Servlet    web.xml

      ```xml
      <!DOCTYPE web-app PUBLIC
       "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
       "http://java.sun.com/dtd/web-app_2_3.dtd" >
      <!-- SpringMVC的核心控制器 -->
      <web-app>
        <display-name>Archetype Created Web Application</display-name>
        <!-- 前端控制器 -->
        <servlet>
          <servlet-name>dispatcherServlet</servlet-name>
          <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
          <!-- 配置Servlet的初始化参数，读取springmvc的配置文件，创建spring容器 -->
          <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
          </init-param>
          <!-- 配置servlet启动时加载对象 -->
          <load-on-startup>1</load-on-startup>
        </servlet>
        <servlet-mapping>
          <servlet-name>dispatcherServlet</servlet-name>
          <url-pattern>/</url-pattern>
        </servlet-mapping>
        
      </web-app>
      ```

   4. 创建 spring mvc 的配置文件

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:mvc="http://www.springframework.org/schema/mvc"
             xmlns:context="http://www.springframework.org/schema/context"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="
              http://www.springframework.org/schema/beans
              http://www.springframework.org/schema/beans/spring-beans.xsd
              http://www.springframework.org/schema/mvc
              http://www.springframework.org/schema/mvc/spring-mvc.xsd
              http://www.springframework.org/schema/context
              http://www.springframework.org/schema/context/spring-context.xsd">
      
          <!--开启注解扫描-->
          <context:component-scan base-package="cn.itcast"></context:component-scan>
      
          <!-- 视图解析器对象 -->
          <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
              <property name="prefix" value="/WEB-INF/pages/"></property>
              <property name="suffix" value=".jsp"></property>
          </bean>
      
          <!-- 开启SpringMVC框架注释的支持 -->
          <mvc:annotation-driven></mvc:annotation-driven>
      </beans>
      ```

   5. 编写控制器并使用注解配置    HelloController.java

      ```java
      package cn.itcast.controller;
      
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.RequestMethod;
      
      /**
       *  控制器类
       */
      @Controller
      @RequestMapping(path = "/user")
      public class HelloController {
      
          @RequestMapping(path = "/hello")
          public String sayHello() {
              System.out.println("Hello SpringMVC");
              return "success";
          }
          
      }
      ```

# 第三章 请求参数的绑定

1. 绑定的机制

   ```jsp
   <a href="/findAccount?accountId=10">查询账户</a>
   ```

   ```java
       /**
        * 查询用户
        * @param accountId
        * @return
        */
       @RequestMapping(path = "/findAccount")
       public String findAccount(Integer accountId) {
           System.out.println("查询了账户" + accountId);
           return "success";
       }
   ```

2. 支持的数据类型

   ```
   基本类型参数：
   	包括基本类型和String类型
   POJO类型参数：
   	包括实体类，以及关联的实体类
   数组和集合类型参数：
   	包括List结构和Map结构的集合(包括数组)
   
   SpringMVC绑定请求参数是自动实现的，但是要想使用，必须遵循使用要求。
   ```

3. 使用要求

   ```
   如果是基本类型或者String类型：
   	要求我们的参数名称必须和控制器中方法的形参名称保持一致。(严格区分大小写)
   如果是POJO类型，或者他的关联对象：
   	要求表单中参数名称和POJO类的属性名称保持一致。并且控制器方法的参数类型是POJO类型。
   如果是集合类型，有两种方式：
   		第一种：
   			要求集合类型的请求参数必须在 POJO 中。在表单中请求参数名称要和 POJO 中集合属性名称相同。
   			给 List 集合中的元素赋值，使用下标。
   			给 Map 集合中的元素赋值，使用键值对。
   		第二种：
   			接收的请求参数是 json 格式数据。需要借助一个注解实现。
   ```

4. 使用示例

   1. 基本类型和String类型作为参数

      ```jsp
      <a href="/findAccount1?accountId=10&accountName=zhangsan">查询账户</a>
      ```

      ```java
      	/**
           * 查询用户
           * @param accountId
           * @return
           */
          @RequestMapping(path = "/findAccount1")
          public String findAccount(Integer accountId,String accountName) {
              System.out.println("查询了账户" + accountId + accountName);
              return "success";
          }
      ```

   2. POJO类型作为参数

      1. Account.java

         ```java
         package cn.itcast.domain;
         
         import java.io.Serializable;
         
         /**
          *  账户信息
          */
         public class Account implements Serializable {
         
             private Integer id;
             private String name;
             private Float money;
             private Address address;
         
             public Integer getId() {
                 return id;
             }
         
             public void setId(Integer id) {
                 this.id = id;
             }
         
             public String getName() {
                 return name;
             }
         
             public void setName(String name) {
                 this.name = name;
             }
         
             public Float getMoney() {
                 return money;
             }
         
             public void setMoney(Float money) {
                 this.money = money;
             }
         
             public Address getAddress() {
                 return address;
             }
         
             public void setAddress(Address address) {
                 this.address = address;
             }
         
             @Override
             public String toString() {
                 return "Account{" +
                         "id=" + id +
                         ", name='" + name + '\'' +
                         ", money=" + money +
                         ", address=" + address +
                         '}';
             }
         }
         ```

      2. Address.java

         ```java
         package cn.itcast.domain;
         
         import java.io.Serializable;
         
         /**
          *  地址的实体类
          */
         public class Address implements Serializable {
         
             private String provinceName;
             private String cityName;
         
             public String getProvinceName() {
                 return provinceName;
             }
         
             public void setProvinceName(String provinceName) {
                 this.provinceName = provinceName;
             }
         
             public String getCityName() {
                 return cityName;
             }
         
             public void setCityName(String cityName) {
                 this.cityName = cityName;
             }
         
             @Override
             public String toString() {
                 return "Address{" +
                         "provinceName='" + provinceName + '\'' +
                         ", cityName='" + cityName + '\'' +
                         '}';
             }
         }
         ```

      3. index.jsp

         ```jsp
             <form action="/saveAccount" method="post">
                 账户名称：<input type="text" name="name"><br>
                 账户金额：<input type="text" name="money"><br>
                 账户省份：<input type="text" name="address.provinceName"><br>
                 账户城市：<input type="text" name="address.cityName"><br>
                 <input type="submit" value="保存">
             </form>
         ```

      4. paramsTest.java

         ```java
             /**
              * 保存账户
              * @param account
              * @return
              */
             @RequestMapping("/saveAccount")
             public String saveAccount(Account account) {
         
                 System.out.println("保存了账户。。。。" + account);
                 return "success";
             }
         ```

   3. POJO类中包含集合类型参数

      1. User.java

         ```java
         package cn.itcast.domain;
         
         import java.io.Serializable;
         import java.util.List;
         import java.util.Map;
         
         public class User implements Serializable {
         
             private String username;
             private String password;
             private Integer age;
             private List<Account> accounts;
             private Map<String,Account> accountMap;
         
             public String getUsername() {
                 return username;
             }
         
             public void setUsername(String username) {
                 this.username = username;
             }
         
             public String getPassword() {
                 return password;
             }
         
             public void setPassword(String password) {
                 this.password = password;
             }
         
             public Integer getAge() {
                 return age;
             }
         
             public void setAge(Integer age) {
                 this.age = age;
             }
         
             public List<Account> getAccounts() {
                 return accounts;
             }
         
             public void setAccounts(List<Account> accounts) {
                 this.accounts = accounts;
             }
         
             public Map<String, Account> getAccountMap() {
                 return accountMap;
             }
         
             public void setAccountMap(Map<String, Account> accountMap) {
                 this.accountMap = accountMap;
             }
         
             @Override
             public String toString() {
                 return "User{" +
                         "username='" + username + '\'' +
                         ", password='" + password + '\'' +
                         ", age=" + age +
                         ", accounts=" + accounts +
                         ", accountMap=" + accountMap +
                         '}';
             }
         }
         ```

      2. index.jsp

         ```jsp
         <form action="/updateAccount" method="post">
                 用户名称：<input type="text" name="username"><br>
                 用户密码：<input type="password" name="password"><br>
                 用户年龄：<input type="text" name="age"><br>
                 账户1名称：<input type="text" name="account[0].name"><br>
                 账户1金额：<input type="text" name="accpunt[0].money"><br>
                 账户2名称：<input type="text" name="account[1].name"><br>
                 账户2金额：<input type="text" name="accpunt[1].money"><br>
                 账户3名称：<input type="text" name="accountMap['one'].name"><br>
                 账户3金额：<input type="text" name="accountMap['one'].money"><br>
                 账户4名称：<input type="text" name="accountMap['two'].name"><br>
                 账户4金额：<input type="text" name="accountMap['two'].money"><br>
                 <input type="submit" value="保存">
             </form>
         ```

      3. paramsTest.java

         ```java
             /**
              * 更新账户
              * @param user
              * @return
              */
             @RequestMapping("/updateAccount")
             public String updateAccount(User user) {
                 System.out.println("更新了账户。。。。" + user);
                 return "success";
             }
         ```

   4. 自定义类型转换器

      1. 定义一个类，实现Converter接口，该接口有两个泛型。

         1. Converter.java   自带，不需要自己写

            ```java
            public interface Converter<S, T> {
            	/**
            	* 实现类型转换的方法
            	*/
            	@Nullable
            	T convert(S source);
            }
            ```

         2. StringToDateConverter.java

            ```java
            package cn.itcast.test;
            
            import org.springframework.core.convert.converter.Converter;
            import org.springframework.util.StringUtils;
            
            import java.text.DateFormat;
            import java.text.SimpleDateFormat;
            import java.util.Date;
            
            /**
             *  自定义类型转换器
             */
            public class StringToDateConverter implements Converter<String, Date> {
                /**
                 * 用于把String类型转换成日期类型
                 * @param source
                 * @return
                 */
                @Override
                public Date convert(String source) {
                    DateFormat format = null;
                    try {
                        if(StringUtils.isEmpty(source)) {
                            throw new NullPointerException("请输入要转换的日期");
                        }
                        format = new SimpleDateFormat("yyyy-MM-dd");
                        Date date = format.parse(source);
                        return date;
                    } catch (Exception e) {
                        throw new RuntimeException("输入日期有误");
                    }
                }
            }
            
            ```

         3. 在spring配置文件中配置类型转换器

            ```xml
                <!--配置类型转换器工厂-->
                <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
                    <!--给工厂注入一个新的类型转换器-->
                    <property name="converters">
                        <array>
                            <!--配置自定义类型转换器-->
                            <bean class="cn.itcast.test.StringToDateConverter"></bean>
                        </array>
                    </property>
                </bean>
            ```

         4. 在annotation-driven标签中引用配置的类型转换服务

            ```xml
                <!--引用自定义类型转换器-->
                <mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>
            ```

## 第四章 常用注解

1. RequestParam

   1. 使用说明

      ```
      作用：
      	把请求中指定名称的参数给控制器中的形参赋值。
      属性：
      	value：请求参数中的名称。
      	required：请求参数中是否必须提供此参数。默认值：true。表示必须提供，如果不提供将报错。
      ```

   2. 使用示例

      ```jsp
          <%--RequestParam注解的使用--%>
          <a href="/anno/testRequestParam?name=zhangsan">testRequestParam</a><br>
      ```

      ```java
          /**
           * requestParams注解的使用
           * @return
           */
          @RequestMapping("/testRequestParam")
          public String testRequestParam(@RequestParam("name") String username) {
              System.out.println("username:" + username);
              return "success";
          }
      ```

2. RequestBody

   1. 使用说明

      ```
      作用：
      	用于获取请求体内容。直接使用得到是 key=value&key=value...结构的数据。
      	get 请求方式不适用。
      属性：
      	required：是否必须有请求体。默认值是:true。当取值为 true 时,get 请求方式会报错。如果取值为false，get请求得到的是null。
      ```

   2. 使用示例

      ```jsp
          <%--RequestBody注解的使用--%>
          <form action="/anno/testRequestBody" method="post">
              用户名称<input type="text" name="username">
              用户密码<input type="password" name="password">
              用户年龄<input type="text" name="age">
              <input type="submit" value="提交">
          </form>
      ```

      ```java
          /**
           * RequestBody注解的使用
           * @param body
           * @return
           */
          @RequestMapping("/testRequestBody")
          public String testRequestBody(@RequestBody String body) {
              System.out.println(body);
              return "success";
          }
      ```

3. PathVariable

   1. 使用说明

      ```
      作用：
      	用于绑定url中的占位符。例如：请求url中/delete/{id}，这个{id}就是url占位符。
      	url支持占位符是spring3.0之后加入的。是springmvc支持rest风格的一个重要标志。
      属性：
      	value：用于指定url中占位符名称。
      	required：是否必须提供占位符。
      ```

   2. 使用示例

      ```jsp
              <%-- PathVariable注解 --%>
              <a href="/anno/testPathVariable/10">testPathVariable</a>
      ```

      ```java
          /**
           * PathVariable注解
           * @param id
           * @return
           */
          @RequestMapping("/testPathVariable/{id}")
          public String testPathVariable(@PathVariable("id") String id) {
              System.out.println("id:" + id);
              return "success";
          }
      ```

   3. REST风格URL

      ```
      什么是rest：
      	REST(英文：Representational State Transfer，简称REST)描述了一个架构样式的网络系统，比如web应用程序。它首次出现在2000年Roy Fielding的博士论文中，它是HTTP规范的主要编程者之一。在目前主流的三种web服务交互方案中，REST相比于SOAP(Simple Object Access protocol，简单对象访问协议)以及XML-RPC更加简单明了，无论是对URL的处理还是对Payload的编码，REST都倾向于用更加简单清凉的方法设计和实现。值得注意的是 REST 并没有一个明确的标准，而更像是一种设计的风格。
      	它本身并没有什么实用性，其核心价值在于如何设计出符合 REST 风格的网络接口。
      restful的优点
      	它结构清晰、符合标准、易于理解、扩展方便，所以正得到越来越多网站的采用。
      restful的特性
      	资源(Resources)：网络上的一个实体，或者说是网络上的一个具体信息。
      	它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的存在。可以用一个 URI(统一资源定位符)指向它，每种资源对应一个特定的 URI 。要获取这个资源，访问它的 URI 就可以，因此 URI 即为每一个资源的独一无二的识别符。
      	表现层(Requestation)：把资源具体呈现出来的形式，叫做它的表现层(Representation)。
      	比如，文本可以用 txt 格式表现，也可以用 HTML 格式、XML 格式、JSON 格式表现，甚至可以采用二进制格式。
      	状态转化(State Transfer)：每发出一个请求，就代表了客户端和服务器的一次交互过程。
      		HTTP 协议，是一个无状态协议，即所有的状态都保存在服务器端。因此，如果客户端想要操作服务器，必须通过某种手段，让服务器端发生“状态转化”(State Transfer)。而这种转化是建立在表现层之上的，所以就是“表现层状态转化”。具体说，就是 HTTP 协议里面，四个表示操作方式的动词：GET 、POST 、PUT、Delete。它们分别对应四种基本操作：GET 用来获取资源，POST 用来新建资源，PUT 用来更新资源，DELETE 用来删除资源。
      	restful的示例：
      		/account/1 HTTP GET ： 得到 id = 1 的 account
      		/account/1 HTTP DELETE： 删除 id = 1 的 account
      		/account/1 HTTP PUT： 更新 id = 1 的 account
      		/account HTTP POST： 新增 account
      ```

4. RequestHeader

   1. 使用说明

      ```
      作用：
      	用于获取请求消息头。
      属性：
      	value：提供消息头名称
      	required：是否必须有此消息头
      注：
      	在实际开发中一般不怎么用
      ```

   2. 使用示例

      ```jsp
              <%--RequestHeader注解--%>
              <a href="/anno/testRequestHeader">testRequestHeader</a>
      ```

      ```java
          /**
           * RequestHeader注解
           * @param requestHeader
           * @return
           */
          @RequestMapping("/testRequestHeader")
          public String testRequestHeader(@RequestHeader(value = "Accept-Language",required = false) String requestHeader) {
              System.out.println(requestHeader);
              return "success";
          }
      ```

5. CookieValue

   1. 使用说明

      ```
      作用：
      	用于把指定cookie名称的值传入控制器方法参数
      属性：
      	value：指定cookie的名称
      	required：是否必须有此cookie
      ```

   2. 使用示例

      ```jsp
              <%--CookieValue注解--%>
              <a href="/anno/testCookieValue">testCookieValue</a><br>
      ```

      ```java
          /**
           * CookieValue注解
           * @param cookieValue
           * @return
           */
          @RequestMapping("/testCookieValue")
          public String testCookieValue(@CookieValue(value = "JSESSIONID") String cookieValue) {
              System.out.println(cookieValue);
              return "success";
          }
      ```

6. ModelAttribute

   1. 使用说明

      ```
      作用：
      	该注解是 SpringMVC4.3 版本以后新加入的。它可以用于修饰方法和参数。
      	出现在方法上，表示当前方法会在控制器的方法执行之前，先执行。它可以修饰没有返回值的方法，也可以修饰有具体返回值的方法
      	出现在参数上，获取指定的数据给参数赋值。
      属性：
      	value：用于获取数据的 key。key 可以是 POJO 的属性名称，也可以是 map 结构的 key。
      应用场景：
      	当表单提交数据不是完整的实体类数据时，保证没有提交数据的字段使用数据库对象原来的数据。
      	例如：
      		我们在编辑一个用户时，用户有一个创建信息字段，该字段的值是不允许被修改的。在提交表单数据是肯定没有此字段的内容，一旦更新会把该字段内容置为 null，此时就可以使用此注解解决问题。
      ```

   2. 使用示例

      1. 基于POJO属性的基本使用

         ```jsp
                 <%--ModelAttributes注解--%>
                 <a href="/anno/testModelAttribute">testModelAttribute</a><br>
         ```

         ```java
             /**
              * 接受请求的方法
              * @param user
              * @return
              */
             @RequestMapping("/testModelAttribute")
             public String testModelAttribute(User user) {
                 System.out.println("执行了控制器的方法" + user.getUsername());
                 return "success";
             }
         
             /**
              * 被ModelAttribute修饰的方法
              * @param user
              */
             @ModelAttribute
             public void showModel(User user) {
                 System.out.println("执行了showModel方法" + user.getUsername());
             }
         ```

      2. 基于Map的应用场景示例1：ModelAttribute修饰方法带返回值

         ```jsp
                 <%--修改用户信息--%>
                 <form action="/anno/updateUser" method="post">
                     用户名称<input type="text" name="username">
                     用户年龄<input type="text" name="age">
                     <input type="submit" value="提交"><br>
                 </form>
         ```

         ```java
             /**
              * 查询数据库用户信息
              * @param
              */
             @ModelAttribute
             public User showModel(String username) {
                 User abc = findUserByName(username);
                 System.out.println("执行了showModel方法" + abc);
                 return abc;
             }
             /**
              * 接受请求的方法
              * @param user
              * @return
              */
             @RequestMapping("/updateUser")
             public String testModelAttribute(User user) {
                 System.out.println("执行了控制器的方法：修改用户" + user);
                 return "success";
             }
             /**
              * 模拟数据库查询
              * @param username
              * @return
              */
             private User findUserByName(String username) {
                 User user = new User();
                 user.setUsername(username);
                 user.setAge(19);
                 user.setPassword("123456");
                 return user;
             }
         ```

      3. 基于Map的应用场景示例1：ModelAttribute修饰方法不带返回值

         ```jsp
                 <%--修改用户信息--%>
                 <form action="/anno/updateUser" method="post">
                     用户名称<input type="text" name="username">
                     用户年龄<input type="text" name="age">
                     <input type="submit" value="提交"><br>
                 </form>
         ```

         ```java
             /**
              * 查询数据库用户信息
              * @param
              */
             @ModelAttribute
             public void showModel(String username, Map<String,User> map) {
                 User user = findUserByName(username);
                 map.put("abc",user);
             }
             /**
              * 接受请求的方法
              * @param user
              * @return
              */
             @RequestMapping("/updateUser")
             public String testModelAttribute(@ModelAttribute("abc")User user) {
                 System.out.println("执行了控制器的方法：修改用户" + user);
                 return "success";
             }
             /**
              * 模拟数据库查询
              * @param username
              * @return
              */
             private User findUserByName(String username) {
                 User user = new User();
                 user.setUsername(username);
                 user.setAge(19);
                 user.setPassword("123456");
                 return user;
             }
         ```

7. SessionAttribute

   1. 使用说明

      ```
      作用：
      	用于多次执行控制器方法间的参数共享
      属性：
      	value：用于指定存入的属性名称
      	type：用于指定存入的数据类型
      ```

   2. 使用步骤

      ```jsp
              <%--SessionAttribute注解的使用--%>
              <a href="/springmvc/testPut">存入SessionAttribute</a><br>
      
              <a href="/springmvc/testGet">取出SessionAttribute</a><br>
      
              <a href="/springmvc/testClean">清空SessionAttribute</a><br>
      ```

      ```java
      package cn.itcast.domain;
      
      import org.springframework.stereotype.Controller;
      import org.springframework.ui.Model;
      import org.springframework.ui.ModelMap;
      import org.springframework.web.bind.annotation.ModelAttribute;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.SessionAttributes;
      import org.springframework.web.bind.support.SessionStatus;
      
      /**
       * SessionAttribute注解的使用
       */
      @RequestMapping("/springmvc")
      @Controller("sessionAttributeController")
      @SessionAttributes(value = {"username","password"},types = {Integer.class})
      public class SessionAttributeController {
          /**
           * 把数据存入SessionAttribute
           * @param model
           * @return
           * Model是spring提供的一个接口，该接口有一个实现类ExtendeModelMap
           * 该类继承了ModelMap，而ModelMap就是LinkedHashMap
           */
          @RequestMapping("/testPut")
          public String testPut(Model model) {
              model.addAttribute("username","塔斯特");
              model.addAttribute("password","123456");
              model.addAttribute("age",13);
              System.out.println("存入成功");
              return "success";
          }
          @RequestMapping("/testGet")
          public String testGet(ModelMap model) {
              System.out.println(model.get("username") + ";" + model.get("password") + ";" + model.get("age"));
              return "success";
          }
          @RequestMapping("/testClean")
          public String testClean(SessionStatus sessionStatus) {
              sessionStatus.setComplete();
              return "success";
          }
      }
      ```

      

   

