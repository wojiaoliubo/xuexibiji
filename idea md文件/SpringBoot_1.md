# 约定大于配置！！！！！！！

## SpringBoot简介

1. 回顾什么是Spring

   >  Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson  。 
   >
   >  **Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。** 

2. Spring是如何简化Java开发的

   >  为了降低Java开发的复杂性，Spring采用了以下4种关键策略： 
   >
   > 1.  基于POJO的轻量级和最小侵入性编程，所有东西都是bean； 
   > 2.  通过IOC，依赖注入（DI）和面向接口实现松耦合； 
   > 3.  基于切面（AOP）和惯例进行声明式编程； 
   > 4.  通过切面和模版减少样式代码，RedisTemplate，xxxTemplate； 

3. 什么是SpringBoot

   >  	学过javaweb的同学就知道，开发一个web应用，从最初开始接触Servlet结合Tomcat, 跑出一个Hello Wolrld程序，是要经历特别多的步骤；后来就用了框架Struts，再后来是SpringMVC，到了现在的SpringBoot，过一两年又会有其他web框架出现；你们有经历过框架不断的演进，然后自己开发项目所有的技术也在不断的变化、改造吗？建议都可以去经历一遍； 
   >
   > ​	 言归正传，什么是SpringBoot呢，就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，官方说是简化开发，约定大于配置，  you can "just run"，能迅速的开发web应用，几行代码开发一个http接口。 
   >
   >  	所有的技术框架的发展似乎都遵循了一条主线规律：从一个复杂应用场景 衍生 一种规范框架，人们只需要进行各种配置而不需要自己去实现它，这时候强大的配置功能成了优点；发展到一定程度之后，人们根据实际生产应用情况，选取其中实用功能和设计精华，重构出一些轻量级的框架；之后为了提高开发效率，嫌弃原先的各类配置过于麻烦，于是开始提倡“约定大于配置”，进而衍生出一些一站式的解决方案。 
   >	
   >  	是的这就是Java企业级应用->J2EE->spring->springboot的过程。 
   >	
   >  	Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用。 
   >	
   >  	简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 。 
   >
   > ​	 Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的发展，生态足够完善，Spring Boot 已经当之无愧成为 Java 领域最热门的技术。 
   >
   > **Spring Boot的主要优点：** 
   >
   > * 为所有Spring开发者更快的入门 
   > * 开箱即用，提供各种默认配置来简化配置
   > * 内嵌式容器简化Web项目
   > * 没有冗余代码生成和XML配置的要求

## Hello World

1. 准备工作

   > ​		我们将学习如何快速的创建一个Spring Boot应用，并且实现一个简单的Http请求处理。通过这个例子对Spring Boot有一个初步的了解，并体验其结构简单、开发快速的特性。 
   >
   > 
   >
   > 我们的环境准备：
   >
   > * java version “1.8.0_181”
   > * Maven - 3.6.1
   > * SpringBoot 2.x 最新版
   >
   > 开发工具：
   >
   > * IDEA

2. 创建基础项目说明

   > Spring官方提供了非常方便的工具让我们快速构建应用
   >
   > Spring Initializr： https://start.spring.io/ 
   >
   > **项目创建方式一：**使用Spring Initializr 的 Web页面创建项目 
   >
   > 1.  打开  https://start.spring.io/ 
   > 2.  填写项目信息 
   > 3.  点击”Generate Project“按钮生成项目；下载此项目 
   > 4.  解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕。 
   > 5.  如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。 
   >
   > **项目创建方式二：**使用 IDEA 直接创建项目 
   >
   > 1.  创建一个新项目 
   > 2.  选择spring initalizr ， 可以看到默认就是去官网的快速构建工具那里实现 
   > 3.  填写项目信息 
   > 4.  选择初始化的组件（初学勾选 Web 即可） 
   > 5.  填写项目路径 
   > 6.  等待项目构建成功 
   >
   > **项目结构分析：** 
   >
   > 通过上面步骤完成了基础项目的创建。就会自动生成以下文件。 
   >
   > 1.  程序的主启动类 
   > 2.  一个 application.properties 配置文件 
   > 3.  一个 测试类 
   > 4.  一个 pom.xml 

3. pom.xml分析

   > 打开pom.xml，看看Spring Boot项目的依赖： 

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
   	<modelVersion>4.0.0</modelVersion>
   	<parent>
   		<groupId>org.springframework.boot</groupId>
   		<artifactId>spring-boot-starter-parent</artifactId>
   		<version>2.1.15.RELEASE</version>
   		<relativePath/> <!-- lookup parent from repository -->
   	</parent>
   	<groupId>com.kuang</groupId>
   	<artifactId>helloword</artifactId>
   	<version>0.0.1-SNAPSHOT</version>
   	<name>helloword</name>
   	<description>Demo project for Spring Boot</description>
   	<properties>
   		<java.version>1.8</java.version>
   	</properties>
   	<dependencies>
   
   		<dependency>
   			<groupId>org.springframework.boot</groupId>
   			<artifactId>spring-boot-starter</artifactId>
   		</dependency>
   
   		<!--spring-boot-starter：所有的springboot依赖都是使用这个开头的-->
   
   		<!--单元测试-->
   		<dependency>
   			<groupId>org.springframework.boot</groupId>
   			<artifactId>spring-boot-starter-test</artifactId>
   			<scope>test</scope>
   		</dependency>
   		<!--web依赖：tomcat，dispatcherServlet，xml-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
               <version>RELEASE</version>
               <scope>compile</scope>
           </dependency>
   		<dependency>
   			<groupId>org.junit.jupiter</groupId>
   			<artifactId>junit-jupiter-api</artifactId>
   			<version>5.7.0</version>
   			<scope>test</scope>
   		</dependency>
   	</dependencies>
   
   	<build>
   		<!--打jar包插件-->
   		<plugins>
   			<plugin>
   				<groupId>org.springframework.boot</groupId>
   				<artifactId>spring-boot-maven-plugin</artifactId>
   			</plugin>
   		</plugins>
   	</build>
   
   </project>
   ```

4. 编写一个http接口

   > 1. 在主程序的同级目录下，新建一个controller包，一定要在同级目录下，否则识别不到 
   > 2. 在包中新建一个HelloController类 
   >
   > ```java
   > @RestController
   > public class HelloController {
   > 
   >     // 接口：http://localhost:8080/hello
   >     @RequestMapping("/hello")
   >     public String hello() {
   >         //调用业务，接受前端的参数
   >         return "hello,World";
   >     }
   > }
   > ```
   >
   > 3. 编写完毕后，从主程序启动项目，浏览器发起请求，看页面返回；控制台输出了 Tomcat 访问的端口号！ 
   >
   > ![1616757651372](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616757651372.png)
   >
   > 简单几步，就完成了一个web接口的开发，SpringBoot就是这么简单。所以我们常用它来建立我们的微服务项目！ 

5. 将项目打成jar包，点击maven的package

   > ![1616757773387](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616757773387.png)

6. 了解

   > 1. 可以在 application.properties 中修改端口号
   >
   >    ```properties
   >    # 更改项目端口号
   >    server.port=8081
   >    ```
   >
   > 2. banner.txt 放在application.properties同级目录下可以产生意想不到的效果

## 运行原理探究

### pom.xml

1. 父依赖

   > 其中它主要是依赖一个父项目，主要是管理项目的资源过滤及插件！
   >
   > ```xml
   >     <parent>
   >         <groupId>org.springframework.boot</groupId>
   >         <artifactId>spring-boot-starter-parent</artifactId>
   >         <version>2.4.4</version>
   >         <relativePath/> <!-- lookup parent from repository -->
   >     </parent>
   > ```
   >
   > 点进去，发现还有一个父依赖
   >
   > ```xml
   >   <parent>
   >     <groupId>org.springframework.boot</groupId>
   >     <artifactId>spring-boot-dependencies</artifactId>
   >     <version>2.4.4</version>
   >   </parent>
   > ```
   >
   > 这里才是真正管理SpringBoot应用里面所有依赖版本的地方，SpringBoot的版本控制中心；
   >
   > 以后我们导入依赖默认是不需要写版本的，但是如果导入的包没有在依赖中管理着就需要手动配置了；

2. 启动器 spring-boot-starter

   > ```xml
   > <dependency>
   >             <groupId>org.springframework.boot</groupId>
   >             <artifactId>spring-boot-starter</artifactId>
   >         </dependency>
   > ```
   >
   > springboot-boot-starter-xxx：就是spring-boot的场景启动器
   >
   > spring-boot-starter-web：帮我们导入了web模块正常运行所依赖的组件；
   >
   > SpringBoot将所有的功能场景都抽取出来，做成一个个的starter（启动器），只需要在项目中引入这些starter即可，所有相关的以来都会导入进来，我们要用什么功能就导入什么样的场景启动器即可，我们未来也将而已自己定义starter；

### 主启动类

1. 默认的主启动类

   > ```java
   > //@SpringBootApplication：标注一个主程序类
   > // 说明这个类是一个springboot应用
   > @SpringBootApplication
   > public class Springboot01Application {
   > 
   >     public static void main(String[] args) {
   >         //将springboot应用启动
   >         //以为是一个启动方法，没想到启动了一个服务
   >         SpringApplication.run(Springboot01Application.class, args);
   >     }
   > 
   > }
   > ```
   >
   > 但是一个简单的启动类并不简单！我们来分析一下这些注解都干了什么。

2. @SpringBootApplication

   > 作用：标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；
   >
   > 进入这个注解，可以看到上面还有很多其他注解！
   >
   > ```java
   > @SpringBootConfiguration
   > @EnableAutoConfiguration
   > @ComponentScan(
   >     excludeFilters = {@Filter(
   >     type = FilterType.CUSTOM,
   >     classes = {TypeExcludeFilter.class}
   > ), @Filter(
   >     type = FilterType.CUSTOM,
   >     classes = {AutoConfigurationExcludeFilter.class}
   > )}
   > )
   > public @interface SpringBootApplication {
   >     // ....
   > }
   > ```

3. @ComponentScan

   > 这个注解在Spring中很重要，它对应XML配置中的元素。
   >
   > 作用：自动扫描并加载符合条件的组件或者bean，将这个bean定义加载到IOC容器中

4. @SpringBootConfiguration

   > 作用：SpringBoot的配置类，标注在某个类上，表示这是一个SpringBoot的配置类；
   >
   > 我们继续进去这个注解查看
   >
   > ```java
   > @Configuration
   > public @interface SpringBootConfiguration {
   >     @AliasFor(
   >         annotation = Configuration.class
   >     )
   >     boolean proxyBeanMethods() default true;
   > }
   > ```
   >
   > 这里的@Configuration，说明这是一个配置类，配置类就是对应Spring的xml配置文件；
   >
   > 继续进入@Configuration
   >
   > ```java
   > @Component
   > public @interface Configuration {
   >     @AliasFor(
   >         annotation = Component.class
   >     )
   >     String value() default "";
   > 
   >     boolean proxyBeanMethods() default true;
   > }
   > ```
   >
   > 这就说明，启动类本身也是Spring中的一个组件而已，负责启动应用！
   >
   > 我们回到 SpringBootApplication 注解中继续查看。

5. @EnableAutoConfiguration

   > **@EnableAutoConfiguration：开启自动配置功能**
   >
   > 以前我们需要自己配置的东西，而现在SpringBoot可以自动帮我们配置；
   >
   > @EnableAutoConfiguration告诉SpringBoot开启自动配置功能，这样自动配置才能生效；
   >
   > 点进注解继续查看：
   >
   > 
   >
   > **@AutoConfigurationPackage：自动配置包**
   >
   > 点进注解查看
   >
   > ```java
   > @Import({Registrar.class})
   > public @interface AutoConfigurationPackage {
   >     String[] basePackages() default {};
   > 
   >     Class<?>[] basePackageClasses() default {};
   > }
   > ```
   >
   > **@Import：**Spring底层注解@import，给容器中导入一个组件
   >
   > Registrar.class作用：将主启动类的所在包及包下面所有子包里面的所有组件扫描到Spring容器；
   >
   > 这个分析完了，退到上一步，继续看
   >
   > 
   >
   > **@Import({AutoConfigurationImportSelector.class})：给容器导入组件；**
   >
   > AutoConfigurationImportSelector：自动配置导入选择器，那么他会导入哪些组建的选择器呢？我们点进去这个类看源码：
   >
   > 1. 这个类中有一个这样的方法
   >
   >    ```java
   >    // 获取候选的配置
   >    protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
   >        
   >        // 这里的 getSpringFactoriesLoaderFactoryClass() 方法返回的就是
   >        // 我们最开始看的启动自动导入配置文件的注解类：EnableAutoConfiguration
   >        List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
   >        
   >        Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
   >        
   >        return configurations;
   >    }
   >    ```
   >
   > 2. 这个方法又调用了 SpringFactoriesLoader 类的静态方法！我们进入 SpringFactoriesLoader 类loadFactoryNames() 方法 
   >
   >    ```java
   >    public static List<String> loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader) {
   >        
   >            ClassLoader classLoaderToUse = classLoader;
   >        
   >            if (classLoader == null) {
   >                classLoaderToUse = SpringFactoriesLoader.class.getClassLoader();
   >            }
   >    
   >            String factoryTypeName = factoryType.getName();
   >        	// 这里它有调用了 LoadSpringFactories 方法
   >            return (List)loadSpringFactories(classLoaderToUse).getOrDefault(factoryTypeName, Collections.emptyList());
   >        
   >    }
   >    ```
   >
   > 3.  我们继续点击查看 loadSpringFactories 方法 
   >
   >    ```java
   >    private static Map<String, List<String>> loadSpringFactories(ClassLoader classLoader) {
   >            Map<String, List<String>> result = (Map)cache.get(classLoader);
   >            if (result != null) {
   >                return result;
   >            } else {
   >                HashMap result = new HashMap();
   >    
   >                try {
   >                    Enumeration urls = classLoader.getResources("META-INF/spring.factories");
   >    
   >                    while(urls.hasMoreElements()) {
   >                        URL url = (URL)urls.nextElement();
   >                        UrlResource resource = new UrlResource(url);
   >                        Properties properties = PropertiesLoaderUtils.loadProperties(resource);
   >                        Iterator var6 = properties.entrySet().iterator();
   >    
   >                        while(var6.hasNext()) {
   >                            Entry<?, ?> entry = (Entry)var6.next();
   >                            String factoryTypeName = ((String)entry.getKey()).trim();
   >                            String[] factoryImplementationNames = StringUtils.commaDelimitedListToStringArray((String)entry.getValue());
   >                            String[] var10 = factoryImplementationNames;
   >                            int var11 = factoryImplementationNames.length;
   >    
   >                            for(int var12 = 0; var12 < var11; ++var12) {
   >                                String factoryImplementationName = var10[var12];
   >                                ((List)result.computeIfAbsent(factoryTypeName, (key) -> {
   >                                    return new ArrayList();
   >                                })).add(factoryImplementationName.trim());
   >                            }
   >                        }
   >                    }
   >    
   >                    result.replaceAll((factoryType, implementations) -> {
   >                        return (List)implementations.stream().distinct().collect(Collectors.collectingAndThen(Collectors.toList(), Collections::unmodifiableList));
   >                    });
   >                    cache.put(classLoader, result);
   >                    return result;
   >                } catch (IOException var14) {
   >                    throw new IllegalArgumentException("Unable to load factories from location [META-INF/spring.factories]", var14);
   >                }
   >            }
   >    }
   >    ```
   >
   > 4. 发现一个多次出现的文件：spring.factories，全局搜索它
   >
   >    ![1616763525533](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616763525533.png)

6. spring.factories

   > 我们根据源头打开spring.factories，看到了很多自动配置的文件；这就是自动配置的根源所在！
   >
   > ![1616765258499](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616765258499.png)
   >
   > 
   >
   > **WebMvcAutoConfiguration**
   >
   > 我们在上面的自动配置类随便找一个打开看看，比如：WebMvcAutoConfiguration
   >
   > ![1616765675146](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616765675146.png)
   >
   > 可以看到这些一个个的都是JavaConfig配置类，而且都注入一些Bean，可以找一些自己认识的类，看着熟悉一下！
   >
   > 所以，自动配置真正实现是从classpath中搜寻所有的 META-INF/spring.factories配置文件，并将其中对应的 org.springframework.boot.autoconfigure  包下的配置项，通过反射实例化为对应标注了 
   >
   > @Configuration 的 JavaConfig 形式的 IOC 容器配置类，然后将这些都汇总为一个实例并加载到 IOC 容器中。
   >
   > 
   >
   > **结论：**
   >
   > 1. SpringBoot 在启动的时候从类路径下的 META-INF/spring.factories 中获取 EnableAutoConfiguration指定的值。
   > 2. 将这些值作为自动配置类导入容器，自动配置类就会生效，帮我们进行配置工作；以前我们需要手动配置的东西，现在springboot帮我们做了。
   > 3. 整个javaEE的整体解决方案和自动配置都在 spring-boot-autoconfigure-2.4.4.jar 这个包中；
   > 4. 它会把所有需要导入的组件，以类名的方式返回，这些组件就会被添加到容器中。
   > 5. 容器中也会存在非常多的xxxAutoConfiguration的文件（@Bean），就是这些类给容器中导入了这个场景需要的所有组件，并自动配置，@Configuration，JavaConfig！
   > 6. 有了自动配置类，免去了我们手动编写配置文件的工作！

### SpringApplication

1. 不简单的方法

   > 我最初以为就是运行了一个main方法，没想到却开启了一个服务；
   >
   > ```java
   > //@SpringBootApplication：标注这个类是一个springboot的应用
   > @SpringBootApplication
   > public class Springboot01Application {
   > 
   >     public static void main(String[] args) {
   >         //将springboot应用启动
   >         SpringApplication.run(Springboot01Application.class, args);
   >     }
   > 
   > }
   > ```
   >
   > **SpringApplication.run 分析：**
   >
   > 分析该方法主要分两部分，一部分是 SpringApplication 的实例化，二是 run 方法的执行；

2. SpringApplication

   > **这个类主要做了以下四件事情：**
   >
   > 1. 推断应用的类型是普通项目还是Web项目
   > 2. 查找并加载所有的可用初始化器，设置到initializers属性中
   > 3. 找出所有的应用程序监听器，设置到listeners属性中
   > 4. 推断并设置main方法的定义类，找到运行的主类
   >
   > 查看构造器：
   >
   > ```java
   >  public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
   >      
   >      // ....
   >      
   > 		// 推断应用的类型是普通项目还是Web项目
   >      this.webApplicationType = WebApplicationType.deduceFromClasspath();
   >     	// 查找并加载所有的可用初始化器，设置到initializers属性中
   >      this.setInitializers(this.getSpringFactoriesInstances(ApplicationContextInitializer.class));
   >      // 找出所有的应用程序监听器，设置到listeners属性中
   >      this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
   >      // 推断并设置main方法的定义类，找到运行的主类
   >      this.mainApplicationClass = this.deduceMainApplicationClass();
   >  }
   > ```
   >
   > ![](D:\桌面\001\idea笔记\idea图片\mybatis\run方法流程分析.jpg)
   >
   > 关于SpringBoot，谈谈你的理解：
   >
   > * 自动装配
   > * run()
   >
   > 
   >
   > **全面接管SpringMVC()**

## yaml语法学习

1. 配置文件

   > SpringBoot使用一个全局的配置文件，配置文件名称是固定的
   >
   > * application.properties （properties只能存储键值对）
   >   * 语法结构：key=value
   > * application.yaml
   >   * 语法结构：key: 空格 value
   >
   > **配置文件的作用：**修改SpringBoot自动配置的默认值，因为SpringBoot在底层都给我们自动装配好了；
   >
   > 比如我们可以在配置文件中修改Tomcat默认启动的端口号！
   >
   > ```yaml
   > server.port=8081
   > ```

2. yaml概述

   > YAML 是 "YAML Ain't a Markup Language" （YAML不是一种标记语言）的递归缩写。 在开发这种语言时，YAML的意思其实是： "Yet Another Markup Language"（仍是一种标记语言） 
   >
   > 
   >
   > **这种语言以数据作为中心，而不是以标记语言为重点！**
   >
   > 
   >
   > 以前的配置文件，大多数都是用 xml 来配置的；比如一个简单的端口配置，我们来对比下 yaml 和 xml
   >
   > 传统 xml 配置：
   >
   > ```xml
   > <server>
   > 	<port>8081</port>
   > </server>
   > ```
   >
   > yaml 配置：
   >
   > ```yaml
   > server:
   >   port: 8081
   > ```

3. yaml 基础语法

   > 说明：语法要求严格
   >
   > 1. 空格不能省略
   > 2. 以缩进来控制层级关系，只要是左边对齐的一列数据都是同一个层级的。
   > 3. 属性和值的大小写都是十分敏感的。
   >
   > 
   >
   > **字面量：普通的值 [数字，布尔值，字符串]**
   >
   > 字面量直接写在后面就可以，字符串默认不用加上双引号或者单引号；
   >
   > ```yaml
   > k: v
   > ```
   >
   > 注意：
   >
   > * " " 双引号，不会转义字符串里面的特殊字符，特殊字符会作为本身想表示的意思；
   >
   >   比如：name: "kuang \n shen"  输出：kuang  换行  shen
   >
   > * ' ' 单引号，会转义特殊字符，特殊字符最终会变成和普通字符一样输出
   >
   >   比如：name: 'kuang \n shen'  输出：kuang  \n  shen
   >
   > 
   >
   > **对象、Map（键值对）**
   >
   > ```yaml
   > # 对象、Map格式
   > k:
   >   v1: 
   >   v2:
   > ```
   >
   > 在下一行来写对象的属性和值的关系，注意缩进；比如：
   >
   > ```yaml
   > student:
   >   name: qinjiang
   >   age: 3
   > ```
   >
   > 行内写法
   >
   > ```yaml
   > student1: {name: qinjiang,age: 3}
   > ```
   >
   > 
   >
   > **数组（List、set）**
   >
   > 用 - 值表示数组中的一个元素，比如：
   >
   > ```yaml
   > pets:
   >   - cat
   >   - dog
   >   - pig
   > ```
   >
   > 行内写法
   >
   > ```yaml
   > pets1: [cat,dog,pig]
   > ```
   >
   > 
   >
   > **修改SpringBoot的默认端口号**
   >
   > 配置文件中添加，端口号的参数，就可以切换端口
   >
   > ```yaml
   > server:
   >   port: 8081
   > ```

## 注入配置文件

yaml 文件更强大的地方在于，它可以给我们的实体类直接注入匹配值！

1. yaml 注入配置文件

> 1. 在 springboot 项目中的 resources 目录下新建一个文件application.yaml
>
> 2. 编写一个实体类 Dog；
>
>    ```java
>    @Component // 注册 bean 到容器中
>    public class Dog {
>    
>        private String name;
>        private Integer age;
>        
>        // 有参无参构造、get、set方法、toString()方法
>    }
>    ```
>
> 3. 思考，我们原来是如何给 bean 注入属性值的！@Value，给狗狗类测试一下：
>
>    ```java
>    @Component // 注册 bean 到容器中
>    public class Dog {
>        @Value("旺财")
>        private String name;
>        @Value("3")
>        private Integer age;
>    }
>    ```
>
> 4. 在 SpringBoot 的测试类下注入狗狗输出一下；
>
>    ```java
>    @SpringBootTest
>    class Springboot02ConfigApplicationTests {
>    
>        @Autowired
>        private Dog dog;
>    
>        @Test
>        void contextLoads() {
>            System.out.println(dog);
>        }
>    
>    }
>    ```
>
>    结果成功输出，@Value注入成功，这是我们原来的办法。
>
>    ![1616817964288](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616817964288.png)
>
> 5. 我们在编写一个复杂一点的实体类：Person 类
>
>    ```java
>    @Component // 注册 bean 到容器
>    public class Person {
>    
>        private String name;
>        private Integer age;
>        private Boolean happy;
>        private Date birth;
>        private Map<String,Object> maps;
>        private List<Object> lists;
>        private Dog dog;
>        
>        //有参无参构造、get、set方法、toString()方法
>    }
>    ```
>
> 6. 我们来使用 yaml 配置的方式进行注入，写的时候要注意区别和优势。
>
>    ```yaml
>    person:
>      name: qinjiang
>      age: 33
>      happy: false
>      birth: 2000/01/01
>      maps: {k1: v1,k2: v2}
>      lists: 
>        - code
>        - girl
>        - music
>      dog:
>        name: 旺财
>        age: 1
>    ```
>
> 7. 把值注入到 Person 类当中！
>
>    ```java
>    /*
>        @ConfigurationProperties 作用：
>        将配置文件中配置的每一个属性的值，映射到这个组件中；
>        告诉SpringBoot本类中所有属性和配置文件中相关的配置进行绑定
>        参数 prefix = "person"：将配置文件中的 person 下面的所有属性一一对应
>     */
>    @Component // 注册bean
>    @ConfigurationProperties(prefix = "person")
>    public class Person {
>    
>        private String name;
>        private Integer age;
>        private Boolean happy;
>        private Date birth;
>        private Map<String,Object> maps;
>        private List<Object> lists;
>        private Dog dog;
>    }
>    ```
>
> 8. IDEA 提示：springboot 配置注解处理器没有找到，让我们看文档，我们可以查看文档，找到一个依赖。
>
>    ![1616818762715](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616818762715.png)
>
>    ![1616818819274](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616818819274.png)
>
> 9. 确认以上配置都OK后，我们去测试类中测试一下：
>
>    ```java
>    @SpringBootTest
>    class Springboot02ConfigApplicationTests {
>    
>        @Autowired
>        private Person person;
>    
>        @Test
>        void contextLoads() {
>            System.out.println(person);
>        }
>    
>    }
>    ```
>
>    结果：所有值全部注入成功！
>
>    ![1616819047368](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616819047368.png)
>
>    **yaml 配置注入到实体类完全ok**

2. 加载指定的配置文件

   > **@PropertySource：**加载指定的配置文件
   >
   > **@configurationProperties：**默认从全局配置文件中获取值；
   >
   > 1. 我们去在 resources 目录下新建一个 **person.properties** 文件
   >
   >    ```properties
   >    name=kuangshen
   >    ```
   >
   > 2. 然后在我们的代码中指定加载person.properties文件
   >
   >    ```java
   >    @PropertySource(value = "classpath:person.properties")
   >    @Component //注册bean
   >    public class Person {
   >    
   >        @Value("${name}")
   >        private String name;
   >    
   >        ......  
   >    }
   >    ```
   >
   > 3. 再次输出测试一下：指定配置文件绑定成功！

3. 配置文件占位符

   > 配置文件还可以编写占位符生成随机数
   >
   > ```yaml
   > person:
   >     name: qinjiang${random.uuid} # 随机uuid
   >     age: ${random.int}  # 随机int
   >     happy: false
   >     birth: 2000/01/01
   >     maps: {k1: v1,k2: v2}
   >     lists:
   >       - code
   >       - girl
   >       - music
   >     dog:
   >       name: ${person.hello:other}_旺财
   >       age: 1
   > ```

4. 回顾 properties 

   > 我们上面采用的yaml方法都是最简单的方式，开发中最常用的；也是springboot所推荐的！那我们来唠唠其他的实现方式，道理都是相同的；写还是那样写；配置文件除了yml还有我们之前常用的properties ， 我们没有讲，我们来唠唠！ 
   >
   > 【注意】properties配置文件在写中文的时候，会有乱码 ， 我们需要去IDEA中设置编码格式为UTF-8； 
   >
   > settings-->FileEncodings 中配置； 
   >
   > ![1616821060528](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616821060528.png)
   >
   > **测试步骤：** 
   >
   > 1. 新建一个实体类User 
   >
   >    ```java
   >    @Component //注册bean
   >    public class User {
   >        private String name;
   >        private int age;
   >        private String sex;
   >    }
   >    ```
   >
   > 2. 编辑配置文件 user.properties 
   >
   >    ```properties
   >    user1.name=kuangshen
   >    user1.age=18
   >    user1.sex=男
   >    ```
   >
   > 3. 我们在User类上使用@Value来进行注入！ 
   >
   >    ```java
   >    @Component //注册bean
   >    @PropertySource(value = "classpath:user.properties")
   >    public class User {
   >        //直接使用@value
   >        @Value("${user.name}") //从配置文件中取值
   >        private String name;
   >        @Value("#{9*2}")  // #{SPEL} Spring表达式
   >        private int age;
   >        @Value("男")  // 字面量
   >        private String sex;
   >    }
   >    ```
   >
   > 4. Springboot测试 
   >
   >    ```java
   >    @SpringBootTest
   >    class DemoApplicationTests {
   >    
   >        @Autowired
   >        User user;
   >    
   >        @Test
   >        public void contextLoads() {
   >            System.out.println(user);
   >        }
   >    
   >    }
   >    ```
   >
   >    结果输出正常

5. 对比小结

   >  @Value这个使用起来并不友好！我们需要为每个属性单独注解赋值，比较麻烦；我们来看个功能对比图 
   >
   > ![1616821224831](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616821224831.png)
   >
   > 1. @ConfigurationProperties只需要写一次即可 ， @Value则需要每个字段都添加 
   > 2. 松散绑定：这个什么意思呢? 比如我的yml中写的last-name，这个和lastName是一样的， - 后面跟着的字母默认是大写的。这就是松散绑定。可以测试一下 
   > 3. JSR303数据校验 ， 这个就是我们可以在字段是增加一层过滤器验证 ， 可以保证数据的合法性 
   > 4. 复杂类型封装，yml中可以封装对象 ， 使用value就不支持 
   >
   > **结论：** 
   >
   > 配置yml和配置properties都可以获取到值 ， 强烈推荐 yml；
   >
   > 如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value；
   >
   > 如果说，我们专门编写了一个JavaBean来和配置文件进行一一映射，就直接@configurationProperties，不要犹豫！

## JSR303数据校验

1. 先看看如何使用

   > SpringBoot 中可以用 @validated 来校验数据，如果数据异常则会统一抛出异常，方便异常中心统一处理。我们这里来写个注解让我们的 name 只能支持 Email 格式；
   >
   > ```java
   > @Component // 注册bean放入容器
   > @ConfigurationProperties(prefix = "person")
   > @Validated // 数据校验
   > public class Person {
   > 
   >     @Email(message = "邮箱格式错误") // name必须是邮箱格式
   >     private String name;
   > }
   > ```
   >
   > ![1616830867227](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616830867227.png)
   >
   > 使用数据校验，可以保证数据的正确性；

2. 常见参数

   > ```java
   > @NotNull(message="名字不能为空")
   > private String userName;
   > @Max(value=120,message="年龄最大不能查过120")
   > private int age;
   > @Email(message="邮箱格式错误")
   > private String email;
   > 
   > /*
   > 空检查
   > @Null       验证对象是否为null
   > @NotNull    验证对象是否不为null, 无法查检长度为0的字符串
   > @NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
   > @NotEmpty   检查约束元素是否为NULL或者是EMPTY.
   >     
   > Booelan检查
   > @AssertTrue     验证 Boolean 对象是否为 true  
   > @AssertFalse    验证 Boolean 对象是否为 false
   >     
   > 长度检查
   > @Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
   > @Length(min=, max=) string is between min and max included.
   >     
   > 日期检查
   > @Past       验证 Date 和 Calendar 对象是否在当前时间之前  
   > @Future     验证 Date 和 Calendar 对象是否在当前时间之后  
   > @Pattern    验证 String 对象是否符合正则表达式的规则
   >     
   > .......等等
   > 除此以外，我们还可以自定义一些数据校验规则
   > */ 
   > ```

## 多环境切换

profile 是 Spring 对不同环境提供不同功能的支持，可以通过激活不同的环境版本，实现快速切换环境；

1. 多配置文件

   > 我们在主配置文件编写的时候，文件名可以是application-{profile}.properties/yaml，用来指定多个环境版本；
   >
   > 
   >
   > **例如：**
   >
   > application-test.properties  代表测试环境配置
   >
   > application-dev.properties   代表开发环境配置
   >
   > 但是SpringBoot并不会直接启动这些配置文件，它**默认使用application.properties主配置文件；**
   >
   > 我们需要通过一个配置来选择需要激活的环境：
   >
   > ```properties
   > # 比如在配置文件中指定使用 dev 环境，我们可以通过设置不同的端口号进行测试：
   > # 我们启动SpringBoot，就可以看到已经切换到 dev 下的配置了：
   > spring.profiles.active=dev
   > ```

2. yaml 的多文档块

   > 和 properties 配置文件中一样，但是使用yaml去实现不需要创建多个配置文件更加方便了！
   >
   > ```yaml
   > server:
   >   port: 8081
   > #选择要激活那个环境块
   > spring:
   >   profiles:
   >     active: prod
   > 
   > ---
   > server:
   >   port: 8083
   > spring:
   >   profiles: dev #配置环境的名称
   > 
   > 
   > ---
   > 
   > server:
   >   port: 8084
   > spring:
   >   profiles: prod  #配置环境的名称
   > ```
   >
   > **注意：如果 yml 和 properties 同时都配置了端口，并且没有激活其他环境，默认会使用 properties 配置文件的！**

3. 配置文件加载位置

   > **外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！**
   >
   > 官方外部配置文件说明参考文档
   >
   > ![1616833464724](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616833464724.png)
   >
   > springboot 启动时会扫描以下位置的 application.properties 或者 application.yaml 文件作为Springboot 的默认配置文件：
   >
   > ```
   > 优先级1：项目路径下的config文件夹配置文件
   > 优先级2：项目路径下配置文件
   > 优先级3：资源路径下的config文件夹配置文件
   > 优先级4：资源路径下配置文件
   > ```
   >
   > 优先级由高到低，高优先级的配置会覆盖低优先级的配置；
   >
   > **SpringBoot 会从这四个位置全部加载主配置文件；互补配置；**
   >
   > 我们在最低级的配置文件中设置一个项目访问路径的配置来测试互补问题；
   >
   > ```yaml
   > #配置项目的访问路径
   > server.servlet.context-path=/kuang
   > ```

## 自动配置原理

配置文件中到底能写什么？怎么写？

SpringBoot官方文档中有大量的配置，我们无法全部记住

![1616834391868](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616834391868.png)

1. 分析自动配置原理

   > 我们以 **HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理；
   >
   > ```java
   > //
   > // Source code recreated from a .class file by IntelliJ IDEA
   > // (powered by FernFlower decompiler)
   > //
   > 
   > package org.springframework.boot.autoconfigure.web.servlet;
   > 
   > import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
   > import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
   > import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
   > import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication;
   > import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication.Type;
   > import org.springframework.boot.autoconfigure.web.ServerProperties;
   > import org.springframework.boot.context.properties.EnableConfigurationProperties;
   > import org.springframework.boot.web.server.WebServerFactoryCustomizer;
   > import org.springframework.boot.web.servlet.filter.OrderedCharacterEncodingFilter;
   > import org.springframework.boot.web.servlet.server.ConfigurableServletWebServerFactory;
   > import org.springframework.boot.web.servlet.server.Encoding;
   > import org.springframework.context.annotation.Bean;
   > import org.springframework.context.annotation.Configuration;
   > import org.springframework.core.Ordered;
   > import org.springframework.web.filter.CharacterEncodingFilter;
   > 
   > // 表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；
   > @Configuration(proxyBeanMethods = false)
   > 
   > // 启动指定类的 ConfigurationProperties 功能；
   > 	// 进入这个 ServerProperties 查看，将配置文件中对应的值和 ServerProperties 绑定起来；
   > 	// 并把 ServerProperties 加入到 ioc 容器中
   > @EnableConfigurationProperties({ServerProperties.class})
   > 
   > //Spring 底层 @Conditional 注解
   > 	// 根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
   > 	// 这里的意思就是判断当前应用是否是web应用，如果是，当前配置生效
   > @ConditionalOnWebApplication(
   >     type = Type.SERVLET
   > )
   > 
   > // 判断当前项目有没有这个类 CharacterEncodingFilter：SpringMVC 中进行乱码解决的过滤器
   > @ConditionalOnClass({CharacterEncodingFilter.class})
   > 
   > // 判断配置文件中是否存在某个配置：server.servlet.encoding.enable;
   > 	// 如果不存在，判断也是成立的
   > 	// 即使我们配置文件中不配置server.servlet.encoding.enable=true，也是默认生效的。
   > @ConditionalOnProperty(
   >     prefix = "server.servlet.encoding",
   >     value = {"enabled"},
   >     matchIfMissing = true
   > )
   > public class HttpEncodingAutoConfiguration {
   >     // 他已经和 SpringBoot 的配置文件映射了
   >     private final Encoding properties;
   > 	// 只有一个有参构造的情况下，参数的值就会从容器中拿
   >     public HttpEncodingAutoConfiguration(ServerProperties properties) {
   >         this.properties = properties.getServlet().getEncoding();
   >     }
   > 	
   >     // 给容器中添加一个组件，这个组件的某些值需要从 properties 中获取
   >     @Bean
   >     @ConditionalOnMissingBean // 判断容器中没有这个组件？
   >     public CharacterEncodingFilter characterEncodingFilter() {
   >         CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
   >         filter.setEncoding(this.properties.getCharset().name());
   >         filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.web.servlet.server.Encoding.Type.REQUEST));
   >         filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.web.servlet.server.Encoding.Type.RESPONSE));
   >         return filter;
   >     }
   > 
   >     @Bean
   >     public HttpEncodingAutoConfiguration.LocaleCharsetMappingsCustomizer localeCharsetMappingsCustomizer() {
   >         return new HttpEncodingAutoConfiguration.LocaleCharsetMappingsCustomizer(this.properties);
   >     }
   > 
   >     static class LocaleCharsetMappingsCustomizer implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory>, Ordered {
   >         private final Encoding properties;
   > 
   >         LocaleCharsetMappingsCustomizer(Encoding properties) {
   >             this.properties = properties;
   >         }
   > 
   >         public void customize(ConfigurableServletWebServerFactory factory) {
   >             if (this.properties.getMapping() != null) {
   >                 factory.setLocaleCharsetMappings(this.properties.getMapping());
   >             }
   > 
   >         }
   > 
   >         public int getOrder() {
   >             return 0;
   >         }
   >     }
   > }
   > 
   > ```
   >
   > **一句话总结：根据当前不同的条件判断，决定这个配置类是否生效！**
   >
   > * 一旦这个配置类生效，这个配置类就会给容器中添加各种组件；
   > * 这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；
   > * 所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；
   > * 配置文件能配置什么就可以参照某个功能对应的属性类
   >
   > ```java
   > @ConfigurationProperties(
   >     prefix = "server",
   >     ignoreUnknownFields = true
   > )
   > public class ServerProperties {
   >     // .....
   > }
   > ```
   >
   > 我们去配置文件里面试试前缀。
   >
   > ![1616837953410](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616837953410.png)
   >
   > **这就是自动装配的原理！**

2. **精髓**

   > 1. SpringBoot 启动会加载大量的自动配置类。
   > 2. 我们看我们需要的功能有没有在 SpringBoot 默认写好的自动配置类当中；
   > 3. 我们再来看这个自动配置类中到底配置了那些组件；（只要我们要用的组件存在其中，我们就不需要在手动配置了）
   > 4. 给容器中自动配置类添加组件的时候，会从 properties 类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；
   >
   >  **xxxxAutoConfigurartion：自动配置类；**给容器中添加组件 
   >
   >  **xxxxProperties:封装配置文件中相关属性；** 
   >
   > ![1616839382667](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616839382667.png)

3. 了解：@Conditional

   > 了解完自动装配的原理后，我们来关注一个细节问题，**自动配置类必须在一定的条件下才能生效；**
   >
   > **@Conditional派生注解（Spring注解版原生的@Conditional作用）** 
   >
   > 作用：必须是 @Conditional 指定的条件成立，才能给容器中添加组件，配置里面的所有内容才能生效；
   >
   > ![1616838502278](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616838502278.png)
   >
   > **那么多的自动配置类，必须在一定的条件下才能生效；也就是说，我们加载了这么多的配置类，但不是所有的都生效了。**
   >
   > 
   >
   > 我们怎么知道哪些自动配置类生效？
   >
   > **我们可以通过启动 debug=true 属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；**
   >
   > ```properties
   > #开启springboot的调试类
   > debug=true
   > ```
   >
   > **Positive matches:（自动配置类启用的：正匹配）**
   >
   > **Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配）**
   >
   > **Unconditional classes: （没有条件的类）**

## SpringBoot Web 开发

要解决的问题：

* 导入静态资源....
* 首页
* jsp，模板引擎 Thymeleaf
* 装配扩展 SpringMVC
* 增删改查
* 拦截器
* 国际化！

### 静态资源

```java
protected void addResourceHandlers(ResourceHandlerRegistry registry) {
    super.addResourceHandlers(registry);
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
    } else {
        ServletContext servletContext = this.getServletContext();
        this.addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
        this.addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
            registration.addResourceLocations(this.resourceProperties.getStaticLocations());
            if (servletContext != null) {
                registration.addResourceLocations(new Resource[]{new ServletContextResource(servletContext, "/")});
            }

        });
    }
}
```

总结：

1. 在 springboot，我们可以使用以下方式处理静态资源
   - webjars                  `localhost:8080/webjars/`
   - public、static、/**、resources         `localhost:8080/`
2. 优先级：resources > static > public

### 首页如何定制

### 模板引擎

结论，只需要使用thymeleaf，只需要导入对应的依赖就可以了！我们将html页面放在我们的templates目录下即可。

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--所有的html元素，都有可能被 thymeleaf 接管：th: 元素名-->
<h1>test</h1>
<div th:text="${msg}"></div>
<div th:utext="${msg}"></div>
<br>
<h3 th:each="user:${users}" th:text="${user}"></h3>
</body>
</html>
```

```java
// 在templates目录下的所有页面，只能通过controller来跳转！
// 这个需要模板引擎的支持： thymeleaf
@Controller
public class IndexController {

    @RequestMapping("/test")
    public String index(Model model) {
        model.addAttribute("msg","<h1>hello,springboot</h1>");

        model.addAttribute("users", Arrays.asList("qinjiang","kuangshen"));
        return "test";
    }
}
```

### 装配扩展视图解析器

```java
package com.kuang.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.View;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.Locale;
// 如果，你想div一些定制化的功能，只要写这个组件，然后将它交给springboot，springboot就会帮我们自动装配！
// 扩展 springmvc         dispatherservlet
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    // public interface ViewResolver  实现了视图解析接口的类，我们就可以把它看作视图解析器。

    @Bean
    public ViewResolver myViewResolver() {
        return new MyViewResolver();
    }


    // 自定义了一个自己的视图解析器 MyViewResolver
    public static class MyViewResolver implements ViewResolver{
        @Override
        public View resolveViewName(String s, Locale locale) throws Exception {
            return null;
        }
    }
}
```

在springboot中，有非常多的 xxxx Configuration 会帮助我们进行扩展配置，只要看见了这个东西，我们就要注意了！

```java
// 如果我们要扩展springmvc，官方建议我们这样去做！
@Configuration
@EnableWebMvc // 这玩意就是导入了一个类：DelegatingWebMvcConfiguration：从容器中，获取所有的WebMvcConfig；
public class MyMvcConfig implements WebMvcConfigurer {

    // 视图跳转
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/kuang").setViewName("success");
    }
}
```

### 低配版员工管理系统（无数据库）

1. idea代码页面

   ![1617270647275](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617270647275.png)

2. 代码：

   1. LoginHandlerInterceptor

      ```java
      package com.kuang.config;
      
      import org.springframework.web.servlet.HandlerInterceptor;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      
      public class LoginHandlerInterceptor implements HandlerInterceptor {
      
          @Override
          public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
      
              //登录成功之后，应该有用户的session
              Object loginUser = request.getSession().getAttribute("loginUser");
      
              if (loginUser == null) {
                  request.setAttribute("msg","没有权限，请先登录");
                  request.getRequestDispatcher("/index.html").forward(request,response);
                  return false;
              } else {
                  return true;
              }
          }
      
      }
      ```

   2. MyMvcConfig

      ```java
      package com.kuang.config;
      
      import org.springframework.context.annotation.Configuration;
      import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
      import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
      import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
      
      @Configuration
      public class MyMvcConfig implements WebMvcConfigurer {
      
          @Override
          public void addViewControllers(ViewControllerRegistry registry) {
              registry.addViewController("/").setViewName("index");
              registry.addViewController("/index.html").setViewName("index");
              registry.addViewController("/main.html").setViewName("dashboard");
          }
      
          @Override
          public void addInterceptors(InterceptorRegistry registry) {
              registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**").excludePathPatterns("/index.html","/","/user/login","/asserts/**");
          }
      }
      ```

   3. EmployeeController

      ```java
      package com.kuang.controller;
      
      import com.kuang.dao.DepartmentDao;
      import com.kuang.dao.EmployeeDao;
      import com.kuang.pojo.Department;
      import com.kuang.pojo.Employee;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Controller;
      import org.springframework.ui.Model;
      import org.springframework.web.bind.annotation.GetMapping;
      import org.springframework.web.bind.annotation.PathVariable;
      import org.springframework.web.bind.annotation.PostMapping;
      import org.springframework.web.bind.annotation.RequestMapping;
      import java.util.Collection;
      
      @Controller
      public class EmployeeController {
      
          @Autowired
          EmployeeDao employeeDao;
      
          @Autowired
          DepartmentDao departmentDao;
      
          // 去到员工列表页面
          @RequestMapping("/emps")
          public String list(Model model) {
              Collection<Employee> employees = employeeDao.getAll();
              model.addAttribute("emps",employees);
              return "emp/list";
          }
      
          // 去到员工的增添页面
          @GetMapping("/emp")
          public String toAddpage(Model model) {
              // 查出所有部门的信息
              Collection<Department> departments = departmentDao.getDepartments();
              model.addAttribute("departments",departments);
              return "emp/add";
          }
      
          @PostMapping("/emp")
          public String addEmp(Employee employee) {
              System.out.println(employee);
              // 添加的操作 forward
              employeeDao.save(employee); // 调用底层业务方法保存员工信息
              return "redirect:/emps";
          }
      
          // 去到员工的修改页面
          @GetMapping("/emp/{id}")
          public String toUpdateEmp(@PathVariable("id")Integer id,Model model) {
              // 查出原来的数据
              Employee employee = employeeDao.getEmployById(id);
              model.addAttribute("emp",employee);
      
              // 查出所有部门的信息
              Collection<Department> departments = departmentDao.getDepartments();
              model.addAttribute("departments",departments);
              return "emp/update";
          }
      
          @PostMapping("/updateEmp")
          public String updateEmp(Employee employee) {
              employeeDao.save(employee);
              return "redirect:/emps";
          }
      
          // 删除员工
          @GetMapping("/delemp/{id}")
          public String deleteEmp(@PathVariable("id") Integer id) {
              employeeDao.delete(id);
              return "redirect:/emps";
          }
      }
      ```

   4. LoginController

      ```java
      package com.kuang.controller;
      
      import org.springframework.stereotype.Controller;
      import org.springframework.ui.Model;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.RequestParam;
      import javax.servlet.http.HttpSession;
      
      @Controller
      public class LoginController {
      
          @RequestMapping("/user/login")
          public String login(@RequestParam("username") String username,
                              @RequestParam("password") String password,
                              Model model,
                              HttpSession session) {
              // 具体的业务
              if (!(username == null) && "123456".equals(password)) {
                  session.setAttribute("loginUser",username);
                  return "redirect:/main.html";
              } else {
                  // 告诉用户，你登陆失败
                  model.addAttribute("msg","用户名或者密码错误!");
                  return "index";
              }
          }
      
          @RequestMapping("/user/logout")
          public String logout(HttpSession session) {
              session.invalidate();
              return "redirect:/index.html";
          }
      }
      ```

   5. DepartmentDao

      ```java
      package com.kuang.dao;
      
      import com.kuang.pojo.Department;
      import org.springframework.stereotype.Repository;
      import java.util.Collection;
      import java.util.HashMap;
      import java.util.Map;
      
      // 部门：dao
      @Repository
      public class DepartmentDao {
      
          // 模拟数据库中的数据
      
          private static Map<Integer, Department> departments = null;
      
          static {
              departments = new HashMap<Integer,Department>(); // 创建了一个部门表
      
              departments.put(101,new Department(101,"教学部"));
              departments.put(102,new Department(102,"市场部"));
              departments.put(103,new Department(103,"教研部"));
              departments.put(104,new Department(104,"运营部"));
              departments.put(105,new Department(105,"后勤部"));
          }
      
          // 获得所有部门信息
          public Collection<Department> getDepartments() {
              return departments.values();
          }
      
          // 通过id得到部门
          public Department getDepartmentById(Integer id) {
              return departments.get(id);
          }
      }
      ```

   6. EmployeeDao

      ```java
      package com.kuang.dao;
      
      import com.kuang.pojo.Department;
      import com.kuang.pojo.Employee;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.stereotype.Repository;
      import java.util.Collection;
      import java.util.Date;
      import java.util.HashMap;
      import java.util.Map;
      
      // 员工Dao
      @Repository
      public class EmployeeDao {
      
          // 模拟数据库中的数据
      
          private static Map<Integer, Employee> employees = null;
          // 员工有所属的部门
      
          @Autowired
          DepartmentDao departmentDao;
      
          static {
              employees = new HashMap<Integer,Employee>(); // 创建了一个员工表
      
              employees.put(1001,new Employee(1001,"AA","2894318351@qq.com",1,new Department(101,"教学部"),new Date()));
              employees.put(1002,new Employee(1002,"BB","2894318352@qq.com",0,new Department(102,"市场部"),new Date()));
              employees.put(1003,new Employee(1003,"CC","2894318353@qq.com",1,new Department(103,"教研部"),new Date()));
              employees.put(1004,new Employee(1004,"DD","2894318354@qq.com",0,new Department(104,"运营部"),new Date()));
              employees.put(1005,new Employee(1005,"EE","2894318355@qq.com",0,new Department(105,"后勤部"),new Date()));
          }
          // 主键自增
          private static Integer initId = 1006;
          // 增加一个员工
          public void save(Employee employee) {
              if(employee.getId() == null) {
                  employee.setId(initId++);
              }
      
              employee.setDepartment(departmentDao.getDepartmentById(employee.getDepartment().getId()));
      
              employees.put(employee.getId(),employee);
          }
      
          // 查询全部员工信息
          public Collection<Employee> getAll() {
              return employees.values();
          }
      
          // 通过id查询员工
          public Employee getEmployById(Integer id) {
              return employees.get(id);
          }
          // 删除员工
          public void delete(Integer id) {
              employees.remove(id);
          }
      }
      ```

   7. Department

      ```java
      package com.kuang.pojo;
      
      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;
      
      // 部门表
      @Data
      @AllArgsConstructor
      @NoArgsConstructor
      public class Department {
      
          private Integer id;
          private String department;
      
      }
      ```

   8. Employee

      ```java
      package com.kuang.pojo;
      
      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;
      import java.util.Date;
      
      // 员工表
      @Data
      @NoArgsConstructor
      @AllArgsConstructor
      public class Employee {
      
          private Integer id;
          private String lastName;
          private String email;
          private Integer gender;// 0：女  1：男
      
          private Department department;
          private Date birth;
      
      }
      ```

   9. commons.html

      ```html
      <!DOCTYPE html>
      <html lang="en" xmlns:th="http://www.thymeleaf.org">
      
      <!--头部导航栏-->
      <nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0" th:fragment="topbar">
          <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">[[${session.loginUser}]]</a>
          <input class="form-control form-control-dark w-100" type="text" placeholder="搜索" aria-label="Search">
          <ul class="navbar-nav px-3">
              <li class="nav-item text-nowrap">
                  <a class="nav-link" th:href="@{/user/logout}">注销</a>
              </li>
          </ul>
      </nav>
      
      <!--侧边栏-->
      <nav class="col-md-2 d-none d-md-block bg-light sidebar" th:fragment="sidebar">
          <div class="sidebar-sticky">
              <ul class="nav flex-column">
                  <li class="nav-item">
                      <a th:class="${active=='main.html'?'nav-link active':'nav-link'}" th:href="@{/main.html}">
                          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home">
                              <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
                              <polyline points="9 22 9 12 15 12 15 22"></polyline>
                          </svg>
                          首页 <span class="sr-only">(current)</span>
                      </a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file">
                              <path d="M13 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V9z"></path>
                              <polyline points="13 2 13 9 20 9"></polyline>
                          </svg>
                          Orders
                      </a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-shopping-cart">
                              <circle cx="9" cy="21" r="1"></circle>
                              <circle cx="20" cy="21" r="1"></circle>
                              <path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path>
                          </svg>
                          Products
                      </a>
                  </li>
                  <li class="nav-item">
                      <a th:class="${active=='list.html'?'nav-link active':'nav-link'}" th:href="@{/emps}">
                          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-users">
                              <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                              <circle cx="9" cy="7" r="4"></circle>
                              <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
                              <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
                          </svg>
                          员工管理
                      </a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-bar-chart-2">
                              <line x1="18" y1="20" x2="18" y2="10"></line>
                              <line x1="12" y1="20" x2="12" y2="4"></line>
                              <line x1="6" y1="20" x2="6" y2="14"></line>
                          </svg>
                          Reports
                      </a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-layers">
                              <polygon points="12 2 2 7 12 12 22 7 12 2"></polygon>
                              <polyline points="2 17 12 22 22 17"></polyline>
                              <polyline points="2 12 12 17 22 12"></polyline>
                          </svg>
                          Integrations
                      </a>
                  </li>
              </ul>
      
              <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
                  <span>Saved reports</span>
                  <a class="d-flex align-items-center text-muted" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-plus-circle"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
                  </a>
              </h6>
              <ul class="nav flex-column mb-2">
                  <li class="nav-item">
                      <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                              <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                              <polyline points="14 2 14 8 20 8"></polyline>
                              <line x1="16" y1="13" x2="8" y2="13"></line>
                              <line x1="16" y1="17" x2="8" y2="17"></line>
                              <polyline points="10 9 9 9 8 9"></polyline>
                          </svg>
                          Current month
                      </a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                              <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                              <polyline points="14 2 14 8 20 8"></polyline>
                              <line x1="16" y1="13" x2="8" y2="13"></line>
                              <line x1="16" y1="17" x2="8" y2="17"></line>
                              <polyline points="10 9 9 9 8 9"></polyline>
                          </svg>
                          Last quarter
                      </a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                              <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                              <polyline points="14 2 14 8 20 8"></polyline>
                              <line x1="16" y1="13" x2="8" y2="13"></line>
                              <line x1="16" y1="17" x2="8" y2="17"></line>
                              <polyline points="10 9 9 9 8 9"></polyline>
                          </svg>
                          Social engagement
                      </a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                              <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                              <polyline points="14 2 14 8 20 8"></polyline>
                              <line x1="16" y1="13" x2="8" y2="13"></line>
                              <line x1="16" y1="17" x2="8" y2="17"></line>
                              <polyline points="10 9 9 9 8 9"></polyline>
                          </svg>
                          Year-end sale
                      </a>
                  </li>
              </ul>
          </div>
      </nav>
      
      </html>
      ```

   10. add.html

       ```html
       <!DOCTYPE html>
       <!-- saved from url=(0052)http://getbootstrap.com/docs/4.0/examples/dashboard/ -->
       <html lang="en" xmlns:th="http://www.thymeleaf.org">
       
       	<head>
       		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
       		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
       		<meta name="description" content="">
       		<meta name="author" content="">
       
       		<title>Dashboard Template for Bootstrap</title>
       		<!-- Bootstrap core CSS -->
       		<link href="asserts/css/bootstrap.min.css" rel="stylesheet">
       
       		<!-- Custom styles for this template -->
       		<link href="asserts/css/dashboard.css" rel="stylesheet">
       		<style type="text/css">
       			/* Chart.js */
       			
       			@-webkit-keyframes chartjs-render-animation {
       				from {
       					opacity: 0.99
       				}
       				to {
       					opacity: 1
       				}
       			}
       			
       			@keyframes chartjs-render-animation {
       				from {
       					opacity: 0.99
       				}
       				to {
       					opacity: 1
       				}
       			}
       			
       			.chartjs-render-monitor {
       				-webkit-animation: chartjs-render-animation 0.001s;
       				animation: chartjs-render-animation 0.001s;
       			}
       		</style>
       	</head>
       
       	<body>
       
       		<div th:replace="commons/commons.html::topbar"></div>
       
       		<div class="container-fluid">
       			<div class="row">
       
       				<div th:replace="commons/commons.html::sidebar(active='list.html')"></div>
       
       				<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
       					<form th:action="@{/emp}" method="post">
       						<div class="form-group">
       							<label>LastName</label>
       							<input type="text" name="lastName" class="form-control" placeholder="kuangshen">
       						</div>
       						<div class="form-group">
       							<label>Email</label>
       							<input type="email" name="email" class="form-control" placeholder="299456@qq.com">
       						</div>
       						<div class="form-group">
       							<label>Gender</label><br/>
       							<div class="form-check form-check-inline">
       								<input class="form-check-input" type="radio" name="gender" value="1">
       								<label class="form-check-label">男</label>
       							</div>
       							<div class="form-check form-check-inline">
       								<input class="form-check-input" type="radio" name="gender" value="0">
       								<label class="form-check-label">女</label>
       							</div>
       						</div>
       						<div class="form-group">
       							<label>department</label>
       							<!--我们在controller接受的是一个employee，所以我们需要提交的是其中的的一个属性-->
       							<select class="form-control" name="department.id">
       								<option th:each="dept:${departments}" th:text="${dept.getDepartment()}" th:value="${dept.getId()}"></option>
       
       							</select>
       						</div>
       						<div class="form-group">
       							<label>Birth</label>
       							<input type="text" name="birth" class="form-control" placeholder="kuangstudy">
       						</div>
       						<button type="submit" class="btn btn-primary">添加</button>
       					</form>
       				</main>
       			</div>
       		</div>
       
       		<!-- Bootstrap core JavaScript
           ================================================== -->
       		<!-- Placed at the end of the document so the pages load faster -->
       		<script type="text/javascript" src="asserts/js/jquery-3.2.1.slim.min.js"></script>
       		<script type="text/javascript" src="asserts/js/popper.min.js"></script>
       		<script type="text/javascript" src="asserts/js/bootstrap.min.js"></script>
       
       		<!-- Icons -->
       		<script type="text/javascript" src="asserts/js/feather.min.js"></script>
       		<script>
       			feather.replace()
       		</script>
       
       		<!-- Graphs -->
       		<script type="text/javascript" src="asserts/js/Chart.min.js"></script>
       		<script>
       			var ctx = document.getElementById("myChart");
       			var myChart = new Chart(ctx, {
       				type: 'line',
       				data: {
       					labels: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
       					datasets: [{
       						data: [15339, 21345, 18483, 24003, 23489, 24092, 12034],
       						lineTension: 0,
       						backgroundColor: 'transparent',
       						borderColor: '#007bff',
       						borderWidth: 4,
       						pointBackgroundColor: '#007bff'
       					}]
       				},
       				options: {
       					scales: {
       						yAxes: [{
       							ticks: {
       								beginAtZero: false
       							}
       						}]
       					},
       					legend: {
       						display: false,
       					}
       				}
       			});
       		</script>
       
       	</body>
       
       </html>
       ```

   11. list.html

       ```html
       <!DOCTYPE html>
       <!-- saved from url=(0052)http://getbootstrap.com/docs/4.0/examples/dashboard/ -->
       <html lang="en" xmlns:th="http://www.thymeleaf.org">
       
       	<head>
       		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
       		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
       		<meta name="description" content="">
       		<meta name="author" content="">
       
       		<title>Dashboard Template for Bootstrap</title>
       		<!-- Bootstrap core CSS -->
       		<link href="asserts/css/bootstrap.min.css" rel="stylesheet">
       
       		<!-- Custom styles for this template -->
       		<link href="asserts/css/dashboard.css" rel="stylesheet">
       		<style type="text/css">
       			/* Chart.js */
       			
       			@-webkit-keyframes chartjs-render-animation {
       				from {
       					opacity: 0.99
       				}
       				to {
       					opacity: 1
       				}
       			}
       			
       			@keyframes chartjs-render-animation {
       				from {
       					opacity: 0.99
       				}
       				to {
       					opacity: 1
       				}
       			}
       			
       			.chartjs-render-monitor {
       				-webkit-animation: chartjs-render-animation 0.001s;
       				animation: chartjs-render-animation 0.001s;
       			}
       		</style>
       	</head>
       
       	<body>
       
       		<div th:replace="commons/commons.html::topbar"></div>
       
       		<div class="container-fluid">
       			<div class="row">
       
       				<div th:replace="commons/commons.html::sidebar(active='list.html')"></div>
       
       				<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
       
       					<h2><a class="btn btn-sm btn-success" th:href="@{/emp}">添加员工</a></h2>
       
       					<div class="table-responsive">
       						<table class="table table-striped table-sm">
       							<thead>
       								<tr>
       									<th>id</th>
       									<th>lastName</th>
       									<th>email</th>
       									<th>gender</th>
       									<th>department</th>
       									<th>birth</th>
       									<th>操作</th>
       								</tr>
       							</thead>
       							<tbody>
       								<tr th:each="emp:${emps}">
       									<td th:text="${emp.getId()}"></td>
       									<td th:text="${emp.getLastName()}"></td>
       									<td th:text="${emp.getEmail()}"></td>
       									<td th:text="${emp.getGender()==0?'女':'男'}"></td>
       									<td th:text="${emp.department.getDepartment()}"></td>
       									<td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm:ss')}"></td>
       									<td>
       										<a class="btn btn-sm btn-primary" th:href="@{'/emp/'+${emp.getId()}}">编辑</a>
       										<a class="btn btn-sm btn-danger" th:href="@{'/delemp/'+${emp.getId()}}">删除</a>
       									</td>
       								</tr>
       							</tbody>
       						</table>
       					</div>
       				</main>
       			</div>
       		</div>
       
       		<!-- Bootstrap core JavaScript
           ================================================== -->
       		<!-- Placed at the end of the document so the pages load faster -->
       		<script type="text/javascript" src="asserts/js/jquery-3.2.1.slim.min.js"></script>
       		<script type="text/javascript" src="asserts/js/popper.min.js"></script>
       		<script type="text/javascript" src="asserts/js/bootstrap.min.js"></script>
       
       		<!-- Icons -->
       		<script type="text/javascript" src="asserts/js/feather.min.js"></script>
       		<script>
       			feather.replace()
       		</script>
       
       		<!-- Graphs -->
       		<script type="text/javascript" src="asserts/js/Chart.min.js"></script>
       		<script>
       			var ctx = document.getElementById("myChart");
       			var myChart = new Chart(ctx, {
       				type: 'line',
       				data: {
       					labels: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
       					datasets: [{
       						data: [15339, 21345, 18483, 24003, 23489, 24092, 12034],
       						lineTension: 0,
       						backgroundColor: 'transparent',
       						borderColor: '#007bff',
       						borderWidth: 4,
       						pointBackgroundColor: '#007bff'
       					}]
       				},
       				options: {
       					scales: {
       						yAxes: [{
       							ticks: {
       								beginAtZero: false
       							}
       						}]
       					},
       					legend: {
       						display: false,
       					}
       				}
       			});
       		</script>
       
       	</body>
       
       </html>
       ```

   12. update.html

       ```html
       <!DOCTYPE html>
       <!-- saved from url=(0052)http://getbootstrap.com/docs/4.0/examples/dashboard/ -->
       <html lang="en" xmlns:th="http://www.thymeleaf.org">
       
       	<head>
       		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
       		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
       		<meta name="description" content="">
       		<meta name="author" content="">
       
       		<title>Dashboard Template for Bootstrap</title>
       		<!-- Bootstrap core CSS -->
       		<link th:href="@{/asserts/css/bootstrap.min.css}" rel="stylesheet">
       
       		<!-- Custom styles for this template -->
       		<link th:href="@{/asserts/css/dashboard.css}" rel="stylesheet">
       		<style type="text/css">
       			/* Chart.js */
       			
       			@-webkit-keyframes chartjs-render-animation {
       				from {
       					opacity: 0.99
       				}
       				to {
       					opacity: 1
       				}
       			}
       			
       			@keyframes chartjs-render-animation {
       				from {
       					opacity: 0.99
       				}
       				to {
       					opacity: 1
       				}
       			}
       			
       			.chartjs-render-monitor {
       				-webkit-animation: chartjs-render-animation 0.001s;
       				animation: chartjs-render-animation 0.001s;
       			}
       		</style>
       	</head>
       
       	<body>
       
       		<div th:replace="commons/commons.html::topbar"></div>
       
       		<div class="container-fluid">
       			<div class="row">
       
       				<div th:replace="commons/commons.html::sidebar(active='list.html')"></div>
       
       				<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
       					<form th:action="@{/updateEmp}" method="post">
       
       						<!--修改用户的关键-->
       						<input type="hidden" name="id" th:value="${emp.getId()}">
       
       						<div class="form-group">
       							<label>LastName</label>
       							<input th:value="${emp.getLastName()}" type="text" name="lastName" class="form-control" placeholder="kuangshen">
       						</div>
       						<div class="form-group">
       							<label>Email</label>
       							<input th:value="${emp.getEmail()}" type="email" name="email" class="form-control" placeholder="299456@qq.com">
       						</div>
       						<div class="form-group">
       							<label>Gender</label><br/>
       							<div class="form-check form-check-inline">
       								<input th:checked="${emp.getGender()==1}" class="form-check-input" type="radio" name="gender" value="1">
       								<label class="form-check-label">男</label>
       							</div>
       							<div class="form-check form-check-inline">
       								<input th:checked="${emp.getGender()==0}" class="form-check-input" type="radio" name="gender" value="0">
       								<label class="form-check-label">女</label>
       							</div>
       						</div>
       						<div class="form-group">
       							<label>department</label>
       							<!--我们在controller接受的是一个employee，所以我们需要提交的是其中的的一个属性-->
       							<select class="form-control" name="department.id">
       								<option th:selected="${dept.getId()==emp.getDepartment().getId()}" th:each="dept:${departments}" th:text="${dept.getDepartment()}" th:value="${dept.getId()}"></option>
       
       							</select>
       						</div>
       						<div class="form-group">
       							<label>Birth</label>
       							<input th:value="${#dates.format(emp.getBirth(),'yyyy/MM/dd HH:mm:ss')}" type="text" name="birth" class="form-control" placeholder="kuangstudy">
       						</div>
       						<button type="submit" class="btn btn-primary">修改</button>
       					</form>
       				</main>
       			</div>
       		</div>
       
       		<!-- Bootstrap core JavaScript
           ================================================== -->
       		<!-- Placed at the end of the document so the pages load faster -->
       		<script type="text/javascript" src="asserts/js/jquery-3.2.1.slim.min.js"></script>
       		<script type="text/javascript" src="asserts/js/popper.min.js"></script>
       		<script type="text/javascript" src="asserts/js/bootstrap.min.js"></script>
       
       		<!-- Icons -->
       		<script type="text/javascript" src="asserts/js/feather.min.js"></script>
       		<script>
       			feather.replace()
       		</script>
       
       		<!-- Graphs -->
       		<script type="text/javascript" src="asserts/js/Chart.min.js"></script>
       		<script>
       			var ctx = document.getElementById("myChart");
       			var myChart = new Chart(ctx, {
       				type: 'line',
       				data: {
       					labels: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
       					datasets: [{
       						data: [15339, 21345, 18483, 24003, 23489, 24092, 12034],
       						lineTension: 0,
       						backgroundColor: 'transparent',
       						borderColor: '#007bff',
       						borderWidth: 4,
       						pointBackgroundColor: '#007bff'
       					}]
       				},
       				options: {
       					scales: {
       						yAxes: [{
       							ticks: {
       								beginAtZero: false
       							}
       						}]
       					},
       					legend: {
       						display: false,
       					}
       				}
       			});
       		</script>
       
       	</body>
       
       </html>
       ```

   13. dashboard.html

       ```html
       <!DOCTYPE html>
       <!-- saved from url=(0052)http://getbootstrap.com/docs/4.0/examples/dashboard/ -->
       <html lang="en" xmlns:th="http://www.thymeleaf.org">
       	<head>
       		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
       		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
       		<meta name="description" content="">
       		<meta name="author" content="">
       
       		<title>Dashboard Template for Bootstrap</title>
       		<!-- Bootstrap core CSS -->
       		<link th:href="@{/asserts/css/bootstrap.min.css}" rel="stylesheet">
       
       		<!-- Custom styles for this template -->
       		<link th:href="@{/asserts/css/dashboard.css}" rel="stylesheet">
       		<style type="text/css">
       			/* Chart.js */
       			
       			@-webkit-keyframes chartjs-render-animation {
       				from {
       					opacity: 0.99
       				}
       				to {
       					opacity: 1
       				}
       			}
       			
       			@keyframes chartjs-render-animation {
       				from {
       					opacity: 0.99
       				}
       				to {
       					opacity: 1
       				}
       			}
       			
       			.chartjs-render-monitor {
       				-webkit-animation: chartjs-render-animation 0.001s;
       				animation: chartjs-render-animation 0.001s;
       			}
       		</style>
       	</head>
       
       	<body>
       
       		<!--顶部导航栏-->
       		<div th:replace="commons/commons.html::topbar"></div>
       
       		<div class="container-fluid">
       			<div class="row">
       
       				<!--侧边栏-->
       				<!--传递参数给组件-->
       				<div th:replace="commons/commons.html::sidebar(active='main.html')"></div>
       
       				<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
       					<div class="chartjs-size-monitor" style="position: absolute; left: 0px; top: 0px; right: 0px; bottom: 0px; overflow: hidden; pointer-events: none; visibility: hidden; z-index: -1;">
       						<div class="chartjs-size-monitor-expand" style="position:absolute;left:0;top:0;right:0;bottom:0;overflow:hidden;pointer-events:none;visibility:hidden;z-index:-1;">
       							<div style="position:absolute;width:1000000px;height:1000000px;left:0;top:0"></div>
       						</div>
       						<div class="chartjs-size-monitor-shrink" style="position:absolute;left:0;top:0;right:0;bottom:0;overflow:hidden;pointer-events:none;visibility:hidden;z-index:-1;">
       							<div style="position:absolute;width:200%;height:200%;left:0; top:0"></div>
       						</div>
       					</div>
       					<div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pb-2 mb-3 border-bottom">
       						<h1 class="h2">Dashboard</h1>
       						<div class="btn-toolbar mb-2 mb-md-0">
       							<div class="btn-group mr-2">
       								<button class="btn btn-sm btn-outline-secondary">Share</button>
       								<button class="btn btn-sm btn-outline-secondary">Export</button>
       							</div>
       							<button class="btn btn-sm btn-outline-secondary dropdown-toggle">
                       <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg>
                       This week
                     </button>
       						</div>
       					</div>
       
       					<canvas class="my-4 chartjs-render-monitor" id="myChart" width="1076" height="454" style="display: block; width: 1076px; height: 454px;"></canvas>
       
       					
       				</main>
       			</div>
       		</div>
       
       		<!-- Bootstrap core JavaScript
           ================================================== -->
       		<!-- Placed at the end of the document so the pages load faster -->
       		<script type="text/javascript" th:src="@{/asserts/js/jquery-3.2.1.slim.min.js}" ></script>
       		<script type="text/javascript" th:src="@{/asserts/js/popper.min.js}" ></script>
       		<script type="text/javascript" th:src="@{/asserts/js/bootstrap.min.js}" ></script>
       
       		<!-- Icons -->
       		<script type="text/javascript" th:src="@{/asserts/js/feather.min.js}" ></script>
       		<script>
       			feather.replace()
       		</script>
       
       		<!-- Graphs -->
       		<script type="text/javascript" th:src="@{/asserts/js/Chart.min.js}" ></script>
       		<script>
       			var ctx = document.getElementById("myChart");
       			var myChart = new Chart(ctx, {
       				type: 'line',
       				data: {
       					labels: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
       					datasets: [{
       						data: [15339, 21345, 18483, 24003, 23489, 24092, 12034],
       						lineTension: 0,
       						backgroundColor: 'transparent',
       						borderColor: '#007bff',
       						borderWidth: 4,
       						pointBackgroundColor: '#007bff'
       					}]
       				},
       				options: {
       					scales: {
       						yAxes: [{
       							ticks: {
       								beginAtZero: false
       							}
       						}]
       					},
       					legend: {
       						display: false,
       					}
       				}
       			});
       		</script>
       
       	</body>
       
       </html>
       ```

   14. index.html

       ```html
       <!DOCTYPE html>
       <html lang="en" xmlns:th="http://www.thymeleaf.org">
       	<head>
       		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
       		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
       		<meta name="description" content="">
       		<meta name="author" content="">
       		<title>Signin Template for Bootstrap</title>
       		<!-- Bootstrap core CSS -->
       		<link th:href="@{/asserts/css/bootstrap.min.css}" rel="stylesheet">
       		<!-- Custom styles for this template -->
       		<link th:href="@{/asserts/css/signin.css}" rel="stylesheet">
       	</head>
       
       	<body class="text-center">
       		<form class="form-signin" th:action="@{/user/login}">
       			<img class="mb-4" th:src="@{/asserts/img/bootstrap-solid.svg}" alt="" width="72" height="72">
       			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>
       
       			<!-- 如果msg的值为空，则不显示消息 -->
       			<p style="color: #ff0000" th:text="${msg}"></p>
       
       			<label class="sr-only">Username</label>
       			<input type="text" name="username" class="form-control" th:placeholder="#{login.username}" required="" autofocus="">
       			<label class="sr-only">Password</label>
       			<input type="password" name="password" class="form-control" th:placeholder="#{login.password}" required="">
       			<div class="checkbox mb-3">
       				<label>
                 <input type="checkbox" value="remember-me"> [[#{login.remember}]]
               </label>
       			</div>
       			<button class="btn btn-lg btn-primary btn-block" type="submit">[[#{login.btn}]]</button>
       			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
       			<a class="btn btn-sm">中文</a>
       			<a class="btn btn-sm">English</a>
       		</form>
       
       	</body>
       
       </html>
       ```

       怎么制作一个网页？

       1. 前端：页面长什么样子

       2. 设计数据库（数据库的设计）

       3. 前端让他能够独立运行，独立化工程

       4. 数据接口如何对接：json，对象  all  in  one！

       5. 前后端联调测试！

          > 1. 有一套自己熟悉的后台模板：工作必要！！！  x-admin
          > 2. 前端界面，至少自己能够通过前端框架组合出来一个网站页面
          >    - index
          >    - about
          >    - blog
          >    - post
          >    - user
          > 3. 让这个网站能够独立运行！

## 回顾

* SpringBoot是什么？
* 微服务
* HelloWorld
* 探究源码，得出自动装配的原理
* 配置  yaml
* 多文档环境切换
* 静态资源映射
* Thymeleaf     th:xxx
* SpringBoot如何扩展MVC     javaconfig
* 如何修改SpringBoot的默认配置
* CRUD
* 国际化
* 拦截器
* 定制首页，错误页

这周：

* JDBC
* **Mybatis：重点**
* **Druid：重点**
* **Shiro：安全    重点**
* **Spring Security：安全    重点** 
* 异步任务，邮件发送，定时任务
* Swagger
* Dubbo + Zookeeper

## Spring Data

### Spring Data简介

> ​		对于数据访问层，无论是 SQL(关系型数据库) 还是 NOSQL(非关系型数据库)，Spring Boot 底层都是采用 Spring Data 的方式进行统一处理。 
>
> ​		Spring Boot 底层都是采用 Spring Data 的方式进行统一处理各种数据库，Spring Data 也是 Spring 中与 Spring Boot、Spring Cloud 等齐名的知名项目。 
>
> ​		Sping Data 官网：https://spring.io/projects/spring-data 
>
> ​		数据库相关的启动器 ：可以参考官方文档：
>
> https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter

### 整合JDBC

1. 创建测试项目测试数据源

   > 1. 创建一个新项目测试：
   >
   > ![1617274380544](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617274380544.png)
   >
   > 2. 项目建好后，发现自动帮我们导入了如下的启动器：
   >
   >    ```xml
   >            <!--JDBC-->
   >            <dependency>
   >                <groupId>org.springframework.boot</groupId>
   >                <artifactId>spring-boot-starter-jdbc</artifactId>
   >            </dependency>
   >            <!--MySQL驱动-->
   >            <dependency>
   >                <groupId>mysql</groupId>
   >                <artifactId>mysql-connector-java</artifactId>
   >                <scope>runtime</scope>
   >            </dependency>
   >    ```
   >
   > 3. 编写yaml配置文件连接数据库：
   >
   >    ```yaml
   >    spring:
   >      datasource:
   >        username: root
   >        password: 15591177926
   >        url: jdbc:mysql://localhost:3306/day14?useUnicode=true&characterEncoding=utf-8
   >        driver-class-name: com.mysql.cj.jdbc.Driver
   >    ```
   >
   > 4. 配置完着一些东西，我们就可以直接去使用了，因为SpringBoot已经默认帮我们配置进行了自动配置，去测试类测试一下：
   >
   >    ```java
   >    @SpringBootTest
   >    class Springboot04DataApplicationTests {
   >    
   >        @Autowired
   >        DataSource dataSource;
   >    
   >        @Test
   >        void contextLoads() throws SQLException {
   >    
   >            // 查看默认的数据源：com.zaxxer.hikari.HikariDataSource
   >            System.out.println(dataSource.getClass());
   >    
   >            // 获取数据库连接
   >            Connection connection = dataSource.getConnection();
   >            System.out.println(connection);
   >    
   >            // xxxx Template：SpringBoot已经配置好的模板bean，拿来即用
   >    
   >    
   >            //释放连接
   >            connection.close();
   >        }
   >    
   >    }
   >    ```
   >
   >    结果：我们可以看到他默认给我们配置的数据源为：class
   >
   >    com.zaxxer.hikari.HikariDataSource ， 我们并没有手动配置 
   >
   >    我们来全局搜索一下，找到数据源的所有自动配置都在：DataSourceAutoConfiguration文件 ：
   >
   >    ```java
   >    @Import(
   >        {Hikari.class, Tomcat.class, Dbcp2.class, Generic.class, DataSourceJmxConfiguration.class}
   >    )
   >    protected static class PooledDataSourceConfiguration {
   >        protected PooledDataSourceConfiguration() {
   >        }
   >    }
   >    ```
   >
   >    **HikariDataSource 号称 Java WEB 当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat jdbc 等连接池更加优秀；** 
   >
   >    **可以使用 spring.datasource.type 指定自定义的数据源类型，值为 要使用的连接池实现的完全限定名。** 
   
2. JDBCTemplate

   > 1. 有了数据源(com.zaxxer.hikari.HikariDataSource)，然后可以拿到数据库连接(java.sql.Connection)，有了连接，就可以使用原生的 JDBC 语句来操作数据库； 
   > 2. 即使不使用第三方第数据库操作框架，如 MyBatis等，Spring 本身也对原生的JDBC 做了轻量级的封装，即JdbcTemplate。 
   > 3. 数据库操作的所有 CRUD 方法都在 JdbcTemplate 中。 
   > 4. Spring Boot 不仅提供了默认的数据源，同时默认已经配置好了 JdbcTemplate 放在了容器中，程序员只需自己注入即可使用 
   > 5. JdbcTemplate 的自动配置是依赖 org.springframework.boot.autoconfigure.jdbc 包下的 JdbcTemplateConfiguration 类 
   >
   >  **JdbcTemplate主要提供以下几类方法：** 
   >
   > * execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句； 
   > * update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句； 
   > * query方法及queryForXXX方法：用于执行查询相关语句； 
   > * call方法：用于执行存储过程、函数相关语句。 

3. 测试

   > 编写一个Controller，注入jdbcTemplate，编写测试方法进行访问测试；
   >
   > ```java
   > package com.kuang.controller;
   > 
   > 
   > import org.springframework.beans.factory.annotation.Autowired;
   > import org.springframework.jdbc.core.JdbcTemplate;
   > import org.springframework.web.bind.annotation.GetMapping;
   > import org.springframework.web.bind.annotation.PathVariable;
   > import org.springframework.web.bind.annotation.RequestMapping;
   > import org.springframework.web.bind.annotation.RestController;
   > 
   > import java.util.List;
   > import java.util.Map;
   > 
   > @RestController
   > public class JDBCController {
   > 
   >     @Autowired
   >     JdbcTemplate jdbcTemplate;
   > 
   >     // 查询数据库的所有信息
   >     // 没有实体类，数据库中的东西，怎么获取？  Map
   >     @GetMapping("/userList")
   >     public List<Map<String,Object>> userList() {
   >         String sql = "select * from user";
   >         List<Map<String, Object>> list_maps = jdbcTemplate.queryForList(sql);
   >         return list_maps;
   >     }
   > 
   >     // 添加用户   springboot自动提交事务
   >     @GetMapping("/addUser")
   >     public String addUser() {
   >         String sql = "insert into day14.user(username,password) value('吴奇刚','123')";
   >         jdbcTemplate.update(sql);
   >         return "add-ok";
   >     }
   > 
   >     // 修改用户
   >     @GetMapping("/updateUser/{id}")
   >     public String updateUser(@PathVariable("id") int id) {
   >         String sql = "update day14.user set username=?,password=? where id=" + id;
   > 
   >         // 封装
   >         Object[] objects = new Object[2];
   >         objects[0] = "张泽瑞sb";
   >         objects[1] = "zzzzzz";
   >         jdbcTemplate.update(sql,objects[0],objects[1]);
   > 
   >         return "update-ok";
   >     }
   > 
   >     // 删除用户   springboot自动提交事务
   >     @GetMapping("/deleteUser/{id}")
   >     public String deleteUser(@PathVariable("id") int id) {
   >         String sql = "delete from day14.user where id=?";
   >         jdbcTemplate.update(sql,id);
   >         return "delete-ok";
   >     }
   > }
   > ```
   >
   > 测试请求，结果正常；

### 整合Druid

1. Druid简介

   > Java程序很大一部分要操作数据库，为了提高性能操作数据库的时候，又不得不使用数据库连接池。
   >
   > Druid 是阿里巴巴开源平台上一个数据库连接池实现，结合了 C3P0、DBCP 等 DB 池的优点，同时加入了日志监控。 
   >
   > Druid 可以很好的监控 DB 池连接和 SQL 的执行情况，天生就是针对监控而生的 DB 连接池。
   >
   > Druid已经在阿里巴巴部署了超过600个应用，经过一年多生产环境大规模部署的严苛考验。 
   >
   > Spring Boot 2.0 以上默认使用 Hikari 数据源，可以说 Hikari 与 Driud 都是当前 Java Web 上最优秀的数据源，我们来重点介绍 Spring Boot 如何集成 Druid 数据源，如何实现数据库监控。 
   >
   > Github地址：https://github.com/alibaba/druid/ 
   >
   > **com.alibaba.druid.pool.DruidDataSource 基本配置参数如下：** 
   >
   > ![1617352022722](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617352022722.png)
   >
   > ![1617352041648](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617352041648.png)
   >
   > ![1617352077832](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617352077832.png)

2. 配置数据源

   > 1. 添加上 Druid 数据源依赖（这里使用了druid启动器 所以后面不需要写DruidConfig）
   >
   >    ```xml
   >    		<!--druid场景启动器-->
   >            <dependency>
   >                <groupId>com.alibaba</groupId>
   >                <artifactId>druid-spring-boot-starter</artifactId>
   >                <version>1.1.23</version>
   >            </dependency>
   >    
   >            <dependency>
   >                <groupId>org.projectlombok</groupId>
   >                <artifactId>lombok</artifactId>
   >            </dependency>
   >            <!--Druid-->
   >            <dependency>
   >                <groupId>com.alibaba</groupId>
   >                <artifactId>druid</artifactId>
   >                <version>1.2.5</version>
   >            </dependency>
   >    ```
   >
   > 2. 切换数据源，设置属性
   >
   >    ```yaml
   >    spring:
   >      datasource:
   >        name: druidDataSource
   >        type: com.alibaba.druid.pool.DruidDataSource
   >        druid:
   >          driver-class-name: com.mysql.cj.jdbc.Driver
   >          url: jdbc:mysql://localhost:3306/day14?useUnicode=true&characterEncoding=utf-8
   >          username: root
   >          password: 15591177926
   >          filters: stat,wall,log4j,config
   >          max-active: 100
   >          initial-size: 1
   >          max-wait: 60000
   >          min-idle: 1
   >          time-between-eviction-runs-millis: 60000
   >          min-evictable-idle-time-millis: 300000
   >          validation-query: select 'x'
   >          test-while-idle: true
   >          test-on-borrow: false
   >          test-on-return: false
   >          pool-prepared-statements: true
   >          max-open-prepared-statements: 50
   >          max-pool-prepared-statement-per-connection-size: 20
   >          stat-view-servlet:
   >            enabled: true
   >            login-password: 123456
   >            login-username: admin
   >    
   >    ```
   >
   > 3. 导入 log4j 的依赖
   >
   >    ```xml
   >            <!--log4j-->
   >            <dependency>
   >                <groupId>log4j</groupId>
   >                <artifactId>log4j</artifactId>
   >                <version>1.2.17</version>
   >            </dependency>
   >    ```
   >
   >    配置完毕后，访问 http://localhost:8080/druid/login.html
   >
   >    ![1617352395428](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617352395428.png)

### 整合MyBatis

官方文档：http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/

Maven仓库地址：https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.1

![1617430817867](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617430817867.png)

1. 整合测试

   > 1. 导入Mybatis所需要的依赖
   >
   >    ```xml
   >            <!--mybatis-spring-boot-starter：整合-->
   >            <dependency>
   >                <groupId>org.mybatis.spring.boot</groupId>
   >                <artifactId>mybatis-spring-boot-starter</artifactId>
   >                <version>2.1.3</version>
   >            </dependency>
   >    ```
   >
   > 2. 配置数据库连接信息（不变）
   >
   >    ```yaml
   >    spring:
   >      datasource:
   >        name: druidDataSource
   >        type: com.alibaba.druid.pool.DruidDataSource
   >        druid:
   >          driver-class-name: com.mysql.cj.jdbc.Driver
   >          url: jdbc:mysql://localhost:3306/day14?useUnicode=true&characterEncoding=utf-8
   >          username: root
   >          password: 15591177926
   >          filters: stat,wall,log4j,config
   >          max-active: 100
   >          initial-size: 1
   >          max-wait: 60000
   >          min-idle: 1
   >          time-between-eviction-runs-millis: 60000
   >          min-evictable-idle-time-millis: 300000
   >          validation-query: select 'x'
   >          test-while-idle: true
   >          test-on-borrow: false
   >          test-on-return: false
   >          pool-prepared-statements: true
   >          max-open-prepared-statements: 50
   >          max-pool-prepared-statement-per-connection-size: 20
   >          stat-view-servlet:
   >            enabled: true
   >            login-password: 123456
   >            login-username: admin
   >    
   >    ```
   >
   > 3. 测试数据库是否连接成功
   >
   > 4. 创建实体类，导入Lombok
   >
   >    ```java
   >    package com.kuang.pojo;
   >    
   >    import lombok.AllArgsConstructor;
   >    import lombok.Data;
   >    import lombok.NoArgsConstructor;
   >    
   >    @Data
   >    @NoArgsConstructor
   >    @AllArgsConstructor
   >    public class Department {
   >    
   >        private Integer id;
   >        private String departmentName;
   >    
   >    }
   >    ```
   >
   > 5. 创建mapper目录以及对应的Mapper接口
   >
   >    ```java
   >    //@Mapper : 表示本类是一个 MyBatis 的 Mapper
   >    @Mapper
   >    @Repository
   >    public interface DepartmentMapper {
   >    
   >        // 获取所有部门信息
   >        List<Department> getDepartments();
   >    
   >        // 通过id获得部门
   >        Department getDepartment(Integer id);
   >    
   >    }
   >    ```
   >
   > 6. 对应的Mapper映射文件
   >
   >    ```xml
   >    <?xml version="1.0" encoding="UTF-8" ?>
   >    <!DOCTYPE mapper
   >            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
   >            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   >    
   >    <mapper namespace="com.kuang.mapper.DepartmentMapper">
   >    
   >        <select id="getDepartments" resultType="Department">
   >           select * from department;
   >        </select>
   >    
   >        <select id="getDepartment" resultType="Department" parameterType="int">
   >           select * from department where id = #{id};
   >        </select>
   >    
   >    </mapper>
   >    ```
   >
   > 7. maven配置资源过滤问题
   >
   >    ```xml
   >    <resources>
   >        <resource>
   >            <directory>src/main/java</directory>
   >            <includes>
   >                <include>**/*.xml</include>
   >            </includes>
   >            <filtering>true</filtering>
   >        </resource>
   >    </resources>
   >    ```
   >
   > 8. 编写部门的DepartmentController进行测试
   >
   >    ```java
   >    @RestController
   >    public class DepartmentController {
   >        
   >        @Autowired
   >        DepartmentMapper departmentMapper;
   >        
   >        // 查询全部部门
   >        @GetMapping("/getDepartments")
   >        public List<Department> getDepartments(){
   >            return departmentMapper.getDepartments();
   >        }
   >    
   >        // 查询全部部门
   >        @GetMapping("/getDepartment/{id}")
   >        public Department getDepartment(@PathVariable("id") Integer id){
   >            return departmentMapper.getDepartment(id);
   >        }
   >        
   >    }
   >    ```
   >
   >    启动项目进行测试

2. 再次进行测试

   > 1. 新建一个pojo类User
   >
   >    ```java
   >    package com.kuang.pojo;
   >    
   >    import lombok.AllArgsConstructor;
   >    import lombok.Data;
   >    import lombok.NoArgsConstructor;
   >    
   >    @Data
   >    @AllArgsConstructor
   >    @NoArgsConstructor
   >    public class User {
   >    
   >        private int id;
   >        private String username;
   >        private String password;
   >    
   >    }
   >    
   >    ```
   >
   > 2. 新建一个 UserMapper 接口
   >
   >    ```java
   >    package com.kuang.mapper;
   >    
   >    import com.kuang.pojo.User;
   >    import org.apache.ibatis.annotations.Mapper;
   >    import org.springframework.stereotype.Repository;
   >    
   >    import java.util.List;
   >    
   >    @Mapper // 这个注解表示了这是一个 mybatis 的mapper类:Dao
   >    @Repository
   >    public interface UserMapper {
   >    
   >    
   >        List<User> queryUserList();
   >    
   >        User queryUserById(int id);
   >    
   >        int addUser(User user);
   >    
   >        int updateUser(User user);
   >    
   >        int deleteUser(int id);
   >    }
   >    ```
   >
   > 3. 编写UserMapper.xml配置文件
   >
   >    ```xml
   >    <?xml version="1.0" encoding="UTF-8" ?>
   >    <!DOCTYPE mapper
   >            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
   >            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   >    <mapper namespace="com.kuang.mapper.UserMapper">
   >        
   >        <select id="queryUserList" resultType="User">
   >            select * from user
   >        </select>
   >    
   >        <select id="queryUserById" resultType="User">
   >            select * from user where id = #{id}
   >        </select>
   >    
   >        <insert id="addUser" parameterType="User">
   >            insert into user (username,password) values (#{username},#{password})
   >        </insert>
   >    
   >        <update id="updateUser" parameterType="User">
   >            update user set username=#{username},password=#{password} where id = #{id}
   >        </update>
   >    
   >        <delete id="deleteUser" parameterType="int">
   >            delete from user where id = #{id}
   >        </delete>
   >        
   >    </mapper>
   >    ```
   >
   > 4. 编写 UserController 类进行测试
   >
   >    ```java
   >    package com.kuang.controller;
   >    
   >    import com.kuang.mapper.UserMapper;
   >    import com.kuang.pojo.User;
   >    import org.springframework.beans.factory.annotation.Autowired;
   >    import org.springframework.web.bind.annotation.GetMapping;
   >    import org.springframework.web.bind.annotation.PathVariable;
   >    import org.springframework.web.bind.annotation.RestController;
   >    
   >    import java.util.List;
   >    
   >    @RestController
   >    public class UserController {
   >    
   >        @Autowired
   >        UserMapper userMapper;
   >    
   >        @GetMapping("/queryUserList")
   >        public List<User> queryUserList() {
   >            List<User> userList = userMapper.queryUserList();
   >            return userList;
   >        }
   >    
   >        @GetMapping("/queryUserById/{id}")
   >        public User queryUserById(@PathVariable("id") int id) {
   >            User user = userMapper.queryUserById(id);
   >            return user;
   >        }
   >    
   >        @GetMapping("/addUser")
   >        public String addUser(User user) {
   >            user.setUsername("张泽瑞");
   >            user.setPassword("250250");
   >            userMapper.addUser(user);
   >            return "addUser Success";
   >        }
   >    
   >        @GetMapping("/updateUser")
   >        public String updateUser(User user) {
   >            user.setId(4);
   >            user.setUsername("吴奇刚");
   >            user.setPassword("123123123");
   >            userMapper.updateUser(user);
   >            return "updateUser Success";
   >        }
   >    
   >        @GetMapping("/deleteUser/{id}")
   >        public String deleteUser(@PathVariable("id") int id) {
   >            userMapper.deleteUser(id);
   >            return "deleteUser Success";
   >        }
   >    }
   >    
   >    ```
   >
   >    测试结果完成。

## Spring Security

1. 安全简介

   > ​		在 Web 开发中，安全一直是非常重要的一个方面。安全虽然属于应用的非功能性需求，但是应该在应用开发的初期就考虑进来。如果在应用开发的后期才考虑安全的问题，就可能陷入一个两难的境地：一方面，应用存在严重的安全漏洞，无法满足用户的要求，并可能造成用户的隐私数据被攻击者窃取；另一方面，应用的基本架构已经确定，要修复安全漏洞，可能需要对系统的架构做出比较重大的调整，因而需要更多的开发时间，影响应用的发布进程。因此，从应用开发的第一天就应该把安全相关的因素考虑进来，并在整个应用的开发过程中。 
   >
   > ​		市面上存在比较有名的：Shiro，Spring Security ！ 
   >
   > ​		这里需要阐述一下的是，每一个框架的出现都是为了解决某一问题而产生了，那么Spring Security框架的出现是为了解决什么问题呢？ 
   >
   > ​		首先我们看下它的官网介绍：Spring Security官网地址
   >
   > ​		Spring Security is a powerful and highly customizable authentication and access-control framework. It is the de-facto standard for securing Spring-based applications.
   >
   > ​		Spring Security is a framework that focuses on providing both authentication and authorization to Java applications. Like all Spring projects, the real power of Spring Security is found in how easily it can be extended to meet custom requirements
   >
   > ​		Spring Security是一个功能强大且高度可定制的身份验证和访问控制框架。它实际上是保护基于spring的应用程序的标准。
   >
   > ​		Spring Security是一个框架，侧重于为Java应用程序提供身份验证和授权。与所有Spring项目一样，Spring安全性的真正强大之处在于它可以轻松地扩展以满足定制需求
   >
   > ​		从官网的介绍中可以知道这是一个权限框架。想我们之前做项目是没有使用框架是怎么控制权限的？对于权限 一般会细分为功能权限，访问权限，和菜单权限。代码会写的非常的繁琐，冗余。
   >
   > ​		怎么解决之前写权限代码繁琐，冗余的问题，一些主流框架就应运而生而Spring Scecurity就是其中的一种。
   >
   > ​		Spring 是一个非常流行和成功的 Java 应用开发框架。Spring Security 基于 Spring 框架，提供了一套 Web 应用安全性的完整解决方案。一般来说，Web 应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分。用户认证指的是验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认证过程。用户授权指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。
   >
   > ​		对于上面提到的两种应用情景，Spring Security 框架都有很好的支持。在用户认证方面，Spring Security 框架支持主流的认证方式，包括 HTTP 基本认证、HTTP 表单验证、HTTP 摘要认证、OpenID 和 LDAP 等。在用户授权方面，Spring Security 提供了基于角色的访问控制和访问控制列表（Access Control List，ACL），可以对应用中的领域对象进行细粒度的控制。

2. 实战环境搭建

   > 1. 新建一个初始的 springboot 项目模块，thymeleaf 模块
   >
   > 2. 导入静态资源
   >
   > 3. controller 跳转
   >
   >    ```java
   >    package com.kuang.controller;
   >    
   >    import org.springframework.stereotype.Controller;
   >    import org.springframework.web.bind.annotation.GetMapping;
   >    import org.springframework.web.bind.annotation.PathVariable;
   >    import org.springframework.web.bind.annotation.RequestMapping;
   >    
   >    @Controller
   >    public class RouterController {
   >    
   >        @RequestMapping({"/","/index"})
   >        public String index() {
   >            return "index";
   >        }
   >    
   >        @RequestMapping("/tologin")
   >        public String toLogin() {
   >            return "views/login";
   >        }
   >    
   >        @RequestMapping("/level1/{id}")
   >        public String level1(@PathVariable("id") int id) {
   >            return "views/level1/" + id;
   >        }
   >    
   >        @RequestMapping("/level2/{id}")
   >        public String level2(@PathVariable("id") int id) {
   >            return "views/level2/" + id;
   >        }
   >    
   >        @RequestMapping("/level3/{id}")
   >        public String level3(@PathVariable("id") int id) {
   >            return "views/level3/" + id;
   >        }
   >    
   >    }
   >    ```
   >
   > 4. 测试实验环境是否OK！

3. 认识 Spring Security

   > ​		Spring Security 是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！ 
   >
   > 记住几个类 ：
   >
   > * WebSecurityConfigurerAdapter：自定义Security策略 
   >
   > * AuthenticationManagerBuilder：自定义认证策略 
   >
   > * @EnableWebSecurity：开启WebSecurity模式  @Enablexxxx开启某个功能
   >
   >   Spring Security的两个主要目标是 “认证” 和 “授权”（访问控制）。 
   >
   >   **“认证”（Authentication）** 
   >
   >   身份验证是关于验证您的凭据，如用户名/用户ID和密码，以验证您的身份。
   >
   >   身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。 
   >
   >   **“授权” （Authorization）** 
   >
   >   授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置，几乎任何内容）的完全权限。 
   >
   >   这个概念是通用的，而不是只在Spring Security 中存在。 

4. 认证和授权

   > 目前，我们的测试环境，是谁都可以访问的，我们使用 Spring Security 增加上认证和授权的功能 
   >
   > 1. 引入 Spring Security 模块 
   >
   >    ```xml
   >    <dependency>
   >        <groupId>org.springframework.boot</groupId>
   >        <artifactId>spring-boot-starter-security</artifactId>
   >    </dependency>
   >    ```
   >
   > 2. 编写 Spring Security 配置类
   >
   >    参考官网： https://spring.io/projects/spring-security 
   >
   >    查看我们自己项目中的版本，找到对应的帮助文档
   >
   >    ![1617436205231](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617436205231.png)
   >
   > 3. 编写基础配置类
   >
   >    ```java
   >    package com.kuang.config;
   >    
   >    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
   >    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
   >    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
   >    
   >    // AOP：不用改变原有的代码，就可以对方法进行操作
   >    @EnableWebSecurity // 开启 WebSecurity 模式
   >    public class SecurityConfig extends WebSecurityConfigurerAdapter {
   >    
   >        // 链式编程
   >        @Override
   >        protected void configure(HttpSecurity http) throws Exception {
   >    
   >        }
   >    }
   >    
   >    ```
   >
   > 4. 定制请求的授权规则
   >
   >    ```java
   >        @Override
   >        protected void configure(HttpSecurity http) throws Exception {
   >            // 首页所有人可以访问，功能页只有对应有权限的人可以访问
   >            // 请求授权的规则
   >            http.authorizeRequests()
   >                    .antMatchers("/").permitAll()
   >                    .antMatchers("/level1/**").hasRole("vip1")
   >                    .antMatchers("/level2/**").hasRole("vip2")
   >                    .antMatchers("/level3/**").hasRole("vip3");
   >        }
   >    ```
   >
   > 5. 测试一下：发现除了首页都进不去了！因为我们目前没有登录的角色，因为请求需要登录的角色拥有对应的权限才可以！
   >
   > 6. 在configure( )方法中加入以下配置，来气自动配置的登录功能 
   >
   >    ```java
   >            // 没有权限会默认回到登录页面
   >    		// /login 请求来到登录页
   >    		// /login？error 重定向到这里表示登录失败
   >            http.formLogin();
   >    ```
   >
   > 7. 测试一下：发现，没有权限的时候，会跳转到登录的页面！
   >
   >    ![1617436635824](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617436635824.png)
   >
   > 8. 查看刚才登录页的注释信息
   >
   >    我们可以定义认证规则，重写configure(AuthenticationManagerBuilder auth)方法
   >
   >    ```java
   >    //定义认证规则
   >    @Override
   >    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   >       
   >       //在内存中定义，也可以在jdbc中去拿....
   >       auth.inMemoryAuthentication()
   >              .withUser("kuangshen").password("123456").roles("vip2","vip3")
   >              .and()
   >              .withUser("root").password("123456").roles("vip1","vip2","vip3")
   >              .and()
   >              .withUser("guest").password("123456").roles("vip1","vip2");
   >    }
   >    ```
   >
   > 9. 测试，我们可以使用这些账号登录进行测试！发现会报错
   >
   >    There is no PasswordEncoder mapped for the id “null” 
   >
   >    ![1617437882756](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617437882756.png)
   >
   > 10. 原因，我们要将前端传过来的密码进行某种方式加密，否则就无法登录，修改代码
   >
   >     ```java
   >         // 认证，springboot 2.1.x 可以直接使用
   >         // 密码编码：PasswordEncoder
   >         // 在Spring Security 5.0+ 新增了很多的加密方式,也改变了密码的格式
   >         // 要想我们的项目还能够正常登录，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
   >         // spring security 官方推荐的是使用bcrypt加密方式
   >         @Override
   >         protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   >     
   >             //这些数据正常应该从数据库中选择
   >     
   >             auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
   >                     .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","yip3")
   >                     .and()
   >                     .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
   >                     .and()
   >                     .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1");
   >         }
   >     ```
   >
   > 11. 测试，发现登录成功，并且每个角色只能访问自己认证下的规则！
   
5. 权限控制和注销

   > 1. 开启自动配置和注销的功能
   >
   >    ```java
   >    //定制请求的授权规则
   >    @Override
   >    protected void configure(HttpSecurity http) throws Exception {
   >       //....
   >       //开启自动配置的注销的功能
   >          // /logout 注销请求
   >       http.logout();
   >    }
   >    ```
   >
   > 2. 我们在前端，增加一个住校的按钮，index.html导航栏中
   >
   >    ```html
   >    <a class="item" th:href="@{/logout}">
   >       <i class="address card icon"></i> 注销
   >    </a>
   >    ```
   >
   > 3. 测试，大仙登陆成功后注销，会跳到登陆页面
   >
   > 4. 但是，我们想让他注销成功后，依旧可以转到首页
   >
   >    ```java
   >    // .logoutSuccessUrl("/"); 注销成功来到首页
   >    http.logout().logoutSuccessUrl("/");
   >    ```
   >
   > 5. 测试 ok
   >
   > 6. 我们现在又来一个需求：用户没有登录的时候，导航栏上只显示登录按钮，用户登录之后，导航栏可以显示登录的用户信息及注销按钮！还有就是，比如kuangshen这个用户，它只有 vip2，vip3功能，那么登录则只显示这两个功能，而vip1的功能菜单不显示！这个就是真实的网站情况了！该如何做呢？ 
   >
   >    我们需要结合thymeleaf中的一些功能 ， sec：authorize="isAuthenticated()":是否认证登录！来显示不同的页面 
   >
   >    Maven依赖：
   >
   >    ```xml
   >            <dependency>
   >                <groupId>org.thymeleaf.extras</groupId>
   >                <artifactId>thymeleaf-extras-springsecurity5</artifactId>
   >            </dependency>
   >    ```
   >
   > 7. 修改前端页面
   >
   >    导入命名空间
   >
   >    ```html
   >    xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5"
   >    ```
   >
   >    修改导航栏，增加认证判断
   >
   >    ```html
   >    <!--登录注销-->
   >    <div class="right menu">
   >    
   >       <!--如果未登录-->
   >       <div sec:authorize="!isAuthenticated()">
   >           <a class="item" th:href="@{/login}">
   >               <i class="address card icon"></i> 登录
   >           </a>
   >       </div>
   >    
   >       <!--如果已登录-->
   >       <div sec:authorize="isAuthenticated()">
   >           <a class="item">
   >               <i class="address card icon"></i>
   >              用户名：<span sec:authentication="principal.username"></span>
   >              角色：<span sec:authentication="principal.authorities"></span>
   >           </a>
   >       </div>
   >    
   >       <div sec:authorize="isAuthenticated()">
   >           <a class="item" th:href="@{/logout}">
   >               <i class="address card icon"></i> 注销
   >           </a>
   >       </div>
   >    </div>
   >    ```
   >
   > 8. 重启测试，我们可以登录试试看，登录成功后确实，显示了我们想要的页面； 
   >
   > 9. 如果注销404了，就是因为它默认防止csrf跨站请求伪造，因为会产生安全问题，我们可以将请求改为post表单提交，或者在spring security中关闭csrf功能；我们试试：在 配置中增加  http.csrf().disable(); 
   >
   >    ```java
   >    http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
   >    http.logout().logoutSuccessUrl("/");
   >    ```
   >
   > 10. 我们继续下面的角色功能块认证完成！
   >
   >     ```html
   >     <!-- sec:authorize="hasRole('vip1')" -->
   >     <div class="column" sec:authorize="hasRole('vip1')">
   >        <div class="ui raised segment">
   >            <div class="ui">
   >                <div class="content">
   >                    <h5 class="content">Level 1</h5>
   >                    <hr>
   >                    <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
   >                    <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
   >                    <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
   >                </div>
   >            </div>
   >        </div>
   >     </div>
   >     
   >     <div class="column" sec:authorize="hasRole('vip2')">
   >        <div class="ui raised segment">
   >            <div class="ui">
   >                <div class="content">
   >                    <h5 class="content">Level 2</h5>
   >                    <hr>
   >                    <div><a th:href="@{/level2/1}"><i class="bullhorn icon"></i> Level-2-1</a></div>
   >                    <div><a th:href="@{/level2/2}"><i class="bullhorn icon"></i> Level-2-2</a></div>
   >                    <div><a th:href="@{/level2/3}"><i class="bullhorn icon"></i> Level-2-3</a></div>
   >                </div>
   >            </div>
   >        </div>
   >     </div>
   >     
   >     <div class="column" sec:authorize="hasRole('vip3')">
   >        <div class="ui raised segment">
   >            <div class="ui">
   >                <div class="content">
   >                    <h5 class="content">Level 3</h5>
   >                    <hr>
   >                    <div><a th:href="@{/level3/1}"><i class="bullhorn icon"></i> Level-3-1</a></div>
   >                    <div><a th:href="@{/level3/2}"><i class="bullhorn icon"></i> Level-3-2</a></div>
   >                    <div><a th:href="@{/level3/3}"><i class="bullhorn icon"></i> Level-3-3</a></div>
   >                </div>
   >            </div>
   >        </div>
   >     </div>
   >     ```
   >
   > 11. 测试

6. 记住我

   > 现在的情况，我们只要登录之后，关闭浏览器，再登录，就会让我们重新登录，但是很多网站的情况，就是有一个记住密码的功能，这个该如何实现呢？很简单 
   >
   > 1. 开启记住我功能
   >
   >    ```java
   >    //定制请求的授权规则
   >    @Override
   >    protected void configure(HttpSecurity http) throws Exception {
   >    //。。。。。。。。。。。
   >       //记住我
   >       http.rememberMe();
   >    }
   >    ```
   >
   > 2. 我们再次启动项目测试一下，发现登录页多了一个记住我功能，我们登录之后关闭 浏览器，然后重新打开浏览器访问，发现用户依旧存在！
   >
   >    思考：如何实现的呢？其实非常简单
   >
   >    我们可以查看浏览器的cookie
   >
   >    ![1617518639998](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617518639998.png)
   >
   > 3. 我们点击注销的时候，可以发现，spring security 帮我们自动删除了这个 cookie 
   >
   >    ![1617518659153](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617518659153.png)
   >
   > 4. 结论：登录成功后，将cookie发送给浏览器保存，以后登录带上这个cookie，只要通过检查就可以免登录了。如果点击注销，则会删除这个cookie，具体的原理我们在JavaWeb阶段都讲过了，这里就不在多说了！ 

7. 定制登录页

   > 现在这个登录页面都是spring security 默认的，怎么样可以使用我们自己写的Login界面呢？ 
   >
   > 1.  在刚才的登录页配置后面指定 loginpage 
   >
   >    ```java
   >    http.formLogin().loginPage("/tologin");
   >    ```
   >
   > 2. 然后前端也需要指向我们自己定义的 login请求 
   >
   >    ```html
   >    <a class="item" th:href="@{/tologin}">
   >       <i class="address card icon"></i> 登录
   >    </a>
   >    ```
   >
   > 3. 我们登录，需要将这些信息发送到哪里，我们也需要配置，login.html 配置提交请求及方式，方式必须为post:
   >
   >    在 loginPage()源码中的注释上有写明：
   >
   >    ![1617518765565](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1617518765565.png)
   >
   >    ```html
   >    <form th:action="@{/login}" method="post">
   >       <div class="field">
   >           <label>Username</label>
   >           <div class="ui left icon input">
   >               <input type="text" placeholder="Username" name="username">
   >               <i class="user icon"></i>
   >           </div>
   >       </div>
   >       <div class="field">
   >           <label>Password</label>
   >           <div class="ui left icon input">
   >               <input type="password" name="password">
   >               <i class="lock icon"></i>
   >           </div>
   >       </div>
   >       <input type="submit" class="ui blue submit button"/>
   >    </form>
   >    ```
   >
   > 4. 这个请求提交上来，我们还需要验证处理，怎么做呢？我们可以查看formLogin()方法的源码！我们配置接收登录的用户名和密码的参数！ 
   >
   >    ```java
   >    http.formLogin()
   >      .usernameParameter("username")
   >      .passwordParameter("password")
   >      .loginPage("/toLogin")
   >      .loginProcessingUrl("/login"); // 登陆表单提交请求
   >    ```
   >
   > 5. 在登陆页增加记住我的多选框
   >
   >    ```html
   >    <input type="checkbox" name="remember"> 记住我
   >    ```
   >
   > 6. 后端验证处理
   >
   >    ```java
   >    //定制记住我的参数！
   >    http.rememberMe().rememberMeParameter("remember");
   >    ```
   >
   > 7. 测试，OK

8. 完整配置代码

   ```java
   package com.kuang.config;
   
   import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
   import org.springframework.security.config.annotation.web.builders.HttpSecurity;
   import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
   import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
   import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
   
   // AOP：不用改变原有的代码，就可以对方法进行操作
   @EnableWebSecurity // 开启 WebSecurity 模式
   public class SecurityConfig extends WebSecurityConfigurerAdapter {
   
       // 链式编程
       @Override
       protected void configure(HttpSecurity http) throws Exception {
           // 首页所有人可以访问，功能页只有对应有权限的人可以访问
           // 请求授权的规则
           http.authorizeRequests()
                   .antMatchers("/").permitAll()
                   .antMatchers("/level1/**").hasRole("vip1")
                   .antMatchers("/level2/**").hasRole("vip2")
                   .antMatchers("/level3/**").hasRole("vip3");
   
           // 没有权限会默认回到登录页面，需要开启登录的页面
               // /login
           http.formLogin().loginPage("/tologin");
   
           // 注销：开启了注销功能，跳到首页
           // 防止网站工具：get，post
           http.csrf().disable(); // 关闭csrf功能
           http.logout().logoutSuccessUrl("/tologin");
   
           //开启记住我功能   cookie,默认保存两周，自定义接受前端的参数
           http.rememberMe().rememberMeParameter("remember");
       }
   
       // 认证，springboot 2.1.x 可以直接使用
       // 密码编码：PasswordEncoder
       // 在Spring Security 5.0+ 新增了很多的加密方式,也改变了密码的格式
       // 要想我们的项目还能够正常登录，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
       // spring security 官方推荐的是使用bcrypt加密方式
       @Override
       protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   
           //这些数据正常应该从数据库中选择
   
           auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                   .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","yip3")
                   .and()
                   .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
                   .and()
                   .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1");
       }
   }
   ```

   









