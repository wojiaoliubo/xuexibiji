# Spring框架共四天

1. 第一天：spring框架概述以及spring中基于XML的IOC配置
2. 第二天：spring中基于注解的IOC和ioc的案例
3. 第三天：spring中的aop和基于XML以及注解的AOP配置
4. 第四天：spring中的JdbcTemlate以及Spring事务控制

## Spring第一天

1. spring概述

   ```
   spring是什么？
   	Spring 是分层的 Java SE/EE 应用 full-stack 轻量级开源框架，以 IoC（Inverse Of Control：
   反转控制）和 AOP（Aspect Oriented Programming：面向切面编程）为内核，提供了展现层 Spring 
   MVC 和持久层 Spring JDBC 以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多
   著名的第三方框架和类库，逐渐成为使用最多的 Java EE 企业应用开源框架。
   spring的两大核心
   spring的发展历程和优势
   spring体系结构
   ```

2. 程序的耦合以及解耦

   ```
   曾经案例中的问题
   工厂模式解耦
   ```

3. IOC概念和spring中的IOC

   ```
   spring中基于XML的IOC环境搭建
   ```

4. 依赖注入（Dependency Injection）

5. 作业：

   ## 工厂模式

   ```java
   package com.itheima.factory;
   
   import java.io.IOException;
   import java.io.InputStream;
   import java.util.Enumeration;
   import java.util.HashMap;
   import java.util.Map;
   import java.util.Properties;
   
   /**
    *  一个创建Bean对象的工厂
    *
    *  Bean：计算机英语中，有可重用组建的含义。
    *  JavaBean：用java语言编写的可重用组件。
    *      javabean不等于实体类，javabean的范围远大于实体类
    *
    *    他就是创建我们的service和dao对象。
    *
    *    第一个：需要一个配置文件来配置我们的service和dao
    *      配置文件的内容：唯一标志 = 全限定类型名  （key value的形式）
    *    第二个：通过读取配置文件中配置的内容，反射创建对象
    *
    *    我的配置文件可以实xml也可以是properties
    */
   public class BeanFactory {
       //定义一个prperties对象
       private static Properties props;
   
       //定义一个Map，用于存放我们要创建的对象，我们把它称之为容器
       private static Map<String,Object> beans;
   
       //使用静态代码块为properties对象赋值
   
       static {
           try {
               //1.实例化对象
               props = new Properties();
               //2.获取properties对象的流对象
               InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
               props.load(in);
               //实例化容器
               beans = new HashMap<String,Object>();
               //取出配置文件中所有的key
               Enumeration<Object> keys = props.keys();
               //遍历枚举
               while(keys.hasMoreElements()){
                   //取出每个key
                   String key = keys.nextElement().toString();
                   //根据key获取value
                   String beanPath = props.getProperty(key);
                   //反射创建对象
                   Object value = Class.forName(beanPath).newInstance();
                   //把key value存入容器
                   beans.put(key,value);
               }
           } catch (Exception e) {
               throw new ExceptionInInitializerError("初始化properties失败");
           }
       }
   
       /**
        * 根据bean的名称获取对象
        * @param beanName
        * @return
        */
       public static Object getBean(String beanName) {
           return beans.get(beanName);
       }
   
       /**
        * 根据Bean的名称获取bean对象
        * @param beanName
        * @return
        */
   //    public static Object getBean(String beanName) {
   //        Object bean = null;
   //        try {
   //            String beanPath = props.getProperty(beanName);
   //            bean = Class.forName(beanPath).newInstance();//每次都会调用默认构造函数创建对象
   //        } catch (Exception e) {
   //            e.printStackTrace();
   //        }
   //        return bean;
   //    }
   }
   ```

# spring第二天：spring基于注解的IOC以及Ioc的案例

1. spring中ioc的常用注解

2. 案例使用xml方式和注解方式实现单表的CRUD操作

   > 持久层技术选择：dbutils

3. 改造基于注解的ioc案例，使用纯注解的方式实现

   > spring的一些新注解使用

4. spring和Junit整合

## spring第三天

1. 完善我们的account案例
2. 分析案例中的问题
3. 回顾之前讲过的一个技术：动态代理
4. 动态代理的另一种实现方式
5. 解决案例中的问题
6. AOP的概念
7. spring中的AOP相关术语
8. spring中基于XML和注解的AOP配置

## spring第四天

1. spring中的JdbcTemplate

   ```
   JdbcTemplate的作用：
   	它就是用于和数据库交互的，实现对表的CRUD操作
   如何创建该对象：
   对象中的常用方法：
   ```

   