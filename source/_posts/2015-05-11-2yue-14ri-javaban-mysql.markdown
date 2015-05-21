---
layout: post
title: "2月14日Java班mysql"
date: 2015-05-11 14:58:41 +0800
comments: true
categories: Mysql
---
<!--more-->
第一部分 笔记


1：关于事务在命令窗口的练习：
事务  就是一连串命令的集合，只要这部命令都执行完毕，才能更新到数据库中，否则退出！！即使有一行比如 null.length()的存在空指针，则不成功。
一旦执行失败则回滚。
```mysql
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> update t_person set age=age+1 where Name='yezhao';
Query OK, 2 rows affected (0.35 sec)
Rows matched: 2  Changed: 2  Warnings: 0

mysql> select * from t_person where Name='yezhao';
+----+--------+-----+--------+
| Id | Name   | Age | Gender |
+----+--------+-----+--------+
|  1 | yezhao |  25 |      1 |
|  7 | yezhao |  26 |      1 |
+----+--------+-----+--------+
2 rows in set (0.02 sec)
```
然而在navicat的显示结果是之前的信息  也就是  24  25，当在cmd窗口执行完commit之后：

```mysql
mysql> commit;
Query OK, 0 rows affected (0.11 sec)

mysql> select * from t_person where Name='yezhao';
+----+--------+-----+--------+
| Id | Name   | Age | Gender |
+----+--------+-----+--------+
|  1 | yezhao |  25 |      1 |
|  7 | yezhao |  26 |      1 |
+----+--------+-----+--------+
2 rows in set (0.00 sec)
```
navicat也会发生相应的变化：
1        yezhao        25        1
7        yezhao        26        1


java的conn.setAutoCommit(false);//相当于begin的作用
所以：只有commit之后mysql信息才会在不同的客户端同步，现在尝试在java中的使用，已在练习部分给出，


2.在没有批量导入数据之前，一般是需要花费很长的时间，现在采用addbatch进行。
ps.clearParameters()的作用是清空上一次数据的值，不影响到下一次


而一般数据量大的情况下，导入可能会出现问题，这样如果批量导入在事务中运行，将会更加保险，已在练习给出。
注意加入事物的时候，抛出异常时候采用conn.rollback();把executeUpdate()改成addBatch()，在最后再ps.executeBatch()


3. 
mysql获取最后一次插入的id号函数，在java中也可以实现。
```mysql
mysql> select last_insert_id();
+------------------+
| last_insert_id() |
+------------------+
|                0 |
+------------------+
1 row in set (0.17 sec)
```
有没有发现是0 ，即使是我最新插入也是这样；调查后发现是：
```mysql
mysql> insert into t_person(Name,Age,Gender) values ('悟空',30,1);
Query OK, 1 row affected (0.04 sec)

mysql> select last_insert_id();
+------------------+
| last_insert_id() |
+------------------+
|            11317 |
+------------------+
1 row in set (0.00 sec)
```
如果你添加的时候，不要写上主键的值，则是可以！如果直接加上主键的值，则显示的是0！



第二部分 练习
1.一个比较戳版本的JDBCUtils的版本，虽然没有错误，但是未关闭连接，且整体逻辑性不强，并多次加载驱动

```java
/**
* 
*/
package com.jdbc.test;

import java.util.Properties;
import java.io.*;
import java.sql.*;
/**
* @author    叶昭良
* @time      2015年2月17日下午11:31:53
* @version   com.jdbc.testJDBCUtils V1.0
*/
public class JDBCUtils
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                
                try
                {
                        
        //查询的测试分为三个阶段    无参 的两个   有参的一个
                        //测试1   success
                        /**
                         *  mysql> select * from t_person;
                         *  6 rows in set (0.35 sec)
                         */
                        /*
                        Connection conn = JDBCUtils.createConnection() ;
                        ResultSet rs = JDBCUtils.executeQuery(conn, "select * from t_person", null);
                        int i = 0;
                        while(rs.next())
                        {
                                i++;
                        }
                        System.out.println("总的返回了"+i+"条记录");
                        */
                        //测试2   success
                        /**
                         *  mysql> select * from t_person;
                         *  6 rows in set (0.35 sec)
                         */
                        /*
                        ResultSet rs = JDBCUtils.executeQuery( "select * from t_person", null);
                        int i = 0;
                        while(rs.next())
                        {
                                i++;
                        }
                        System.out.println("总的返回了"+i+"条记录");
                        */
                        
                        //测试3    success
                        /* mysql> select * from t_person where name='xinran'
                         * 3 rows in set (0.00 sec)
                         */
/*                        ResultSet rs = JDBCUtils.executeQuery("select * from t_person where name = ?", "xinran");
                        int i = 0;
                        while(rs.next())
                        {
                                i++;
                        }
                        System.out.println("总的返回了"+i+"条记录");*/
                        
        //插入阶段的测试   类似查询分为三个阶段    无参 的两个   有参的一个
                        /**
                         * 测试成功！ //1
                         */
                        /*Connection conn = JDBCUtils.createConnection() ;
                        int i = JDBCUtils.executeUpdate(conn, "insert into t_person(Id,Name,Age,Gender) values(8,'lizi',82,1)", null);

                        //System.out.println("总的返回了"+i+"条记录");

            */        
                        /**
                         * 测试成功！ //2 没想到只要两行语句 则进行了一次插入
                         */
                        /*String sql = "insert into t_person(Id,Name,Age,Gender) values(9,'孙悟空',12,1)"; //年龄不能过大
                        JDBCUtils.executeUpdate(sql, null);*/
                        
                        /**
                         *  测试成功！  //3  连有参的形式也是成功了
                         *  mysql> select * from t_person;
                        +----+---------+-----+--------+
                        | Id | Name    | Age | Gender |
                        +----+---------+-----+--------+
                        |  1 | yezhao  |  24 |      1 |
                        |  3 | xinran  |  32 |      0 |
                        |  4 | xinran  |  32 |      4 |
                        |  5 | xinran  |  32 |      4 |
                        |  6 | yezhao1 |  25 |      0 |
                        |  7 | yezhao  |  25 |      1 |
                        |  8 | lizi    |  82 |      1 |
                        |  9 | 孙悟空  |  12 |      1 |
                        | 10 | 蝴蝶    |   3 |      0 |
                        +----+---------+-----+--------+
                        9 rows in set (0.00 sec)
                         */
                        String sql = "insert into t_person(Id,Name,Age,Gender) values(?,?,?,?)"; //年龄不能过大
                        JDBCUtils.executeUpdate(sql, 11,"孟彩兰",13,1);
                        
                } catch (SQLException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
        }
        
        
        public static  Connection createConnection() throws SQLException
        {
                InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("com/jdbc/test/sql.properties");
                 
                //InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("sql.properties");
                Properties ps  = new Properties();
                Connection c1 = null;
                try
                {
                        ps.load(is);
                        String mysqlDriver = ps.getProperty("mysqlDriver");
//                        System.out.println(mysqlDriver);
                        
                        String connectDatabase = ps.getProperty("connectDatabase");
//                        System.out.println(connectDatabase);
                        
                        String userName = ps.getProperty("userName");
//                        System.out.println(userName);
                        
                        String Password = ps.getProperty("Password");
//                        System.out.println(Password);
                        
                        Class.forName(mysqlDriver);
                        
                        c1 = DriverManager.getConnection(connectDatabase,userName,Password);
                        System.out.println("连接成功");
                } catch (IOException | ClassNotFoundException | SQLException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("数据库驱动加载失败"+e.getMessage());
                        return null;
                }
                return c1;
        }

        public static int executeUpdate(String sql,Object... parameter) throws SQLException
        {
                Connection conn = JDBCUtils.createConnection();
                PreparedStatement ps = conn.prepareStatement(sql);
                int i = 1;
                if(parameter != null)
                {
                        for(Object pa1:parameter)
                        {
                                ps.setObject(i, pa1);
                                i++;
                        }
                }
                int rows = ps.executeUpdate();
                System.out.println("有"+rows+"条记录被影响");
                return rows;
        }
        
        public static int executeUpdate(Connection conn,String sql,Object... parameter) throws SQLException
        {
                PreparedStatement ps = conn.prepareStatement(sql);
                int i = 1;
                if(parameter != null)
                {
                        for(Object pa1:parameter)
                        {
                                ps.setObject(i, pa1);
                                i++;
                        }
                }
                int rows = ps.executeUpdate();
                System.out.println("有"+rows+"条记录被影响");
                return rows;
        }
        
        public static ResultSet executeQuery(String sql,Object... parameter) throws SQLException
        {
                Connection conn = JDBCUtils.createConnection();
                PreparedStatement ps = conn.prepareStatement(sql);
                int i =1;
                if(parameter != null)
                {
                        for(Object pa1:parameter)
                        {
                                ps.setObject(i, pa1);
                                i++;
                        }
                }
                ResultSet rs = ps.executeQuery();
                return rs;
        }
        
        public static ResultSet executeQuery(Connection conn,String sql,Object... parameter) throws SQLException
        {
                PreparedStatement ps = conn.prepareStatement(sql);
                int i =1;
                if(parameter != null)
                {
                        for(Object pa1:parameter)
                        {
                                ps.setObject(i, pa1);
                                i++;
                        }
                }
                ResultSet rs = ps.executeQuery();
                return rs;
        }
        
        static void closeQuietly(PreparedStatement ps)
        {
                if(ps!= null)
                {
                        try
                        {
                                ps.close();
                        }catch(SQLException e)
                        {
                        
                        }
                }
        }
        
        static void closeQuietly(Connection conn)
        {
                if(conn!= null)
                {
                        try
                        {
                                conn.close();
                        }catch(SQLException e)
                        {
                        
                        }
                }
        }
        static void closeQuietly(ResultSet rs)
        {
                if(rs!= null)
                {
                        try
                        {
                                rs.close();
                        }catch(SQLException e)
                        {
                        
                        }
                }
        }
}
```


新一个版本的改进，主要是利用杨老师讲的 静态代码块仅仅一次加载驱动！ 并关闭有些使用的资源，ResultSet资源不关闭，由调用者去关闭

2.新的版本真是比较清晰。

```java
/**
* 
*/
package com.jdbc.test;

import java.util.Properties;
import java.io.*;
import java.sql.*;
/**
* @author    叶昭良
* @time      2015年2月17日下午11:31:53
* @version   com.jdbc.testJDBCUtils V1.0
*                         V2.0  改变配置文件变为为final，并把sql.properties文件提到
*                                 com.jdbc目录下 com/jdbc/test/sql.properties
*                                 测试通过
*          V3.0 改变为静态代码段 加载驱动！ 并把加载配置文件
*               也放在静态代码段中！仅仅加载一次即可！而不需要
*               一直加载
*          V4.0 修正了没有close的问题！！防止一直连着。 明白了吃异常的过程
*                在类库中不知道怎么处理！直接抛出异常即可！让调用者自己去处理！
*                咱们提供方法，不提供异常的处理！打印异常不叫做处理！而叫做“吃异常”
*          V5.0  不吃异常的方式就是 throw new RuntimeException ，静态代码段
*                  无法throw checkedException检查异常
*                没想到经过这样的过程封装比我原先的好看多了！而且逻辑更加清晰
*                
*/
public class JDBCUtils
{

        /**
         * @param args
         */
        private static final String mysqlDriver;
        private static final String connectDatabase;
        private static final String userName;
        private static final String Password;
        static
        {
                InputStream is = null; 
                //InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("sql.properties");
                Properties ps  = new Properties();
                Connection c1 = null;
                try
                {
                        is = JDBCUtils.class.getClassLoader().getResourceAsStream(""
                                        + "com/jdbc/test/sql.properties");
                        ps.load(is);
                        mysqlDriver = ps.getProperty("mysqlDriver");
//                        System.out.println(mysqlDriver);
                        connectDatabase = ps.getProperty("connectDatabase");
        //                        System.out.println(connectDatabase);
                        userName = ps.getProperty("userName");
        //                        System.out.println(userName);
                        Password = ps.getProperty("Password");
        //                        System.out.println(Password);
                        //仅仅在静态代码段中加载一次驱动即可（在运行中）
                        
                } catch (IOException e)
                {
                        // TODO Auto-generated catch block
                        //System.out.println("配置文件加载失败"+e.getMessage());
                        throw new RuntimeException("配置文件sql.properties加载失败"+e.getMessage());
                        //return null;
                }
                try
                {
                        Class.forName(mysqlDriver); //仅仅在程序运行中加载一次驱动即可！！！
                } catch (ClassNotFoundException e)
                {
                        // TODO Auto-generated catch block
//                        System.out.println("驱动加载失败"+e.getMessage());
                        throw new RuntimeException("驱动加载失败"+e.getMessage());
                }

        }
        
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                
                try
                {
                        
        //查询的测试分为三个阶段    无参 的两个   有参的一个
                        //测试1   success
                        /**
                         *  mysql> select * from t_person;
                         *  6 rows in set (0.35 sec)
                         */
                        /*
                        Connection conn = JDBCUtils.createConnection() ;
                        ResultSet rs = JDBCUtils.executeQuery(conn, "select * from t_person", null);
                        int i = 0;
                        while(rs.next())
                        {
                                i++;
                        }
                        System.out.println("总的返回了"+i+"条记录");
                        */
                        //测试2   success
                        /**
                         *  mysql> select * from t_person;
                         *  6 rows in set (0.35 sec)
                         */
                        /*
                        ResultSet rs = JDBCUtils.executeQuery( "select * from t_person", null);
                        int i = 0;
                        while(rs.next())
                        {
                                i++;
                        }
                        System.out.println("总的返回了"+i+"条记录");
                        */
                        
                        //测试3    success
                        /* mysql> select * from t_person where name='xinran'
                         * 3 rows in set (0.00 sec)
                         */
/*                        ResultSet rs = JDBCUtils.executeQuery("select * from t_person where name = ?", "xinran");
                        int i = 0;
                        while(rs.next())
                        {
                                i++;
                        }
                        System.out.println("总的返回了"+i+"条记录");*/
                        
        //插入阶段的测试   类似查询分为三个阶段    无参 的两个   有参的一个
                        /**
                         * 测试成功！ //1
                         */
                        /*Connection conn = JDBCUtils.createConnection() ;
                        int i = JDBCUtils.executeUpdate(conn, "insert into t_person(Id,Name,Age,Gender) values(8,'lizi',82,1)", null);

                        //System.out.println("总的返回了"+i+"条记录");

            */        
                        /**
                         * 测试成功！ //2 没想到只要两行语句 则进行了一次插入
                         */
                        /*String sql = "insert into t_person(Id,Name,Age,Gender) values(9,'孙悟空',12,1)"; //年龄不能过大
                        JDBCUtils.executeUpdate(sql, null);*/
                        
                        /**
                         *  测试成功！  //3  连有参的形式也是成功了
                         *  mysql> select * from t_person;
                        +----+---------+-----+--------+
                        | Id | Name    | Age | Gender |
                        +----+---------+-----+--------+
                        |  1 | yezhao  |  24 |      1 |
                        |  3 | xinran  |  32 |      0 |
                        |  4 | xinran  |  32 |      4 |
                        |  5 | xinran  |  32 |      4 |
                        |  6 | yezhao1 |  25 |      0 |
                        |  7 | yezhao  |  25 |      1 |
                        |  8 | lizi    |  82 |      1 |
                        |  9 | 孙悟空  |  12 |      1 |
                        | 10 | 蝴蝶    |   3 |      0 |
                        +----+---------+-----+--------+
                        9 rows in set (0.00 sec)
                         */
                        String sql = "insert into t_person(Id,Name,Age,Gender) values(?,?,?,?)"; //年龄不能过大
                        JDBCUtils.executeUpdate(sql, 12,"孟江虎",15,0);
                        
                } catch (SQLException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
        }
        
        /**
         * 创建数据库的连接
         * @return   返回mysql的连接
         */
        public static  Connection createConnection() 
        {
                Connection c1  = null;
                try
                {
                        
                  c1 = DriverManager.getConnection(connectDatabase,userName,Password);
                        System.out.println("连接成功");
                } catch (SQLException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("数据库连接创建失败"+e.getMessage());
                        return null;
                }
                return c1;
        }
        /**
         * 
         * @param sql        sql的update,delete,insert等修改数据库的语句
         * @param parameter  参数化的update,delete,insert等的设置 
         * @return           返回被影响的函数
         * @throws SQLException   抛出SQLException由用户自己去特殊处理
         */
        public static int executeUpdate(String sql,Object... parameter) throws SQLException
        {
                Connection conn = null;
                PreparedStatement ps = null;
                int rows = 0;
                try
                {
                        conn = JDBCUtils.createConnection();
                        ps = conn.prepareStatement(sql);
                        int i = 1;
                        if(parameter != null)
                        {
                                for(Object pa1:parameter)
                                {
                                        ps.setObject(i, pa1);
                                        i++;
                                }
                        }
                        rows = ps.executeUpdate();
                }finally
                {
                        JDBCUtils.closeQuietly(ps);
                }
                System.out.println("有"+rows+"条记录被影响");
                return rows;
        }
        /**
         * @param conn       数据库的连接
         * @param sql        sql的update,delete,insert等修改数据库的语句
         * @param parameter  参数化的update,delete,insert等的设置 
         * @return           返回被影响的函数
         * @throws SQLException   抛出SQLException由用户自己去特殊处理
         */
        public static int executeUpdate(Connection conn,String sql,Object... parameter) throws SQLException
        {
                PreparedStatement ps = null;
                int rows = 0;
                try
                {
                        ps = conn.prepareStatement(sql);
                        int i = 1;
                        if(parameter != null)
                        {
                                for(Object pa1:parameter)
                                {
                                        ps.setObject(i, pa1);
                                        i++;
                                }
                        }
                        rows = ps.executeUpdate();
                }finally
                {
                        JDBCUtils.closeQuietly(ps);
                }
                
                System.out.println("有"+rows+"条记录被影响");
                return rows;
        }
        /**
         * 
         * @param sql        sql的select不修改数据库的语句
         * @param parameter  参数化的查询，参数集的设置 
         * @return           返回查询的数据集
         * @throws SQLException   抛出SQLException由用户自己去特殊处理
         */
        public static ResultSet executeQuery(String sql,Object... parameter) throws SQLException
        {
                Connection conn = null;
                PreparedStatement ps = null;
                try
                {
                        conn = JDBCUtils.createConnection();
                        ps = conn.prepareStatement(sql);
                        int i =1;
                        if(parameter != null)
                        {
                                for(Object pa1:parameter)
                                {
                                        ps.setObject(i, pa1);
                                        i++;
                                }
                        }
                        ResultSet rs = ps.executeQuery();
                        //因为finally是肯定会被执行的片段  即使他放在return之后
                        return rs;
                }finally
                {
                        JDBCUtils.closeQuietly(ps);
                }
                
                
        }
        /**
         * @param conn       数据库连接
         * @param sql        sql的select不修改数据库的语句
         * @param parameter  参数化的查询，参数集的设置 
         * @return           返回查询的数据集
         * @throws SQLException   抛出SQLException由用户自己去特殊处理
         */
        public static ResultSet executeQuery(Connection conn,String sql,Object... parameter) throws SQLException
        {
                PreparedStatement ps = null;
                try
                {
                        ps = conn.prepareStatement(sql);
                        int i =1;
                        if(parameter != null)
                        {
                                for(Object pa1:parameter)
                                {
                                        ps.setObject(i, pa1);
                                        i++;
                                }
                        }
                        ResultSet rs = ps.executeQuery();
                        return rs;
                }finally
                {
                        JDBCUtils.closeQuietly(ps);
                }
                
        }
        /**
         *  关闭PreparedStatment连接
         * @param ps   PreparedStatment对象
         */
        static void closeQuietly(PreparedStatement ps)
        {
                if(ps!= null)
                {
                        try
                        {
                                ps.close();
                        }catch(SQLException e)
                        {
                        
                        }
                }
        }
        /**
         *  关闭Connection连接
         * @param connn   Connection对象
         */
        static void closeQuietly(Connection conn)
        {
                if(conn!= null)
                {
                        try
                        {
                                conn.close();
                        }catch(SQLException e)
                        {
                        
                        }
                }
        }
        /**
         *  关闭ResultSet连接
         * @param rs   ResultSet对象
         */
        static void closeQuietly(ResultSet rs)
        {
                if(rs!= null)
                {
                        try
                        {
                                rs.close();
                        }catch(SQLException e)
                        {
                        
                        }
                }
        }
}
```
原来自己又没有理解清楚，我们实际用的较多executeUpdate和executeQuery的无conn
的形式，而带conn只是一个中间过程！ 并且无conn的会调用有conn的形式！同时这也是一个比较好的习惯！只要改一个地方就都改了！

而且并且改掉代码的重复片段，一举多得，并且思路更清晰！
学习，札记。

```java
/**
         * 
         * @param sql        sql的update,delete,insert等修改数据库的语句
         * @param parameter  参数化的update,delete,insert等的设置 
         * @return           返回被影响的函数
         * @throws SQLException   抛出SQLException由用户自己去特殊处理
         */
        public static int executeUpdate(String sql,Object... parameter) throws SQLException
        {
                Connection conn = null;
                try
                {
                        conn = JDBCUtils.createConnection();
                        return executeUpdate(conn, sql, parameter);
                }finally
                {
                        JDBCUtils.closeQuietly(conn);
                }
        }

        /**
         * 
         * @param sql        sql的select不修改数据库的语句
         * @param parameter  参数化的查询，参数集的设置 
         * @return           返回查询的数据集
         * @throws SQLException   抛出SQLException由用户自己去特殊处理
         */
        public static ResultSet executeQuery(String sql,Object... parameter) throws SQLException
        {
                Connection conn = null;
                try
                {
                        conn = JDBCUtils.createConnection();
                        return executeQuery(conn,sql, parameter);
                }finally
                {
                        JDBCUtils.closeQuietly(conn);
                }
                
        }
```
漂亮，优雅！！！美丽

3测试了一个多次插入的案例：

```java
/**
                 *  测试结果： 单个连接 插入100条数据耗时3767 
                 */
/*                        long startTime = System.currentTimeMillis();
                        String sql = "insert into t_person(Id,Name,Age,Gender) values(?,?,?,?)";
                        Connection conn = null;
                        try
                        {
                                conn = JDBCUtils.createConnection();
                                //原先有数据！并且主键不能重复 所以只好从18开始
                                for(int i = 15; i < 115; i++)
                                {
                                        JDBCUtils.executeUpdate(conn,sql, i,"SpringDay",17,1);
                                }
                                System.out.println("单个连接 插入100条数据耗时"+(System.currentTimeMillis()
                                                -startTime));

                        }catch(SQLException e)
                        {
                                System.out.println("数据库连接异常");
                        }finally
                        {
                                JDBCUtils.closeQuietly(conn);
                        }*/
                /**
                 * 一个连接 插入一条数据 100条后耗时7850  
                 *   可见这种方式还是相对较慢的！！！！
                 *   总结   在多次插入数据库中！最好是一次连接，不要关闭，插入完在关闭
                 */
                long startTime = System.currentTimeMillis();
                String sql = "insert into t_person(Id,Name,Age,Gender) values(?,?,?,?)";
                try
                {
                        for(int i = 116; i < 216; i++)
                        {
                                JDBCUtils.executeUpdate(sql, i,"SpringDay",17,1);
                        }
                        System.out.println("一个连接 插入一条数据 100条后耗时"+(System.currentTimeMillis()
                                        -startTime));
                }catch(SQLException e)
                {
                        System.out.println("连接失败！主键有问题"+e.getMessage());
                }
```
总结 在多次插入数据库中！最好是一次连接，不要关闭，插入完在关闭


再次改进！不需要把executeQuery的connection关闭

```java
*                由于PreparedStatement 来自Statement
*                rs.getStatement() 表示返回一个statement，而不会关闭PreparedStatement
*                常见了一个closeAll(),这也是为什么 暂时不关闭查询连接的原因，同时不关闭
*                方便进行多次查询，反复查询

/**
         * 从resultSet中关闭所有的连接
         * @param rs
         */
        static void closeAll(ResultSet rs)
        {
                if(rs == null)
                {
                        return;
                }
                try
                {
                        JDBCUtils.closeQuietly(rs.getStatement().getConnection());
                        JDBCUtils.closeQuietly(rs.getStatement());
                        JDBCUtils.closeAll(rs);
                }catch(SQLException e)
                {
                        
                }
        }
```
并且必须修正 executeQuery的函数，把原先的JDBCUtils.closeQuietly(ps)关掉！ 否则报错！出现异常，然后就可以很happy的使用JDBCUtils了
```java
  ResultSet rs = null;
                String sql = "select * from t_person";
                try
                {
                        rs = executeQuery(sql);
                        while(rs.next())
                        {
                                String Name= rs.getString("Name");
                                System.out.println(Name);
                        }
                }catch(SQLException e)
                {
                        System.out.println("居然报错");
                }finally
                {
                        JDBCUtils.closeAll(rs);
                }
```


4午饭前版本的JDBCUtils：

```java
/**
* 
*/
package com.jdbc.test;

import java.util.Properties;
import java.io.*;
import java.sql.*;
/**
* @author    叶昭良
* @time      2015年2月17日下午11:31:53
* @version   com.jdbc.testJDBCUtils V1.0
*                         V2.0  改变配置文件变为为final，并把sql.properties文件提到
*                                 com.jdbc目录下 com/jdbc/test/sql.properties
*                                 测试通过
*          V3.0 改变为静态代码段 加载驱动！ 并把加载配置文件
*               也放在静态代码段中！仅仅加载一次即可！而不需要
*               一直加载
*          V4.0 修正了没有close的问题！！防止一直连着。 明白了吃异常的过程
*                在类库中不知道怎么处理！直接抛出异常即可！让调用者自己去处理！
*                咱们提供方法，不提供异常的处理！打印异常不叫做处理！而叫做“吃异常”
*          V5.0  不吃异常的方式就是 throw new RuntimeException ，静态代码段
*                  无法throw checkedException检查异常
*                没想到经过这样的过程封装比我原先的好看多了！而且逻辑更加清晰
*          V6.0 有没有理解清楚，我们实际用的较多executeUpdate和executeQuery的无conn
*               的形式，而带conn只是一个中间过程！ 并且无conn的会调用有conn的形式！
*              再次修改！ 注意观看executeUpdate的实现
*          V7.0  再次改进！不需要把executeQuery的connection关闭
*                由于PreparedStatement 来自Statement
*                rs.getStatement() 表示返回一个statement，而不会关闭PreparedStatement
*                常见了一个closeAll(),这也是为什么 暂时不关闭查询连接的原因，同时不关闭
*                方便进行多次查询，反复查询
*                
*           V8.0  cachedRowSet 是一个缓冲的mysql字符集，连接中断！把所有数据缓冲到客户端！
*                 不需要再次连接，所以关闭Connection也是可以
*                 但是这个栈内存  暂时不使用 
*                
*/
public class JDBCUtils
{

        /**
         * @param args
         */
        private static final String mysqlDriver;
        private static final String connectDatabase;
        private static final String userName;
        private static final String Password;
        static
        {
                InputStream is = null; 
                //InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("sql.properties");
                Properties ps  = new Properties();
                Connection c1 = null;
                try
                {
                        //is = JDBCUtils.class.getClassLoader().getResourceAsStream(""
                        //                + "com/jdbc/test/sql.properties");
                        is = JDBCUtils.class.getResourceAsStream("sql.properties");
                        //这种情况省略宝和getClassLoader()
                        ps.load(is);
                        mysqlDriver = ps.getProperty("mysqlDriver");
//                        System.out.println(mysqlDriver);
                        connectDatabase = ps.getProperty("connectDatabase");
        //                        System.out.println(connectDatabase);
                        userName = ps.getProperty("userName");
        //                        System.out.println(userName);
                        Password = ps.getProperty("Password");
        //                        System.out.println(Password);
                        //仅仅在静态代码段中加载一次驱动即可（在运行中）
                        
                } catch (IOException e)
                {
                        // TODO Auto-generated catch block
                        //System.out.println("配置文件加载失败"+e.getMessage());
                        throw new RuntimeException("配置文件sql.properties加载失败"+e.getMessage());
                        //return null;
                }
                try
                {
                        Class.forName(mysqlDriver); //仅仅在程序运行中加载一次驱动即可！！！
                } catch (ClassNotFoundException e)
                {
                        // TODO Auto-generated catch block
//                        System.out.println("驱动加载失败"+e.getMessage());
                        throw new RuntimeException("驱动加载失败"+e.getMessage());
                }

        }
        
        public static void main(String[] args)
        {
                
                ResultSet rs = null;
                String sql = "select * from t_person";
                try
                {
                        rs = executeQuery(sql);
                        while(rs.next())
                        {
                                String Name= rs.getString("Name");
                                System.out.println(Name);
                        }
                }catch(SQLException e)
                {
                        System.out.println("居然报错");
                }finally
                {
                        JDBCUtils.closeAll(rs);
                }
        
/*                try
                {
                        ResultSet rs = JDBCUtils.executeQuery( "select * from t_person", null);
                        int i = 0;
                        while(rs.next())
                        {
                                i++;
                        }
                        System.out.println("总的返回了"+i+"条记录");
                }catch(SQLException e)
                {
                        System.out.println("juran");
                }
                */
                        
        }
        
        /**
         * 创建数据库的连接
         * @return   返回mysql的连接
         */
        public static  Connection createConnection() 
        {
                Connection c1  = null;
                try
                {
                        
                  c1 = DriverManager.getConnection(connectDatabase,userName,Password);
                        System.out.println("连接成功");
                } catch (SQLException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("数据库连接创建失败"+e.getMessage());
                        return null;
                }
                return c1;
        }
        /**
         * 
         * @param sql        sql的update,delete,insert等修改数据库的语句
         * @param parameter  参数化的update,delete,insert等的设置 
         * @return           返回被影响的函数
         * @throws SQLException   抛出SQLException由用户自己去特殊处理
         */
        public static int executeUpdate(String sql,Object... parameter) throws SQLException
        {
                Connection conn = null;
                try
                {
                        conn = JDBCUtils.createConnection();
                        return executeUpdate(conn, sql, parameter);
                }finally
                {
                        JDBCUtils.closeQuietly(conn);
                }
        }
        /**
         * @param conn       数据库的连接
         * @param sql        sql的update,delete,insert等修改数据库的语句
         * @param parameter  参数化的update,delete,insert等的设置 
         * @return           返回被影响的函数
         * @throws SQLException   抛出SQLException由用户自己去特殊处理
         */
        public static int executeUpdate(Connection conn,String sql,Object... parameter) throws SQLException
        {
                PreparedStatement ps = null;
                int rows = 0;
                try
                {
                        ps = conn.prepareStatement(sql);
                        int i = 1;
                        if(parameter != null)
                        {
                                for(Object pa1:parameter)
                                {
                                        ps.setObject(i, pa1);
                                        i++;
                                }
                        }
                        rows = ps.executeUpdate();
                }finally
                {
                        JDBCUtils.closeQuietly(ps);
                }
                
                System.out.println("有"+rows+"条记录被影响");
                return rows;
        }
        /**
         * 
         * @param sql        sql的select不修改数据库的语句
         * @param parameter  参数化的查询，参数集的设置 
         * @return           返回查询的数据集
         * @throws SQLException   抛出SQLException由用户自己去特殊处理
         */
        public static ResultSet executeQuery(String sql,Object... parameter) throws SQLException
        {
                Connection conn = null;
                
                conn = JDBCUtils.createConnection();
                return executeQuery(conn,sql, parameter);
                /*finally
                {
                        JDBCUtils.closeQuietly(conn);
                }*/
                
        }
        /**
         * @param conn       数据库连接
         * @param sql        sql的select不修改数据库的语句
         * @param parameter  参数化的查询，参数集的设置 
         * @return           返回查询的数据集
         * @throws SQLException   抛出SQLException由用户自己去特殊处理
         */
        public static ResultSet executeQuery(Connection conn,String sql,Object... parameter) throws SQLException
        {
                PreparedStatement ps = null;
                ps = conn.prepareStatement(sql);
                int i =1;
                if(parameter != null)
                {
                        for(Object pa1:parameter)
                        {
                                ps.setObject(i, pa1);
                                i++;
                        }
                }
                ResultSet rs = ps.executeQuery();
                return rs;        
        }
        /**
         * 从resultSet中关闭所有的连接
         * @param rs
         */
        static void closeAll(ResultSet rs)
        {
                if(rs == null)
                {
                        return;
                }
                try
                {
                        JDBCUtils.closeQuietly(rs.getStatement().getConnection());
                        JDBCUtils.closeQuietly(rs.getStatement());
                        JDBCUtils.closeAll(rs);
                }catch(SQLException e)
                {
                        
                }
        }
        /**
         *  关闭PreparedStatment连接
         * @param ps   PreparedStatment对象
         */
        //static void closeQuietly(PreparedStatement ps)
        static void closeQuietly(Statement ps)
        {
                if(ps!= null)
                {
                        try
                        {
                                ps.close();
                        }catch(SQLException e)
                        {
                        
                        }
                }
        }
        /**
         *  关闭Connection连接
         * @param connn   Connection对象
         */
        static void closeQuietly(Connection conn)
        {
                if(conn!= null)
                {
                        try
                        {
                                conn.close();
                        }catch(SQLException e)
                        {
                        
                        }
                }
        }
        /**
         *  关闭ResultSet连接
         * @param rs   ResultSet对象
         */
        static void closeQuietly(ResultSet rs)
        {
                if(rs!= null)
                {
                        try
                        {
                                rs.close();
                        }catch(SQLException e)
                        {
                        
                        }
                }
        }
}
```
思路总结：
1：利用配置表，载入mysql和数据库信息
2：利用静态代码段载入驱动（只需要一次）
3： 编写 带connection的executeUpdate和executeQuery,并且在executeResult不要关闭任何信息，待统一关闭
4： 编写 不带Connection的executeUpdate和executeQuery,内部调用带Connction版本的而寒暑
5： 编写closeQuietly函数  和closeAll函数，在调用executeQuery,并关闭closeAll即可

练习使用ctrl+O 打开方法。。（大年30 刚喝完半碗酒  继续。。。）


5事务的练习1：

```java
/**
* 
*/
package com.jdbc.test;

/**
* @author    叶昭良
* @time      2015年2月18日下午1:43:28
* @version   com.jdbc.testtransaction1 V1.0
*/
import java.sql.*;
public class transaction1
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                Connection conn = null;
                PreparedStatement ps1 = null;
                PreparedStatement ps2 = null;
                try
                {
                    conn = JDBCUtils.createConnection();
                    conn.setAutoCommit(false);
                    ps1 = conn.prepareStatement("Update t_person Set Age=Age+1 where Name='yezhao'");
                    ps1.executeUpdate();
                    ps2 = conn.prepareStatement("Update t_person Set Age=Age-1 where Name='xinran'");
                    ps2.executeUpdate();
                    conn.commit();
                } catch (SQLException e)
                {
                    try
                        {
                                conn.rollback();
                        } catch (SQLException e1)
                        {
                                // TODO Auto-generated catch block
                                e1.printStackTrace();
                        }
                }
        }

}
```
测试navicat 和cmd都更新了。


6batch+事务的总和练习：

```java
/**
* 
*/
package com.jdbc.test;

/**
* @author    叶昭良
* @time      2015年2月18日下午2:31:56
* @version   com.jdbc.testtestBatchInsertAndUpdate V1.0
*/
import java.sql.*;
public class testBatchInsertAndUpdate
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                //在没有批量导入数据之前，一般是需要花费很长的时间，现在采用addbatch进行
                Connection conn = null;
                PreparedStatement ps = null;
                String sql="insert into t_person(Id,Name,Age,Gender)values(?,?,?,?)";
                //测试了删除操作，success!
                //String sql= "delete from t_person where Name=?";
                long startTime = System.currentTimeMillis();
                /**
                 * 不添加批处理后，添加1000条后耗时845
                 *  不添加批处理  添加11000条后   耗时2604  ,所以我不知道批处理的好处啊！！！
                 *                           反正即使2530s左右
                 * 
                   添加批处理后                添加1000条后耗时799
                                                 添加11000条后耗时3206
                                                 添加11000条后耗时3143(每个1000提交一次）


                 */
                try
                {
                        //之前表中已有 215行
                        conn = JDBCUtils.createConnection();
                        conn.setAutoCommit(false);//相当于begin的作用
                        ps = conn.prepareStatement(sql);
                        for(int i = 316;i < 11316;i++)
                        {
                                //JDBCUtils.executeUpdate(conn, sql, i,"Autumn",20,1);
                                //ps.clearParameters();
                                ps.setInt(1, i);
                                ps.setString(2,"Winter");
                                ps.setInt(3, 21);
                                ps.setBoolean(4, true);
                                //ps.executeUpdate();
                                
                                
                                /**
                                 * if(i%1000 == 0) //每个1000次提交一次！！！这样好！
                                 * {
                                 *    ps.executeBatch();
                                 *  }
                                 *  少于1000次的最后一笔  再用一次ps.executeBatch();
                                 */
                                ps.addBatch(); //据说是装到箱子的作用
                                if(i%1000 == 0)
                                {
                                        ps.executeBatch();
                                }
                        }
                                //JDBCUtils.executeUpdate(conn, sql, i,"Autumn",20,1);
                                //测试删除使用的
                                //JDBCUtils.executeUpdate(conn, sql, "Autumn");
                        //}
                        ps.executeBatch(); //把箱子里面的数据 直接一次性提交mysql
                        conn.commit();
                        System.out.println("添加1000条后耗时"+(System.currentTimeMillis()
                                        -startTime));
                }catch(SQLException e)
                {
                        try
                        {
                                conn.rollback();
                        } catch (SQLException e1)
                        {
                                // TODO Auto-generated catch block
                                e1.printStackTrace();
                        }
                }finally
                {
                        JDBCUtils.closeQuietly(conn);
                }
                
        }

}
```
7导入手机账簿，并进行手机号所在地的查询

```java
/**
* 
*/
package com.jdbc.test;

/**
* @author    叶昭良
* @time      2015年2月18日下午3:49:22
* @version   com.jdbc.testphoneCheck V1.0
*/
import java.io.*;
import java.sql.*;
import java.util.Scanner;


public class phoneCheck
{

        /**
         * @param args
         */
/**
* ID,MobileNumber,MobileArea,MobileType,AreaCode,PostCode,0
        连接成功
        总共添加了300105条数据.耗时：64712
*/
        private static void load()
        {
                //导入csv数据，利用BufferedInputStream
          InputStream is = null;
          InputStreamReader isr = null;
          BufferedReader bis = null; //一行一行读入数据
          
          Connection conn = null;
          PreparedStatement ps = null;
                int len = 0;
                int i = 0;
                long startms = System.currentTimeMillis();
                try
                {
                        is = new FileInputStream("e://phone.csv");
                    isr = new InputStreamReader(is);
                        bis = new BufferedReader(isr);
                        
                        System.out.println(bis.readLine());
                        //第一行字段舍去
                        String apple=  null;
                        String[] applePiles = null;
                        //第一次测试  字符串的行读入
/*                        System.out.println(bis.readLine());
                        System.out.println(bis.readLine());*/
                        /*apple =bis.readLine();
                        //字符串的一次失误！！ 导致插入失败！！＂＂是默认存在的　必须删掉
                        apple = apple.replaceAll("\"", "");
                        //第二次测试 字符串分隔
                        String[] splitArray = apple.split(",");
                        for(String apple1 :splitArray)
                        {
                                System.out.println(apple1);
                        }*/
                        conn = JDBCUtils.createConnection();
                        conn.setAutoCommit(false);
                        
                        String sql = "insert into phone(MobileNumber,MobileArea,MobileType,Area"
                                        + "Code,PostCode) values(?,?,?,?,?);";
                        ps = conn.prepareStatement(sql);
                        /**
                         * 370中国电信
                                371中国电信
                                之后 报错 ，可能是为空的原因 
                         */
                        while((apple=bis.readLine()) != null)
                        {
                                apple = apple.replaceAll("\"", "");
                                applePiles = apple.split(",");
                                
                                ps.clearParameters();
                                //ps.setInt(1, Integer.parseInt(applePiles[1].equalsIgnoreCase("")?"1111":applePiles[4]));
                                //ps.setInt(1, Integer.parseInt(applePiles[1]));
                                ps.setString(1, applePiles[1]);
                                ps.setString(2, applePiles[2]);
                                //System.out.println(i+applePiles[3]+applePiles[4]);
                                ps.setString(3, applePiles[3]);
//                                ps.setInt(4, Integer.parseInt(applePiles[4]));//.equalsIgnoreCase("")?"1111":applePiles[4]));
//                                ps.setInt(5, Integer.parseInt(applePiles[5]));//.equalsIgnoreCase("")?"1111":applePiles[4]));
                                ps.setString(4, applePiles[4]);
                                ps.setString(5, applePiles[5]);
                                ps.addBatch();

                                i++;
                                if(i%2000==0)
                                {
                                        ps.executeBatch();
                                }
                        }
                        ps.executeBatch();
                        conn.commit();
                        long endMs = System.currentTimeMillis();
                        System.out.println("总共添加了"+i+"条数据"+".耗时："+(endMs-startms));
                }catch(IOException e)
                {
                        throw new RuntimeException("读取文件异常");
                }catch(SQLException e)
                {
                        try
                        {
                                conn.rollback();
                        } catch (SQLException e1)
                        {
                                // TODO Auto-generated catch block
                                e1.printStackTrace();
                        } //又忘记了
                        throw new RuntimeException("数据库插入异常！");
                        
                }
        }
        
        public static void checkPhoneNumber(String PhoneNumber)
        {

                
                PhoneNumber = "%"+PhoneNumber+"%";
                String sql = "select * from phone where MobileNumber like ?";
                ResultSet rs  = null;
                try
                {
                        rs = JDBCUtils.executeQuery(sql, PhoneNumber);
                        if(!rs.next())
                        {
                                System.out.println("当前版本没有"+PhoneNumber+"的信息");
//                                return;
                        }
                        //rs.next();
                        System.out.println(PhoneNumber+"手机号来自"+rs.getString("MobileArea")+rs.getString("MobileType"));
                } catch (SQLException e)
                {
                        // TODO Auto-generated catch block
                        return;
                }finally
                {
                        JDBCUtils.closeAll(rs);
                }
                
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                //导入一次即可
                //load();
                while(true)
                {
                        System.out.println("请输入手机号(至少7位)");
                        Scanner sc = new Scanner(System.in);
                        String PhoneNumber = sc.nextLine();
                        
                        if(PhoneNumber.equalsIgnoreCase("exit")||PhoneNumber.equalsIgnoreCase("quit"))
                        {
                                return;
                        }
                        PhoneNumber = PhoneNumber.substring(0,7);
                        checkPhoneNumber(PhoneNumber);
                }
                
        }

}

```

JDBCUtils 第一遍重写：

//再次书写的时候  注意 对于吃掉的异常的处理
                        //第二  在静态代码段中  加载mysql驱动
                        //静态代码块的两个作用：加载配置控件，加载驱动
                        
                        //try....finally  可以在throw SQLException的情况下使用，最后关闭需要的连接
                        //进一步体会两个executeUpdate的区别（executeQuery也是一样）
                        //一个抛出异常 一个则负责接住 并继续关闭的相应的连接
                        //try....finally 和try...catch....finally的配合使用
                        
                        //在查询时候的编写 则是都抛出！ 不负责关闭！！ 等到查询的时候！ 统一的去关闭
                        //因为可能存在多处查询
复制代码
JDBCUtils 第二遍重写：
* @time      2015年2月19日下午9:52:52
*                          10:20完成            28min左右

//第三次重写 意识到了  静态代码快不能加载 检查性的异常! 在try---catch的时候
        //一定得throw new RuntimeException 才不至于报错
        
        //居然忘记了先重写 Connection的创建！直接close,!有点错乱的感觉！
        //应该是按照逻辑的顺序进行编写，先是创建连接！ 最后在关闭
        //只不过当时是这样想的！ 其实这些关闭会一直反复的用到，
        //就想着先把他们给写上！也是符合逻辑！因为逻辑里面他们是反复被
        //调用的小插件
        
        //同时在创建connection再疑问是否抛出异常？ 最终抛出了？
        //为什么？  因为这个异常是自己可以处理的
        //连连接都拿不到 ！那就直接返回吧！肯定程序是有问题的
        
        //至于executeUpate executeQuery的异常都交由 调用者去处理他的异常
复制代码
JDBCUtils 第三遍重写：

* @time      2015年2月19日下午10:21:46
*                10:40  结束      19min


  *  书写第四遍的时候  遇到一个问题 在executeUpdate的时候  是不是改在
         *  try...finally   的try块就应该返回了 而不是finally的后面
         *  如果在后面 肯定是错误的！！！因为资源已经关闭
复制代码
JDBCUtils 第四遍重写：
* @time      2015年2月19日下午10:41:37
*                        10:55     14min

