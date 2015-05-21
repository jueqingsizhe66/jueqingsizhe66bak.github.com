---
layout: post
title: "2月27日Java周末班试题"
date: 2015-05-11 14:58:41 +0800
comments: true
categories: JavaBasic
---
<!--more-->

0105班Java周末班试题（JDBC 9-16）
阶段性考试（考试6个小时，讲评+重写6个小时）：
记录完成每个题所需要的时长。
第一题、SQL题
表格见图片附件
题目：编写SQL语句完成下面的功能：
条件查询：
1. 在GRADE表中查找80-90份的学生学号和分数
2. 在GRADE 表中查找课程编号为003学生的平均分
3. 在GRADE 表中查询学习各门课程的人数
4. 查询所有姓张的学生的学号和姓名
5. 查询分数在80-90分的学生的学号、姓名、分数
6. 查询学习了’C语言’课程的学生学号、姓名和分数
第二题
不参考、不对照任何老师、同学的代码编写JDBCUtils。并记录写完需要的时间。
第三题
编写手机号码查询归属地案例。并记录写完需要的时间

![](第一题表格.png)

考试未通过，中途出去，fail;


第一题：
费时：4个小时左右
表设计阶段：


```mysql
;;;创建一个考试数据库
create database exam;  
use exam;
````



错误分析： 
curriculum是一个基础表！ 连接grade表！Grade表连接student_info表 ，认为student_info存在外键
经过折腾：认为student_info没有外键，而grade表存在两个外键



Curriculum 表的设计：
curriculum_no 课程编号 :

```mysql
    varchar(20)not null auto_increment primary key, 
    ;;后来思考 int改变为varchar(20)因为也许会出现非数字
corriculum_name课程名称   varchar(20) not null default ‘’
Credit学分 ： real(5,2) not null default 0.0     
    ;;;(real默认是double类型)
```

其间遇到无法显示汉字的情况：
1：数据库级别修改
修改数据库级别，命令如下：use edu(换成你要修改的数据库名，在这里我的数据库为edu),，然后执行命令：
```mysql
alter database exam character set utf-8;
或者
SET NAMES 'utf8'; 
```

2：数据表级别的修改
```mysql
ALTER TABLE table_name DEFAULT CHARSET utf8;
```
3: 发现经过这两个大小修改还是不行,于是也对字段进行修改
```mysql
alter table curriculum modify curriculum_no varchar(20) character set utf8
alter table curriculum modify curriculum_name varchar(20) character set utf8;
```
但是依然不行！！说明之前插入的只好清空，再插入一遍， 然后就可以了



grade表的设计：


此表的设计一波三折，在添加外键的时候多次出现错误，下面先给出错误的，再列出正确的添加方式


错误分析表：
```mysql
stu_no 学号：  varchar(20) not null auto_increment primary key,
  ;;后来思考 int改变为varchar(20)因为也许会出现非数字
课程编号  curriculum_no : int not null, foreign key(curriculum_no) references Curriculum


(Curriculum_no) on delete restrict on update cascade
Score 分数 :    real(5,2) not null default 0.0
```
外键知识：
1.MYSQL在建外键后,会自动建一个同名的索引,引入外键的缺点是会使速度和性能下降

在创建表的时候添加外键：
```mysql
create table grade(
   stu_no varchar(20) not null default '',
    curriculum varchar(20) not null default '',constraint hello foreign key
     (curriculum) references curriculum(curriculum_no) on delete restrict
     on update cascade,
    stu_score real(5,2) not null default 0.00);

```

在创建表后天添加外键：
添加一个叫hello的外键：
```mysql
alter table grade add constraint hello foreign key(stu_curriculum_no) ref
erences curriculum(curriculum_no) on delete restrict on update cascade;

```

查看grade表信息：
```mysql
| grade | CREATE TABLE `grade` (
  `stu_no` varchar(20) NOT NULL,
  `stu_curriculum_no` varchar(20) NOT NULL DEFAULT '',
  `stu_score` double(5,2) NOT NULL DEFAULT '0.00',
  PRIMARY KEY (`stu_no`),
  KEY `hello` (`stu_curriculum_no`),
  CONSTRAINT `hello` FOREIGN KEY (`stu_curriculum_no`) REFERENCES `curriculum` (
`curriculum_no`) ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
```
其中Key hello (stu_curriculum_no) 就是所谓的索引，一般可以这样
查看表索引  show index from grade;
删除表索引  alter table grade drop index hello 

删除表中的外键：
ALTER TABLE grade DROP FOREIGN KEY fk_member;
当然一般在删除的时候需要
show create table grade 这样可以查看 CONSTRAINT后的名字  比如fk_memeber在本例中就是hello        

后来再次思考，grade表不可能有主键，因为学号可能多次出现在不同记录中！于是删除主键
alter table grade drop primary key;
（添加主键的方式：alter table table_test add primary key(id);）


我再次新建一张表grade1目的是测试，在同一个库中的确不能存在两个相同的外键名，否则报错，
下面我用hello1则是通过。（应该是此原因）
```mysql
create table grade1(
     stuno varchar(20) not null default '',
    curi varchar(20) not null default '',constraint hello1 foreign key
     (curi) references curriculum(curriculum_no),
     score real(5,2) not null default 0.00);
```
ERROR 1022 (23000): Can't write; duplicate key in table 'grade'的解决方案：外键名字改成没用过的就可以（整个数据库exam范围内）
下面再次新建一张表grade2 ,目的是测试不同表是不是可以使用字段名字相同的充当外键，当然是没问题啦！
因为在设计表的时候，曾经怕如果curi改为curriculum_no会不会报错的问题。所以就一起尝试了

```mysql
create table grade2(
     stu_no varchar(20) not null default '',
    curi varchar(20) not null default '',constraint hello2 foreign key
     (curi) references curriculum(curriculum_no) on delete restrict on update
    cascade,
     score real(5,2) not null default 0.00);
```
现在先把grade表放在一边 ，待会设计完student_info表之后再聊grade表的事情。

Student_info表的设计


不合理的设计（主键同时是外键，不好）：
学号 student_no : int  not null auto_increment primary key, foreign key(student_no) references 


grade on grade(student_no) on delete restrict on update cascade,（后来修改为 主键！因为一个表即为


主键和外键叠加不好！！ 同时让grade的stu_no 也增加一个外键  连接到student_info表）
姓名  stu_name  : varchar(20) not null default ‘’,
性别  stu_sex    : char(2)  not null default ‘男‘，
出生年月 stu_date (stu_birth): date not null default ‘1990-01-04’
家庭住址 stu_family (stu_addr): varchar(20) not null default ‘’,
备注     stu_note : varchar(30) not null default ‘’,



然后再次回到grade表，添加外键，

```mysql
alter table grade add constraint fuck foreign key(stu_no) references stud
ent_info(stu_no) ;
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint f
ails (`exam`.`#sql-167c_1`, CONSTRAINT `fuck` FOREIGN KEY (`stu_no`) REFERENCES
`student_info` (`stu_no`))

```
报错了！！！为什么？ 
原来在非空表的情况不能添加外键
于是： delete from grade;

```mysql
alter table grade add constraint pig foreign key(stu_no) references
    student_info(stu_no) on delete restrict on update cascade;
```

最终我的grade表的结构：

```mysql
| grade | CREATE TABLE `grade` (
  `stu_no` varchar(20) NOT NULL DEFAULT '',
  `curriculum` varchar(20) NOT NULL DEFAULT '',
  `stu_score` double(5,2) NOT NULL DEFAULT '0.00',
  KEY `hello` (`curriculum`),
  KEY `pig` (`stu_no`),
  CONSTRAINT `hello` FOREIGN KEY (`curriculum`) REFERENCES `curriculum` (`curric
ulum_no`) ON UPDATE CASCADE,
  CONSTRAINT `pig` FOREIGN KEY (`stu_no`) REFERENCES `student_info` (`stu_no`) O
N UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
```



其实经过前面n过程的之后，你会发现这几张表在你的脑海中特别清晰：
1.学生的
2.成绩的
3.课程的



最终的结果是：


curriculum表的设计：
```mysql
create table curriculum(
    curriculum_no int not null auto_increment primary key,
    curriculum_name varchar(20) not null default '',
    curriculum_credit real(5,2) not null  default 0.0);
```
curriculum表的数据添加：
```mysql
insert into curriculum(curriculum_no,curriculum_name,curriculum_credit) v
alues('0002','C语言',3.0);

insert into curriculum <curriculum_no,curriculum_name,curriculum_credit)
values ('0001','计算机基础',2.2);
insert into curriculum (curriculum_no,curriculum_name,curriculum_credit) values('0003','编译原理',2.5);
insert into curriculum (curriculum_no,curriculum_name,curriculum_credit) values('0004','错做系统',3.5);
```


student_info表的设计：
```mysql
create table student_info(
     stu_no varchar(20) not null default '' primary key,
     stu_name varchar(20) not  null default '',
    stu_sex char(4) not null default '男',
     stu_birth date not null default '1990-01-07',
     stu_addr varchar(20) not null default '',
     stu_note varchar(30));
```
student_info表的数据添加：
```mysql
insert into student_info (stu_no,stu_name,stu_sex,stu_birth,stu_addr) val
ues('0001','张三','男',1989-05-17,'北京');

insert into student_info (stu_no,stu_name,stu_sex,stu_birth,stu_addr,stu_
note) values('0002','李四','女',1990-06-22,'内蒙古','此女子性格刁蛮');

insert into student_info(stu_no,stu_name,stu_sex,stu_birth,stu_addr) valu
es('0003','王五','男',1992-02-18,'江西');
```

grade表的设计
```mysql
create table grade3(
    stu_no varchar(20) not null default '',constraint pig34 foreign key(stu_n
o) references student_info(stu_no) on delete restrict on update cascade,
     curriculum varchar(20) not null default '',constraint hellodff foreign ke
y(curriculum) references curriculum(curriculum_no) on delete restrict on update
cascade,
    stu_score real(5,2) not null default 0.00);
```
grade表的数据添加（截取部分）：
```mysql
insert into grade values('0002','0001',75),('0002','0002',60),('0002','00
03',79),('0002','0004',76);
insert into grade(stu_no,curriculum,stu_score) values('0003','0002',78);
注意了：如果外键表的0003没有的话，则无法添加0003，并报错！很正常，因为外键表student_info没有！！
```




题目结果：
```mysql
mysql> select * from grade;
+--------+------------+-----------+
| stu_no | curriculum | stu_score |
+--------+------------+-----------+
| 0001   | 0001       |     80.00 |
| 0001   | 0002       |     85.00 |
| 0001   | 0003       |     90.00 |
| 0001   | 0004       |     92.00 |
| 0002   | 0001       |     75.00 |
| 0002   | 0002       |     60.00 |
| 0002   | 0003       |     79.00 |
| 0002   | 0004       |     76.00 |
| 0003   | 0001       |     80.00 |
| 0003   | 0002       |     78.00 |
| 0003   | 0003       |     97.00 |
| 0003   | 0004       |     86.00 |
+--------+------------+-----------+
12 rows in set (0.00 sec)
```
1. 在GRADE表中查找80-90份的学生学号和分数
```mysql
mysql> select stu_no as 学好 ,stu_score as 分数 from grade where stu_score>80.0
and stu_score<90.0;
+------+-------+
| 学好 | 分数  |
+------+-------+
| 0001 | 85.00 |
| 0003 | 86.00 |
+------+-------+
2 rows in set (0.00 sec)
```



2. 在GRADE 表中查找课程编号为003学生的平均分
```mysql
mysql> select avg(stu_score) from grade where curriculum='0003';
+----------------+
| avg(stu_score) |
+----------------+
|      88.666667 |
+----------------+
1 row in set (0.04 sec)
```




3. 在GRADE 表中查询学习各门课程的人数

```mysql
mysql> select count(*),curriculum from grade group by curriculum;
+----------+------------+
| count(*) | curriculum |
+----------+------------+
|        3 | 0001       |
|        3 | 0002       |
|        3 | 0003       |
|        3 | 0004       |
+----------+------------+
4 rows in set (0.00 sec)
```




4. 查询所有姓张的学生的学号和姓名
```mysql
mysql> select stu_no 学号,stu_name 姓名  from student_info where stu_name like '
张%';
+------+------+
| 学号 | 姓名 |
+------+------+
| 0001 | 张三 |
+------+------+
1 row in set (0.00 sec)
```




5. 查询分数在80-90分的学生的学号、姓名、分数
```mysql
mysql> select s.stu_no 学号,s.stu_name 姓名,g.curriculum 课程,g.stu_score 分数 f
rom grade g
    -> left join student_info s on g.stu_no=s.stu_no;
+------+------+------+-------+
| 学号 | 姓名 | 课程 | 分数  |
+------+------+------+-------+
| 0001 | 张三 | 0001 | 80.00 |
| 0001 | 张三 | 0002 | 85.00 |
| 0001 | 张三 | 0003 | 90.00 |
| 0001 | 张三 | 0004 | 92.00 |
| 0002 | 李四 | 0001 | 75.00 |
| 0002 | 李四 | 0002 | 60.00 |
| 0002 | 李四 | 0003 | 79.00 |
| 0002 | 李四 | 0004 | 76.00 |
| 0003 | 王五 | 0001 | 80.00 |
| 0003 | 王五 | 0002 | 78.00 |
| 0003 | 王五 | 0003 | 97.00 |
| 0003 | 王五 | 0004 | 86.00 |
+------+------+------+-------+
12 rows in set (0.00 sec)
```
进一步求解：

```mysql
mysql> select s.stu_no 学号,s.stu_name 姓名,c.curriculum_name 课程名称,g.stu_sco
re 分数 from grade g
    -> left join student_info s on g.stu_no=s.stu_no
    -> left join curriculum c on g.curriculum=c.curriculum_no;
+------+------+------------+-------+
| 学号 | 姓名 | 课程名称   | 分数  |
+------+------+------------+-------+
| 0001 | 张三 | 计算机基础 | 80.00 |
| 0001 | 张三 | C语言      | 85.00 |
| 0001 | 张三 | 编译原理   | 90.00 |
| 0001 | 张三 | 操作系统   | 92.00 |
| 0002 | 李四 | 计算机基础 | 75.00 |
| 0002 | 李四 | C语言      | 60.00 |
| 0002 | 李四 | 编译原理   | 79.00 |
| 0002 | 李四 | 操作系统   | 76.00 |
| 0003 | 王五 | 计算机基础 | 80.00 |
| 0003 | 王五 | C语言      | 78.00 |
| 0003 | 王五 | 编译原理   | 97.00 |
| 0003 | 王五 | 操作系统   | 86.00 |
+------+------+------------+-------+
12 rows in set (0.00 sec)


mysql> select s.stu_no 学号,s.stu_name 姓名,c.curriculum_name 课程名称,g.stu_sco
re 分数 from grade g
    -> left join student_info s on g.stu_no=s.stu_no
    -> left join curriculum c on g.curriculum=c.curriculum_no
    -> where g.stu_score>80 and g.stu_score<90;
+------+------+----------+-------+
| 学号 | 姓名 | 课程名称 | 分数  |
+------+------+----------+-------+
| 0001 | 张三 | C语言    | 85.00 |
| 0003 | 王五 | 操作系统 | 86.00 |
+------+------+----------+-------+
2 rows in set (0.00 sec)
```



6. 查询学习了’C语言’课程的学生学号、姓名和分数

```mysql
mysql> select s.stu_no 学号,s.stu_name 姓名,c.curriculum_name 课程名称,g.stu_sco
re 分数 from grade g
    -> left join curriculum c on g.curriculum=c.curriculum_no
    -> left join student_info s on g.stu_no=s.stu_no
    -> where c.curriculum_name='c语言';
+------+------+----------+-------+
| 学号 | 姓名 | 课程名称 | 分数  |
+------+------+----------+-------+
| 0001 | 张三 | C语言    | 85.00 |
| 0002 | 李四 | C语言    | 60.00 |
| 0003 | 王五 | C语言    | 78.00 |
+------+------+----------+-------+
3 rows in set (0.00 sec)
```


7. 查询学习了’C语言’课程的学生学号、姓名和分数 并排序
```mysql
mysql> select s.stu_no 学号,s.stu_name 姓名,c.curriculum_name 课程名称,g.stu_sco
re 分数 from grade g
    -> left join curriculum c on g.curriculum=c.curriculum_no
    -> left join student_info s on g.stu_no=s.stu_no
    -> where c.curriculum_name='c语言'
    -> order by g.stu_score;
+------+------+----------+-------+
| 学号 | 姓名 | 课程名称 | 分数  |
+------+------+----------+-------+
| 0002 | 李四 | C语言    | 60.00 |
| 0003 | 王五 | C语言    | 78.00 |
| 0001 | 张三 | C语言    | 85.00 |
+------+------+----------+-------+
3 rows in set (0.00 sec)
```




第二题：
费时：3个小时（重写5遍）
http://www.rupeng.com/forum/thread-44516-1-1.html
sql_properties：

```
mysqlDriver=com.mysql.jdbc.Driver
connectDatabase=jdbc:mysql://localhost/study?seUnicode=true&characterEncoding=UTF8
userName=root
Password=root
```
JDBCUtils:
```java
package com.jdbc.test;

/**
* @author    叶昭良
* @time      2015年2月19日下午10:41:37
*                        10:55     14min
* @version   com.jdbc.testrewrite4 V1.0
*/
import java.util.*;
import java.sql.*;
import java.io.*;
public class rewrite4
{
        private final static String mysqlDriver;
        private final static String connectDatabase;
        private final static String userName;
        private final static String Password;
        
        static
        {
                Properties prop = new Properties();
                InputStream in = rewrite4.class.getResourceAsStream("sql.properties");
                try
                {
                        prop.load(in);
                        mysqlDriver = prop.getProperty("mysqlDriver");
                        connectDatabase = prop.getProperty("connectDatabase");
                        userName = prop.getProperty("userName");
                        Password = prop.getProperty("Password");
                } catch (IOException e)
                {
                        // TODO Auto-generated catch block
                        throw new RuntimeException("配置文件加载失败");
                }
                try
                {
                        Class.forName(mysqlDriver);
                } catch (ClassNotFoundException e)
                {
                        // TODO Auto-generated catch block
                        throw new RuntimeException("mysql驱动加载失败");
                }
        }
        public static Connection createConnection()
        {
                Connection conn = null;
                try
                {
                        conn = DriverManager.getConnection(connectDatabase);
                } catch (SQLException e)
                {
                        // TODO Auto-generated catch block
                        throw new RuntimeException("未能正确创建连接");
                }
                return conn;
                
        }
        public static int executeUpdate(String sql,Object... param) throws SQLException
        {
                Connection conn = rewrite4.createConnection();
                return rewrite4.executeUpdate(conn,sql, param);
        }
        public static int executeUpdate(Connection conn,String sql,Object... param) throws SQLException
        {
                PreparedStatement ps = null;
                int row = 1;
                try
                {
                        ps = conn.prepareStatement(sql);
                        for(Object par:param)
                        {
                                ps.setObject(row, par);
                                row++;
                        }                
                        return ps.executeUpdate();
                }finally
                {
                        rewrite4.closeQuietly(ps);
                }

        }
        
        public static ResultSet executeQuery(String sql,Object... param) throws SQLException
        {
                Connection conn = rewrite4.createConnection();
                return rewrite4.executeQuery(conn, sql, param);
        }
        public static ResultSet executeQuery(Connection conn,String sql,Object... param) throws SQLException
        {
                ResultSet  rs = null;
                PreparedStatement ps = null;
                
                ps = conn.prepareStatement(sql);
                int row =1;
                for(Object par:param)
                {
                        ps.setObject(row, par);
                        row++;
                }
                rs = ps.executeQuery();
                return rs;
                
        }
        public static void closeQuietly(Connection conn)
        {
                if(conn != null)
                {
                        try
                        {
                                conn.close();
                        } catch (SQLException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }
                }
        }
        public static void closeQuietly(Statement stmt)
        {
                if(stmt != null)
                {
                        try
                        {
                                stmt.close();
                        } catch (SQLException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }
                }
        }
        public static void closeQuietly(ResultSet rs)
        {
                if(rs != null)
                {
                        try
                        {
                                rs.close();
                        } catch (SQLException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }
                }
        }
        public static void closeAll(ResultSet rs)
        {
                if(rs != null)
                {
                        try
                        {
                                rs.close();
                        } catch (SQLException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }
                }
        }
        
        public static void rollback(Connection conn)
        {
                if(conn != null)
                {
                        try
                        {
                                conn.rollback();
                        } catch (SQLException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }
                }
        }
}
```


第三题：
费时1个小时左右

```java
/**
* 
*/
package com.jdbc.test;

/**
* @author    叶昭良
* @time      2015年2月18日下午3:49:22
*                        下午5:00左右
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
                                //每个2000步之后进行提交一次 到数据库
                                if(i%2000==0)
                                {
                                        ps.executeBatch();
                                }
                        }
                        //提交 最后的几百步
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
                        
                }finally
                {
                        //用完之后记得差屁股
                        JDBCUtils.closeQuietly(ps);
                        JDBCUtils.closeQuietly(conn);
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
                        //用完之后就差屁股
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
加上对话框的设计：

```java
/**
* 
*/
package com.jdbc.test;

import java.sql.ResultSet;
import java.sql.SQLException;



import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;



/**
* @author    叶昭良
* @time      2015年2月22日下午7:48:03
* @version   com.jdbc.testPhoneCheckUpdate V1.0
*/
import GTKEncapsulate.*;
public class PhoneCheckUpdate
{

        /**
         * @param args
         */
        private static  OOLabel olLabel;
        private static  OOEntry oeCerti;
        private static OOButton obClick;
        private static OOBox obBox;
        private static OOWindow owPig;
        public PhoneCheckUpdate()
        {
                olLabel = new  OOLabel("请输入电话的前六位：");
                oeCerti = new OOEntry();
                oeCerti.setTextMaxLength(6);
                oeCerti.setText("151010");
                
                obBox = new OOBox();
                obClick = new OOButton("查询");
                owPig = new OOWindow();
                owPig.setWidgetSize(200, 50);
                //show
                owPig.show();
                owPig.setExitAfterDestroy(true);
                olLabel.show();
                oeCerti.show();
                obBox.show();
                obClick.show();
                
                //contains
                owPig.add(obBox);        
                obBox.add(olLabel);
                obBox.add(oeCerti);
                obBox.add(obClick);
                
                //添加事件
                obClick.addClickedListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                String text= oeCerti.getText();
                                OOMessageDialog omd = new OOMessageDialog(checkPhoneNumber(text));
                                omd.run();
                                omd.destroy();
                                
                        }
                });
        }
        private static String checkPhoneNumber(String PhoneNumber)
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
                                return "当前版本没有"+PhoneNumber+"的信息";
                                
//                                return;
                        }
                        //rs.next();
                        System.out.println(PhoneNumber+"手机号来自"+rs.getString("MobileArea")+rs.getString("MobileType"));
                        return PhoneNumber+"手机号来自"+rs.getString("MobileArea")+rs.getString("MobileType");
                        
                } catch (SQLException e)
                {
                        // TODO Auto-generated catch block
                        return null;
                }finally
                {
                        //用完之后就差屁股
                        JDBCUtils.closeAll(rs);
                }
                
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                GTK.gtk_init();
                PhoneCheckUpdate pcu = new PhoneCheckUpdate();
                
                GTK.gtk_main();
        }

}
```

