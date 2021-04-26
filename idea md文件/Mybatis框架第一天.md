# Mybatis



## 第一天：mybatis入门

1. mybatis的概述
2. mybatis的环境搭建
3. mybatis入门案例
4. 自定义mybatis框架（主要的目的是为了让大家了解mybatis中执行细节）

## 第二天：mybatis基本使用

1. mabatis的单表crud操作
2. mybatis的参数和返回值设置
3. mybatis的dao编写
4. mybatis配置的细节
   - 几个标签的使用

## 第三天：mybatis的深入和多表

1. mybatis的深入和多表
2. mybatis的连接池
3. mybatis的事务控制及设计的方法
4. mybatis的多表查询
   - 一对多（多对一）
   - 多对多

## 第四天：mybatis的缓存和注解开发

1. mybatis中的加载时机（查询的时机）
2. mybatis中的一级缓存和二级缓存
3. mybatis的注解开发
   - 单表CRUD
   - 多表查询

# 第一天

1. 什么是框架？

   ```txt
   它是我们软件开发中的一套解决方案，不同的框架解决的是不同的问题。
   使用框架的好处：
   	框架封装了很多的细节，使开发者可以使用极简的的方式实现功能。大大提高了开发效率。
   ```

2. 三层架构

   ```txt
   表现层：
   	是用于展示数据
   业务层：
   	是处理业务需求
   持久层：
   	是和数据库交互的  （Mybatis就是持久层框架）
   ```

3. 持久层技术解决方案

   ```
   JDBC技术：
   	Connection
   	PreparedStatement
   	RreultSet
   Spring的JdbcTemplate：
   	Spring中对jdbc的简单封装
   Apache的DBUtils：
   	它和Spring的JdbcTemplate很像，也是对Jdbc的简单封装
   	
   以上这些都不是框架
   	JDBC是规范
   	Spring的JdbcTemplate和Apache的DBUtils都只是工具类
   ```

4. mybatis的概述

   ```
   mybatis是一个持久层框架，用java编写的。
   它封装了jdbc操作的很多细节，使开发者只需要关注sql语句本身，而无序关注注册驱动，创建连接等繁琐过程。
   它使用了ORM思想实现了结果集的封装。
   
   ORM：
   	Object Relational Mappging 对象关系映射
   	简单地说：
   		就是把数据库表和实体类及实体类的属性对应起来
   		让我们可以操作实体类就实现操作数据库表	
   		user				 User
   		id					 userId
   		user_name			 userName
   		
   今天我们需要做到
   	实体类中的属性和数据库表的字段名称保持一致。
   		user				User
   		id					id
   		user_name			user_name
   ```

5. mybatis的入门

   ```java
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.5</version>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>8.0.20</version>
           </dependency>
               // 使用mybatis时需要导入这两个jar包
   ```

   1. mybatis的环境搭建

      1. 创建maven工程并导入坐标

      2. 创建实体类和dao接口

         - 实体类：User.java

           ```java
           package com.itheima.domain;
           
           import javax.xml.crypto.Data;
           import java.io.Serializable;
           
           /**
            *
            */
           public class User implements Serializable {
           
               private Integer id;
               private String username;
               private Data birthday;
               private String sex;
               private String address;
           
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
           
               public Data getBirthday() {
                   return birthday;
               }
           
               public void setBirthday(Data birthday) {
                   this.birthday = birthday;
               }
           
               public String getSex() {
                   return sex;
               }
           
               public void setSex(String sex) {
                   this.sex = sex;
               }
           
               public String getAddress() {
                   return address;
               }
           
               public void setAddress(String address) {
                   this.address = address;
               }
           
               @Override
               public String toString() {
                   return "User{" +
                           "id=" + id +
                           ", username='" + username + '\'' +
                           ", birthday=" + birthday +
                           ", sex='" + sex + '\'' +
                           ", address='" + address + '\'' +
                           '}';
               }
           }
           
           ```

         - dao接口：IUserDao.java

           ```java
           package com.itheima.dao;
           
           import com.itheima.domain.User;
           
           import java.util.List;
           
           /**
            * 用户的持久层接口
            */
           public interface IUserDao {
               /**
                * 查询所有的操作
                * @return
                */
               List<User> findAll();
           }
           ```

      3. 创建Mybatis的主配置文件

         * 主配置文件：SqlMapConfig.xml

           ```xml
           <?xml version="1.0" encoding="UTF-8"?>
           <!DOCTYPE configuration
                   PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
                   "http://mybatis.org/dtd/mybatis-3-config.dtd">
           <!--以上为固定写法-->
           
           <!--mybatis的主配置文件-->
           <configuration>
               <!--配置环境-->
               <environments default="mysql">
                   <!--配置mysql的环境-->
                   <!--此处的id值应与default的值相同-->
                   <environment id="mysql">
                       <!--配置事务管理器的类型-->
                       <transactionManager type="JDBC"></transactionManager>
                       <!--配置数据源（连接池）-->
                       <dataSource type="POOLED">
                           <!--配置连接数据库的四个基本信息-->
                           <property name="driver" value="com.mysql.jdbc.Driver"/>
                           <property name="url" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
                           <property name="username" value="root"/>
                           <property name="password" value="15591177926"/>
                       </dataSource>
                   </environment>
               </environments>
               <!--指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件-->
               <mappers>
                   <mapper resource="com/itheima/dao/IUserDao.xml"></mapper>
               </mappers>
           </configuration>
           ```

      4. 创建映射配置文件

         * 映射配置文件：IUserDap.xml

           ```xml
           <?xml version="1.0" encoding="UTF-8"?>
           <!DOCTYPE mapper
                   PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                   "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
           <!--以上为固定写法-->
           
           <mapper namespace="com.itheima.dao.IUserDao">
               <!--配置查询所有 id值应与接口中的方法名一致-->
               <select id="findAll">
                   <!--sql语句-->
                   select * from user
               </select>
           </mapper>
           ```

      * 环境搭建的注意事项：
        1. 创建IUserDao.xml 和 IUserDao.java时，所取的名称是为了和之前的知识保持一致。在Mybatis中它把持久层的操作接口名称和映射文件也叫做：Mapper。所以：IUserDao 和 IUserMapper是一样的。
        2. 在idea中创建目录的时候，它和包是不一样的。包在创建的时候：com.itheima.dao是三级目录，而目录在创建时：com.itheima.dao是一级目录。
        3. mybatis的映射配置文件位置必须和dao接口的包结构相同
        4. 映射配置文件的mapper标签的namespace属性必须是dao接口的全限定类名
        5. 映射配置文件的操作配置，id属性的取值必须是dao接口的方法名
      * 当我们遵从了第三、四、五点之后，我们在开发中就无需再写dao的实现类。

   2. mybatis的入门案例

      1. 读取配置文件
      2. 创建SqlSessionFactory工厂
      3. 创建SqlSession
      4. 创建Dao接口中的代理对象
      5. 执行dao中的方法
      6. 释放资源

      * 注意事项

        ```
        不要忘记在映射配置中告知mybatis要封装到哪个实体类中
        配置的方式：在映射配置文件中指定实体类的全限定类名
        ```

      * 代码演示：

      1. User

      ```java
      package com.itheima.domain;
      
      import java.util.Date;
      import java.io.Serializable;
      
      /**
       *
       */
      public class User implements Serializable {
      
          private Integer id;
          private String username;
          private Data birthday;
          private String sex;
          private String address;
      
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
      
          public Data getBirthday() {
              return birthday;
          }
      
          public void setBirthday(Data birthday) {
              this.birthday = birthday;
          }
      
          public String getSex() {
              return sex;
          }
      
          public void setSex(String sex) {
              this.sex = sex;
          }
      
          public String getAddress() {
              return address;
          }
      
          public void setAddress(String address) {
              this.address = address;
          }
      
          @Override
          public String toString() {
              return "User{" +
                      "id=" + id +
                      ", username='" + username + '\'' +
                      ", birthday=" + birthday +
                      ", sex='" + sex + '\'' +
                      ", address='" + address + '\'' +
                      '}';
          }
      }
      ```

      2. MybatisTest

      ```java
      package com.itheima.test;
      
      import com.itheima.dao.IUserDao;
      import com.itheima.domain.User;
      import org.apache.ibatis.session.SqlSession;
      import org.apache.ibatis.session.SqlSessionFactory;
      import org.apache.ibatis.session.SqlSessionFactoryBuilder;
      
      
      import java.io.InputStream;
      import java.util.List;
      
      /**
       *  mybatis的入门案例
       */
      public class MybatisTest {
          /**
           * 入门案例
           * @param args
           */
          public static void main(String[] args) throws Exception {
              //1.读取配置文件
              InputStream in = MybatisTest.class.getClassLoader().getResourceAsStream("com/SqlMapConfig.xml");
              //2.创建SqlSessionFactory工厂
              SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
              SqlSessionFactory factory= builder.build(in);
              //3.使用工厂生产一个SqlSession对象
              SqlSession session = factory.openSession();
              //4.使用SqlSession创建Dao接口的代理对象
              IUserDao userDao = session.getMapper(IUserDao.class);
              //5.使用代理对象执行方法
              List<User> list = userDao.findAll();
              for (User user : list) {
                  System.out.println(user);
              }
              //6.释放资源
              session.close();
              in.close();
          }
      }
      ```

      3. 映射配置文件：IUserDap.xml

         ```xml
         <?xml version="1.0" encoding="UTF-8"?>
         <!DOCTYPE mapper
                 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
         <!--以上为固定写法-->
         
         <mapper namespace="com.itheima.dao.IUserDao">
             <!--配置查询所有 id值应与接口中的方法名一致-->
             <select id="findAll" resultMap="com.itheima.domain.User">
                 <!--sql语句-->
                 select * from user
             </select>
         </mapper>
         ```

      * mybatis基于注解的入门案例：

        把 IUserDao.xml 移除，在dao接口的方法上使用 @Select 注解，并且指定SQL语句，同时需要在SqlMapConfig.xml  中的mapper配置时，使用class属性指定dao接口的全限定类名。

        * 代码演示：

          * dao接口：IUserDao.java

            ```java
            package com.itheima.dao;
            
            import com.itheima.domain.User;
            import org.apache.ibatis.annotations.Select;
            
            import java.util.List;
            
            /**
             * @author 黑马程序员
             * @Company http://www.ithiema.com
             *
             * 用户的持久层接口
             */
            public interface IUserDao {
            
                /**
                 * 查询所有操作
                 * @return
                 */
                // ！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！
                @Select("select * from user")
                List<User> findAll();
            }
            ```

          * 主配置文件：SqlMapConfig.xml

            ```xml
            <?xml version="1.0" encoding="UTF-8"?>
            <!DOCTYPE configuration
                    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
                    "http://mybatis.org/dtd/mybatis-3-config.dtd">
            <!-- mybatis的主配置文件 -->
            <configuration>
                <!-- 配置环境 -->
                <environments default="mysql">
                    <!-- 配置mysql的环境-->
                    <environment id="mysql">
                        <!-- 配置事务的类型-->
                        <transactionManager type="JDBC"></transactionManager>
                        <!-- 配置数据源（连接池） -->
                        <dataSource type="POOLED">
                            <!-- 配置连接数据库的4个基本信息 -->
                            <property name="driver" value="com.mysql.jdbc.Driver"/>
                            <property name="url" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
                            <property name="username" value="root"/>
                            <property name="password" value="15591177926"/>
                        </dataSource>
                    </environment>
                </environments>
            
                <!-- 指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件
                     如果使用注解来配置的话，此处应该使用class属性指定被注解的dao全限定类名
                 -->
                <mappers>
                    <!-- ！！！！！！！！！！！！！！！！！！！！！！！！！！！！ -->
                    <mapper class="com.itheima.dao.IUserDao"/>
                </mappers>
            </configuration>
            ```

   3. 明确

      ```
      我们在实际开发中，都是越简便越好，所以都是采用不写dao实现类的方式。不管使用XML还是注解配置。
      
      但是Mybatis它是支持写dao实现类的
      ```

6. 自定义Mybatis的分析：

   ```
   mybatis在使用代理dao的方式实现增删改查时做了什么事呢？
   	只有两件事：
   		第一：创建代理对象
   		第二：在代理对象中调用selectList
   		
   		
   自定义mybatis能通过入门案例看到的类
   	class Resource
   	class SqlSessionFactoryBuilder
   	interface SqlSessionFactory
   	interface SqlSession
   	
	详情见D盘idea_date
   ```
   
   