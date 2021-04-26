# 今日内容

1. 数据库连接池
2. Spring JDBC : JDBC Template

## 数据库连接池

1. 概念：其实就是一个容器(集合)，存放数据库连接的容器。

   当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器。

2. 好处：

   1. 节约资源
   2. 用户访问高效

3. 实现：

   1. 标准接口：DataSource   javax.sql包下的

      1. 方法：

         - 获取连接 :

           ```java
           getConnection()
           ```
        
         - 归还连接：
         
            ```java
            Connection.close() // 如果连接对象Connection是从连接池中获取的，那么调用Connection.close()方法，则不会再关闭连接了。而是归还连接
            ```
   
   2. 一般我们不去实现它，有数据库厂商来实现
   
      1. C3P0：数据库连接池技术
      2. Druid：数据库连接池实现技术，由阿里巴巴提供的
   
4. C3P0：数据库连接池技术

   - 步骤：

     1. 导入jar包 (两个) c3p0-0.9.5.2.jar mchange-commons-java-0.2.12.jar
        - 不要忘记导入数据库驱动jar包
     2. 定义配置文件：
        - 名称： c3p0.properties 或者 c3p0-config.xml
        - 路径：直接将文件放在src目录下即可。
     3. 创建核心对象 数据库连接池对象 ComboPooledDataSource
     4. 获取连接： getConnection

   - 代码：

     ```java
     package cn.itcast.datasource.c3p0;
     
     import com.mchange.v2.c3p0.ComboPooledDataSource;
     
     import javax.sql.DataSource;
     import java.sql.Connection;
     import java.sql.SQLException;
     
     /**
      *  c3p0的演示
      */
     public class C3P0Demo1 {
         public static void main(String[] args) throws SQLException {
             //1.创建数据库连接池对象
             DataSource ds = new ComboPooledDataSource();
             //2.获取连接池对象
             Connection conn = ds.getConnection();
             //3.打印
             System.out.println(conn);
         }
     }
     
     ```

5. Druid：数据库连接池实现技术，由阿里巴巴提供的

   1. 步骤：

      1. 导入jar包 druid-1.0.9.jar
      2. 定义配置文件：
         - 是properties形式的
         - 可以叫任意名称，可以放在任意目录下
      3. 加载配置文件。Properties
      4. 获取数据库连接池对象：通过工厂来来获取  DruidDataSourceFactory
      5. 获取连接：getConnection
      6. 代码演示

      ```java
      package cn.itcast.datasource.druid;
      
      import com.alibaba.druid.pool.DruidDataSourceFactory;
      
      import javax.sql.DataSource;
      import java.io.IOException;
      import java.io.InputStream;
      import java.sql.Connection;
      import java.util.Properties;
      
      /**
       *  Druid演示
       */
      public class DruidDemo {
          public static void main(String[] args) throws Exception {
              //1.导入jar包
              //2.定义配置文件
              //3.加载配置文件
              Properties pro = new Properties();
              InputStream is = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
              pro.load(is);
              //4.获取连接池对象
              DataSource ds = DruidDataSourceFactory.createDataSource(pro);
              //5.获取连接
              Connection conn = ds.getConnection();
              System.out.println(conn);
          }
      }
      ```

   2. 定义工具类

      1. 定义一个类 JDBCUtils

      2. 提供静态代码块加载配置文件，初始化连接池对象

      3. 提供方法

         1. 获取连接方法：通过数据库连接池获取连接
         2. 释放资源
         3. 获取连接池的方法

      4. 代码演示

         ```java
         package cn.itcast.datasource.utils;
         
         import com.alibaba.druid.pool.DruidDataSourceFactory;
         
         import javax.sql.DataSource;
         import java.io.IOException;
         import java.sql.Connection;
         import java.sql.ResultSet;
         import java.sql.SQLException;
         import java.sql.Statement;
         import java.util.Properties;
         
         /**
          *  Druid连接池的工具类
          */
         public class JDBCUtils {
             //1.定义一个成员变量 DataSource
             private static DataSource ds;
         
             static{
                 try {
                     //1.加载配置文件
                     Properties pro = new Properties();
                     pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
                     //2.获取DataSource
                     ds = DruidDataSourceFactory.createDataSource(pro);
                 } catch (IOException e) {
                     e.printStackTrace();
                 } catch (Exception e) {
                     e.printStackTrace();
                 }
         
             }
             /**
              *  获取链接的方法
              */
             public static Connection getConnection() throws SQLException {
                 return ds.getConnection();
             }
         
             /**
              *  释放资源
              */
             public static void close(Statement stmt,Connection conn) {
                 if(stmt != null) {
                     try {
                         stmt.close();
                     } catch (SQLException e) {
                         e.printStackTrace();
                     }
                 }
                 if(conn != null) {
                     try {
                         conn.close();//归还连接
                     } catch (SQLException e) {
                         e.printStackTrace();
                     }
                 }
             }
         
             public static void close(ResultSet rs,Statement stmt, Connection conn) {
                 if(rs != null) {
                     try {
                         rs.close();
                     } catch (SQLException e) {
                         e.printStackTrace();
                     }
                 }
                 if(stmt != null) {
                     try {
                         stmt.close();
                     } catch (SQLException e) {
                         e.printStackTrace();
                     }
                 }
                 if(conn != null) {
                     try {
                         conn.close();//归还连接
                     } catch (SQLException e) {
                         e.printStackTrace();
                     }
                 }
             }
             /**
              *  获取连接池的方法
              */
             public static  DataSource getDataSource() {
                 return ds;
             }
         
         }
         ```

      5. 案例：使用工具类给表添加数据

         ```java
         package cn.itcast.datasource.druid;
         
         import cn.itcast.datasource.utils.JDBCUtils;
         
         import java.sql.Connection;
         import java.sql.PreparedStatement;
         import java.sql.SQLException;
         
         /**
          *  使用新的工具类
          */
         public class DruidDemo2 {
             public static void main(String[] args) {
                 Connection conn = null;
                 PreparedStatement pstmt = null;
                 /*
                     完成一个添加的操作,给account表添加一条记录
                  */
                 //1.获取连接
                 try {
                     //1.获取连接
                     conn = JDBCUtils.getConnection();
                     //2.定义sql
                     String sql = "insert into account values(null,?,?)";
                     //3.获取pstmt
                     pstmt = conn.prepareStatement(sql);
                     //4.给？赋值
                     pstmt.setString(1,"zhaofei");
                     pstmt.setDouble(2,1000);
                     //5.执行sql
                     int count = pstmt.executeUpdate();
                     System.out.println(count);
                 } catch (SQLException e) {
                     e.printStackTrace();
                 } finally {
                     JDBCUtils.close(pstmt,conn);
                 }
             }
         }
         
         ```

## Spring JDBC

- Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象简化JDBC的开发

- 步骤：

  1. 导入jar包

  2. 创建JdbcTemplate对象。依赖于数据源DataSource

     ```java
     JdbcTemplate template = new JdbcTemplate(ds);
     ```

  3. 调用JdbcTemplate的方法来完成CRUD的操作

     - ```java
       update()// 执行DML语句。增、删、改语句
       ```

     - ```java
       queryForMap() // 查询结果将结果集封装为map集合，将列名作为key，将值作为value 将这条记录封装为一个map集合
       //注意：这个方法查询的结果集长度只能是1
       ```

     - ```java
       queryForList() // 查询结果将结果集封装为list集合
       //将每一条记录封装为一个Map集合，再将Map集合装载到List集合中
       ```

     - ```java
       query() // 查询结果，将结果封装为JavaBean对象
           query的参数：RowMapper
           	- 一般我们使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
           	- new BeanPropertyRowMapper<类型>(类型.class)
       ```

     - ```java
       queryForObject // 查询结果，将结果封装为对象
           - 一般用于聚合函数的查询
       ```

  4. 练习：

     - 需求：

       1. 修改1号数据的 salary 为 10000
       2. 添加一条记录
       3. 删除刚才添加的记录
       4. 查询id为1的记录，将其封装为Map集合
       5. 查询所有记录，将其封装为List
       6. 查询所有记录，将其封装为Emp对象的List集合
       7. 查询总记录数

     - 代码演示

       ```java
       package cn.itcast.datasource.jdbctemplate;
       
       import cn.itcast.datasource.utils.JDBCUtils;
       import cn.itcast.domain.Emp;
       import com.sun.jmx.snmp.SnmpUnknownAccContrModelException;
       import org.junit.Test;
       import org.springframework.jdbc.core.BeanPropertyRowMapper;
       import org.springframework.jdbc.core.JdbcTemplate;
       import org.springframework.jdbc.core.RowMapper;
       
       import java.sql.Date;
       import java.sql.ResultSet;
       import java.sql.SQLException;
       import java.util.List;
       import java.util.Map;
       
       public class JdbcTemplateDemo2 {
       
           //Juint单元测试，可以让方法独立执行
       
           /**
            * 1.修改1号数据的 salary 为 10000
            */
           @Test
           public void test1() {
               JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
               String sql = "update emp set salary = ? where id = ?";
               int count = template.update(sql, 10000, 1);
               System.out.println(count);
           }
           /**
            * 2.添加一条记录
            */
           @Test
           public void test2() {
               JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
               String sql = "insert into emp values(?,?,?,?,?,?)";
               int count = template.update(sql, null, "吴奇刚", "男", 20000, null, 1);
               System.out.println(count);
           }
           /**
            * 3.删除刚才添加的记录
            */
           @Test
           public void test3() {
               JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
               String sql = "delete from emp where id = ?";
               template.update(sql,6);
           }
           /**
            * 4.查询id为1的记录，将其封装为Map集合
            * 注意：查询的结果集长度只能是1
            */
           @Test
           public void test4() {
               JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
               String sql = "select * from emp where id = ?";
               Map<String, Object> map = template.queryForMap(sql,1);
               System.out.println(map);
           }
           /**
            * 5.查询所有记录，将其封装为List
            */
           @Test
           public void test5() {
               JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
               String sql = "select * from emp";
               List<Map<String, Object>> list = template.queryForList(sql);
               for (Map<String, Object> stringObjectMap : list) {
                   System.out.println(stringObjectMap);
               }
           }
           /**
            * 6. 查询所有记录，将其封装为Emp对象的List集合
            */
           @Test
           public void test6() {
               JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
               String sql = "select * from emp";
               List<Emp> list = template.query(sql, new RowMapper<Emp>() {
       
                   @Override
                   public Emp mapRow(ResultSet rs, int i) throws SQLException {
                       Emp emp = new Emp();
                       int id = rs.getInt("id");
                       String name = rs.getString("name");
                       String gender = rs.getString("gender");
                       double salary = rs.getDouble("salary");
                       Date join_date = rs.getDate("join_date");
                       int dept_id = rs.getInt("dept_id");
       
                       emp.setId(id);
                       emp.setName(name);
                       emp.setGender(gender);
                       emp.setSalary(salary);
                       emp.setJoin_date(join_date);
                       emp.setDept_id(dept_id);
       
                       return emp;
                   }
               });
       
               for (Emp emp : list) {
                   System.out.println(emp);
               }
           }
           @Test
           public void test6_2() {
               JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
               String sql = "select * from emp";
               List<Emp> list = template.query(sql, new BeanPropertyRowMapper<Emp>(Emp.class));
               for (Emp emp : list) {
                   System.out.println(emp);
               }
           }
           /**
            *  7.查询总记录数
            */
           @Test
           public void test7() {
               JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
               String sql = "select count(id) from emp";
               Long total = template.queryForObject(sql,Long.class);
               System.out.println(total);
           }
       }
       ```

       



