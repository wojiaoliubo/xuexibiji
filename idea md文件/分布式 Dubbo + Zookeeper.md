# 分布式理论

## 什么是分布式系统

> ​		在《分布式系统原理与范型》一书中有如下定义：“分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统”；
>
> ​		分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是**利用更多的机器，处理更多的数据**。
>
> ​		分布式系统（distributed system）是建立在网络之上的软件系统。
>
> ​		首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。

## Dubbo文档

> ​		随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及流动计算架构势在必行，急需**一个治理系统**确保架构有条不紊的演进。
>
> ​		在Dubbo的官网文档有这样一张图
>
> ![](D:\桌面\001\idea笔记\idea图片\dubbo.jpg)

## 单一应用架构

> ​		当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。 
>
> ​		![](D:\桌面\001\idea笔记\idea图片\单一应用架构.png)
>
> ​		适用于小型网站，小型管理系统，将所有功能都部署到一个功能里，简单易用。
>
> **缺点：**
>
> 		1. 性能扩展比较难
>   		2. 协同开发问题
>                 		3. 不利于升级维护

## 垂直应用架构

> ​		当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。 
>
> ![](D:\桌面\001\idea笔记\idea图片\垂直应用.jpg)
>
> ​		通过切分业务来实现各个模块独立部署，降低了维护和部署的难度，团队各司其职更易管理，性能扩展也更方便，更有针对性。
>
> **缺点：**
>
> * 公用模块无法重复利用，开发性的浪费

## 分布式服务架构

> ​		当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的**分布式服务框架(RPC)**是关键。 
>
> ![](D:\桌面\001\idea笔记\idea图片\分布式应用架构.png)

## 流动计算架构

> ​		当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于**提高机器利用率的资源调度和治理中心**(SOA)[ Service Oriented Architecture]是关键。
>
> ![](D:\桌面\001\idea笔记\idea图片\面向服务的分布式架构（SOA）.png)

# 什么是RPC

> ​		RPC【Remote Procedure Call】是指**远程过程调用**，是一种进程间通信方式，他是一种技术的思想，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。
>
> ​		也就是说两台服务器A，B，一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。为什么要用RPC呢？就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不同的系统间的通讯，甚至不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署应用。RPC就是要像调用本地的函数一样去调远程函数；
>
> 推荐阅读文章：https://www.jianshu.com/p/2accc2840a1b
>
> **RPC基本原理** 
>
> ![](D:\桌面\001\idea笔记\idea图片\RPC基本原理.png)
>
> **步骤解析**
>
> ![](D:\桌面\001\idea笔记\idea图片\RPC步骤解析.png)
>
> **RPC两个核心模块：通讯，序列化。**

# 测试环境搭建

## Dubbo

> ​		Apache Dubbo |ˈdʌbəʊ| 是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。
>
> dubbo官网 http://dubbo.apache.org/zh-cn/index.html
>
> 1. 了解Dubbo的特性
> 2. 查看官方文档
>
> **dubbo基本概念**
>
> ![](D:\桌面\001\idea笔记\idea图片\Dubbo架构.jpg)
>
> **服务提供者**（Provider）：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务。
>
> **服务消费者**（Consumer）：调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
>
> **注册中心**（Registry）：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者
>
> **监控中心**（Monitor）：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心
>
> **调用关系说明**
>
> l 服务容器负责启动，加载，运行服务提供者。
>
> l 服务提供者在启动时，向注册中心注册自己提供的服务。
>
> l 服务消费者在启动时，向注册中心订阅自己所需的服务。
>
> l 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
>
> l 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
>
> l 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

## Dubbo环境搭建

> 点进dubbo官方文档，推荐我们使用Zookeeper 注册中心
>
> 什么是zookeeper呢？可以查看官方文档

## Windows下安装zookeeper

1. 下载zookeeper：地址：https://downloads.apache.org/zookeeper/zookeeper-3.6.3/apache-zookeeper-3.6.3-bin.tar.gz，我们下载3.6.3，解压zookeeper

2. 安装

   1. 下载好的安装包，解压到你喜欢的目录，在根目录下简历data和logs两个文件夹。

      ![1619165984490](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619165984490.png)

   2. 接着打开/conf目录，复制一份zoo_sample.cfg文件，重命名为zoo.cfg

      ![1619166069634](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619166069634.png)

   3. 编辑zoo.cfg文件，修改内容dataDir=zookeeper根目录\data，dataLogDir=zookeeper根目录\logs

      ![1619166175699](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619166175699.png)

      这里要特别注意zookeeper服务启动时会启动一个AdminServer的服务，端口会占用8080，如果你有启动别的项目占了8080端口就会报错无法启动，所以在这添加配置 admin.serverPort=8888来将启动端口修改（8888随便填的，不冲突就行）。 

   4. 打开/bin目录，通过运行zkServer.cmd脚本来启动服务。如果双击后窗口一闪而过，啥信息也没看着，就右键编辑这个脚本，在末尾添加一句pause后保存。 

      ![1619166301860](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619166301860.png)

      运行命令启动服务：

      ![1619166384520](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619166384520.png)

      长这样没报错就是启动成功了，这个窗口先不能关，一关服务就关闭了，等一下测试完后我们再将这个启动脚本注册成系统服务。 

      再运行zkCli.cmd脚本，出来下图就是测试没问题了。

      ![1619166426880](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619166426880.png)

      ![1619166475438](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619166475438.png)

      安装成功。

   5. 使用zkCli.cmd测试 

      ```bash
      ls /  # 列出zookeeper根下保存的所有节点
      ```

      ```bash
      [zk: 127.0.0.1:2181(CONNECTED) 4] ls /
      [zookeeper]
      ```

      create –e /kuangshen1 123：创建一个kuangshen1节点，值为123 

      ![1619166679976](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619166679976.png)

      get /kuangshen：获取/kuangshen1节点的值

      ![1619166773941](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619166773941.png)

      我们再来查看一下节点

      ![1619166816077](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619166816077.png)

## windows下安装dubbo-admin

> ​		dubbo本身并不是一个服务软件。它其实就是一个jar包，能够帮你的java程序连接到zookeeper，并利用zookeeper消费、提供服务。 
>
> ​		但是为了让用户更好的管理监控众多的dubbo服务，官方提供了一个可视化的监控程序dubbo-admin，不过这个监控即使不装也不影响使用。
>
> ​		我们这里来安装一下：
>
> 1. 下载 dubbo-admin
>
>    地址： https://github.com/apache/dubbo-admin/tree/master 
>
> 2. 解压进入目录
>
>    修改 dubbo-admin\src\main\resources \application.properties 指定zookeeper地址 
>
>    ```properties
>    server.port=7001
>    spring.velocity.cache=false
>    spring.velocity.charset=UTF-8
>    spring.velocity.layout-url=/templates/default.vm
>    spring.messages.fallback-to-system-locale=false
>    spring.messages.basename=i18n/message
>    spring.root.password=root
>    spring.guest.password=guest
>    // 注册中心的地址
>    dubbo.registry.address=zookeeper://127.0.0.1:2181
>    ```
>
> 3. 打包dubbo-admin
>
>    我使用idea导入后打包
>
> 4. 执行 dubbo-admin\target下的dubbo-admin-0.0.1-SNAPSHOT.jar
>
>    【注意：zookeeper的服务一定要打开】
>
>    执行完毕，我们去访问一下http://localhost:7001/，这时候我们需要输入登陆账户和密码，我们都是默认的root-root
>
>    登陆成功后，查看页面
>
>    ![1619170165322](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619170165322.png)
>
>    安装完成！

## 框架搭建

1. 启动zookeeper

2. IDEA创建一个空项目

3. 创建一个模块，实现服务提供者：provider-server，选择web依赖即可

4. 项目创建完毕，我们写一个服务，比如卖票的服务；

   1. 编写接口

      ```java
      package com.kuang.service;
      
      public interface TicketService {
      
          public String getTicket();
      
      }
      ```

   2. 编写实现类

      ```java
      package com.kuang.service.impl;
      
      import com.kuang.service.TicketService;
      
      public class TicketServiceImpl implements TicketService {
      
          @Override
          public String getTicket() {
              return "《狂神说Java》";
          }
      }
      ```

5. 创建一个模块，实现服务消费者：consumer-server，选择web依赖即可

6. 项目创建完毕，我们写一个服务，比如用户的服务；

   1. 编写service

      ```java
      package com.kuang.service;
      
      public class UserService {
          // 我们需要去拿注册中心的服务
      }
      ```

      需求：现在我们的用户想使用买票的服务，这要怎么弄呢？

## 服务提供者

1. 将服务提供者注册到注册中心，我们需要整合Dubbo和zookeeper，所以需要导包

   我们从dubbo官网进入github，看下方的帮助文档，找到dubbo-springboot，找到依赖包

   ```xml
   
   ```

   









