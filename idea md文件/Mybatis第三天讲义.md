# Mybatis框架课程第三天

1. mybatis中的连接池以及事务控制                                                       原理部分了解，应用部分会用

   1. mybatis中连接池使用及分析
   2. mybatis事务控制的分析

2. mybatis基于XML配置的动态SQL语句使用                                        会用即可     

   1. mappers配置文件中的几个标签：

      ```xml
      <if>
      <where>
      <foreach>
      <sql>
      ```

3. mybatis中的多表操作                                                                          掌握应用

   1. 一对多
   2. 一对一(?)
   3. 多对多

## mybatis中的连接池以及事务控制

1. 连接池：

   ```
   我们在实际开发中都会使用连接池。
   因为它可以减少我们获取连接所消耗的时间。
   
   连接池就是用于存储连接的容器。
   容器就是一个集合对象，该集合必须是线程安全的，不能两个线程拿到同一连接。该集合还必须实现队列的特性：先进先出。
   ```

2. mybatis中的连接池：

   ```
   mybatis连接池提供了3中方式的配置：
   	配置的位置：
   		主配置文件SqlMapConfig.xml中的dataSource标签，type属性就是表示采用何种连接池方式。
       type属性的取值：
       	POOLED		采用传统的javax.sql.DataSource规范中的连接池，mybatis中有针对规范的实现。
       	UNPOOLED	采用传统的获取连接的方式，虽然也实现了Javax.sql.DataSource接口，但是没有实现					池的思想。
       	JNDI		采用服务器提供的JNDI技术实现，来获取DataSource对象，不同的服务器所能拿到的					DataSource是不一样的。注意如果不是web或者maven的war工程，是不能使用的。我们课					 程中使用的时tomcat服务器，采用的连接池就是dbcp连接池。
   ```

3. mybatis中的事务

   ```
   什么是事务
   事务的四大特性ACID
   不考虑隔离性会产生的3个问题
   解决办法：四种隔离级别
   
   它是sqlsession对象的commit
   ```

## Mybatis的动态SQL语句

1. 动态SQL之<if>标签

   - 持久层Dao接口：IUserDao.java

     ```java
         /**
          * 根据传入的参数条件查询
          * @param user 查询的条件：有可能有用户名，有可能有用户名，有可能有性别，有可能有地址，还有可能都有
          * @return
          */
         List<User> findUserByCondition(User user);
     ```

   - 持久层Dao映射配置：IUserDao.xml

     ```xml
         <!--根据条件查询-->
         <select id="findUserByCondition" resultMap="userMap" parameterType="user">
             select * from user where 1=1
             <if test="userName != null">
                 and username = #{userName}
             </if>
             <if test="userSex != null">
                 and sex = #{userSex}
             </if>
         </select>
         <!--注意：<if>标签中的test属性中写的是对象的属性名-->
     ```

   - 测试

     ```java
         /**
          * 测试条件查询
          */
         @Test
         public void testFindByCondition() {
             User u = new User();
             u.setUserName("老王");
     //        u.setUserSex("女");
             List<User> users = userDao.findUserByCondition(u);
             for (User user : users) {
                 System.out.println(user);
             }
         }
     ```

2. 动态SQL之<where>标签：目的是为了简化上面的where 1 = 1的条件拼装。

   - 持久层Dao映射配置

     ```xml
         <select id="findUserByCondition" resultMap="userMap" parameterType="user">
             select * from user
             <where>
             <if test="userName != null">
                 and username = #{userName}
             </if>
             <if test="userSex != null">
                 and sex = #{userSex}
             </if>
             </where>
         </select>
     ```

3. 动态标签之<foreach>标签：传入多个id查询用户信息。

   1. 在QueryVo中加入一个List集合用于封装参数

      ```java
      package com.itheima.domain;
      
      import java.util.List;
      
      public class QueryVo {
          
          private List<Integer> ids;
          
          public List<Integer> getIds() {
              return ids;
          }
      
          public void setIds(List<Integer> ids) {
              this.ids = ids;
          }
      }
      ```

   2. 持久层Dao接口

      ```java
          /**
           * 根据QueryVo中提供的id集合查询用户信息
           * @return
           */
          List<User> findUserInIds(QueryVo vo);
      ```

   3. 持久层Dao映射配置

      ```xml
          <!--根据queryvo中的id集合实现查询用户列表-->
          <select id="findUserInIds" resultMap="userMap" parameterType="queryvo">
              select * from user
              <where>
                  <if test="ids != null and ids.size() > 0">
                      <foreach collection="ids" open="and id in ( " close=")" item="uid" separator=",">
                          #{uid}
                      </foreach>
                  </if>
              </where>
          </select>
      <!--
      SQL语句：
      	select 字段 from user where id in (?)
      	<foreach>标签用于遍历集合，它的属性：
      	coolection：代表要遍历的集合元素
      	open：代表语句开始部分
      	close：代表结束部分
      	item：代表遍历集合的每个元素，生成的变量名
      	sperator：代表分隔符
      -->
      ```

   4. 测试：

      ```java
          /**
           * 测试子查询foreach标签的使用
           */
          @Test
          public void testFindInIds() {
              QueryVo vo = new QueryVo();
              List<Integer> list = new ArrayList<Integer>();
              list.add(41);
              list.add(42);
              list.add(43);
              list.add(46);
              vo.setIds(list);
              List<User> ids = userDao.findUserInIds(vo);
              for (User user : ids) {
                  System.out.println(user);
              }
          }
      ```

4. Mybatis中简化编写的SQL片段

   - Sql中可将重复的sql提取出来，使用时用include引用即可，最终达到sql重用的目的

   1. 定义代码块：IUserDao.xml

      ```xml
          <!--了解的内容，抽取重复的sql语句-->
          <sql id="defaultUser">
              select * from user
          </sql>
      ```

   2. 引用代码片段

      ```xml
          <!--查询所有-->
          <select id="findAll" resultMap="userMap">
              <include refid="defaultUser"></include>
          </select>
      
      	<!--根据queryvo中的id集合实现查询用户列表-->
          <select id="findUserInIds" resultMap="userMap" parameterType="queryvo">
              <include refid="defaultUser"></include>
              <where>
                  <if test="ids != null and ids.size() > 0">
                      <foreach collection="ids" open="and id in ( " close=")" item="uid" separator=",">
                          #{uid}
                      </foreach>
                  </if>
              </where>
          </select>
      ```

## Mybatis多表查询

1. 表之间的关系：

   ```
   表之间的关系有几种：
   	一对多
   	多对一
   	一对一
   	多对多
   举例：
   	用户和订单就是一对多
   	订单和用户就是多对一
   		一个用户可以下多个订单
   		多个订单属于同一个用户
       人和身份证就是一对一
       	一个人只能有一个身份证号
       	一个身份证号只能属于一个人
       老师和学生之间就是多对多
       	一个学生可以对多个老师教过
       	一个老师可以教多个学生
   特例：
   	如果拿出每一个订单，他都只能属于一个用户。
   	所以Mybatis就把多对一看成了一对一。
   ```

2. mybatis中的多表查询：

   ```
   示例：用户和账户
   	一个用户可以有多个账户
   	一个账户只能属于一个用户（多个账户也可以属于同一个用户）
   步骤：
   	1.建立两张表：用户表、账户表
   		让用户表和账户表之间具备一对多的关系：需要使用外键在账户表中添加
   	2.建立两个实体类：用户实体类和账户实体类
   		让用户和账户的实体类能体现出来一对多的关系
   	3.建立两个配置文件
   		用户的配置文件
   		账户的配置文件
   	4.实现配置：
   		当我们查询用户时，可以同时得到用户所包含的账户信息
   		当我们查询账户时，可以同时得到账户的所属用户信息
   ```

   ### 一对一(多对一)查询

   ```
   需求
   	查询所有的账户信息，关联查询下单用户信息
   注意：
   	因为一个账户信息只能供某个用户使用，所以从查询账户信息发出关联查询用户信息为一对一查询。如果从用户信息发出查询用户下的账户信息则为一对多查询，因为一个用户可以有多个账户。
   ```

   1. 方式一

      1. 定义账户信息实体类Account.java

         ```java
         package com.itheima.domain;
         
         import java.io.Serializable;
         
         public class Account implements Serializable {
         
             private Integer id;
             private Integer uid;
             private Double money;
         
             public Integer getId() {
                 return id;
             }
         
             public void setId(Integer id) {
                 this.id = id;
             }
         
             public Integer getUid() {
                 return uid;
             }
         
             public void setUid(Integer uid) {
                 this.uid = uid;
             }
         
             public Double getMoney() {
                 return money;
             }
         
             public void setMoney(Double money) {
                 this.money = money;
             }
         
             @Override
             public String toString() {
                 return "Account{" +
                         "id=" + id +
                         ", uid=" + uid +
                         ", money=" + money +
                         '}';
             }
         }
         
         ```

      2. 编写sql语句

         ```sql
         select a.*,u.username,u.address from account a , user u where u.id = a.uid;
         ```

      3. 定义AccountUser类

         ```
         为了能够封装上边SQL语句的查询结果，定义AccountUser类，其中要包含账户信息还要包含用户信息，所以我们在定义AccountUser类时可以继承Account类。
         ```

         ```java
         package com.itheima.domain;
         
         public class AccountUser extends Account{
         
             private String username;
             private String address;
         
             public String getUsername() {
                 return username;
             }
         
             public void setUsername(String username) {
                 this.username = username;
             }
         
             public String getAddress() {
                 return address;
             }
         
             public void setAddress(String address) {
                 this.address = address;
             }
         
             @Override
             public String toString() {
                 return super.toString() + "      AccountUser{" +
                         "username='" + username + '\'' +
                         ", address='" + address + '\'' +
                         '}';
             }
         }
         ```

      4. 定义账户的持久层Dao接口：IAccountDao.java

         ```java
         package com.itheima.dao;
         
         import com.itheima.domain.Account;
         import com.itheima.domain.AccountUser;
         
         import java.util.List;
         
         public interface IAccountDao {
                 List<AccountUser> findAllAccount();
         }
         ```

      5. 定义IAccountDao.xml文件中的查询配置信息

         ```xml
             <!--查询所有账户，同时包含用户名和地址信息-->
             <select id="findAllAccount" resultType="accountuser">
                 select a.*,u.username,u.address from account a , user u where u.id = a.uid;
             </select>
         ```

      6. 测试：AccountTest.java

         ```java
             /**
              * 测试查询所有账户，还要获取当前账户的所属用户信息
              */
             @Test
             public void testFindAllAccount() {
                 List<AccountUser> aus = accountDao.findAllAccount();
                 for (AccountUser au : aus) {
                     System.out.println(au);
                 }
             }
         ```

      7. 小结

         ```
         定义专门的类作为输出类型，其中定义了sql查询结果集中所有的字段。此方法较为简单，企业中使用普遍。
         ```

   2. 方式二

      ```
      	使用resultMap，定义专门的resultMap用于映射一对一查询结果。
      	通过面向对象的(has a)关系可以得知，我们可以在Account类中加入一个User类的对象来代表这个账户是哪个用户的。
      ```

      1. 修改Account类：Account.java

         ```java
         package com.itheima.domain;
         
         import java.io.Serializable;
         
         public class Account implements Serializable {
         
             private Integer id;
             private Integer uid;
             private Double money;
         //!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
             //从表实体应该包含一个主表实体的对象引用
             private User user;
         
             public User getUser() {
                 return user;
             }
         
             public void setUser(User user) {
                 this.user = user;
             }
         //!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
             public Integer getId() {
                 return id;
             }
         
             public void setId(Integer id) {
                 this.id = id;
             }
         
             public Integer getUid() {
                 return uid;
             }
         
             public void setUid(Integer uid) {
                 this.uid = uid;
             }
         
             public Double getMoney() {
                 return money;
             }
         
             public void setMoney(Double money) {
                 this.money = money;
             }
         
             @Override
             public String toString() {
                 return "Account{" +
                         "id=" + id +
                         ", uid=" + uid +
                         ", money=" + money +
                         '}';
             }
         }
         ```

      2. 修改AccountDao接口中的方法：

         ```java
             /**
              * 查询所有账户,同时获取账户的所属用户名称以及他的地址信息
              * @return
              */
             List<Account> findAll();
         ```

      3. 重新定义AccountDao.xml文件

         ```xml
         <!--定义封装account和user的resultMap-->
             <resultMap id="accountUserMap" type="account">
                 <id property="id" column="aid"></id>
                 <result property="uid" column="uid"></result>
                 <result property="money" column="money"></result>
                 <!--一对一的关系映射，配置封装user的内容-->
                 <association property="user" column="uid" javaType="user">
                     <id property="id" column="id"></id>
                     <result  property="username" column="username"></result>
                     <result property="address" column="address"></result>
                     <result property="sex" column="sex"></result>
                     <result property="birthday" column="birthday"></result>
                 </association>
             </resultMap>
         
             <!--查询所有账户-->
             <select id="findAll" resultMap="accountUserMap">
                 select u.*,a.id as aid,a.uid,a.money from account a,user u where u.id = a.uid;
             </select>
         ```

      4. 在AccountTest类中加入测试方法

         ```java
             /**
              * 测试查询所有账户
              */
             @Test
             public void testFindAll() {
                 List<Account> accounts = accountDao.findAll();
                 for (Account account : accounts) {
                     System.out.println("-------------------每个Account的信息----------");
                     System.out.println(account);
                     System.out.println(account.getUser());
                 }
             }
         ```

   ### 一对多查询

   ```
   需求：
   	查询所有用户信息及用户关联的账户信息。
   分析：
   	用户信息和他的账户信息作为一对多关系，并且查询过程中如果没用户没有账户信息，此时也要将用户信息查询出来，我们想到了左外连接查询比较合适。
   ```

   1. 编写SQL语句

      ```sql
      select u.*,a.id as aid,a.uid,a.money from user u left outer join account a on u.id = a.uid;
      ```

   2. User类中加入List<Account>

      ```java
      package com.itheima.domain;
      
      import java.io.Serializable;
      import java.util.Date;
      import java.util.List;
      
      public class User implements Serializable {
      
          private Integer id;
          private String username;
          private String address;
          private String sex;
          private Date birthday;
      //!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
          //一对多关系映射，主表实体应该包含从表实体的集合引用
          private List<Account> accounts;
      
          public List<Account> getAccounts() {
              return accounts;
          }
      
          public void setAccounts(List<Account> accounts) {
              this.accounts = accounts;
          }
      //!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
          public Integer getId() {
              return id;
          }
      
          public void setId(Integer id) {
              this.id = id;
          }
      
          public String getUsername() {
              return username;
          }
      
          public void setUsername(String username) {
              this.username = username;
          }
      
          public String getAddress() {
              return address;
          }
      
          public void setAddress(String address) {
              this.address = address;
          }
      
          public String getSex() {
              return sex;
          }
      
          public void setSex(String sex) {
              this.sex = sex;
          }
      
          public Date getBirthday() {
              return birthday;
          }
      
          public void setBirthday(Date birthday) {
              this.birthday = birthday;
          }
      
          @Override
          public String toString() {
              return "User{" +
                      "id=" + id +
                      ", username='" + username + '\'' +
                      ", address='" + address + '\'' +
                      ", sex='" + sex + '\'' +
                      ", birthday=" + birthday +
                      '}';
          }
      }
      ```

   3. 用户持久层Dao接口中加入查询方法

      ```java
          /**
           * 查询所有的用户，同时获取到用户下所有的账户的信息
           * @return
           */
          List<User> findAll();
      ```

   4. 用户持久层Dao映射文件配置

      ```xml
          <!--定义User的resultMap-->
          <resultMap id="userAccountMap" type="user">
              <id property="id" column="id"></id>
              <result property="username" column="username"></result>
              <result property="address" column="address"></result>
              <result property="sex" column="sex"></result>
              <result property="birthday" column="birthday"></result>
              <!--配置user对象中accounts集合的映射-->
              <!--collection是用于建立一对多中集合属性的对应关系
      			ofType是用于指定集合元素的数据类型
      -->
              <collection property="accounts" column="uid" ofType="account">
                  <id property="id" column="aid"></id>
                  <result property="uid" column="uid"></result>
                  <result property="money" column="money"></result>
              </collection>
          </resultMap>
      
          <!--查询所有-->
          <select id="findAll" resultMap="userAccountMap">
              select u.*,a.id as aid,a.uid,a.money from user u left outer join account a on u.id = a.uid;
          </select>
      <!--
      	collection：定义了用户关联的账户信息。表示关联结果集
      	property="accList"：关联查询的结果集存储在User对象上的哪个属性
      	ofType="account"：指定关联查询的结果集中的对象类型即List中的对象类型。此处可以使用别名，也可以使用全限定类名。
      -->
      ```

   5. 测试方法

      ```java
          /**
           * 测试查询所有账户
           */
          @Test
          public void testFindAll() {
              List<User> users = userDao.findAll();
              for (User user : users) {
                  System.out.println("---------每个用户的信息------------");
                  System.out.println(user);
                  System.out.println(user.getAccounts());
              }
          }
      ```

   ### 多对多

   ```
   示例：用户和角色
   	一个用户可以有多个角色
   	一个角色可以赋予多个用户
   步骤：
   	1.建立两张表：用户表，角色表
   		让用户表和角色表具有多对多的关系。需要使用中间表，中间表中包含各自的主键，在中间表中是外键。
   	2.建立两个实体类：用户实体类和角色实体类
   		让用户和角色的实体类体现出来多对多的关系
   		各自包含对方一个集合引用
       3.建立两个配置文件
       	用户的配置文件
       	角色的配置文件
       4.实现配置：
       	当我们查询用户时，可以同时得到用户所包含的角色信息
       	当我们查询角色时，可以同时得到角色所赋予的用户信息
   ```

   ![](D:\桌面\001\idea笔记\idea图片\mybatis\用户角色分析.png)

   1. 编写SQL

      ```
      需求：
      	实现查询所有对象并且加载她所分配的用户信息。
      分析：
      	查询角色我们需要用到Role表，但角色分配的用户松门并不能直接找到用户信息，二十要通过中间表(USER_ROLE表)才能关联到用户信息。
      ```

      ```sql
      select u.*,r.id as rid,r.role_name,r.role_desc from role r
              left outer join user_role ur on r.id = ur.rid
              left outer join user u on u.id = ur.uid
      ```

   2. 编写角色实体类

      ```java
      package com.itheima.domain;
      
      import java.io.Serializable;
      import java.util.List;
      
      public class Role implements Serializable {
      
          private Integer roleId;
          private String roleName;
          private String roleDesc;
      
          //多对多的关系映射，一个角色可以赋予多个用户
          private List<User> users;
      
          public List<User> getUsers() {
              return users;
          }
      
          public void setUsers(List<User> users) {
              this.users = users;
          }
      
          public Integer getRoleId() {
              return roleId;
          }
      
          public void setRoleId(Integer roleId) {
              this.roleId = roleId;
          }
      
          public String getRoleName() {
              return roleName;
          }
      
          public void setRoleName(String roleName) {
              this.roleName = roleName;
          }
      
          public String getRoleDesc() {
              return roleDesc;
          }
      
          public void setRoleDesc(String roleDesc) {
              this.roleDesc = roleDesc;
          }
      
          @Override
          public String toString() {
              return "Role{" +
                      "roleId=" + roleId +
                      ", roleName='" + roleName + '\'' +
                      ", roleDesc='" + roleDesc + '\'' +
                      '}';
          }
      }
      
      ```

   3. 编写Role持久层接口

      ```java
      package com.itheima.dao;
      
      import com.itheima.domain.Role;
      
      import java.util.List;
      
      public interface IRoleDao {
      
          /**
           * 查询所有角色
           * @return
           */
          List<Role> findAll();
      }
      ```

   4. 编写映射文件

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <!DOCTYPE mapper
              PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <mapper namespace="com.itheima.dao.IRoleDao">
      
          <!--定义role表的resultMap-->
          <resultMap id="roleMap" type="role">
              <id property="roleId" column="rid"></id>
              <result property="roleName" column="role_name"></result>
              <result property="roleDesc" column="role_desc"></result>
              <collection property="users" ofType="user">
                  <id property="id" column="id"></id>
                  <result property="username" column="username"></result>
                  <result property="address" column="address"></result>
                  <result property="sex" column="sex"></result>
                  <result property="birthday" column="birthday"></result>
              </collection>
          </resultMap>
      
          <!--查询所有-->
          <select id="findAll" resultMap="roleMap">
              select u.*,r.id as rid,r.role_name,r.role_desc from role r
              left outer join user_role ur on r.id = ur.rid
              left outer join user u on u.id = ur.uid
          </select>
      </mapper>
      ```

   5. 测试

      ```java
      package com.itheima.test;
      
      import com.itheima.dao.IRoleDao;
      import com.itheima.dao.IUserDao;
      import com.itheima.domain.Role;
      import org.apache.ibatis.io.Resources;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      import org.junit.After;
      import org.junit.Before;
      import org.junit.Test;
      
      import java.io.IOException;
      import java.io.InputStream;
      import java.util.List;
      
      /**
       * 测试mybatis的crud操作
       */
      public class RoleTest {
          private InputStream in;
          private SqlSession sqlSession;
          private IRoleDao roleDao;
      
          @Before//用于在测试方法执行之前执行
          public void init() throws IOException {
              //1.读取字节文件，生成字节输入流
              in = Resources.getResourceAsStream("SqlMapConfig.xml");
              //2.获取SqlSessionFactory对象
              SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
              //3.获取Sqlsession对象
              sqlSession = factory.openSession();
              //4.获取dao的代理对象
              roleDao = sqlSession.getMapper(IRoleDao.class);
          }
      
          @After//用于在测试方法执行之后执行
          public void destroy() throws IOException {
              //提交事务
              sqlSession.commit();
              //6.释放资源
              sqlSession.close();
              in.close();
          }
      
          @Test
          public void testFindAll() {
              List<Role> roles = roleDao.findAll();
              for (Role role : roles) {
                  System.out.println("--------每个角色的信息");
                  System.out.println(role);
                  System.out.println(role.getUsers());
              }
          }
      
      }
      ```

   1. User 到 Role 的多对多

      ```
      	从 User 出发，我们也可以发现一个用户可以具有多个角色，这样用户到角色的关系也还是一对多关系。这样
      我们就可以认为 User 与 Role 的多对多关系，可以被拆解成两个一对多关系来实现。
      ```

      1. 编写SQL

         ```sql
                 select u.*,r.id as rid,r.role_name,r.role_desc from user u
                 left outer join user_role ur on ur.uid = u.id
                 left outer join role r on r.id = ur.rid
         ```

      2. 编写用户实体类

         ```java
         package com.itheima.domain;
         
         import java.io.Serializable;
         import java.util.Date;
         import java.util.List;
         
         public class User implements Serializable {
         
             private Integer id;
             private String username;
             private String address;
             private String sex;
             private Date birthday;
         
             private List<Role> roles;
         
             public List<Role> getRoles() {
                 return roles;
             }
         
             public void setRoles(List<Role> roles) {
                 this.roles = roles;
             }
         
             public Integer getId() {
                 return id;
             }
         
             public void setId(Integer id) {
                 this.id = id;
             }
         
             public String getUsername() {
                 return username;
             }
         
             public void setUsername(String username) {
                 this.username = username;
             }
         
             public String getAddress() {
                 return address;
             }
         
             public void setAddress(String address) {
                 this.address = address;
             }
         
             public String getSex() {
                 return sex;
             }
         
             public void setSex(String sex) {
                 this.sex = sex;
             }
         
             public Date getBirthday() {
                 return birthday;
             }
         
             public void setBirthday(Date birthday) {
                 this.birthday = birthday;
             }
         
             @Override
             public String toString() {
                 return "User{" +
                         "id=" + id +
                         ", username='" + username + '\'' +
                         ", address='" + address + '\'' +
                         ", sex='" + sex + '\'' +
                         ", birthday=" + birthday +
                         '}';
             }
         }
         ```

      3. 编写持久层接口

         ```java
         package com.itheima.dao;
         
         import com.itheima.domain.User;
         
         import java.util.List;
         
         /**
          * 用户的持久层接口
          */
         public interface IUserDao {
             /**
              * 查询所有的用户，同时获取到用户下所有的账户的信息
              * @return
              */
             List<User> findAll();
         
             /**
              * 根据id查询用户信息
              * @return
              */
             User findById(Integer id);
         
         }
         ```

      4. 持久层配置文件

         ```xml
         <?xml version="1.0" encoding="UTF-8"?>
         <!DOCTYPE mapper
                 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
         <mapper namespace="com.itheima.dao.IUserDao">
         
             <!--定义User的resultMap-->
             <resultMap id="userRoleMap" type="user">
                 <id property="id" column="id"></id>
                 <result property="username" column="username"></result>
                 <result property="address" column="address"></result>
                 <result property="sex" column="sex"></result>
                 <result property="birthday" column="birthday"></result>
                 <collection property="roles" ofType="role">
                     <id property="roleId" column="rid"></id>
                     <result property="roleName" column="role_name"></result>
                     <result property="roleDesc" column="role_desc"></result>
                 </collection>
             </resultMap>
         
             <!--查询所有-->
             <select id="findAll" resultMap="userRoleMap">
                 select u.*,r.id as rid,r.role_name,r.role_desc from user u
                 left outer join user_role ur on ur.uid = u.id
                 left outer join role r on r.id = ur.rid
             </select>
         
             <!--根据id查询用户-->
             <select id="findById" resultType="user" parameterType="INT">
                 select * from user where id = #{uid};
             </select>
         
         </mapper>
         ```

      5. 测试

         ```java
         package com.itheima.test;
         
         import com.itheima.dao.IUserDao;
         import com.itheima.domain.User;
         import org.apache.ibatis.io.Resources;
         import org.apache.ibatis.session.SqlSession;
         import org.apache.ibatis.session.SqlSessionFactory;
         import org.apache.ibatis.session.SqlSessionFactoryBuilder;
         import org.junit.After;
         import org.junit.Before;
         import org.junit.Test;
         
         import java.io.IOException;
         import java.io.InputStream;
         import java.util.List;
         
         /**
          * 测试mybatis的crud操作
          */
         public class UserTest {
             private InputStream in;
             private SqlSession sqlSession;
             private IUserDao userDao;
         
             @Before//用于在测试方法执行之前执行
             public void init() throws IOException {
                 //1.读取字节文件，生成字节输入流
                 in = Resources.getResourceAsStream("SqlMapConfig.xml");
                 //2.获取SqlSessionFactory对象
                 SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
                 //3.获取Sqlsession对象
                 sqlSession = factory.openSession();
                 //4.获取dao的代理对象
                 userDao = sqlSession.getMapper(IUserDao.class);
             }
         
             @After//用于在测试方法执行之后执行
             public void destroy() throws IOException {
                 //提交事务
                 sqlSession.commit();
                 //6.释放资源
                 sqlSession.close();
                 in.close();
             }
         
             @Test
             public void testFindAll() {
                 List<User> users = userDao.findAll();
                 for (User user : users) {
                     System.out.println("--------用户-------");
                     System.out.println(user);
                     System.out.println(user.getRoles());
                 }
             }
         }
         ```

## JNDI数据源

```
JNDI：Java Naming and Directory Interface。是SUN公司推出的一套规范，属于JavaEE技术之一。目的是模仿windows系统中的注册表。
```



