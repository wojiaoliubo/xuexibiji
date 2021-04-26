# SSM综合练习介绍

1. 功能介绍

   1. 环境搭建
      1. 主要讲解maven工程搭建，以及基于oracle数据库的商品表信息，并完成SSM整合。
   2. 商品查询
      1. 基于SSM整合基础上完成商品查询，要掌握主要页面main.jsp及商品页面product-list.jsp页面的创建。
   3. 商品添加
      1. 进一步巩固SSM整合，并完成商品添加功能，要注意事务操作以及product-add.jsp页面生成。
   4. 订单查询
      1. 订单的操作查询，它主要完成简单的多表查询操作，查询订单时，需要查询出与订单关联的其它表中信息。
   5. 订单分页查询
      1. 订单分页查询，我们使用的是mybatis分页插件PageHelper，要掌握PageHelper的基本使用。
   6. 订单详情查询
      1. 订单详情是用于查询某一个订单的信息，这个知识点主要考核学生对复杂的多表查询操作的掌握。
   7. Spring Security概述
      1. Spring Security是Spring项目组中用来提供安全认证服务的框架，它的使用很复杂，我们在课程中只介绍了spring Security的基本操作，大家要掌握spring Security框架的配置及基本的认证与授权操作。
   8. 用户管理
      1. 用户管理中我们会介绍基于spring Security的用户登录、退出操作。以及用户查询、添加、详情等操作，这些功能的练习是对前期SSM知识点的进一步巩固。
   9. 角色管理
      1. 角色管理主要完成角色查询、角色添加。
   10. 资源权限管理
       1. 资源权限管理主要完成查询、添加操作，他的操作与角色管理类似，角色管理以及资源权限管理都是对权限管理的补充。
   11. 权限关联与控制
       1. 主要会讲解用户角色关联、角色权限关联，这两个操作是为了后续我们完成权限操作的基础，关于授权操作，我们会在服务器及页面端分别讲解。
   12. AOP日志处理
       1. AOP日志处理，我们使用spring AOP切面来完成系统级别的日志收集。

2. 数据库介绍

   1. 产品表

      ![1616305213902](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616305213902.png)

   2. 订单表

      ![1616305240513](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616305240513.png)

   3. 会员表

      ![1616305382500](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616305382500.png)

   4. 旅客表

      ![1616305407569](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616305407569.png)

   5. 用户表

      ![1616305481700](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616305481700.png)

   6. 角色表

      ![1616305510826](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616305510826.png)

   7. 资源权限表

      ![1616305533592](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616305533592.png)

   8. 日志表

      ![1616305554064](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616305554064.png)

