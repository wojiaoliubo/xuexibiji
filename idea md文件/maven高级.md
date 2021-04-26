# maven高级应用

1. maven基础回顾
2. maven传统的web工程做一个数据查询操作。
3. maven工程拆分与聚合的思想。
4. 把第二阶段做好的web工程修改成maven拆分与聚合的形式。
5. 私服【远程仓库】。
6. 如何安装第三方jar包。【把第三方jar包安装到本地仓库，把第三方jar包安装到私服。】



## maven基础知识回顾：

```
maven是一个项目管理工具
依赖管理：maven对项目中jar包的管理过程，传统工程我们直接把jar包放置在项目中。
		maven工程真正的jar包放置在仓库中，项目中只用放置jar包的坐标。
仓库的种类：本地仓库、远程仓库【私服】、中央仓库。
仓库之间的关系：当我们启动一个maven工程的时候，maven工程会通过pom文件中jar包的坐标去本地仓库找对应的jar包
			 默认情况下，如果本地仓库没有对应的jar包，maven工程会自动去中央仓库下载jar包到本地仓库。
			 在公司中，如果本地没有对应的jar包，会先从私服下载jar包。
			 如果私服中没有jar包，可以从中央仓库下载，也可以从本地仓库上传。
一键构建：maven自身集成了tomcat插件，可以对项目进行编译。测试，打包，安装，发布等操作。
maven常用命令：clean，compile，test，package，install，deploy。
maven三套生命周期：清理生命周期，默认生命周期，站点生命周期。
```

## -----------------------------------------------

5. 定义pom.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>com.itheima</groupId>
     <artifactId>maven_day02_01</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>war</packaging>
   
       <!--maven工程要导入jar包的坐标，就必须要考虑解决jar包的冲突。
           解决jar包冲突的方式一：
           第一声明优先原则，哪个jar包坐标在靠上的位置，这个jar包就是先声明的。
           先声明的jar包坐标下的依赖包，可以优先进入项目中。
   
           maven导入jar包中的一些概念
           直接依赖：项目中直接导入的jar包就是直接该项目的直接依赖包。
           传递依赖：项目中没有直接导入的jar包，可以通过项目直接依赖jar包传递到项目中。
   
           解决jar包冲突的方式二：
           路径近者优先原则。直接依赖比传递依赖路径近，那么最终进入项目的jar包会是路径近的jar包
   
           解决jar包冲突的方式三【推荐使用】：
           直接排除法
           当我们要排除某个jar包下依赖包，在配置exclusions标签的时候，内部可以不写版本号。
           因为此时依赖包使用的版本号，默认和本jar包一样。
       -->
     <!-- 统一管理jar包版本 -->
     <properties>
       <spring.version>5.0.2.RELEASE</spring.version>
       <slf4j.version>1.6.6</slf4j.version>
       <log4j.version>1.2.12</log4j.version>
       <shiro.version>1.2.3</shiro.version>
       <mysql.version>5.1.6</mysql.version>
       <mybatis.version>3.4.5</mybatis.version>
       <spring.security.version>5.0.1.RELEASE</spring.security.version>
     </properties>
   
     <!--
       maven工程是可以分父子依赖关系的。
       凡是依赖别的项目后，拿到的别的项目的依赖包，都属于传递依赖。
       比如：当前A项目，被B项目依赖，那么我们A项目中所有jar包都会传递到B项目中。
       B项目开发者，如果再在B项目中导入一套ssm框架的jar包，对于B项目是直接依赖。
       那么直接依赖的jar包就会把我们A项目传递过去的jar包覆盖掉。
       为了防止以上情况出现，我们可以把A项目中主要jar包锁住，那么其他依赖该项目的项目中。
       即便是由同名jar包直接依赖，也无法覆盖。
     -->
   
     <!-- 锁定jar包版本 -->
     <dependencyManagement>
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
           <groupId>org.springframework</groupId>
           <artifactId>spring-tx</artifactId>
           <version>${spring.version}</version>
         </dependency>
         <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-test</artifactId>
           <version>${spring.version}</version>
         </dependency>
         <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis</artifactId>
           <version>${mybatis.version}</version>
         </dependency>
       </dependencies>
     </dependencyManagement>
   
     <!-- 项目依赖jar包 -->
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
         <artifactId>spring-context-support</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-web</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-orm</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-beans</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-core</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-test</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-tx</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>${mysql.version}</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>3.1.0</version>
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
       <dependency>
         <groupId>com.github.pagehelper</groupId>
         <artifactId>pagehelper</artifactId>
         <version>5.1.2</version>
       </dependency>
       <dependency>
         <groupId>org.springframework.security</groupId>
         <artifactId>spring-security-web</artifactId>
         <version>${spring.security.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework.security</groupId>
         <artifactId>spring-security-config</artifactId>
         <version>${spring.security.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework.security</groupId>
         <artifactId>spring-security-core</artifactId>
         <version>${spring.security.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework.security</groupId>
         <artifactId>spring-security-taglibs</artifactId>
         <version>${spring.security.version}</version>
       </dependency>
       <dependency>
         <groupId>com.alibaba</groupId>
         <artifactId>druid</artifactId>
         <version>1.0.9</version>
       </dependency>
     </dependencies>
     <!-- 添加tomcat7插件 -->
     <build>
       <plugins>
         <plugin>
           <groupId>org.apache.tomcat.maven</groupId>
           <artifactId>tomcat7-maven-plugin</artifactId>
           <version>2.2</version>
         </plugin>
       </plugins>
     </build>
   </project>
   ```

6. Dao层

   1. pojo模型类

      - Items.java

        ```java
        package com.itheima.doamin;
        
        import java.util.Date;
        
        public class Items {
        
            private Integer id;
            private String name;
            private Double price;
            private String pic;
            private Date createtime;
            private String detail;
        
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
        
            public Double getPrice() {
                return price;
            }
        
            public void setPrice(Double price) {
                this.price = price;
            }
        
            public String getPic() {
                return pic;
            }
        
            public void setPic(String pic) {
                this.pic = pic;
            }
        
            public Date getCreatetime() {
                return createtime;
            }
        
            public void setCreatetime(Date createtime) {
                this.createtime = createtime;
            }
        
            public String getDetail() {
                return detail;
            }
        
            public void setDetail(String detail) {
                this.detail = detail;
            }
        }
        ```

   2. dao层代码

      * ItemsDao.java

        ```java
        package com.itheima.dao;
        
        import com.itheima.doamin.Items;
        
        public interface ItemsDao {
        
            public Items findById(Integer id);
        }
        ```

   3. 配置文件

      * ItemsDao.xml（必须和接口的路径结构保持一致）

        ```xml
        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        
        <mapper namespace="com.itheima.dao.ItemsDao">
            <select id="findById" parameterType="int" resultType="items">
                select * from items where id = #{id}
            </select>
        </mapper>
        ```

      * 在resources中创建applicationContext.xml

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xmlns:context="http://www.springframework.org/schema/context"
               xmlns:aop="http://www.springframework.org/schema/aop"
               xmlns:tx="http://www.springframework.org/schema/tx"
               xmlns:mvc="http://www.springframework.org/schema/mvc"
               xsi:schemaLocation="http://www.springframework.org/schema/beans
        			    http://www.springframework.org/schema/beans/spring-beans.xsd
        			    http://www.springframework.org/schema/context
        			    http://www.springframework.org/schema/context/spring-context.xsd
        			    http://www.springframework.org/schema/aop
        			    http://www.springframework.org/schema/aop/spring-aop.xsd
        			    http://www.springframework.org/schema/tx
        			    http://www.springframework.org/schema/tx/spring-tx.xsd
        			    http://www.springframework.org/schema/mvc
        			    http://www.springframework.org/schema/mvc/spring-mvc.xsd">
            
            <!--dao层配置文件开始-->
        
            <!--配置连接池-->
            <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
                <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
                <property name="url" value="jdbc:mysql://localhost:3306/maven"></property>
                <property name="username" value="root"></property>
                <property name="password" value="15591177926"></property>
            </bean>
        
            <!--配置生产SqlSession对象的工厂-->
            <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
                <property name="dataSource" ref="dataSource"/>
                <!--扫描pojo包，给包下所有pojo对象起别名-->
                <property name="typeAliasesPackage" value="com.itheima.doamin"/>
            </bean>
        
            <!--扫描接口包路径，生成包下所有接口的代理对象并且放入spring容器中-->
            <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
                <property name="basePackage" value="com.itheima.dao"/>
            </bean>
            
            <!--dao层配置文件结束-->
        ```

      * 导入log4j.properties

        ```properties
        # Set root category priority to INFO and its only appender to CONSOLE.
        #log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
        log4j.rootCategory=debug, CONSOLE, LOGFILE
        
        # Set the enterprise logger category to FATAL and its only appender to CONSOLE.
        log4j.logger.org.apache.axis.enterprise=FATAL, CONSOLE
        
        # CONSOLE is set to be a ConsoleAppender using a PatternLayout.
        log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
        log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
        log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n
        
        # LOGFILE is set to be a File appender using a PatternLayout.
        log4j.appender.LOGFILE=org.apache.log4j.FileAppender
        log4j.appender.LOGFILE.File=d:\axis.log
        log4j.appender.LOGFILE.Append=true
        log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
        log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n
        ```

   4. 单元测试

      - ItemsTest.java

        ```java
        public class ItemsTest {
        
            @Test
            public void findById() {
                //先获取spring容器
                ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
                //dao测试
                //从容器中拿到所需的dao代理对象
                ItemsDao itemsDao = ac.getBean(ItemsDao.class);
                //调用方法
                Items items = itemsDao.findById(1);
                System.out.println(items.getName());
        
            }
        }
        ```

7. Service层

   1. 代码

      - ItemsServiceImpl.java

        ```java
        @Service
        public class ItemsServiceImpl implements ItemsService {
        
            @Autowired
            private ItemsDao itemsDao;
        
            public Items findById(Integer id) {
                return itemsDao.findById(id);
            }
        }
        ```

   2. 配置文件

      - 在applicationContext.xml中配置Service

        ```xml
            <!--业务层配置文件开始-->
        
            <!--组件扫描配置-->
            <context:component-scan base-package="com.itheima.service"/>
        
            <!--AOP面向切面编程，切面就是切入点和通知的组合-->
            <!--配置事务管理器-->
            <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
                <property name="dataSource" ref="dataSource"/>
            </bean>
            <!--配置事务的通知-->
            <tx:advice id="advice">
                <tx:attributes>
                    <tx:method name="save*" propagation="REQUIRED"/>
                    <tx:method name="update*" propagation="REQUIRED"/>
                    <tx:method name="delete*" propagation="REQUIRED"/>
                    <tx:method name="find*" read-only="true"/>
                    <tx:method name="*" propagation="REQUIRED"/>
                </tx:attributes>
            </tx:advice>
        
            <!--配置切面-->
            <aop:config>
                <!--切入点-->
                <aop:pointcut id="pointcut" expression="execution(* com.itheima.service.impl.*.*(..))"/>
                <!--把通知和切入点弄到一起变成切面-->
                <aop:advisor advice-ref="advice" pointcut-ref="pointcut"/>
            </aop:config>
        
            <!--业务层配置文件结束-->
        ```

8. Web 层

   1. 代码

      - ItemsController.java

        ```java
        @Controller
        @RequestMapping("/items")
        public class ItemsController {
        
            @Autowired
            private ItemsService itemsService;
        
            @RequestMapping("/findDetail")
            public String findDetail(Model model) {
        
                Items items = itemsService.findById(1);
                model.addAttribute("item",items);
                return "itemDetail";
            }
        }
        ```

   2. 配置文件

      * springmvc.xml

        > springmvc中主要配置：
        >
        > 	1. 组件扫描
        >  	2. 处理器映射器，处理器适配器
        >  	3. 视图解析器
        >  	4. 释放静态资源

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xmlns:context="http://www.springframework.org/schema/context"
               xmlns:aop="http://www.springframework.org/schema/aop"
               xmlns:tx="http://www.springframework.org/schema/tx"
               xmlns:mvc="http://www.springframework.org/schema/mvc"
               xsi:schemaLocation="http://www.springframework.org/schema/beans
        			    http://www.springframework.org/schema/beans/spring-beans.xsd
        			    http://www.springframework.org/schema/context
        			    http://www.springframework.org/schema/context/spring-context.xsd
        			    http://www.springframework.org/schema/aop
        			    http://www.springframework.org/schema/aop/spring-aop.xsd
        			    http://www.springframework.org/schema/tx
        			    http://www.springframework.org/schema/tx/spring-tx.xsd
        			    http://www.springframework.org/schema/mvc
        			    http://www.springframework.org/schema/mvc/spring-mvc.xsd">
        
            <!--组件扫描-->
            <context:component-scan base-package="com.itheima.controller"/>
        
            <!--处理器映射器，处理器适配器-->
            <mvc:annotation-driven/>
        
            <!--视图解析器-->
            <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
                <property name="prefix" value="/WEB-INF/pages/"/>
                <property name="suffix" value=".jsp"/>
            </bean>
        
            <!--释放静态资源-->
            <mvc:default-servlet-handler/>
        </beans>
        ```

      * web.xml

        > web.xml中三大核心配置：
        >
        > 	1. 编码过滤器
        >  	2. 配置spring核心监听器
        >       	1. 注意要重新制定spring配置文件的路径
        >  	3. pringmvc的核心servlet

        ```xml
        <!DOCTYPE web-app PUBLIC
         "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
         "http://java.sun.com/dtd/web-app_2_3.dtd" >
        
        <web-app xmlns="http://java.sun.com/xml/ns/javaee"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
                  http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
                 version="3.0">
        
            <!--编码过滤器-->
            <filter>
                <filter-name>encoding</filter-name>
                <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
                <init-param>
                    <param-name>encoding</param-name>
                    <param-value>UTF-8</param-value>
                </init-param>
                <init-param>
                    <param-name>forceEncoding</param-name>
                    <param-value>true</param-value>
                </init-param>
            </filter>
            <filter-mapping>
                <filter-name>encoding</filter-name>
                <url-pattern>/*</url-pattern>
            </filter-mapping>
        
            <!--配置spring核心监听器-->
            <listener>
                <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
            </listener>
            <!--重新制定spring配置文件的路径-->
            <context-param>
                <param-name>contextConfigLocation</param-name>
                <param-value>classpath:applicationContext.xml</param-value>
            </context-param>
        
            <!--springmvc的核心servlet-->
            <servlet>
                <servlet-name>springmvc</servlet-name>
                <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
                <init-param>
                    <param-name>contextConfigLocation</param-name>
                    <param-value>classpath:springmvc.xml</param-value>
                </init-param>
                <load-on-startup>1</load-on-startup>
            </servlet>
            <servlet-mapping>
                <servlet-name>springmvc</servlet-name>
                <url-pattern>/</url-pattern>
            </servlet-mapping>
        </web-app>
        ```

9. Jsp 

   1. itemDatil.jsp

      ```jsp
      <%@ page language="java" contentType="text/html; charset=UTF-8"
          pageEncoding="UTF-8"%>
      <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
      <%@ taglib uri="http://java.sun.com/jsp/jstl/fmt"  prefix="fmt"%>    
       
      <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
      <html>
      <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
      <title>Insert title here</title>
      </head>
      <body> 
      	<form>
      		<table width="100%" border=1>
      			<tr>
      				<td>商品名称</td>
      				<td> ${item.name } </td>
      			</tr>
      			<tr>
      				<td>商品价格</td>
      				<td> ${item.price } </td>
      			</tr>
      			<tr>
      				<td>生成日期</td>
      				<td> <fmt:formatDate value="${item.createtime}" pattern="yyyy-MM-dd HH:mm:ss"/> </td>
      			</tr>
      			<tr>
      				<td>商品简介</td>
      				<td>${item.detail} </td>
      			</tr>
      		</table>
      	</form>
      </body>
      </html>
      ```

10. 运行与测试

## --------------------------------------

## 三、分模块构建工程[应用]

具体看Maven-教案-实战(IDEA)

## --------------------------------------------------

## 私服nexus

同上

