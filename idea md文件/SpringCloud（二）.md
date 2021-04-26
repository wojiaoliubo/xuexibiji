# SpringCloud（二）

## 3、SpringCloud入门概述

1. Spring官网：https://spring.io/

   ![1619260710031](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619260710031.png)

   ![](D:\桌面\001\idea笔记\idea图片\springcloud整体.svg)

   SpringCloud，基于SpringBoot提供了一套微服务解决方案，包括服务注册与发现，配置中心，全链路监控，服务网关，负载均衡，熔断器等组件，除了基于Netflix的开源组件做高度抽象封装之外，还有一些选型中立的开源组件。

   SpringCloud利用SpringBoot的开发便利性，巧妙地简化了分布式系统基础设施的开发，SpringCloud为开发人员提供了快速构建分布式系统的一些工具，**包括配置管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策精选，分布式会话等等**，他们都可以用SpringBoot的开发风格做到一键启动和部署。

   SpringBoot并没有重复造轮子，它只是将目前各家公司开发的比较成熟，经得起实际考研的服务框架组合起来，通过SpringBoot风格再进行封装，屏蔽掉了复杂的配置和实现原理，**最终给开发者留出了一套简单易懂，易部署和易维护的分布式系统开发工具包。**

   SpringCloud是分布式微服务架构下的一站式解决方案，是各个微服务架构落地技术的集合体，俗称微服务全家桶。





2. **SpringCloud和SpringBoot关系**

- SpringBoot专注于快速方便的开发单个个体微服务。
- SpringCloud是关注全局的微服务协调整理治理框架，它将SpringBoot开发的一个个单体微服务整合并管理起来，为各个微服务之间提供：配置管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策竞选，分布式会话等等集成服务。
- SpringBoot可以离开SpringCloud独立使用，开发项目，但是SpringCloud离不开SpringBoot，属于依赖关系。
- **SpringBoot专注于快速、方便的开发单个个体微服务，SpringCloud关注全局的服务治理框架。**





3. **Dubbo 和 SpringCloud 技术选型**
   1. **分布式+服务治理Dubbo**

      > 目前成熟的互联网架构：应用服务化拆分 + 消息中间件

   2. 可以看一下社区活跃度

      * https://github.com/dubbo
      * https://github.com/spring-cloud

      **结果：**

      |              | Dubbo         | SpringCloud                  |
      | ------------ | ------------- | ---------------------------- |
      | 服务注册中心 | Zookeeper     | Spring Cloud Netflix Eureka  |
      | 服务调用方式 | RPC           | REST API                     |
      | 服务监控     | Dubbo-monitor | Spring Boot Admin            |
      | 断路器       | 不完善        | Spring Cloud Netflix Hystrix |
      | 服务网关     | 无            | Spring Cloud Netflix Zuul    |
      | 分布式配置   | 无            | Spring Cloud Config          |
      | 服务跟踪     | 无            | Spring Cloud Sleuth          |
      | 消息总线     | 无            | Spring Cloud Bus             |
      | 数据流       | 无            | Spring Cloud Stream          |
      | 批量任务     | 无            | Spring Cloud Task            |

      **最大区别：SpringCloud抛弃了Dubbo的RPC通信，采用的是基于HTTP的REST方式。**

      严格来说，这两种方式各有优势，虽然从一定程度上来说，后者牺牲了服务调用的性能，但也避免了上面提到的原生RPC带来的问题，而且REST比RPC更为灵活，服务提供方和调用方的依赖只依靠一纸契约，不存在代码级别的强依赖，这在强调快速演化的微服务环境下，显得更加适合。

      **品牌机与组装机的区别：**

      很明显，SpringCloud的功能比Dubbo更加强大，涵盖面更广，而且作为Spring的拳头项目，它也能够与SpringFramework，SpringBoot，SpringData，SpringBatch等其他Spring项目完美融合，这些对于微服务而言是至关重要的。使用Dubbo构建的微服务架构就像是组装电脑，各环节我们选择自由度很高，但最终结果很有可能因为一条内存质量不行就点不亮了，总是让人不怎么放心，但如果你是一名高手，那这些都不是问题：而SpringCloud就像是品牌机，在SpringSource的整合下，做了大量的兼容性测试，保证了机器有更高的稳定性，但是如果要在使用非原装组件外的东西，就需要对其基础有足够的了解。

      **社区支持与更新力度：**

      最为重要的是，Dubbo停止了5年左右的更新，虽然2017.7重启了，对于技术发展的新需求，需要由开发者自行拓展升级（比如当当网弄出了DubboX），这对于很多想要采用微服务架构的中小软件组织，显然是不太合适的，中小公司没有这么强大的技术能力去修改Dubbo源码+周边的一整套解决方案，并不是每一个公司都有大力的大牛+真实的线上生产环境测试过。

      **总结：**

      曾风靡国内的开源RPC服务框架Dubbo再重启维护后，令许多用户为之雀跃，但同时，也迎来了一些质疑的声音，互联网技术快速发展，Dubbo是否还能跟的上时代？Dubbp与SpringCloud相比又有何优势和差异？是否会有相关举措保证Dubbo的后续更新频率？

      人物：Dubbo重启维护开发的刘军，主要负责人之一

      刘军，阿里巴巴中间件高级研发工程师，主导了Dubbo重启维护以后的几个发版计划，专注一高性能RPC框架和微服务相关领域，曾负责网易考拉RPC框架的研发及指导在内部使用，参与了服务治理平台、分布式跟踪系统、分布式一致性框架等从无到有的设计与开发过程。

      **解决的问题域不一样：Dubbo的定位是一款RPC框架，SpringCloud的目标是微服务架构下的一站式解决方案。**

4. **SpringCloud能干嘛**

   - Distributed/versioned configuration（分布式/版本控制配置）
   - Service registration and discovery（服务注册与发现）
   - Routing（路由）
   - Service-to-service calls（服务到服务的调用）
   - Load balancing（负载均衡配置）
   - Circuit Breakers（断路器）
   - Distributed messaging（分布式消息管理）
   - ......

5. **SpringCloud在哪下**

   官网：https://spring.io/projects/spring-cloud/

   这个东西的版本号有些特别

   ![1619431447734](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619431447734.png)

   > 1. Spring Cloud是一个由众多独立子项目组成的大型综合项目，每个项目有不同的发行节奏，都维护着自己的发布版本号。Spring Cloud通过一个资源清单BOM（Bill of Materials）来管理每个版本号的子项目清单。为了避免与子项目的发布号混淆，所以没有采用版本号的方式，而是通过命名的方式。
   > 2. 这些版本名称的命名方式采用了伦敦地铁站的名称，同时根据字母表的顺序来对应版本时间顺序，比如：最早的Release版本：Angel，第二个Release版本：Birxton，然后是Camden、Dalston、Edgware，目前最新的是Finchley版本。

   **参考书：**

   * https://www.springcloud.cc/spring-cloud-netflix.html
   * 中文API文档：https://www.springcloud.cc/spring-cloud-dalston.html
   * SpringCloud中文网：https://www.springcloud.cc/