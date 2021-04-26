# Mybatis第四天

1. Mybatis中的延迟加载

   ```
   问题：在一对多中，当我们有一个用户，他有100个账户。
   	 在查询用户的时候，要不要把关联的账户查出来？
   	 在查询账户的时候，要不要把关联的用户查出来？
   	 
   	 在查询用户时，用户下的账户信息应该是，什么时候使用，什么时候查询的。
   	 在查询账户时，账户的所属用户信息应该是随着账户查询时一起查询出来。
   
   什么是延迟加载
   	在真正使用数据时，才发起查询，不用的时候不查询。按需加载（懒加载）
   什么是立即加载
   	不管用不用，只要一调用方法，马上发起查询。
   
   在对应的四种表关系中：一对多，多对一，一对一，多对多
   	一对多，多对多：通常情况下，我们都是采用延迟加载。
   	多对一，一对一：通常情况下，我们都是采用立即加载
   ```

2. Mybatis中的缓存

   ```
   什么是缓存？
   	存在于内存中的临时数据。
   为什么使用缓存
   	减少和数据库的交互次数，提高执行效率。
   什么样的数据能使用缓存，什么样的数据不能使用缓存
   	适用于缓存：
   		经常查询，并且不经常改变的。
   		数据的正确与否对最终结果影响不大的。
       不适用于缓存的：
       	经常改变的数据
       	数据的正确与否对数据的影响很大的。
       	例如：商品的库存，银行的汇率，故事的牌价。
   Mybatis中的一级缓存和二级缓存
   	一级缓存：
   		它指的是Mybatis中SqlSession对象的缓存。
   		当我们执行查询之后，查询的结果会同时存入到SqlSession为我们提供的一块区域中。
   		该区域的结构是一个Map。当我们再次查询同样的数据，mybatis会先去sqlsession中查看是否有，有的话		就会直接拿出来用。
   		当SqlSession对象消失时，mybatis的一级缓存也就消失了。
   	二级缓存：
   		它指的是Mybatis中SqlSessionFactory对象的缓存。由同一个SqlSessionFactory对象创建的			SqlSession共享其缓存。
   		二级缓存的使用步骤：
   			第一步：让mybatis框架支持二级缓存(在SqlMapConfig.xml中配置)
   			第二步：让当前的映射文件支持二级缓存(在IUserDao.xml中配置)
   			第三步：让当前的操作支持二级缓存(在select标签中配置)
   		二级缓存存放的内容是数据，而不是对象，所以地址值不相等
   ```

3. Mybatis中的zhujiekaifa

   ```
   环境搭建
   单表CRUD操作（代理Dao方式）
   多表查询操作
   缓存的配置
   ```

## Mybatis中的延迟加载

### 使用assocation实现延迟加载

1. 账户的持久层DAO接口

   ```java
   package com.itheima.dao;
   
   import com.itheima.domain.Account;
   
   import java.util.List;
   
   public interface IAccountDao {
       /**
        * 查询所有账户,同时获取账户的所属用户名称以及他的地址信息
        * @return
        */
       List<Account> findAll();
   }
   ```

2. 账户的持久层映射文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.itheima.dao.IAccountDao">
   
       <!--定义封装account和user的resultMap-->
       <resultMap id="accountUserMap" type="account">
           <id property="id" column="id"></id>
           <result property="uid" column="uid"></result>
           <result property="money" column="money"></result>
           <!--一对一的关系映射，配置封装user的内容
               select属性指定的内容，查询用户的唯一标志，
               column属性指定的内容，用户根据id查询时所需要的参数的值。
           -->
           <association property="user" javaType="user" select="com.itheima.dao.IUserDao.findById" column="uid" ></association>
       </resultMap>
   
       <!--查询所有账户-->
       <select id="findAll" resultMap="accountUserMap">
           select * from account
       </select>
   
   </mapper>
   <!--
   select：填写我们要调用的select映射的id
   column：填写我们要传递给select映射的参数
   -->
   ```

3. 用户的持久层接口和映射文件

   ```java
   package com.itheima.dao;
   
   import com.itheima.domain.User;
   
   import java.util.List;
   
   /**
    * 用户的持久层接口
    */
   public interface IUserDao {
           /**
        * 根据id查询用户信息
        * @return
        */
       User findById(Integer id);
   }
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.itheima.dao.IUserDao">
       
           <!--根据id查询用户-->
       <select id="findById" resultType="user" parameterType="INT">
           select * from user where id = #{uid};
       </select>
   
   </mapper>
   ```

4. 开启Mybatis的延迟加载策略

   ![](D:\桌面\001\idea笔记\idea图片\mybatis\开启延迟加载支持.png)

   * 在SqiMapConfig.xml中开启延迟加载

     ```xml
         <!--配置参数-->
         <settings>
             <!--开启mybatis支持延迟加载-->
             <setting name="lazyLoadingEnabled" value="true"/>
             <setting name="aggressiveLazyLoading" value="false"/>
         </settings>
     ```

   * 编写测试只查账户信息不查用户信息

     ```java
     package com.itheima.test;
     
     import com.itheima.dao.IAccountDao;
     import com.itheima.domain.Account;
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
     public class AccountTest {
         private InputStream in;
         private SqlSession sqlSession;
         private IAccountDao accountDao;
     
         @Before//用于在测试方法执行之前执行
         public void init() throws IOException {
             //1.读取字节文件，生成字节输入流
             in = Resources.getResourceAsStream("SqlMapConfig.xml");
             //2.获取SqlSessionFactory对象
             SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
             //3.获取Sqlsession对象
             sqlSession = factory.openSession();
             //4.获取dao的代理对象
             accountDao = sqlSession.getMapper(IAccountDao.class);
         }
     
         @After//用于在测试方法执行之后执行
         public void destroy() throws IOException {
             //提交事务
             sqlSession.commit();
             //6.释放资源
             sqlSession.close();
             in.close();
         }
     
         /**
          * 测试查询所有账户
          */
         @Test
         public void testFindAll() {
             List<Account> accounts = accountDao.findAll();
         }
     }
     ```

     * 测试结果如下

       ![](D:\桌面\001\idea笔记\idea图片\mybatis\只查询.png)

   * 既查账户信息也查用户信息时

     ```java
         /**
          * 测试查询所有账户
          */
         @Test
         public void testFindAll() {
             List<Account> accounts = accountDao.findAll();
             for (Account account : accounts) {
                 System.out.println("-------------------每个Account的信息-------------");
                 System.out.println(account);
                 System.out.println(account.getUser());
             }
         }
     ```

     * 测试结果如下：

       ![](D:\桌面\001\idea笔记\idea图片\mybatis\既查账户信息也查用户信息时.png)

## Mybatis中的缓存

## Mybatis中的注解开发

