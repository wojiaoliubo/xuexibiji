## 第二天

1. 回顾mybatis的自定义再分析和环境搭建+完善基于注解的mybatis
2. Mybatis基于代理Dao的CRUD操作                                                 重点内容
3. CRUD中可能遇到的问题：参数的传递以及返回值的封装
4. mybatis中基于传统dao的方式的使用（编写dao实现类）                 ----了解内容
5. mybatis主配置文件中常用的配置（主配置文件：SqlMapConfig.xml）
   1. properties标签
   2. typeAliases标签
   3. mappers标签：package

## Mybatis基于代理Dao的CRUD操作

* 代码演示

  * IUserDao.java

    ```java
    package com.itheima.dao;
    
    import com.itheima.domain.User;
    
    import java.util.List;
    
    /**
     * 用户的持久层接口
     */
    public interface IUserDao {
        /**
         * 查询所有的用户
         * @return
         */
        List<User> findAll();
    
        /**
         * 保存方法
         * @param user
         */
        void saveUser(User user);
    
        /**
         * 更新用户
         * @param user
         */
        void updateUser(User user);
    
        /**
         * 根据id删除用户
         * @param id
         */
        void deleteUser(Integer id);
    
        /**
         * 根据id查询用户信息
         * @return
         */
        User findById(Integer id);
    
        /**
         * 根据名称模糊查询用户信息
         * @param username
         * @return
         */
        List<User> findByName(String username);
    
        /**
         * 查询总用户数
         * @return
         */
        int findTotal();
    }
    ```

  * IUserDao.xml

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.itheima.dao.IUserDao">
    
        <!--查询所有-->
        <select id="findAll" resultType="com.itheima.domain.User">
            select * from user;
        </select>
    
        <!--保存用户-->
        <!--parameterType的含义就是参数的类型-->
        <insert id="saveUser" parameterType="com.itheima.domain.User">
            <!--配置插入数据后，获取数据的id-->
            <selectKey keyProperty="id"  keyColumn="id" order="AFTER" resultType="INT">
                select last_insert_id();
            </selectKey>
            insert into user(username,address,sex,birthday)values(#{username},#{address},#{sex},#{birthday});
        </insert>
    
        <!--更新用户-->
        <update id="updateUser" parameterType="com.itheima.domain.User">
            update user set username=#{username},address=#{address},sex=#{sex},birthday=#{birthday} where id=#{id};
        </update>
    
        <!--删除用户的操作-->
        <delete id="deleteUser" parameterType="java.lang.Integer">
            delete from user where id = #{uid};
        </delete>
    
        <!--根据id查询用户-->
        <select id="findById" resultType="com.itheima.domain.User" parameterType="INT">
            select * from user where id = #{uid};
        </select>
    
        <!--根据名称模糊查询的语法-->
        <select id="findByName" parameterType="string" resultType="com.itheima.domain.User">
            select * from user where username like #{uname};
            <!--select * from user where username like '%${value}%';  不常用 -->
        </select>
        
        <!--获取用户的总记录条数-->
        <select id="findTotal" resultType="int">
            select count(id) from user;
        </select>
    </mapper>
    ```

  * MybatisTest.java

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
    import java.util.Date;
    import java.util.List;
    
    /**
     * 测试mybatis的crud操作
     */
    public class MybatisTest {
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
    
    
        /**
         * 测试查询所有
         */
        @Test
        public void testFindAll() throws IOException {
            //5.执行查询所有的方法
            List<User> users = userDao.findAll();
            for (User user : users) {
                System.out.println(user);
            }
        }
    
        /**
         * 测试保存操作
         */
        @Test
        public void testSave() throws IOException {
            User user = new User();
            user.setUsername("mybatis last insertid");
            user.setAddress("北京市顺义区");
            user.setSex("男");
            user.setBirthday(new Date());
    
            System.out.println("保存操作之前" + user);
            //5.执行保存用户的方法
            userDao.saveUser(user);
    
            System.out.println("保存操作之后" + user);
        }
    
        /**
         * 测试更新操作
         */
        @Test
        public void testUpdate() {
            User user = new User();
            user.setId(50);
            user.setUsername("mybatis UpdateUser");
            user.setAddress("北京市顺义区");
            user.setSex("女");
            user.setBirthday(new Date());
    
            userDao.updateUser(user);
        }
    
        /**
         * 测试更新操作
         */
        @Test
        public void testDelete() {
            //执行删除方法
            userDao.deleteUser(48);
        }
    
        /**
         * 测试根据用户id查询用户信息
         */
        @Test
        public void testFindById() {
            //执行查询一个
            User byId = userDao.findById(50);
            System.out.println(byId);
        }
    
        /**
         * 测试根据用户id查询用户信息
         */
        @Test
        public void testFindByName() {
            //执行模糊查询
            List<User> users = userDao.findByName("%王%");
    //        List<User> users = userDao.findByName("王"); // 对应IUserDao.xml中的第二种方式，不常用
            for (User user : users) {
                System.out.println(user);
            }
        }
    
        /**
         * 测试查询用户信息总数
         */
        @Test
        public void testFindTotal() {
            //执行查询用户数量
            int total = userDao.findTotal();
            System.out.println("用户的数量为：" + total);
        }
    }
    ```

## CRUD中可能遇到的问题：参数的传递以及返回值的封装

* OGNL表达式：

  ```
  Object Graphic Navigation Language
   对象     图       导航       语言
  
  它是通过对象的取值方法来获取数据。在写法上把get给省略了。
  比如：我们获取用户的名称
  	类中的写法：user.getUsername();
  	OGNL表达式写法：user.username
  mybatis中为什么能直接写username，而不用user.呢？
  	因为parameterType中已经提供了属性所属的类，所以此时不需要写对象名。
  ```

* 传递pojo包装对象

  * IUserDao.java中新加入方法

    ```java
        /**
         * 根据QueryVo中的查询条件查询用户
         * @param vo
         * @return
         */
        List<User> findUserByVo(QueryVo vo);
    ```

  * 新建QueryVo.java

    ```java
    package com.itheima.domain;
    
    public class QueryVo {
    
        private User user;
    
        public User getUser() {
            return user;
        }
    
        public void setUser(User user) {
            this.user = user;
        }
    }
    ```

  * 添加IUserDao.xml语句

    ```xml
        <!--根据QueryVo的条件查询用户-->
        <select id="findUserByVo" parameterType="com.itheima.domain.QueryVo" resultType="com.itheima.domain.User">
            select * from user where username like #{user.username};
        </select>
    ```

  * 在MybatisTest.java中执行方法

    ```java
        /**
         * 测试使用QueryVo作为查询条件
         */
        @Test
        public void testFindByVo() {
            QueryVo vo = new QueryVo();
            User user = new User();
            user.setUsername("%王%");
            vo.setUser(user);
            //执行模糊查询
            List<User> users = userDao.findUserByVo(vo);
            for (User u : users) {
                System.out.println(u);
            }
        }
    ```

* 当实体类中定义的属性名称与数据库中定义的属性名称不相同时，查询所有后，由于属性名称不一致，会导致无法把数据封装进去：

  ![](D:\桌面\001\idea笔记\idea图片\mybatis\属性名称不一致时的封装结果.png)

  为了解决以上问题我们有两种解决方案：

  1. 起别名：

     ```xml
         <select id="findAll" resultType="com.itheima.domain.User">
             select id as userId,username as userName,address as userAddress,sex as userSex,birthday as userBirthday from user;
         </select>
     ```

     - 因为是在sql语句上进行修改，所以执行效率较高。

  2. 定义resultMap标签

     ```xml
         <!--配置查询结果的列名和实体类属性名的对应关系-->
         <!--id的值是唯一标识，可以随意起名，type的值代表的是查询的实体类型是哪一个实体类-->
         <resultMap id="userMap" type="com.itheima.domain.User">
             <!--主键字段的对应-->
             <id property="userId" column="id"></id>
             <!--非主键字段对应-->
             <result  property="userName" column="username"></result>
             <result  property="userAddress" column="address"></result>
             <result  property="userSex" column="sex"></result>
             <result  property="userBirthday" column="birthday"></result>
         </resultMap>
     
         <!--查询所有-->
     	<!--此处resultMap的值应与上边resultMap标签的id值相同-->
         <select id="findAll" resultMap="userMap">
             select * from user;
         </select>
     ```

     * 这种方法的开发效率较高

## mybatis中基于传统dao的方式的使用（编写dao实现类）                 ----了解内容

1. 持久层  IUserDao.java  接口

   ```java
   package com.itheima.dao;
   
   import com.itheima.domain.User;
   
   import java.util.List;
   
   /**
    * 用户的持久层接口
    */
   public interface IUserDao {
       /**
        * 查询所有的用户
        * @return
        */
       List<User> findAll();
   
       /**
        * 保存方法
        * @param user
        */
       void saveUser(User user);
   
       /**
        * 更新用户
        * @param user
        */
       void updateUser(User user);
   
       /**
        * 根据id删除用户
        * @param id
        */
       void deleteUser(Integer id);
   
       /**
        * 根据id查询用户信息
        * @return
        */
       User findById(Integer id);
   
       /**
        * 根据名称模糊查询用户信息
        * @param username
        * @return
        */
       List<User> findByName(String username);
   
       /**
        * 查询总用户数
        * @return
        */
       int findTotal();
   
   }
   ```

2. 持久层  UserDaoImpl.java  实现类

   ```java
   package com.itheima.dao.impl;
   
   import com.itheima.dao.IUserDao;
   import com.itheima.domain.User;
   import org.apache.ibatis.session.SqlSession;
   import org.apache.ibatis.session.SqlSessionFactory;
   
   import java.util.List;
   
   /**
    * IUserDao的实现类
    */
   public class UserDaoImpl implements IUserDao {
   
       private SqlSessionFactory factory;
   
       public UserDaoImpl(SqlSessionFactory factory){
           this.factory = factory;
       }
       public List<User> findAll() {
           //1.根据factory获取SqlSession对象
           SqlSession session = factory.openSession();
           //2.调用Sqlsession中的方法，实现查询列表
           List<User> users = session.selectList("com.itheima.dao.IUserDao.findAll");//参数就是能获取到配置信息的key
           //3.释放资源
           session.close();
           return users;
       }
   
       public void saveUser(User user) {
           //1.根据factory获取SqlSession对象
           SqlSession session = factory.openSession();
           //2.调用方法实现保存
           session.insert("com.itheima.dao.IUserDao.saveUser",user);
           //3.提交事务
           session.commit();
           //4.释放资源
           session.close();
       }
   
       public void updateUser(User user) {
           //1.根据factory获取SqlSession对象
           SqlSession session = factory.openSession();
           //2.调用方法实现更新
           session.update("com.itheima.dao.IUserDao.updateUser",user);
           //3.提交事务
           session.commit();
           //4.释放资源
           session.close();
       }
   
       public void deleteUser(Integer userId) {
           //1.根据factory获取SqlSession对象
           SqlSession session = factory.openSession();
           //2.调用方法实现更新
           session.delete("com.itheima.dao.IUserDao.deleteUser",userId);
           //3.提交事务
           session.commit();
           //4.释放资源
           session.close();
       }
   
       public User findById(Integer id) {
           //1.根据factory获取SqlSession对象
           SqlSession session = factory.openSession();
           //2.调用Sqlsession中的方法，实现查询列表
           User user = session.selectOne("com.itheima.dao.IUserDao.findById",id);
           //3.释放资源
           session.close();
           return user;
       }
   
       public List<User> findByName(String username) {
           //1.根据factory获取SqlSession对象
           SqlSession session = factory.openSession();
           //2.调用Sqlsession中的方法，实现查询列表
           List<User> users = session.selectList("com.itheima.dao.IUserDao.findByName",username);
           //3.释放资源
           session.close();
           return users;
       }
   
       public int findTotal() {
           //1.根据factory获取SqlSession对象
           SqlSession session = factory.openSession();
           //2.调用Sqlsession中的方法，实现查询列表
           Integer count= session.selectOne("com.itheima.dao.IUserDao.findTotal");
           //3.释放资源
           session.close();
           return count;
       }
   }
   ```

3. 持久层映射配置 IUserDao.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.itheima.dao.IUserDao">
   
       <!--配置查询结果的列名和实体类属性名的对应关系-->
       <!--id的值是唯一标识，可以随意起名，type的值代表的是查询的实体类型是哪一个实体类-->
   
       <!--查询所有-->
       <select id="findAll" resultType="com.itheima.domain.User">
           select * from user;
       </select>
   
       <!--保存用户-->
       <!--parameterType的含义就是参数的类型-->
       <insert id="saveUser" parameterType="com.itheima.domain.User">
           <!--配置插入数据后，获取数据的id-->
           <selectKey keyProperty="id"  keyColumn="id" order="AFTER" resultType="int">
               select last_insert_id();
           </selectKey>
           insert into user(username,address,sex,birthday)values(#{username},#{address},#{sex},#{birthday});
       </insert>
   
       <!--更新用户-->
       <update id="updateUser" parameterType="com.itheima.domain.User">
           update user set username=#{username},address=#{address},sex=#{sex},birthday=#{birthday} where id=#{id};
       </update>
   
       <!--删除用户的操作-->
       <delete id="deleteUser" parameterType="java.lang.Integer">
           delete from user where id = #{uid};
       </delete>
   
       <!--根据id查询用户-->
       <select id="findById" resultType="com.itheima.domain.User" parameterType="INT">
           select * from user where id = #{uid};
       </select>
   
       <!--根据名称模糊查询的语法-->
       <select id="findByName" parameterType="string" resultType="com.itheima.domain.User">
           select * from user where username like #{uname};
           <!--select * from user where username like '%${value}%';  不常用 -->
       </select>
       
       <!--获取用户的总记录条数-->
       <select id="findTotal" resultType="int">
           select count(id) from user;
       </select>
   
   </mapper>
   ```

4. 测试类 MybatisTest.java

   ```java
   package com.itheima.test;
   
   import com.itheima.dao.IUserDao;
   import com.itheima.dao.impl.UserDaoImpl;
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
   import java.util.Date;
   import java.util.List;
   
   /**
    * 测试mybatis的crud操作
    */
   public class MybatisTest {
       private InputStream in;
       private IUserDao userDao;
   
       @Before//用于在测试方法执行之前执行
       public void init() throws IOException {
           //1.读取字节文件，生成字节输入流
           in = Resources.getResourceAsStream("SqlMapConfig.xml");
           //2.获取SqlSessionFactory对象
           SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
           //3.使用工厂对象创建dao对象
           userDao = new UserDaoImpl(factory);
       }
   
       @After//用于在测试方法执行之后执行
       public void destroy() throws IOException {
           //6.释放资源
           in.close();
       }
   
   
       /**
        * 测试查询所有
        */
       @Test
       public void testFindAll() throws IOException {
           //5.执行查询所有的方法
           List<User> users = userDao.findAll();
           for (User user : users) {
               System.out.println(user);
           }
       }
   
       /**
        * 测试保存操作
        */
       @Test
       public void testSave() throws IOException {
           User user = new User();
           user.setUsername("dao impl user");
           user.setAddress("北京市区");
           user.setSex("男");
           user.setBirthday(new Date());
   
           System.out.println("保存操作之前" + user);
           //5.执行保存用户的方法
           userDao.saveUser(user);
   
           System.out.println("保存操作之后" + user);
       }
   
       /**
        * 测试更新操作
        */
       @Test
       public void testUpdate() {
           User user = new User();
           user.setId(50);
           user.setUsername("userdaoimpl update user");
           user.setAddress("北京市顺义区");
           user.setSex("女");
           user.setBirthday(new Date());
   
           userDao.updateUser(user);
       }
   
       /**
        * 测试删除操作
        */
       @Test
       public void testDelete() {
           //执行删除方法
           userDao.deleteUser(45);
       }
   
       /**
        * 测试根据用户id查询用户信息
        */
       @Test
       public void testFindById() {
           //执行查询一个
           User byId = userDao.findById(50);
           System.out.println(byId);
       }
   
       /**
        * 测试模糊查询
        */
       @Test
       public void testFindByName() {
           //执行模糊查询
           List<User> users = userDao.findByName("%王%");
   //        List<User> users = userDao.findByName("王"); // 对应IUserDao.xml中的第二种方式，不常用
           for (User user : users) {
               System.out.println(user);
           }
       }
   
       /**
        * 测试查询用户信息总数
        */
       @Test
       public void testFindTotal() {
           //执行查询用户数量
           int total = userDao.findTotal();
           System.out.println("用户的数量为：" + total);
       }
   }
   ```

## mybatis主配置文件中常用的配置（主配置文件：SqlMapConfig.xml）

1. properties标签

2. typeAliases标签

3. mappers标签：package

   * SqlMapConfig.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <!DOCTYPE configuration
             PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-config.dtd">
     <configuration>
         <!--配置properties
             可以在标签内部配置连接数据库的信息，也可以通过属性引用外部配置文件信息
             resource属性： 常用的
                 用于指定配置文件的位置，是按照类路径的写发来写，并且必须放在类路径下。
             url属性：
                 要求按照url的写发来写地址
                 URL：Uniform Resource Locator 统一资源定位符。它是可以唯一标识一个资源的位置。
                 他的写法：
                     http：//localhost:8080/mybatisserver/demo1Servlet
                     协议      主机     端口             URI
                 URI:Uniform Resource Identifier 统一资源标识符。它是在应用中可以唯一定位一个资源。
         -->
         <properties url="file:///D:/idea_data/maven_java/maven_java/day02_eesy_01mybatisCRUD/src/main/resources/jdbcConfig.properties">
             <!--<property name="driver" value="com.mysql.jdbc.Driver"/>
             <property name="url" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
             <property name="username" value="root"/>
             <property name="password" value="15591177926"/>-->
         </properties>
     
         <!--使用typeAlias配置别名。他只能配置domain中类的别名-->
         <typeAliases>
             <!--typeAlias用于配置别名。type属性指定的是实体类全限定类名。alias属性指定别名，当指定了别名就不再区分大小写
             <typeAlias type="com.itheima.domain.User" alias="user"></typeAlias>-->
             <!--package用于指定要配置别名的包，当指定后，该包下的实体类都会注册别名，并且类名就是别名，不再区分大小写-->
             <package name="com.itheima.domain"></package>
         </typeAliases>
     
         <!--配置环境-->
         <environments default="mysql">
             <!--配置mysql的环境-->
             <environment id="mysql">
                 <!--配置事务-->
                 <transactionManager type="JDBC"></transactionManager>
                 <!--配置连接池-->
                 <dataSource type="POOLED">
                     <property name="driver" value="${jdbc.driver}"/>
                     <property name="url" value="${jdbc.url}"/>
                     <property name="username" value="${jdbc.username}"/>
                     <property name="password" value="${jdbc.password}"/>
                 </dataSource>
             </environment>
         </environments>
         <!--配置映射文件的位置-->
         <mappers>
             <!--<mapper resource="com/itheima/dao/IUserDao.xml"></mapper>-->
             <!--package标签是用于指定dao接口所在的包，当指定完成之后，就不需要再写mapper以及resource或者class-->
             <package name="com.itheima.dao"/>
         </mappers>
     </configuration>
     ```

   * IUserDao.xml

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <!DOCTYPE mapper
             PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
     <mapper namespace="com.itheima.dao.IUserDao">
     
         <!--配置查询结果的列名和实体类属性名的对应关系-->
         <!--id的值是唯一标识，可以随意起名，type的值代表的是查询的实体类型是哪一个实体类-->
         <resultMap id="userMap" type="com.itheima.domain.User">
             <!--主键字段的对应-->
             <id property="userId" column="id"></id>
             <!--非主键字段对应-->
             <result  property="userName" column="username"></result>
             <result  property="userAddress" column="address"></result>
             <result  property="userSex" column="sex"></result>
             <result  property="userBirthday" column="birthday"></result>
         </resultMap>
     
         <!--查询所有-->
         <select id="findAll" resultMap="userMap">
             <!--select id as userId,username as userName,address as userAddress,sex as userSex,birthday as userBirthday from user;-->
             select * from user;
         </select>
     
         <!--保存用户-->
         <!--parameterType的含义就是参数的类型-->
         <insert id="saveUser" parameterType="user">
             <!--配置插入数据后，获取数据的id-->
             <selectKey keyProperty="userId"  keyColumn="id" order="AFTER" resultType="INT">
                 select last_insert_id();
             </selectKey>
             insert into user(username,address,sex,birthday)values(#{userName},#{userAddress},#{userSex},#{userBirthday});
         </insert>
     
         <!--更新用户-->
         <update id="updateUser" parameterType="User">
             update user set username=#{userName},address=#{userAddress},sex=#{userSex},birthday=#{userBirthday} where id=#{userId};
         </update>
     
         <!--删除用户的操作-->
         <delete id="deleteUser" parameterType="java.lang.Integer">
             delete from user where id = #{uid};
         </delete>
     
         <!--根据id查询用户-->
         <select id="findById" resultMap="userMap" parameterType="INT">
             select * from user where id = #{uid};
         </select>
     
         <!--根据名称模糊查询的语法-->
         <select id="findByName" parameterType="string" resultMap="userMap">
             select * from user where username like #{uname};
             <!--select * from user where username like '%${value}%';  不常用 -->
         </select>
         
         <!--获取用户的总记录条数-->
         <select id="findTotal" resultType="int">
             select count(id) from user;
         </select>
     
         <!--根据QueryVo的条件查询用户-->
         <select id="findUserByVo" parameterType="com.itheima.domain.QueryVo" resultMap="userMap">
             select * from user where username like #{user.username};
         </select>
     </mapper>
     ```

4. 具体操作结合mybatis第二天讲义.pdf

