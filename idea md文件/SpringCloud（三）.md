# SpringCloud（三）

## 4、Rest学习环境搭建：服务提供者

1. **总体介绍**

   - 我们会使用一个Dept部门模块做一个微服务通用案例Consumer消费者（Client）通过REST调用Provider提供者（Server）提供的服务。

   - 回忆Spring，SpringMVC，MyBatis等以往学习的知识。。。

   - Maven的分包分模块架构复习

     > 一个简单的Maven模块结构是这样的：
     >
     > 
     >
     > -- app-parent：一个父项目（app-parent）聚合很多子项目（app-util，app-dao，app-web...）
     >
     > |-- pom.xml
     >
     > |
     >
     > |-- app-core
     >
     > ||---- pom.xml
     >
     > |
     >
     > |-- app.web
     >
     > ||---- pom.xml
     >
     > ...... 

     一个父项目带着多个子Module子模块

     MircroServiceCloud父工程（Project）下初次带着3个子模块（Module）

     * microservicecloud-api【封装的整体entity/接口/公共配置等】
     * microservicecloud-provider-dept-8001【服务提供者】
     * microservicecloud-consumer-dept-80【服务消费者】

2. **SpringCloud版本选择**

   **大版本说明：**

   | Spring Boot | Spring Cloud              | 关系                                           |
   | ----------- | ------------------------- | ---------------------------------------------- |
   | 1.2.x       | Angel版本（天使）         | 兼容Spring Boot 1.2.x                          |
   | 1.3.x       | Brixton版本（布里克斯顿） | 兼容Spring Boot 1.3.x，也兼容Spring Boot 1.4.x |
   | 1.4.x       | Camden版本（卡姆登）      | 兼容Spring Boot 1.4.x，也兼容Spring Boot 1.5.x |
   | 1.5.x       | Dalston版本（多尔斯顿）   | 兼容Spring Boot 1.5.x，不兼容Spring Boot 2.0.x |
   | 1.5.x       | Edgware版本（埃奇韦尔）   | 兼容Spring Boot 1.5.x，不兼容Spring Boot 2.0.x |
   | 2.0.x       | Finchley版本（芬奇利）    | 兼容Spring Boot 2.0.x，不兼容Spring Boot 1.5.x |
   | 2.1.x       | Greenwich版本（格林威治） |                                                |

   **实际开发版本关系：**

   |      |      |      |      |
   | ---- | ---- | ---- | ---- |
   |      |      |      |      |
   |      |      |      |      |
   |      |      |      |      |
   |      |      |      |      |
   |      |      |      |      |
   |      |      |      |      |
   |      |      |      |      |
   |      |      |      |      |

   