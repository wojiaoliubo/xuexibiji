# SpringBoot集成Redis

## 概述

	> 1. SpringData
	>
	>    SpringBoot操作数据都是使用——SpringData
	>
	>    以下是Spring官网中描述的SpringData可以整合的数据源
	>
	>    ![1619096402103](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619096402103.png)
	>
	>    可以发现Spring Data Redis
	>
	> 2. lettuce
	>
	>    在SpringBoot 2.X之后，原来的Jedis被替换为了lettuce
	>
	>    ![1619096505897](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619096505897.png)
	>
	> 3. **Jedis 和 lettuce 区别**
	>
	>    Jedis：采用的是直连的服务，如果有多个线程操作的话是不安全的，就需要使用Jedis Pool连接池去解决，问题相对较多。
	>
	>    lettuce：底层采用Netty，实例可以在多个线程中共享，不存在线程不安全的情况。可以减少线程数据，性能相对较高。

## 部分源码

> 1. 自动配置
>
>    1. 找到 **spring.factories**
>
>        spring-boot-autoconfigure-2.4.4.RELEASE.jar → META-INF → spring.factories 
>
>    2. 在 spring.factories 中搜索 redis
>
>       ![1619097090979](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619097090979.png)
>
>       可以得出配置Redis，只需要配置 RedisAutoConfiguration 即可
>
>    3. 点进 RedisAutoConfiguration
>
>       ![1619097221058](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619097221058.png)
>
>    4. 点进 RedisProperties
>
>       ![1619097340255](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619097340255.png)
>
>    5. 回到 RedisAutoConfiguration，观察它做了什么
>
>       ![1619097644853](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619097644853.png)
>
> 2. **Jedis.pool 不生效**
>
>    1. 在 RedisAutoConfiguration 类中的 RedisTemplate 方法需要传递一个 RedisConnectionFactory 参数。点进这个参数
>
>    2. 这是一个接口，查看实现类
>
>       ![1619097976648](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619097976648.png)
>
>    3. 查看 Jedis 实现类，下载源码
>
>       ![1619098094785](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619098094785.png)
>
>       会发现，这个类中很多没有实现的地方。所以 Jedis Pool 不生效
>
>    4. 查看 Lettuce 的实现类
>
>       ![1619098191012](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619098191012.png)
>
>       没有问题
>
>    5. 这也说明SpringBoot更推荐使用 Lettuce

## 使用

> 1. 新建一个 SpringBoot 项目，勾选上以下
>
>    ![1619098322248](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619098322248.png)
>
> 2. 相关依赖
>
>    ![1619098375612](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1619098375612.png)
>
> 3. 配置连接，application.yml
>
>    ```yaml
>    # 配置 Redis
>    spring:
>      redis:
>        host: 192.168.142.120
>        port: 6379
>    ```
>
> 4. 测试
>
>    ```java
>    
>    ```
>
>    