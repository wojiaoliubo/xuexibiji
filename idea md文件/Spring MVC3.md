# 第一章：SSM整合

1. 环境准备

   1. 创建数据库和表结构

      ```sql
      create database ssm;
      use ssm;
      create table account (
          id int primary key auto_increment,
          name varchar(100),
          money double(7,2)
          );
      select * from account;
      ```

   2. 创建Maven工程

   3. 导入坐标并建立依赖

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      
      <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
      
        <groupId>cn.itcast</groupId>
        <artifactId>ssm</artifactId>
        <version>1.0-SNAPSHOT</version>
        <packaging>war</packaging>
      
        <name>ssm Maven Webapp</name>
        <!-- FIXME change it to the project's website -->
        <url>http://www.example.com</url>
      
        <properties>
          <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
          <maven.compiler.source>1.8</maven.compiler.source>
          <maven.compiler.target>1.8</maven.compiler.target>
          <spring.version>5.0.2.RELEASE</spring.version>
          <slf4j.version>1.6.6</slf4j.version>
          <log4j.version>1.2.12</log4j.version>
          <mysql.version>5.1.6</mysql.version>
          <mybatis.version>3.4.5</mybatis.version>
        </properties>
      
        <dependencies>
          <!-- spring -->
          <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.6.8</version>
          </dependency>
      
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
          </dependency>
      
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
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
          </dependency>
      
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.version}</version>
          </dependency>
      
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
          </dependency>
      
          <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>compile</scope>
          </dependency>
      
          <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
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
      
          <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
          </dependency>
      
          <!-- log start -->
          <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
          </dependency>
      
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
          </dependency>
      
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
          </dependency>
      
          <!-- log end -->
          <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis.version}</version>
          </dependency>
      
          <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.0</version>
          </dependency>
      
          <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
            <type>jar</type>
            <scope>compile</scope>
          </dependency>
        </dependencies>
      
        <build>
          <finalName>ssm</finalName>
          <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
            <plugins>
              <plugin>
                <artifactId>maven-clean-plugin</artifactId>
                <version>3.1.0</version>
              </plugin>
              <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
              <plugin>
                <artifactId>maven-resources-plugin</artifactId>
                <version>3.0.2</version>
              </plugin>
              <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
              </plugin>
              <plugin>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.1</version>
              </plugin>
              <plugin>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.2.2</version>
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

   4. 编写实体类

      ```java
      package cn.itcast.domain;
      
      import java.io.Serializable;
      
      /**
       *  账户实体类
       */
      public class Account implements Serializable {
      
          private Integer id;
          private String name;
          private Double money;
      
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
      
          public Double getMoney() {
              return money;
          }
      
          public void setMoney(Double money) {
              this.money = money;
          }
      }
      ```

   5. 编写业务层接口

      ```java
      package cn.itcast.service;
      
      import cn.itcast.domain.Account;
      
      import java.util.List;
      
      /**
       *  账户的业务层接口
       */
      public interface AccountService {
      
          /**
           * 查询所有账户
           * @return
           */
          public List<Account> findAll();
      
          /**
           * 保存账户
           * @param account
           */
          public void saveAccount(Account account);
      }
      ```

   6. 编写持久层接口

      ```java
      package cn.itcast.dao;
      
      import cn.itcast.domain.Account;
      
      import java.util.List;
      
      /**
       *  账户的持久层接口
       */
      public interface AccountDao {
      
          /**
           * 查询所有账户信息
           * @return
           */
          public List<Account> findAll();
      
          /**
           * 保存账户信息
           * @param account
           */
          public void saveAccount(Account account);
      }
      ```

2. 整合步骤

   1. 保证Spring框架在web工程中独立运行

      1. 第一步编写spring配置文件并导入约束

         * applicationContext.xml

           ```xml
           <?xml version="1.0" encoding="UTF-8"?>
           <beans xmlns="http://www.springframework.org/schema/beans"
                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                  xmlns:context="http://www.springframework.org/schema/context"
                  xmlns:aop="http://www.springframework.org/schema/aop"
                  xmlns:tx="http://www.springframework.org/schema/tx"
                  xsi:schemaLocation="http://www.springframework.org/schema/beans
                   http://www.springframework.org/schema/beans/spring-beans.xsd
                   http://www.springframework.org/schema/context
                   http://www.springframework.org/schema/context/spring-context.xsd
                   http://www.springframework.org/schema/aop
                   http://www.springframework.org/schema/aop/spring-aop.xsd
                   http://www.springframework.org/schema/tx
                   http://www.springframework.org/schema/tx/spring-tx.xsd">
           
               <!--开启注解的扫描,只希望处理service和dao，controller不需要Spring去处理-->
               <context:component-scan base-package="cn.itcast">
                   <!--配置哪些注解不扫描-->
                   <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
               </context:component-scan>
           </beans>
           ```

      2. 第二步：使用注解配置业务层和持久层

         * AccountServiceImpl.java

           ```java
           package cn.itcast.service.impl;
           
           import cn.itcast.domain.Account;
           import cn.itcast.service.AccountService;
           import org.springframework.stereotype.Service;
           
           import java.util.List;
           
           /**
            *  账户的业务层实现类
            */
           @Service("accountService")
           public class AccountServiceImpl implements AccountService {
           
               public List<Account> findAll() {
                   System.out.println("业务层：查询所有的账户信息");
                   return null;
               }
           
               public void saveAccount(Account account) {
                   System.out.println("业务层：保存账户");
               }
           }
           
           ```

      3. 第三步：测试spring能否独立运行

         * TestSpring.java

           ```java
           package cn.itcast.test;
           
           import cn.itcast.service.AccountService;
           import org.junit.Test;
           import org.springframework.context.ApplicationContext;
           import org.springframework.context.support.ClassPathXmlApplicationContext;
           
           public class TestSpring {
           
               @Test
               public void run1() {
                   //加载配置文件
                   ApplicationContext ac = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
                   //获取对象
                   AccountService as = (AccountService) ac.getBean("accountService");
                   //调用方法
                   as.findAll();
               }
           }
           ```

   2. 保证SpringMVC在web工程中独立运行

      1. 第一步：在web.xml中配置核心控制器(DispatcherServlet)

         * web.xml

           ```xml
           <!DOCTYPE web-app PUBLIC
            "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
            "http://java.sun.com/dtd/web-app_2_3.dtd" >
           
           <web-app>
             <display-name>Archetype Created Web Application</display-name>
           
             <!--配置前端控制器-->
             <servlet>
               <servlet-name>dispatcherServlet</servlet-name>
               <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
               <!--加载springmvc.xml配置文件-->
               <init-param>
                 <param-name>contextConfigLocation</param-name>
                 <param-value>classpath:springmvc.xml</param-value>
               </init-param>
               <!--启动服务器，创建该servlet-->
               <load-on-startup>1</load-on-startup>
             </servlet>
             <servlet-mapping>
               <servlet-name>dispatcherServlet</servlet-name>
               <url-pattern>/</url-pattern>
             </servlet-mapping>
           
             <!--解决中文乱码的过滤器-->
             <filter>
               <filter-name>characterEncodingFilter</filter-name>
               <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
               <!--设置具体的编码集-->
               <init-param>
                 <param-name>encoding</param-name><!--参数名称-->
                 <param-value>UTF-8</param-value><!--参数值-->
               </init-param>
             </filter>
             <filter-mapping><!--过滤器映射-->
               <filter-name>characterEncodingFilter</filter-name><!--过滤器名称-->
               <url-pattern>/*</url-pattern><!--URL映射，给所有页面处理乱码-->
             </filter-mapping>
           
           </web-app>
           ```

      2. 第二步：编写SpringMVC的配置文件

         * springmvc.xml

           ```xml
           <?xml version="1.0" encoding="UTF-8"?>
           <beans xmlns="http://www.springframework.org/schema/beans"
                  xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:context="http://www.springframework.org/schema/context"
                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                  xsi:schemaLocation="
                   http://www.springframework.org/schema/beans
                   http://www.springframework.org/schema/beans/spring-beans.xsd
                   http://www.springframework.org/schema/mvc
                   http://www.springframework.org/schema/mvc/spring-mvc.xsd
                   http://www.springframework.org/schema/context
                   http://www.springframework.org/schema/context/spring-context.xsd">
           
               <!--开启注解扫描，只扫描Controller注解-->
               <context:component-scan base-package="cn.itcast">
                   <!--只扫描Controller注解-->
                   <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
               </context:component-scan>
               <!--配置视图解析器对象-->
               <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
                   <property name="prefix" value="/WEB-INF/pages/"></property>
                   <property name="suffix" value=".jsp"></property>
               </bean>
               <!--过滤静态资源-->
               <mvc:resources location="/css/" mapping="/css/**" />
               <mvc:resources location="/images/" mapping="/images/**" />
               <mvc:resources location="/js/" mapping="/js/**" />
               <!--开启SpringMVC注解的支持-->
               <mvc:annotation-driven></mvc:annotation-driven>
           </beans>
           ```

      3. 第三步：编写Controller和jsp页面

         * index.jsp

           ```jsp
           <%@ page contentType="text/html;charset=UTF-8" language="java" %>
           <html>
           <head>
               <title>Title</title>
           </head>
           <body>
           
               <a href="/account/findAll">测试：查询所有</a>
           
           </body>
           </html>
           ```

         * AccountController.java

           ```java
           package cn.itcast.controller;
           
           import org.springframework.stereotype.Controller;
           import org.springframework.web.bind.annotation.RequestMapping;
           
           /**
            *  账户web层
            */
           @Controller
           @RequestMapping("/account")
           public class AccountController {
           
               @RequestMapping("/findAll")
               public String findAll() {
                   System.out.println("表现层：查询所有的账户信息");
                   return "list";
               }
           }
           ```

   3. 整合Spring和SpringMVC

      1. 第一步：配置监听器实现启动服务创建容器

         * web.xml

           ```xml
             <!--配置Spring的监听器,默认只加载WEB-INF目录下的applicationContext.xml配置文件-->
             <listener>
               <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
             </listener>
             <!--手动指定spring配置文件的路径-->
             <context-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:applicationContext.xml</param-value>
             </context-param>
           ```

      2. 第二步：测试

         * AccountController.java

           ```java
           package cn.itcast.controller;
           
           import cn.itcast.service.AccountService;
           import org.springframework.beans.factory.annotation.Autowired;
           import org.springframework.stereotype.Controller;
           import org.springframework.web.bind.annotation.RequestMapping;
           
           /**
            *  账户web层
            */
           @Controller
           @RequestMapping("/account")
           public class AccountController {
           
               @Autowired
               private AccountService accountService;
           
               @RequestMapping("/findAll")
               public String findAll() {
                   System.out.println("表现层：查询所有的账户信息");
                   //调用Service的方法
                   accountService.findAll();
                   return "list";
               }
           }
           ```

   4. 保证Mybatis框架在web工程中独立运行

      1. 第一步：编写AccountDao映射配置文件（或者使用注解的方式进行配置）

         * AccountDao.java

           ```java
           package cn.itcast.dao;
           
           import cn.itcast.domain.Account;
           import org.apache.ibatis.annotations.Insert;
           import org.apache.ibatis.annotations.Select;
           
           import java.util.List;
           
           /**
            *  账户的持久层接口
            */
           public interface AccountDao {
           
               /**
                * 查询所有账户信息
                * @return
                */
               @Select("select * from account")
               public List<Account> findAll();
           
               /**
                * 保存账户信息
                * @param account
                */
               @Insert("insert into account(name,money) values(#{name},#{money})")
               public void saveAccount(Account account);
           }
           ```

      2. 第二步：编写SqlMapConfig配置文件

         * SqlMapConfig.xml

           ```xml
           <?xml version="1.0" encoding="UTF-8" ?>
           <!DOCTYPE configuration
                   PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
                   "http://mybatis.org/dtd/mybatis-3-config.dtd">
           <configuration>
               <!--配置环境-->
               <environments default="mysql">
                   <environment id="mysql">
                       <transactionManager type="JDBC"></transactionManager>
                       <dataSource type="POOLED">
                           <property name="driver" value="com.mysql.jdbc.Driver"/>
                           <property name="url" value="jdbc:mysql://localhost:3306/ssm"/>
                           <property name="username" value="root"/>
                           <property name="password" value="15591177926"/>
                       </dataSource>
                   </environment>
               </environments>
               <!--引入映射配置文件-->
               <mappers>
                   <!--<mapper class="cn.itcast.dao.AccountDao"></mapper>-->
                   <package name="cn.itcast.dao"/>
               </mappers>
           </configuration>
           ```

      3. 第三步：测试运行结果

         * TestMybatis.java

           ```java
           package cn.itcast.test;
           
           import cn.itcast.dao.AccountDao;
           import cn.itcast.domain.Account;
           import org.apache.ibatis.io.Resources;
           import org.apache.ibatis.session.SqlSession;
           import org.apache.ibatis.session.SqlSessionFactory;
           import org.apache.ibatis.session.SqlSessionFactoryBuilder;
           import org.junit.Test;
           
           import java.io.InputStream;
           import java.util.List;
           
           public class TestMybatis {
           
               @Test
               public void run1() throws Exception{
                   //加载mybatis配置文件
                   InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
                   //创建SqlSessonFactory对象
                   SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
                   //创建sqlSession对象
                   SqlSession session = factory.openSession();
                   //获取到代理对象
                   AccountDao dao = session.getMapper(AccountDao.class);
                   //查询所有的账户信息
                   List<Account> list = dao.findAll();
                   for (Account account : list) {
                       System.out.println(account);
                   }
                   //关闭资源
                   session.close();
                   in.close();
               }
           
               @Test
               public void run2() throws Exception{
                   Account account = new Account();
           
                   //加载mybatis配置文件
                   InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
                   //创建SqlSessonFactory对象
                   SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
                   //创建sqlSession对象
                   SqlSession session = factory.openSession();
                   //获取到代理对象
                   AccountDao dao = session.getMapper(AccountDao.class);
           
                   account.setName("吴奇刚");
                   account.setMoney(250d);
                   dao.saveAccount(account);
                   session.commit();
                   session.close();
                   in.close();
               }
           }
           ```

   5. 整合Spring和Mybatis

      1. 第一步：Spring接管Mybatis的Session工厂

         * applicationContext.xml

           ```xml
               <!--Spring整合Mybatis框架-->
               <!--配置连接池-->
               <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
                   <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
                   <property name="jdbcUrl" value="jdbc:mysql:///ssm"></property>
                   <property name="user" value="root"></property>
                   <property name="password" value="15591177926"></property>
               </bean>
               <!--配置工厂对象-->
               <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
                   <property name="dataSource" ref="dataSource"></property>
               </bean>
           ```

      2. 第二步：配置自动扫描所有Mapper接口和文件

         * applocationContext.xml

           ```xml
               <!--配置接口所在的包-->
               <bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
                    <property name="basePackage" value="cn.itcast.dao"></property>
               </bean>
           ```

      3. 第三步：配置spring的事务

         * applicationContext.xml

           ```xml
               <!--配置Spring框架声明式事务管理-->
               <!--配置事务管理器-->
               <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
                   <property name="dataSource" ref="dataSource"></property>
               </bean>
               <!--配置事物的通知-->
               <tx:advice id="txAdvice" transaction-manager="transactionManager">
                   <tx:attributes>
                       <tx:method name="find*" read-only="true"/>
                       <tx:method name="*" isolation="DEFAULT"/>
                   </tx:attributes>
               </tx:advice>
               <!--配置AOP增强-->
               <aop:config>
                   <!--配置切入点表达式-->
                   <aop:advisor advice-ref="txAdvice" pointcut="execution(* cn.itcast.service.impl.*ServiceImpl.*(..))"/>
               </aop:config>
           ```

   6. 测试SSM整合结果

      1. 编写测试jsp

         * index.jsp

           ```jsp
           <%--
             Created by IntelliJ IDEA.
             User: 28943
             Date: 2021/3/20
             Time: 14:21
             To change this template use File | Settings | File Templates.
           --%>
           <%@ page contentType="text/html;charset=UTF-8" language="java" %>
           <html>
           <head>
               <title>Title</title>
           </head>
           <body>
           
               <a href="/account/findAll">测试：查询所有</a>
           
               <h3>测试保存</h3>
           
               <form action="/account/save" method="post">
                   姓名：<input type="text" name="name"><br>
                   金额：<input type="text" name="money"><br>
                   <input type="submit" value="提交">
               </form>
           </body>
           </html>
           ```

      2. 控制器

         * AccountController.java

           ```java
           package cn.itcast.controller;
           
           import cn.itcast.domain.Account;
           import cn.itcast.service.AccountService;
           import org.springframework.beans.factory.annotation.Autowired;
           import org.springframework.stereotype.Controller;
           import org.springframework.ui.Model;
           import org.springframework.web.bind.annotation.RequestMapping;
           
           import javax.servlet.http.HttpServletRequest;
           import javax.servlet.http.HttpServletResponse;
           import java.io.IOException;
           import java.util.List;
           
           /**
            *  账户web层
            */
           @Controller
           @RequestMapping("/account")
           public class AccountController {
           
               @Autowired
               private AccountService accountService;
           
               @RequestMapping("/findAll")
               public String findAll(Model model) {
                   System.out.println("表现层：查询所有的账户信息");
                   //调用Service的方法
                   List<Account> list = accountService.findAll();
                   model.addAttribute("list",list);
                   return "list";
               }
           
               /**
                * 用来保存的
                * @param
                * @return
                */
               @RequestMapping("/save")
               public void save(Account account, HttpServletRequest request, HttpServletResponse response) throws IOException {
                   accountService.saveAccount(account);
                   response.sendRedirect(request.getContextPath() + "/account/findAll");
                   return;
               }
           }
           ```

      3. 测试

         * list.jsp

           ```jsp
           <%--
             Created by IntelliJ IDEA.
             User: 28943
             Date: 2021/3/20
             Time: 14:24
             To change this template use File | Settings | File Templates.
           --%>
           <%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
           <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
           <html>
           <head>
               <title>Title</title>
           </head>
           <body>
               <h3>查询了所有的账户信息</h3>
           
               <c:forEach items="${list}" var="account">
                   ${account.name}
               </c:forEach>
           </body>
           </html>
           ```

           

      

   

   

   

   

   