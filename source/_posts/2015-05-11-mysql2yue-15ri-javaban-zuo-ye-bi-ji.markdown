---
layout: post
title: "mysql2月15日Java班作业笔记"
date: 2015-05-11 14:58:45 +0800
comments: true
categories: Mysql
---
<!--more-->

1、MYSQL官方版下载地址（不推荐初学者使用）：http://www.mysql.com/downloads/
    如鹏网版绿色免安装版MYSQL（推荐初学者使用）：http://pan.baidu.com/s/1c0COfGc
2、 常用概念
       一般我们说的数据库是指DBMS，数据库管理系统，比如“你用了什么数据库”，“你会用什么数据库”,”你把数据库删了”。前两个是DBMS，第三个是指的是真实的数据
库（database）：众多事物的集合
表(table)：就是一类事物的集合
列(column)：某类事物的具体的字段。也叫做字段。

3.解压后运行 mysql文件夹下的mysqld.exe,启动mysql服务器
4.运行mysql
C:\mysql\bin>mysql-uroot –proot
复制代码

5:显示当前所有的仓库


```mysql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema|
| customer           |
| mysql              |
| performance_schema|
| study              |
| t_employees        |
+--------------------+
6 rows in set (0.39sec)
```

6：创建了一个database
```mysql
mysql> create database test;
Query OK, 1 rowaffected (0.10 sec)
```

7:真的有了test
```mysql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema|
| customer           |
| mysql              |
| performance_schema|
| study              |
| t_employees        |
| test               |
+--------------------+
```

8：使用test库

```mysql
mysql> use test;
Database changed
```

9：显示表格，一个表格都没有
```mysql
mysql> show tables;
Empty set (0.04 sec)
```

10：创建表格
主键：primary key 一般一张表一个主键，且该主键一般是没有任何的物理意义或者业务意义的数字，比如身份证ID
Auto_increment:自增长的好处，是可以不必设置该列的值，自动增长（自动增长的步数一般为1，也可以更改，show variables like ‘%auto_inc%’查询如何设置。）
Engine:一把是设置为innodb，也可以有其他的比如myisam，memory等
Not null：是比较好的设计表的方式，据说可以加快表的索引速度，表示非空

Default:  默认 表示默认值 一般是default value;
```mysql
mysql> create table apple(
    -> id int auto_increment primary key,
    -> name varchar(20) not null default '')engine = innodb;
Query OK, 0 rowsaffected (0.69 sec)
```

10：的确是有了一张apple表
```mysql
mysql> show tables;
+----------------+
| Tables_in_test |
+----------------+
| apple          |
+----------------+
1 row in set (0.00sec)
```

11：查看apple表结构

```mysql
mysql> desc apple;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | int(11)     | NO  | PRI | NULL    | auto_increment |
| name  | varchar(20) | NO   |    |         |                |
+-------+-------------+------+-----+---------+----------------+
2 rows in set (0.09sec)
```
类似的查看方式1：


```mysql
mysql> explain apple;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | int(11)     | NO  | PRI | NULL    | auto_increment |
| name  | varchar(20) | NO   |    |         |                |
+-------+-------------+------+-----+---------+----------------+
2 rows in set (0.00sec)
```

类似的查看方式2：


```mysql
mysql> show columns from apple;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | int(11)     | NO  | PRI | NULL    | auto_increment |
| name  | varchar(20) | NO   |    |         |                |
+-------+-------------+------+-----+---------+----------------+
2 rows in set (0.00sec)
```

最本质的查看表结构的方式：

```mysql
mysql> show create table apple\G;
***************************1. row ***************************
       Table: apple
Create Table: CREATETABLE `apple` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) NOT NULL DEFAULT '',
  PRIMARY KEY (`id`)
) ENGINE=InnoDBDEFAULT CHARSET=latin1
1 row in set (0.00sec)
ERROR:
No query specified
```

13插入表数据

```mysql
mysql> insert into apple values (1,'tab'),(2,'ctrl'),(3,'shift');
Query OK, 3 rowsaffected (0.39 sec)
Records: 3  Duplicates: 0 Warnings: 0
```

14 查看表数据

```mysql
mysql> select *from apple;
+----+-------+
| id | name  |
+----+-------+
|  1 | tab  |
|  2 | ctrl |
|  3 | shift |
+----+-------+
3 rows in set (0.00sec)
```

15 插入数据
```mysql
mysql> insert into apple(id,name) values(4,'caps');
Query OK, 1 rowaffected (0.08 sec)
mysql> insert intoapple values (6,'tomato');
Query OK, 1 rowaffected (0.09 sec)
```

为什么这种方式不行呢？ 正在查
```mysql
mysql> insert into apple values ('tomato');
ERROR 1136 (21S01):Column count doesn't match value count at row 1
利用mysql的最佳工具help
mysql> helpauto_increment;
Name:'AUTO_INCREMENT'
Description: ….
Examples:
CREATE TABLE animals(
     id MEDIUMINT NOT NULL AUTO_INCREMENT,
     name CHAR(30) NOT NULL,
     PRIMARY KEY (id)
);
INSERT INTO animals(name) VALUES
    ('dog'),('cat'),('penguin'),
('lax'),('whale'),('ostrich');
```



所以解决方法是：
```mysql
mysql> insert into apple (name) values("backspace");
Query OK, 1 rowaffected (0.10 sec)
mysql> select * from apple;
+----+-----------+
| id | name      |
+----+-----------+
|  1 | tab       |
|  2 | ctrl      |
|  3 | shift     |
|  4 | caps      |
|  6 | tomato    |
|  7 | backspace |
+----+-----------+
6 rows in set (0.00 sec)
```

16:无意中添加了一行没有必要的数据
```mysql
mysql> insert into apple (name) values("luanqibazhao");
Query OK, 1 row affected (0.11 sec)
mysql> select * from apple;
+----+--------------+
| id | name         |
+----+--------------+
|  1 | tab          |
|  2 | ctrl         |
|  3 | shift        |
|  4 | caps         |
|  6 | tomato       |
|  7 | backspace    |
|  8 | luanqibazhao |
+----+--------------+
7 rows in set (0.00 sec)
```

17：删掉id=8的数据
```mysql
mysql> delete * from apple where id=8 andname="luanqibazhao";
ERROR 1064 (42000): You have an error in your SQL syntax; check themanual that
corresponds to your MySQL server version for the right syntax to usenear '* fro
m apple where id=8 and name="luanqibazhao"' at line 1
```

为什么不行？
查看帮助
```mysql
mysql> help delete;
Name: 'DELETE'
Description:
Syntax:
DELETE is a DML statement that removes rows from a table.
Single-Table Syntax
DELETE [LOW_PRIORITY] [QUICK] [IGNORE] FROM tbl_name
    [PARTITION(partition_name,...)]
    [WHERE where_condition]
    [ORDER BY ...]
[LIMIT row_count]
The DELETE statement deletes rows fromtbl_name and returns the number
of deleted rows. To check the number ofdeleted rows, call the
ROW_COUNT() function describedin
```



原来是语法错误
```mysql
mysql> delete from apple where id=8 andname="luanqibazhao";
Query OK, 1 row affected (0.11 sec)
```


真的删掉了！
```mysql
mysql> select * from apple;
+----+-----------+
| id | name      |
+----+-----------+
|  1 |tab       |
|  2 |ctrl      |
|  3 |shift     |
|  4 |caps      |
|  6 |tomato    |
|  7 |backspace |
+----+-----------+
6 rows in set (0.00 sec)
```

18.有时候我们还想着更新某个id的值
```mysql
mysql> update apple set name=caps1 from id=4;
ERROR 1064 (42000): You have an error in yourSQL syntax; check the manual that
corresponds to your MySQL server version forthe right syntax to use near 'from
id =4' at line 1
```

为什么报错呢？可以使用show errors 查看报错信息类似的show warnings;


查询帮助：
```mysql
mysql> help  update
Name: 'UPDATE'
Description:
Syntax:
Single-table syntax:
UPDATE [LOW_PRIORITY] [IGNORE]table_reference
    SETcol_name1={expr1|DEFAULT} [, col_name2={expr2|DEFAULT}] ...
   [WHERE where_condition]
   [ORDER BY ...]
   [LIMIT row_count]
```


帮助中显示并没有from的后缀，于是把from改为where，然后再让caps1加上单引号。
第一个错误修正加上单引号
第二个错误修正 from改为where
```mysql
mysql> update apple set name='caps1' whereid =4;
Query OK, 1 row affected (0.16 sec)
Rows matched: 1  Changed: 1 Warnings: 0
```



到这边为止，是全套的mysql的常用基本指令，包含建库，建表，插入表数据，删除表数据，更新表数据，查询库，查询表，查询表数据。当然可能少了清空表数据（truncate table apple）删除表（drop table apple）,删除库（drop database test）


有时候我们可能修改表的结构

于是我们首先查询表，并增加了一个age字段，该字段是什么数据类型？　保证数据存储，并高效。


```mysql
mysql> desc apple;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | int(11)     | NO   | PRI | NULL    | auto_increment |
| name  | varchar(20) | NO   |     |         |                |
+-------+-------------+------+-----+---------+----------------+
```

小姐了如下的整形数据的设计考虑：
分为两类一类是精准数值类型，另一类为非精准型
包含整数和小数

TinyInt :(-128-127  或者unsigned 是  0-256)
           一般用于  Age 年龄字段（树的年龄可能过大

SmallInt：具体的数值范围可以用help smallint查到
         一般用于技能值
          ids
mediumint  一般用于   Auto_increment
                      Row number 行数是够的！！
                    行数一般也是1600行  最大的暂时有做到8亿，但是数据一大会影响
                    到mysql的存储问题，所以一般是分库分表
int(integer) ：
           一般用于  Auto_increment
                    比如  money  ， salary 就够了（游戏当中的钱　通常会较大）
    
BigInt  : 一般用于科学计算
             人口数 Population

精准性有一类比较特殊的DECIMAL(DEC)
DECIMAL(f,g)   f表示总的位数  g表示小数点后的位数
比如   money DECIMAL(12,2)
           那么存储会消耗  整数部分：（12-2）  = 9(==4个字节 规定 ) + 1/2 =5
                           小数部分： 2/2 = 1
                               总消耗： 5+1  =6 个字节

非精准型：
Float(p)
Real  p表示precise
DOUBLE PRECISE

于是我们选择age 字段为tinyint即可。

```mysql
mysql> alter table apple add age tinyint not null default 0;
Query OK, 0 rows affected (0.89 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

再次查询表结构


```mysql
mysql> desc apple;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | int(11)     | NO   | PRI | NULL    | auto_increment |
| name  | varchar(20) | NO   |     |         |                |
| age   | tinyint(4)  | NO   |     | 0       |                |
+-------+-------------+------+-----+---------+----------------+
3 rows in set (0.05 sec)
```
查询表数据：
```mysql
mysql> select * from apple;
+----+-----------+-----+
| id | name      | age |
+----+-----------+-----+
|  1 | tab       |   0 |
|  2 | ctrl      |   0 |
|  3 | shift     |   0 |
|  4 | caps1     |   0 |
|  6 | tomato    |   0 |
|  7 | backspace |   0 |
+----+-----------+-----+
6 rows in set (0.00 sec)
```

有时候我们可能修改表的结构
于是我们首先查询表，并增加了一个age字段，该字段是什么数据类型，才能保证更加高效。

```mysql
mysql> desc apple;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | int(11)     | NO   | PRI | NULL    | auto_increment |
| name  | varchar(20) | NO   |     |         |                |
+-------+-------------+------+-----+---------+----------------+
```

  
分为两类一类是精准数值类型，另一类为非精准型
  
包含整数和小数
  
  
设计考虑：
  
TinyInt :(-128-127  或者unsigned 是   0-256)
  
           一般用于  Age  年龄字段（树的年龄可能过大
  
  
SmallInt：（可以查询help smallint 来查看数值范围）
  
         一般用于技能值
  
          ids
  
mediumint  一般用于    Auto_increment
  
                      Row number  行数是够的！！
  
                    行数一般也是1600行  最大的暂时有做到8亿，但是数据一大会影响
  
                    到mysql的存储问题，所以一般是分库分表
  
int(integer) ：
  
           一般用于   Auto_increment
  
                    比如   money   salary 就够了
  
   
  
BigInt  : 一般用于科学计算
  
             人口数 Population
  
  
精准性有一类比较特殊的DECIMAL(DEC)
  
DECIMAL(f,g)   f表示总的位数  g表示小数点后的位数
  
比如    money DECIMAL(12,2)
  
           那么存储会消耗  整数部分：（12-2）  =  9(==4个字节 规定 ) + 1/2 =5
  
                           小数部分： 2/2 = 1
  
                               总消耗： 5+1   =6 个字节
  
  
非精准型：
  
Float(p)
  
Real  p表示precise
  
DOUBLE PRECISE
  
  
  
于是我们选择age 字段为tinyint即可。
```mysql
mysql> alter table apple add age tinyint not null default 0;
Query OK, 0 rows affected (0.89 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

再次查询表结构
```mysql
mysql> desc apple;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | int(11)     | NO   | PRI | NULL    | auto_increment |
| name  | varchar(20) | NO   |     |         |                |
| age   | tinyint(4)  | NO   |     | 0       |                |
+-------+-------------+------+-----+---------+----------------+
3 rows in set (0.05 sec)
```
查询结果：
```mysql
mysql> select * from apple;
+----+-----------+-----+
| id | name      | age |
+----+-----------+-----+
|  1 | tab       |   0 |
|  2 | ctrl      |   0 |
|  3 | shift     |   0 |
|  4 | caps1     |   0 |
|  6 | tomato    |   0 |
|  7 | backspace |   0 |
+----+-----------+-----+
6 rows in set (0.00 sec)
```

char与varchar的比较
分两个部分进行比较
1:创建表时候
2:添加表数据的时候

查看help帮助信息

```mysql
mysql> help char
Name: 'CHAR'
Description:
[NATIONAL] CHAR[(M)] [CHARACTER SET charset_name] [COLLATE
collation_name]

A fixed-length string that is always right-padded with spaces to the
specified length when stored. M represents the column length in
characters. The range of M is 0 to 255. If M is omitted, the length is
1.

*Note*: Trailing spaces are removed when CHAR values are retrieved
unless the PAD_CHAR_TO_FULL_LENGTH SQL mode is enabled.
```


sql_mode的pad_char_to_full_length,这是一个特别的模式，将会在下文列出，至于M is from 0 to 255是指char(256)会报错！

再次查看varchar的帮助信息
```mysql
mysql> help varchar
Name: 'VARCHAR'
Description:
[NATIONAL] VARCHAR(M) [CHARACTER SET charset_name] [COLLATE
collation_name]

A variable-length string. M represents the maximum column length in
characters. The range of M is 0 to 65,535. The effective maximum length
of a VARCHAR is subject to the maximum row size (65,535 bytes, which is
shared among all columns) and the character set used. For example, utf8
characters can require up to three bytes per character, so a VARCHAR
column that uses the utf8 character set can be declared to be a maximum
of 21,844 characters. See
```
可以看到最大长度为 65535，
并且在utf8字符的时候（也就是national varchar)的情况下，最大阴虚21844, 也就是national varchar(21845)会报错
21844*3 =65532（计算器）（可能还剩下3个字符，摆放需要的设置符）
说明当前我的版本的mysql 5.6的utf所占用的是三个字节，据说新版本的是4个字节的

新建一个测试的orange表
```mysql
mysql> create table orange(
    -> col1 char(6) not null default '',
    -> col2 varchar(65510));
Query OK, 0 rows affected, 1 warning (0.55 sec)
```

---习惯的查询一下表的样式

新增一个varchar(100)字段做测试

```mysql
mysql>  alter table orange add col3 varchar(100);
ERROR 1118 (42000): Row size too large. The maximum row size for the used tabl
type, not counting BLOBs, is 65535. This includes storage overhead, check the
nual. You have to change some columns to TEXT or BLOBs
mysql> alter table orange add col3 varchar(100);
```
为什么？再添加varchar(100）则会一直报错.Maximum(varchar)=65535 现在使用了65510 ，再加上25呢？如果改为varcahr

```mysql
mysql> alter table orange add col3 varchar(100);
Query OK, 0 rows affected (0.85 sec)
Records: 0  Duplicates: 0  Warnings: 0
```


```mysql
mysql> alter table orange add col3 varchar(22);
ERROR 1118 (42000): Row size too large. The maximum row size for the us
type, not counting BLOBs, is 65535. This includes storage overhead, che
nual. You have to change some columns to TEXT or BLOBs
mysql> alter table orange add col3 varchar(1);
Query OK, 0 rows affected (0.69 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
一直报错！！

再次显示表结构

```mysql
mysql> desc orange;
+-------+----------------+------+-----+---------+-------+
| Field | Type           | Null | Key | Default | Extra |
+-------+----------------+------+-----+---------+-------+
| col1  | char(6)        | NO   |     |         |       |
| col2  | varchar(65510) | YES  |     | NULL    |       |
| col3  | varchar(1)     | YES  |     | NULL    |       |
+-------+----------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```
为什么呢？

```mysql
mysql> show variables like '%sql_mode%';
+---------------+-------------------+
| Variable_name | Value             |
+---------------+-------------------+
| sql_mode      | STRICT_ALL_TABLES |
+---------------+-------------------+
1 row in set (0.00 sec)
```
当前的sql模式是严格表模式，sql_mode变量特别重要，所以我进行了测试！假设我们现在改回去默认的空模式；


```mysql
mysql> set sql_mode='';
Query OK, 0 rows affected (0.00 sec)
复制代码
重新创建一张香蕉表banana:
mysql> create table banana(
    -> col1 char(6),
    -> col2 varchar(65510));
Query OK, 0 rows affected (0.32 sec)
```
显示表结构信息：


```mysql
mysql> desc banana;
+-------+----------------+------+-----+---------+-------+
| Field | Type           | Null | Key | Default | Extra |
+-------+----------------+------+-----+---------+-------+
| col1  | char(6)        | YES  |     | NULL    |       |
| col2  | varchar(65510) | YES  |     | NULL    |       |
+-------+----------------+------+-----+---------+-------+
2 rows in set (0.01 sec)
```
发现还是会报错的：


```mysql
mysql> alter table banana add col3 varchar(100);
ERROR 1118 (42000): Row size too large. The maximum row size for the used tabl
type, not counting BLOBs, is 65535. This includes storage overhead, check the
nual. You have to change some columns to TEXT or BLOBs
```


---终于给我报错了
```mysql
mysql> alter table orange add col5 char(258);
ERROR 1074 (42000): Column length too big for column 'col5' (max = 255); use BLO
B or TEXT instead

mysql> alter table orange add col6 char(65537);
ERROR 1074 (42000): Column length too big for column 'col6' (max = 255); use BLO
B or TEXT instead
```



总结varchar最大量不能超过65530.。。char不能超过256.。。
上面是一些创建char和varchar表的时候可能会出现的一些情况。
接下来测试添加数据：

```mysql
mysql> alter table banana modify col2 varchar(6);
Query OK, 0 rows affected (0.81 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc banana;
+-------+------------+------+-----+---------+-------+
| Field | Type       | Null | Key | Default | Extra |
+-------+------------+------+-----+---------+-------+
| col1  | char(6)    | YES  |     | NULL    |       |
| col2  | varchar(6) | YES  |     | NULL    |       |
+-------+------------+------+-----+---------+-------+
2 rows in set (0.02 sec)
```
不是特别能够显示其中的区别：

```mysql
mysql> insert into banana values('abc','abc');
Query OK, 1 row affected (0.04 sec)

mysql> select * from banana;
+--------+------+
| col1   | col2 |
+--------+------+
| abc    | abc  |
+--------+------+
1 row in set (0.00 sec)
```
如果加入


```mysql
mysql> select concat(col1,'-'),concat(col2,'-') from banana;
+------------------+------------------+
| concat(col1,'-') | concat(col2,'-') |
+------------------+------------------+
| abc   -          | abc-             |
+------------------+------------------+
1 row in set (0.00 sec)
```
再次比较

```mysql
mysql> select length(col1),length(col2) from banana;
+--------------+--------------+
| length(col1) | length(col2) |
+--------------+--------------+
|            6 |            3 |
+--------------+--------------+
1 row in set (0.02 sec)
```
可见在sql_mode=pad_char_to_full_length的情况下，char即使赋的值是abc三个字符，该类型也会填充到6个字符
      而varchar则是三个字符就三个字符
而如果sql_mode = '' 呢？ 
         sql_mode ='strict_all_tables'呢？

如果是sql_mode= ''

```mysql
mysql> set sql_mode='';
Query OK, 0 rows affected (0.00 sec)

mysql> select concat(col1,'-'),concat(col2,'-') from banana;
+------------------+------------------+
| concat(col1,'-') | concat(col2,'-') |
+------------------+------------------+
| abc-             | abc-             |
+------------------+------------------+
1 row in set (0.00 sec)

mysql> select length(col1),length(col2) from banana;
+--------------+--------------+
| length(col1) | length(col2) |
+--------------+--------------+
|            3 |            3 |
+--------------+--------------+
1 row in set (0.00 sec)
```
如果是sql_mode=strict_all_tables?


```mysql
mysql> set sql_mode='strict_all_tables';
Query OK, 0 rows affected (0.00 sec)

mysql> select concat(col1,'-'),concat(col2,'-') from banana;
+------------------+------------------+
| concat(col1,'-') | concat(col2,'-') |
+------------------+------------------+
| abc-             | abc-             |
+------------------+------------------+
1 row in set (0.00 sec)

mysql> select length(col1),length(col2) from banana;
+--------------+--------------+
| length(col1) | length(col2) |
+--------------+--------------+
|            3 |            3 |
+--------------+--------------+
1 row in set (0.00 sec)
```

好了，我们可以小结
在pad_char_to_full_length的情况下，char会自动填充到定义的字符，而varchar不会
   在其他的sql_mode模式下，char和varchar感觉没有太大的区别
  varchar一个表的最大长度是65535
  char在一个表的最大长度是255

所在在存储空间允许的情况下，尽量使用char
在需要更大的文本可以用blod或者text。

字符串类型，一般使用char较多。
char
varchar
smallblod,mediumblod,longblod
smalltext,mediumtext,longtext


枚举类字段的创建是为了让用户只能选取某个值范围。
创建一个枚举表进行测试
```mysql
mysql> create table enum_t(
    -> col1 enum('F','M','UN'));
Query OK, 0 rows affected (0.29 sec)
```

插入数据：
```mysql
mysql> insert into enum_t values('F');
Query OK, 1 row affected (0.10 sec)

mysql> insert into enum_t values('M');
Query OK, 1 row affected (0.06 sec)

mysql> insert into enum_t values('F,M');
Query OK, 1 row affected, 1 warning (0.10 sec)

mysql> insert into enum_t values('UN');
Query OK, 1 row affected (0.07 sec)

mysql> insert into enum_t values('F,M,UN');
Query OK, 1 row affected, 1 warning (0.04 sec)
```


显示数据：
```mysql
mysql> select col1,col1+0 from enum_t;
+------+--------+
| col1 | col1+0 |
+------+--------+
| F    |      1 |
| M    |      2 |
|      |      0 |
| UN   |      3 |
|      |      0 |
+------+--------+
5 rows in set (0.00 sec)
```


发现第三列和第五列的col1的字段值缺失，是因为没有在枚举类中该值！然而mysql还有另外一种类型叫做set，却是可以存在不同的情况


------再次返回到sql_mode有问题的那部分
```mysql
mysql> desc enum_t;
+-------+--------------------+------+-----+---------+-------+
| Field | Type               | Null | Key | Default | Extra |
+-------+--------------------+------+-----+---------+-------+
| col1  | enum('F','M','UN') | YES  |     | NULL    |       |
+-------+--------------------+------+-----+---------+-------+
1 row in set (0.00 sec)

mysql> insert into enum_t values('a');
Query OK, 1 row affected, 1 warning (0.09 sec)
```




---有警告一般是和sql_mode有关，如果是strict_all_tables一般是error


---让我们来看一个和enum特别像的set
```mysql
mysql> create table set_t(
    -> col1 set('F','M','UN'));
Query OK, 0 rows affected (0.25 sec)
```
插入表数据

```mysql
mysql> insert into set_t values('F');
Query OK, 1 row affected (0.07 sec)

mysql> insert into set_t values('M');
Query OK, 1 row affected (0.08 sec)

mysql> insert into set_t values('F,M');
Query OK, 1 row affected (0.08 sec)

mysql> insert into set_t values('UN');
Query OK, 1 row affected (0.05 sec)

mysql> insert into set_t values('F,M,UN');
Query OK, 1 row affected (0.07 sec)
```

显示表数据信息：
```mysql
mysql> select col1,col1+0 from set_t;
+--------+--------+
| col1   | col1+0 |
+--------+--------+
| F      |      1 |
| M      |      2 |
| F,M    |      3 |
| UN     |      4 |
| F,M,UN |      7 |
+--------+--------+
5 rows in set (0.00 sec)
```
可以发现F,M虽然没有定义，但是也是可以使用！！！只要不重复都可以在set中出现，并遵循下面的计算方式：
因为在set模式下，默认存储室
F  1
M  10
UN 100
所以F,M  11
    F,M,UN 111


引入了二进制，于是学习了mysql特殊的一种二进制类型binary,和bit类型（bit(4) 就表示最大值为1111 也就是15）

```mysql
mysql> help binary
Name: 'BINARY'
Description:
BINARY(M)

The BINARY type is similar to the CHAR type, but stores binary byte
strings rather than nonbinary character strings. M represents the
column length in bytes.
```
可以看到binary类似于char只不过是存储二进制形式
```mysql
mysql> create table binary_t(
    -> col1 binary(4));
Query OK, 0 rows affected (0.33 sec)
```



插入数据：超过5个字节就错误了

```mysql
mysql> desc binary_t;
+-------+-----------+------+-----+---------+-------+
| Field | Type      | Null | Key | Default | Extra |
+-------+-----------+------+-----+---------+-------+
| col1  | binary(4) | YES  |     | NULL    |       |
+-------+-----------+------+-----+---------+-------+
1 row in set (0.05 sec)

mysql> insert into binary_t values('a');
Query OK, 1 row affected (0.07 sec)

mysql> insert into binary_t values('abcd');
Query OK, 1 row affected (0.07 sec)

mysql> insert into binary_t values('abcde');
ERROR 1406 (22001): Data too long for column 'col1' at row 1
```

上述的过程类似于char，但是binary和char到底体现在哪里的区别？


```mysql
mysql> select * from binary_t;
+------+
| col1 |
+------+
| a    |
| abcd |
+------+
2 rows in set (0.00 sec)

mysql> select col1='a' from binary_t;
+----------+
| col1='a' |
+----------+
|        0 |
|        0 |
+----------+
2 rows in set (0.00 sec)
```
为什么明明有a却是无法找到？是不是跟sql_mode有关系？

```mysql
mysql> show variables like '%sql_mode%';
+---------------+-------------------+
| Variable_name | Value             |
+---------------+-------------------+
| sql_mode      | STRICT_ALL_TABLES |
+---------------+-------------------+
1 row in set (0.00 sec)
```
重新设置为空状态（默认一般是空的状态），再次尝试：

```mysql
mysql> select * from binary_t;
+------+
| col1 |
+------+
| a    |
| abcd |
+------+
2 rows in set (0.00 sec)

mysql> select col1='a' from binary_t;
+----------+
| col1='a' |
+----------+
|        0 |
|        0 |
+----------+
2 rows in set (0.00 sec)
```
可以看到依然不行，这就是二进制的一些小区别，不是类似于char在pad_char_to_full_length(专门针对char)
这时候需要用专门的测试方法

```mysql
mysql> select col1='a\0\0\0' from binary_t;
+----------------+
| col1='a\0\0\0' |
+----------------+
|              1 |
|              0 |
+----------------+
2 rows in set (0.00 sec)
```
可以了！！因为binary是为默认加上\0\0\0 ，而char在sql='pad_char_to_full_length'会默认加上空格。这就是一个区别，慎用，一般用char即可，
也许binary可能是存储少占空间。

一个特别的数据类型Bit的用法（他是一个跟计算机的最小单元bit位有关系的）
手又痒了，必须再来个bit_t不可

```mysql
mysql> create table bit_t(
    -> ids bit(4));
Query OK, 0 rows affected (0.20 sec)
```

--bit(4) y也就是说  1111  最多是，类似地int(4)  char(4) varchar(4)  binary(4)  等有着其特殊的含义  
---用严格模式测试一下

```mysql
mysql> help int
Name: 'INT'
Description:
INT[(M)] [UNSIGNED] [ZEROFILL]

mysql> insert into bit_t values(1);
Query OK, 1 row affected (0.11 sec)
```
显示数据：
```mysql
mysql> select ids,ids+0,bin(ids) from bit_t;
+------+-------+----------+
| ids  | ids+0 | bin(ids) |
+------+-------+----------+
|     |     1 | 1        |
+------+-------+----------+
1 row in set (0.05 sec)
```
显示二进制信息一般是使用+0进行数据隐式转换，或者用bin函数；


--再查一下表结构
```mysql
mysql> desc bit_t;
+-------+--------+------+-----+---------+-------+
| Field | Type   | Null | Key | Default | Extra |
+-------+--------+------+-----+---------+-------+
| ids   | bit(4) | YES  |     | NULL    |       |
+-------+--------+------+-----+---------+-------+
1 row in set (0.00 sec)
```
再次插入表数据：
```mysql
mysql> insert into bit_t values(1);
Query OK, 1 row affected (0.09 sec)

mysql> insert into bit_t values(0);
Query OK, 1 row affected (0.09 sec)

mysql> insert into bit_t values(3);
Query OK, 1 row affected (0.05 sec)
```
显示新的表的信息
```mysql
mysql> select ids,ids+0,bin(ids) from bit_t;
+------+-------+----------+
| ids  | ids+0 | bin(ids) |
+------+-------+----------+
|     |     1 | 1        |
|     |     1 | 1        |
|      |     0 | 0        |
|     |     3 | 11       |
+------+-------+----------+
4 rows in set (0.00 sec)
```

当插入数据过大则报错：
```mysql
mysql> insert into bit_t values(16);
ERROR 1406 (22001): Data too long for column 'ids' at row 1

mysql> insert into bit_t values(15);
Query OK, 1 row affected (0.11 sec)
mysql> select ids,ids+0,bin(ids) from bit_t;
+------+-------+----------+
| ids  | ids+0 | bin(ids) |
+------+-------+----------+
|     |     1 | 1        |
|     |     1 | 1        |
|      |     0 | 0        |
|     |     3 | 11       |
|     |    15 | 1111     |
+------+-------+----------+
5 rows in set (0.00 sec)
```


最大的的确是4个1111 满足了我们的愿望。

```mysql
mysql> select ids,ids+0,length(bin(ids)) from bit_t;
+------+-------+------------------+
| ids  | ids+0 | length(bin(ids)) |
+------+-------+------------------+
|     |     1 |                1 |
|     |     1 |                1 |
|      |     0 |                1 |
|     |     3 |                2 |
|     |    15 |                4 |
+------+-------+------------------+
5 rows in set (0.06 sec)


mysql> select ids,ids+0,length(ids+0),bin(ids),length(bin(ids)) from bit_t;
+------+-------+---------------+----------+------------------+
| ids  | ids+0 | length(ids+0) | bin(ids) | length(bin(ids)) |
+------+-------+---------------+----------+------------------+
|     |     1 |             1 | 1        |                1 |
|     |     1 |             1 | 1        |                1 |
|      |     0 |             1 | 0        |                1 |
|     |     3 |             1 | 11       |                2 |
|     |    15 |             2 | 1111     |                4 |
+------+-------+---------------+----------+------------------+
5 rows in set (0.00 sec)
```



-----不一样的插入额
```mysql
mysql> insert into bit_t values (4),(5),(6);
Query OK, 3 rows affected (0.03 sec)
Records: 3  Duplicates: 0  Warnings: 0

----insert into bit_t  set ids =4
mysql> insert into bit_t set ids=7;
Query OK, 1 row affected (0.05 sec)
```
再次显示新的数据
```mysql
mysql> select ids,ids+0,length(ids+0),bin(ids),length(bin(ids)) from bit_t;
+------+-------+---------------+----------+------------------+
| ids  | ids+0 | length(ids+0) | bin(ids) | length(bin(ids)) |
+------+-------+---------------+----------+------------------+
|     |     1 |             1 | 1        |                1 |
|     |     1 |             1 | 1        |                1 |
|      |     0 |             1 | 0        |                1 |
|     |     3 |             1 | 11       |                2 |
|     |    15 |             2 | 1111     |                4 |
|     |     4 |             1 | 100      |                3 |
|     |     5 |             1 | 101      |                3 |
|     |     6 |             1 | 110      |                3 |
|     |     7 |             1 | 111      |                3 |
+------+-------+---------------+----------+------------------+
9 rows in set (0.00 sec)
```


-----通过bin函数进行选择
```mysql
mysql> select bin(ids) from bit_t where ids=7;
+----------+
| bin(ids) |
+----------+
| 111      |
+----------+
1 row in set (0.00 sec)

mysql> select bin(ids) from bit_t where bin(ids)=111;
+----------+
| bin(ids) |
+----------+
| 111      |
+----------+
1 row in set (0.00 sec)



mysql> delete from bit_t where bin(ids)=1;
Query OK, 2 rows affected (0.09 sec)
```
显示最新的数据：
```mysql
mysql> select ids,ids+0,length(ids+0),bin(ids),length(bin(ids)) from bit_t;
+------+-------+---------------+----------+------------------+
| ids  | ids+0 | length(ids+0) | bin(ids) | length(bin(ids)) |
+------+-------+---------------+----------+------------------+
|      |     0 |             1 | 0        |                1 |
|     |     3 |             1 | 11       |                2 |
|     |    15 |             2 | 1111     |                4 |
|     |     4 |             1 | 100      |                3 |
|     |     5 |             1 | 101      |                3 |
|     |     6 |             1 | 110      |                3 |
|     |     7 |             1 | 111      |                3 |
+------+-------+---------------+----------+------------------+
7 rows in set (0.00 sec)
```
所以  一句话：bit要注意位数和 bin函数的使用！


在很多情况下，我们可能接触时间这一数据类型于是考察了查询了year,time,datetime,timestamp等信息，显示其值的范围

```mysql
mysql> help year;
Name: 'YEAR'
Description:
Syntax:
YEAR(date)

Returns the year for date, in the range 1000 to 9999, or 0 for the
"zero" date.

mysql> help Timestamp
Name: 'TIMESTAMP'
Description:
TIMESTAMP[(fsp)]

A timestamp. The range is '1970-01-01 00:00:01.000000' UTC to
'2038-01-19 03:14:07.999999' UTC. TIMESTAMP values are stored as the

mysql> help datetime
Name: 'DATETIME'
Description:
DATETIME[(fsp)]

A date and time combination. The supported range is '1000-01-01
00:00:00.000000' to '9999-12-31 23:59:59.999999'. MySQL displays

mysql> help date
Name: 'DATE'
Description:
DATE

A date. The supported range is '1000-01-01' to '9999-12-31'. MySQL

mysql> help time
Name: 'TIME'
Description:
TIME[(fsp)]

A time. The range is '-838:59:59.000000' to '838:59:59.000000'. MySQL
```



创建一张表year_t进行测试：
```mysql
mysql> create table year_t(
    -> col1 year(2),
    -> col2 year(4),
    -> col3 year(100));
Query OK, 0 rows affected, 2 warnings (0.27 sec)
```
显示表结构信息，发现都是year(4)的大小结构

```mysql
mysql> desc year_t;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| col1  | year(4) | YES  |     | NULL    |       |
| col2  | year(4) | YES  |     | NULL    |       |
| col3  | year(4) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
3 rows in set (0.06 sec)
```

插入表信息：
```mysql
mysql> insert into year_t values(77,2000,2055);
Query OK, 1 row affected (0.06 sec)
```
显示表数据：
```mysql
mysql> select * from year_t;
+------+------+------+
| col1 | col2 | col3 |
+------+------+------+
| 1977 | 2000 | 2055 |
+------+------+------+
1 row in set (0.00 sec)
```


为什么是1977？ 而不是2077？ 
查询到一个结果：
year(4)  1901-2155
year(2) 1970-2069
year     1000-9999（这种解释有点不可靠）

为什么再进行desc year_t 显示的多是year(4)暂时未清除。据推测，显示有问题，基本和sql_mode相关 
```mysql
mysql> show variables like '%sql_mode%';
+---------------+------------------------+
| Variable_name | Value                  |
+---------------+------------------------+
| sql_mode      | NO_ENGINE_SUBSTITUTION |
+---------------+------------------------+
1 row in set (0.00 sec)
```


-----总结sql_mode的8种风格
SET sql_mode =''; ---default style
SET sql_mode='PAD_CHAR_TO_FULL_LENGTH';  -- 会填充到一定的长度
SET sql_mode  = 'STRICT_ALL_TABLES';  -- 也就是当超过最大长度 会报错而不是截断
SET sql_mode = 'STRICT_ALL_TABLES,ERROR_FOR_DIVISION_BY_ZERO';
SET sql_mode = 'STRICT_ALL_TABLES, NO_ZERO_DATE,NO_ZERO_IN_DATE';
SET sql_mode = 'ALLOW_INVALID_DATES'; --- 允许无效的日期
SET sql_mode = 'STRICT_ALL_TABLES,ALLOW_INVALID_DATES';
SET sql_mode = ‘real_as_float’;
（real_as_float 是指默认情况real(10,2)而不能使用real(10) 因为real(10,2)其实等效于double，只有设置real_as_float之后才可以使用real(10)）

改成严格模式再次尝试：
```mysql
mysql> set sql_mode='strict_all_tables';
Query OK, 0 rows affected (0.06 sec)
```

再次显示新的模式下的表结构信息：
```mysql
mysql> desc year_t;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| col1  | year(4) | YES  |     | NULL    |       |
| col2  | year(4) | YES  |     | NULL    |       |
| col3  | year(4) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
3 rows in set (0.05 sec)
```


设置了严格模式，发现没有任何改变。

```mysql
mysql> show create table year_t\G;
*************************** 1. row ***************************
       Table: year_t
Create Table: CREATE TABLE `year_t` (
  `col1` year(4) DEFAULT NULL,
  `col2` year(4) DEFAULT NULL,
  `col3` year(4) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1
1 row in set (0.00 sec)
```

再次查看表结构，依然无法解释，于是尝试定义为year(4)的方式，新建了另一张表year_t1

```mysql
mysql> create table year_t1(
    -> col1 year(4),
    -> col2 year(4),
    -> col3 year(4));
Query OK, 0 rows affected (0.33 sec)

mysql> desc year_t1;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| col1  | year(4) | YES  |     | NULL    |       |
| col2  | year(4) | YES  |     | NULL    |       |
| col3  | year(4) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
3 rows in set (0.04 sec)
```
再次插入表的数据：
```mysql
mysql> insert into year_t1 values(77,2000,2055);
Query OK, 1 row affected (0.05 sec)

mysql> select * from year_t1;
+------+------+------+
| col1 | col2 | col3 |
+------+------+------+
| 1977 | 2000 | 2055 |
+------+------+------+
1 row in set (0.00 sec)
```


依然是1977 ，遇到一个暂时无法解答的问题。到底有没有year(2) year(4)的问题？ 暂时留着！


下面介绍比较常见的timestamp的使用方式：
----创建一张苹果表；
```mysql
mysql> create table apple(
    -> timeme timestamp default current_timestamp
    -> on update current_timestamp,
    -> id smallint);
```
之所以是on update current_timestamp是指当表中的某行数据更新时候，顺便会把timeme的数据更新为当前系统的最新的状态（之所以是系统最新状态，可以查看 show variables like "%time_zone%";将在下文给出）


----显示苹果表的信息
```mysql
mysql> desc apple;
+--------+-------------+------+-----+-------------------+-----------------------
------+
| Field  | Type        | Null | Key | Default           | Extra
      |
+--------+-------------+------+-----+-------------------+-----------------------
------+
| timeme | timestamp   | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIME
STAMP |
| id     | smallint(6) | YES  |     | NULL              |
      |
+--------+-------------+------+-----+-------------------+-----------------------
------+
2 rows in set (0.01 sec)
```


插入数据信息：
```mysql
mysql> insert into apple values(now(),200);
Query OK, 1 row affected (0.25 sec)
```
显示表数据：
```mysql
mysql> select * from apple;
+---------------------+------+
| timeme              | id   |
+---------------------+------+
| 2015-02-14 11:54:11 |  200 |
+---------------------+------+
1 row in set (0.00 sec)
```


-------显示系统变量
```mysql
mysql> show variables like "%time_zone%";
+------------------+--------+
| Variable_name    | Value  |
+------------------+--------+
| system_time_zone |        |
| time_zone        | SYSTEM |
+------------------+--------+
2 rows in set, 1 warning (0.00 sec)
```


--------显示字符集
```mysql
mysql> show character set;
+----------+-----------------------------+---------------------+--------+
| Charset  | Description                 | Default collation   | Maxlen |
+----------+-----------------------------+---------------------+--------+
| big5     | Big5 Traditional Chinese    | big5_chinese_ci     |      2 |
| dec8     | DEC West European           | dec8_swedish_ci     |      1 |
| cp850    | DOS West European           | cp850_general_ci    |      1 |
| hp8      | HP West European            | hp8_english_ci      |      1 |
| koi8r    | KOI8-R Relcom Russian       | koi8r_general_ci    |      1 |
```

另外附录在创建表的时候需要考虑下面的信息
------在创建表的时候必须主要的字段 属性    影响存储性能（比如Not NULL)和运行效果 比如zerofill;
1: AUTO_INCREMENT    用于主键 的自动编号
2: LAST_INSERT_ID()
3: UNSIGNED          无符号（一把是int数据）
4: ZEROFILL          填充 0（一把是int数据）
5: NULL/NOT NULL     一般使用非空，增强mysql的性能（一般设置为非空属性）
6: DEFAULT value     默认值
7: CHARACTER SET     字符集

添加一个null行的数据（null不知道的值，类似于java的对象。） 由于on update current_timestamp所以插入当前系统的最新时间
```mysql
mysql> insert into apple values(null,100);
Query OK, 1 row affected (0.15 sec)

mysql> select * from apple;
+---------------------+------+
| timeme              | id   |
+---------------------+------+
| 2015-02-14 11:54:11 |  200 |
| 2015-02-14 13:06:37 |  100 |
+---------------------+------+
2 rows in set (0.00 sec)
```



-----------默认为null时候添加  now()的值
如果添加入不合适的值（再sql_mode = '' 则是警告，如果是sql_mode='strict_all_tables'则是错误提示 errors产生）
```mysql
mysql> insert into apple value("123",100);
Query OK, 1 row affected, 1 warning (0.10 sec)


mysql> show warnings;
+---------+------+---------------------------------------------+
| Level   | Code | Message                                     |
+---------+------+---------------------------------------------+
| Warning | 1265 | Data truncated for column 'timeme' at row 1 |
+---------+------+---------------------------------------------+
1 row in set (0.00 sec)
```



-----当插入的值不合适时候  则用000替换！！
```mysql
mysql> select * from apple;
+---------------------+------+
| timeme              | id   |
+---------------------+------+
| 2015-02-14 11:54:11 |  200 |
| 2015-02-14 13:06:37 |  100 |
| 0000-00-00 00:00:00 |  100 |
+---------------------+------+
3 rows in set (0.00 sec)
```


省略过程得到新的表数据如下：
```mysql
mysql> select * from apple;
+---------------------+------+
| timeme              | id   |
+---------------------+------+
| 2015-02-14 11:54:11 |  200 |
| 2015-02-14 13:06:37 |  100 |
| 0000-00-00 00:00:00 |  100 |
| 2015-02-14 13:09:20 |  400 |
| 0000-00-00 00:00:00 |  500 |
+---------------------+------+
5 rows in set (0.00 sec)
```



----现在我要更新 id=500的值
```mysql
mysql> update apple set id=600 where id=500;
Query OK, 1 row affected (0.27 sec)
Rows matched: 1  Changed: 1  Warnings: 0
mysql> select * from apple;
+---------------------+------+
| timeme              | id   |
+---------------------+------+
| 2015-02-14 11:54:11 |  200 |
| 2015-02-14 13:06:37 |  100 |
| 0000-00-00 00:00:00 |  100 |
| 2015-02-14 13:09:20 |  400 |
| 2015-02-14 13:10:47 |  600 |  <-----------------timestame的值！也跟着变化！因为on update的作用
+---------------------+------+
5 rows in set (0.00 sec)
```
这就是timestamp和year的部分用法。

常用的show命令汇总show tables;
show databases;
show current_user(); 
show user();
show columns from apple; --->显示apple表结构，类似explain 和desc
show variables like '%sql_mode%'   ---->和显式相关，是mysql比较重要的参数
show variables like 'log%'              ---->会显示日志相关信息
show variables like 'max_connect%'  ---->显式mysql供用户的最大连接数
show variables like '%innodb%'       ---->显式innodb相关的信息
show variables like '%data%'           ---->显式mysql 数据的存储相关信息
show variables like '%time_zone%'  ----->显式时区相关信息
show variables like '%character%';   ---->显式字符集和编码的相关信息
一些不常用的show variables
show variables like '%query_cache%'  ---->查询缓存，不想深入，据说是跟优化有关，DBA选手可以深入，有一个工业标准。
show variables like '%key_buffer%'   ------>也和优化相关，也有工业标准值

还有一些没想起来的，以后再写。
另外还可以显示当前系统的状态的详细值
show status; 
show status like '%...%'  ---->显示某个值的信息
mysql> show status like '%buffer%';

信息值不是特别了解。。。

status 显式mysql当前库的使用情况：

```mysql
mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.20, for Win32 (x86)

Connection id:          2             连接的id
Current database:       test     当前数据库
Current user:           root@localhost    当前用户
SSL:                    Not in use             是否使用加密的ssl协议
Using delimiter:        ;          使用的终止符
Server version:         5.6.20 MySQL Community Server (GPL)
Protocol version:       10
Connection:             localhost via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1            数据库的字符集
Client characterset:    gbk          客户端的字符集
Conn.  characterset:    gbk        
TCP port:               3306                      开放的端口号
Uptime:                 5 hours 20 min 1 sec    登录时间

一些当前线程信息
Threads: 1  Questions: 182  Slow queries: 0  Opens: 96  Flush tables: 1
bles: 72  Queries per second avg: 0.009
--------------
```
create database ch charset = utf8;  ---->会改变Db characterset的值
set names utf8  ---------------------------->会改变client characerset 和Conn.characternet的值！


delimiter $$   则每使用一个mysql语句必须是用$$结尾  而不是;分号结尾了！

补充数据类型的分类：（摘自如鹏）
文本：

CHAR(*)：最多255个字节的定长字符串，它的长度必须在创建时指定
VARCHAR(*)：最多255（65535）个字节的可变长度字符串，它的长度必须在创建时指定
TEXT：最大长度为64K字符的变长文本
TINYTEXT：最大长度为255字符的变长文本
MEDUIMTEXT：最大长度为16K字符的变长文本
LONGTEXT：最大长度为4GB字符的变长文本

整数(考虑数据取值后选择尽可能小的类型)

tinyint：1字节。有符号值：-128 到127；无符号值：0到255
smallint：2字节。有符号值：-32768 到32767；无符号值：0到65535
mediumint：3字节。
int：4字节
bigint：8字节


小数(需要指定长度和小数点，也就是显示宽度和小数位数)：

decimal：精确存储的小数，在内部用字符串存储，适合金额等要求精确的类型。别名：NUMERIC（银行，差一点都不行！）
float：4字节，单精度。会近似存储(*)，效率比decimal高。
double：8字节，双精度。会近似存储(*)，效率比decimal高。


日期时间：

DATE：4字节。范围：1000-01-01——9999-12-31
TIME：3字节。范围：-838:59:59——838:59:59
DATETIME：8字节。范围：1000-01-01 00:00:00——9999-12-31 23:59:59


二进制大数据：

TITYBLOB：最大长度为255字节
BLOB：最大长度为64KB
MEDIUMBLOB：最大长度为16MB
LONGBLOB：最大长度为4GB



select的高级使用方式：

1：聚合函数
聚合函数：其实就是统计中常用的最大值（MAX），最小值(MIN)，平均值(AVG),求和（SUM），记录统计（COUNT）等函数
mysql> select count(*) from apple;
mysql> select max(id) from apple;
复制代码
按照具体情况匹配使用

2： limit 限制显示的条数
一个注意点limit是mysql独有的，必须放在最后，甚至比分组（order by）还靠后
Limit 2,4 表示从第二个记录之后的四个记录，即2,3,4,5   
```mysql
mysql> select * from apple limit 2,4;
+----+-----------+
| id | name      |
+----+-----------+
|  3 | shift     |
|  4 | caps      |
|  6 | tomato    |
|  7 | backspace |
+----+-----------+
4 rows in set (0.00 sec)
```
注意： 可用于网页的分页！  用于order by 之后，order by 一般用于where之后
where  ...
order by ...
limit ...
3 排序（order by)

一般是处于select语句的末尾（适用于sqlserver mysql oracle..)

这时候想要复制一张表
参考此文
1.复制表结构及数据到新表

CREATE TABLE 新表
SELECT * FROM 旧表

2.只复制表结构到新表

CREATE TABLE 新表
SELECT * FROM 旧表 WHERE 1=2
即:让WHERE条件不成立.
方法二:(低版本的mysql不支持，mysql4.0.25 不支持，mysql5已经支持了)
CREATE TABLE 新表
LIKE 旧表

3.复制旧表的数据到新表(假设两个表结构一样)

INSERT INTO 新表
SELECT * FROM 旧表

4.复制旧表的数据到新表(假设两个表结构不一样)

INSERT INTO 新表(字段1,字段2,…….)
SELECT 字段1,字段2,…… FROM 旧表
```mysql
mysql> create table b1 select * from t_students;
Query OK, 18 rows affected (0.55 sec)
Records: 18  Duplicates: 0  Warnings: 0
```
b1就是我们测试的数据表；
删除某个字段，比如名字不想显示。参考此文
知道了可以drop column  也可以change 字段信息
MySQL添加字段：

alter table `user_movement_log`   
Add column GatewayId int not null default 0 AFTER `Regionid` (在哪个字段后面添加)  

删除字段：

alter table `user_movement_log` drop column Gatewayid  

调整字段顺序：

ALTER TABLE `user_movement_log` CHANGE `GatewayId` `GatewayId` int not null default 0 AFTER RegionID  

mysql> alter table b1 drop column Name;
Query OK, 0 rows affected (0.52 sec)
Records: 0  Duplicates: 0  Warnings: 0

是的通过此种方式有效：
```mysql
mysql> select * from b1;
+----+--------+-----------------+------+------+
| Id | Gender | Hobbies         | Age  | Test |
+----+--------+-----------------+------+------+
|  0 |       | 羽毛球          |   30 |   31 |
|  1 |       | 编程            |   24 |   31 |
|  2 |        | 旅游            |   27 |   30 |
|  3 |       | 足球 dota       |   25 |   31 |
|  4 |       | 打炮，摄影      |   30 |   31 |
|  6 |       | 篮球            |   34 |   31 |
|  7 |       | 音乐，编程 军事 |   30 |   31 |
|  8 |       | 摄影            |   26 |   31 |
|  9 |       | 电影            |   26 |   31 |
| 10 |       | 消消乐          |   26 |   31 |
| 11 |        | 消消乐          |   24 |   30 |
| 12 |        | 购物            | NULL |   30 |
| 13 |       | 骑行            | NULL |   31 |
| 14 |       | 学术            |   34 |   31 |
| 15 |       | 不清楚          | NULL |   31 |
| 16 |       | 散步            | NULL |   31 |
| 17 |       | NULL            | NULL |   31 |
| 18 |        | 家庭主妇        |   26 |   30 |
+----+--------+-----------------+------+------+
18 rows in set (0.00 sec)
	```
测试分组方法：升序排列（降序用desc）
  规则用 order by 字段名  asc|desc

```mysql
mysql> select * from b1
    -> order by Age asc;
+----+--------+-----------------+------+------+
| Id | Gender | Hobbies         | Age  | Test |
+----+--------+-----------------+------+------+
| 17 |       | NULL            | NULL |   31 |
| 16 |       | 散步            | NULL |   31 |
| 15 |       | 不清楚          | NULL |   31 |
| 13 |       | 骑行            | NULL |   31 |
| 12 |        | 购物            | NULL |   30 |
|  1 |       | 编程            |   24 |   31 |
| 11 |        | 消消乐          |   24 |   30 |
|  3 |       | 足球 dota       |   25 |   31 |
| 18 |        | 家庭主妇        |   26 |   30 |
| 10 |       | 消消乐          |   26 |   31 |
|  9 |       | 电影            |   26 |   31 |
|  8 |       | 摄影            |   26 |   31 |
|  2 |        | 旅游            |   27 |   30 |
|  7 |       | 音乐，编程 军事 |   30 |   31 |
|  4 |       | 打炮，摄影      |   30 |   31 |
|  0 |       | 羽毛球          |   30 |   31 |
| 14 |       | 学术            |   34 |   31 |
|  6 |       | 篮球            |   34 |   31 |
+----+--------+-----------------+------+------+
```
    也可以组合排序：先按年龄的升序，再按id的降序

```mysql
mysql> select * from b1
    -> order by Age asc,Id desc;
+----+--------+-----------------+------+------+
| Id | Gender | Hobbies         | Age  | Test |
+----+--------+-----------------+------+------+
| 17 |       | NULL            | NULL |   31 |
| 16 |       | 散步            | NULL |   31 |
| 15 |       | 不清楚          | NULL |   31 |
| 13 |       | 骑行            | NULL |   31 |
| 12 |        | 购物            | NULL |   30 |
| 11 |        | 消消乐          |   24 |   30 |
|  1 |       | 编程            |   24 |   31 |
|  3 |       | 足球 dota       |   25 |   31 |
| 18 |        | 家庭主妇        |   26 |   30 |
| 10 |       | 消消乐          |   26 |   31 |
|  9 |       | 电影            |   26 |   31 |
|  8 |       | 摄影            |   26 |   31 |
|  2 |        | 旅游            |   27 |   30 |
|  7 |       | 音乐，编程 军事 |   30 |   31 |
|  4 |       | 打炮，摄影      |   30 |   31 |
|  0 |       | 羽毛球          |   30 |   31 |
| 14 |       | 学术            |   34 |   31 |
|  6 |       | 篮球            |   34 |   31 |
+----+--------+-----------------+------+------+
```

4. as作为辅助显示   where 则进行判断，条件显示信息
```mysql
mysql> select Hobbies as 爱好,Age as 年龄 from b1
    -> where Age >24 and Age <30;
+-----------+------+
| 爱好      | 年龄 |
+-----------+------+
| 旅游      |   27 |
| 足球 dota |   25 |
| 摄影      |   26 |
| 电影      |   26 |
| 消消乐    |   26 |
| 家庭主妇  |   26 |
+-----------+------+
6 rows in set (0.02 sec)
```


5：like模糊匹配
     正则表达式的效果，查找含有某某的字符串的信息

下面信息来自如鹏：
通配符过滤使用LIKE 。
1、单字符匹配的通配符为半角下划线“_”（类似于正则表达式的.），它匹配单个出现的字符。以任意字符开头，剩余部分为“erry” ：SELECT * FROM T_Employees WHERE Name LIKE '_erry'
2、多字符匹配的通配符为半角百分号“%”，它匹配任意次数（零或多个）出现的任意字符。 “k%”匹配以“k”开头、任意长度的字符串。检索姓名中包含字母“n”的员工信息 ：SELECT * FROM T_Employees WHERE Name LIKE '%n%'
3、Like性能较差，很容易造成全表扫描，谨慎使用。后面会讲数据库优化(索引等)，项目中做搜索用全文检索。
测试：（不像 = 号必须一样 ， like是模糊匹配）(数据库全部取出来，然后一条一条扫描)

```mysql
mysql> select * from b1 where Hobbies like '%羽毛%';
+----+--------+---------+------+------+
| Id | Gender | Hobbies | Age  | Test |
+----+--------+---------+------+------+
|  0 |       | 羽毛球  |   30 |   31 |
+----+--------+---------+------+------+
1 row in set (0.14 sec)
```

6.控制判断 Null
Null的精确翻译是不知道，连不知道怎么让数据库知道，所以必须用专门的函数is和is not进行判断，类似于bit的bin函数

```mysql
mysql> select Null+1;
+--------+
| Null+1 |
+--------+
|   NULL |
+--------+
1 row in set (0.04 sec)
```
不知道+1还是不知道。NUll相当于是一个对象，这个对象不知道里面的值为多少，所以无法进行=和like的比较
SQL中使用is null、is not null来进行空值判断：
```mysql
mysql> select * from  b1 where Age is NULL;
+----+--------+---------+------+------+
| Id | Gender | Hobbies | Age  | Test |
+----+--------+---------+------+------+
| 12 |        | 购物    | NULL |   30 |
| 13 |       | 骑行    | NULL |   31 |
| 15 |       | 不清楚  | NULL |   31 |
| 16 |       | 散步    | NULL |   31 |
| 17 |       | NULL    | NULL |   31 |
+----+--------+---------+------+------+
5 rows in set (0.00 sec)
```
如果使用=号 和like则没有任何显示
```mysql
mysql> select * from  b1 where Age = NULL;
Empty set (0.01 sec)

mysql> select * from  b1 where Age like '%NULL%';
Empty set (0.00 sec)
```

7 分组（group by)

比如有一个字段叫做部门，如果group by 部门  那么如果部门值相同的id则被分配到一个组中！！！同一组中的值可以进行再select,为了实现这个目的，一般需要了解外键的知识，多表的概念。现在进行测试：
```mysql
mysql> select Age from b1 group by age;
+------+
| Age  |
+------+
| NULL |
|   24 |
|   25 |
|   26 |
|   27 |
|   30 |
|   34 |
+------+
7 rows in set (0.04 sec)
```
可以看到如果对年龄进行分组，总共有7个组
将Age相同的数据行放到一组，分组后的数据可以看作一个临时的结果集，而SELECT  Age语句则取出每组的Age字段的值，这样我们就得到上表的员工年龄段表了
注意点：如果SELECT语句有WHERE子句，则GROUP BY子句必须放到WHERE语句的之后。
通过count(Age)可以看到每个分组到底有多少个人！这是一个相当棒的查询结果
```mysql
mysql> select Age,count(Age) from b1 group by age;
+------+------------+
| Age  | count(Age) |
+------+------------+
| NULL |          0 |
|   24 |          2 |
|   25 |          1 |
|   26 |          4 |
|   27 |          1 |
|   30 |          3 |
|   34 |          2 |
+------+------------+
7 rows in set (0.00 sec)
```
可以想想如果 是对部门进行分组，并存在salary字段，则可以avg(salary)获得某个部门的平均工资（min 最小  max最大  sum对部门工资求和！方便），因为经过分组后，select得到的结果是分组的信息，
而不是单一的信息了
1）计算每个分组中的员工平均工资
```mysql
SELECT Age,AVG(Salary) FROM T_Employees

GROUP BY Age
```
如鹏的总结：分组语句一般和聚合函数一起使用，GROUP BY子句负责将数据分成逻辑组，而聚合函数则对每一个组进行统计计算。


8.联合查询--之leftjoin模式

多表查询，是一个比较实际的运用！，多表查询中涉及到联合查询，先建立三张表，然后添加数据，测试就会明白了
创建第一张表 T_customers ,设计了Id为主键,自动增长，Name为varchar  not null，Age为tinyint就够了

```mysql
mysql> create table T_Customers(
    -> Id int(4) auto_increment primary key,
    -> Name varchar(20) not null default '',
    -> Age tinyint default 0);
Query OK, 0 rows affected (0.31 sec)

并添加完数据：
mysql> INSERT INTO T_Customers(Id,Name,Age)
    -> VALUES(1,'TOM',21);
Query OK, 1 row affected (0.12 sec)

mysql> INSERT INTO T_Customers(Id,Name,Age)
    -> VALUES(2,'MIKE',24);
Query OK, 1 row affected (0.06 sec)

mysql> INSERT INTO T_Customers(Id,Name,Age)
    -> VALUES(3,'JACK',30);
Query OK, 1 row affected (0.09 sec)

mysql> INSERT INTO T_Customers(Id,Name,Age)
    -> VALUES(4,'TOM',25);
Query OK, 1 row affected (0.03 sec)

mysql> INSERT INTO T_Customers(Id,Name,Age)
    -> VALUES(5,'LINDA',30);
Query OK, 1 row affected (0.08 sec)
```
显示表格结构:
```mysql
mysql> desc T_customers;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| Id    | int(4)      | NO   | PRI | NULL    | auto_increment |
| Name  | varchar(20) | NO   |     |         |                |
| Age   | tinyint(4)  | YES  |     | 0       |                |
+-------+-------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
```
显示表数据：
```mysql
mysql> select * from T_customers;
+----+-------+------+
| Id | Name  | Age  |
+----+-------+------+
|  1 | TOM   |   21 |
|  2 | MIKE  |   24 |
|  3 | JACK  |   30 |
|  4 | TOM   |   25 |
|  5 | LINDA |   30 |
+----+-------+------+
5 rows in set (0.00 sec)
```

新建第二张表：T_OrderType
设计ID和T_Customers一样，Name为订单类型名字，varchar（30）且非空
```mysql
mysql> create table T_OrderType(
    -> Id int auto_increment primary key,
    -> Name varchar(30) not null default '');
Query OK, 0 rows affected (0.35 sec)

mysql> desc T_OrderType;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| Id    | int(11)     | NO   | PRI | NULL    | auto_increment |
| Name  | varchar(30) | NO   |     |         |                |
+-------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```

并插入表数据：
不小心检错表名字了 ，进行修改

```mysql
mysql> rename table T_OrderType to T_OrderTypes;
Query OK, 0 rows affected (0.19 sec)
复制代码

mysql> INSERT INTO T_OrderTypes(Id,Name)
    -> VALUES(1,'现货订单');
Query OK, 1 row affected (0.12 sec)

mysql> INSERT INTO T_OrderTypes(Id,Name)
    -> VALUES(2,'预订订单');
Query OK, 1 row affected (0.03 sec)

mysql> INSERT INTO T_OrderTypes(Id,Name)
    -> VALUES(3,'预购订单');
Query OK, 1 row affected (0.09 sec)

mysql> INSERT INTO T_OrderTypes(Id,Name)
    -> VALUES(4,'内部');
Query OK, 1 row affected (0.09 sec)
```
显示订单类型表结构信息

```mysql
mysql> desc T_OrderTypes;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| Id    | int(11)     | NO   | PRI | NULL    | auto_increment |
| Name  | varchar(30) | NO   |     |         |                |
+-------+-------------+------+-----+---------+----------------+
2 rows in set (0.01 sec)
```
显示其中的数据：

```mysql
mysql> select * from T_OrderTypes;
+----+----------+
| Id | Name     |
+----+----------+
|  1 | 现货订单 |
|  2 | 预订订单 |
|  3 | 预购订单 |
|  4 | 内部     |
+----+----------+
4 rows in set (0.00 sec)
```


新建第三张 最重要的表！涉及到一个关键的新的知识点，外键的创建
设计了五个参数
ID                 订单ID  设计和其他两张表类型一致
Number       订单编号 设计为varchar（20） not null
Price             设计为 int类型
CustomerId  外键1   为T_Customers的外键
TypeId           外键2  为T_OrderTypes的外键
参考此文
```mysql
mysql> create table T_Orders(
    -> Id int auto_increment primary key,
    -> Number varchar(20) not null default '',
    -> Price int not null default 0,
    -> CustomerId int, foreign key(CustomerId) references T_customers(Id) on del
ete restrict on update cascade,
    -> TypeId int, foreign key(TypeId) references T_OrderTypes(Id) on delete res
trict on update cascade);
Query OK, 0 rows affected (0.46 sec)

mysql>
```
显示表结构

```mysql
mysql> desc T_Orders;
+------------+-------------+------+-----+---------+----------------+
| Field      | Type        | Null | Key | Default | Extra          |
+------------+-------------+------+-----+---------+----------------+
| Id         | int(11)     | NO   | PRI | NULL    | auto_increment |
| Number     | varchar(20) | NO   |     |         |                |
| Price      | int(11)     | NO   |     | 0       |                |
| CustomerId | int(11)     | YES  | MUL | NULL    |                |
| TypeId     | int(11)     | YES  | MUL | NULL    |                |
+------------+-------------+------+-----+---------+----------------+
5 rows in set (0.03 sec)
```
另外一种更加清晰地显示表结构：

```mysql
mysql> show create table T_Orders\G;
*************************** 1. row ***************************
       Table: T_Orders
Create Table: CREATE TABLE `t_orders` (
  `Id` int(11) NOT NULL AUTO_INCREMENT,
  `Number` varchar(20) NOT NULL DEFAULT '',
  `Price` int(11) NOT NULL DEFAULT '0',
  `CustomerId` int(11) DEFAULT NULL,
  `TypeId` int(11) DEFAULT NULL,
  PRIMARY KEY (`Id`),
  KEY `CustomerId` (`CustomerId`),
  KEY `TypeId` (`TypeId`),
  CONSTRAINT `t_orders_ibfk_1` FOREIGN KEY (`CustomerId`) REFERENCES `t_customer
s` (`Id`) ON UPDATE CASCADE,
  CONSTRAINT `t_orders_ibfk_2` FOREIGN KEY (`TypeId`) REFERENCES `t_ordertypes`
(`Id`) ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8
1 row in set (0.00 sec)
```
可以见到 CONSTRAINT的标志，如多定义没有指定CONSTRAINT 外键符号时，mysql会自动创建一个，比如t_orders_ibfk_1，就是外键符号，
可以通过

```mysql
ALTER TABLE T_Orders DROP FOREIGN KEY `t_orders_ibfk_1`来删除外键
mysql> alter table T_orders drop foreign key t_orders_ibfk_1;
Query OK, 0 rows affected (0.25 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
再次显示表结构信息：
```mysql
mysql> show create table T_orders\G;
*************************** 1. row ***************************
       Table: T_orders
Create Table: CREATE TABLE `t_orders` (
  `Id` int(11) NOT NULL AUTO_INCREMENT,
  `Number` varchar(20) NOT NULL DEFAULT '',
  `Price` int(11) NOT NULL DEFAULT '0',
  `CustomerId` int(11) DEFAULT NULL,
  `TypeId` int(11) DEFAULT NULL,
  PRIMARY KEY (`Id`),
  KEY `CustomerId` (`CustomerId`),
  KEY `TypeId` (`TypeId`),
  CONSTRAINT `t_orders_ibfk_2` FOREIGN KEY (`TypeId`) REFERENCES `t_ordertypes`
(`Id`) ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8
1 row in set (0.00 sec)

ERROR:
No query specified
```
少了t_orders_ibfk_2
那么如何再次添加呢？
参考此文
```mysql
mysql> alter table T_orders add  foreign key hello(CustomerId) references T_cust
omers(Id);
Query OK, 0 rows affected (0.74 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
再次查看表结构信息：

```mysql
mysql> show create table T_orders\G;
*************************** 1. row ***************************
       Table: T_orders
Create Table: CREATE TABLE `t_orders` (
  `Id` int(11) NOT NULL AUTO_INCREMENT,
  `Number` varchar(20) NOT NULL DEFAULT '',
  `Price` int(11) NOT NULL DEFAULT '0',
  `CustomerId` int(11) DEFAULT NULL,
  `TypeId` int(11) DEFAULT NULL,
  PRIMARY KEY (`Id`),
  KEY `TypeId` (`TypeId`),
  KEY `hello` (`CustomerId`),
  CONSTRAINT `t_orders_ibfk_2` FOREIGN KEY (`TypeId`) REFERENCES `t_ordertypes`
(`Id`) ON UPDATE CASCADE,
  CONSTRAINT `t_orders_ibfk_3` FOREIGN KEY (`CustomerId`) REFERENCES `t_customer
s` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
1 row in set (0.00 sec)
```
可以看到又增加了！
插入表结构信息
```mysql
mysql> INSERT INTO T_Orders(Id,Number,Price,CustomerId, TypeId)
    -> VALUES(1,'K001',100,1,1);
Query OK, 1 row affected (0.06 sec)

mysql> INSERT INTO T_Orders(Id,Number,Price,CustomerId, TypeId)
    -> VALUES(2,'K002',200,1,1);
Query OK, 1 row affected (0.04 sec)

mysql> INSERT INTO T_Orders(Id,Number,Price,CustomerId, TypeId)
    -> VALUES(3,'T003',300,1,2);
Query OK, 1 row affected (0.08 sec)

mysql> INSERT INTO T_Orders(Id,Number,Price,CustomerId, TypeId)
    -> VALUES(4,'N002',100,2,2);
Query OK, 1 row affected (0.04 sec)

mysql> INSERT INTO T_Orders(Id,Number,Price,CustomerId, TypeId)
    -> VALUES(5,'N003',500,3,4);
Query OK, 1 row affected (0.04 sec)

mysql> INSERT INTO T_Orders(Id,Number,Price,CustomerId, TypeId)
    -> VALUES(6,'T001',300,4,3);
Query OK, 1 row affected (0.04 sec)

mysql> INSERT INTO T_Orders(Id,Number,Price,CustomerId, TypeId)
    -> VALUES(7,'T002',100,1,1);
Query OK, 1 row affected (0.05 sec)
```

于是我们可以开始进行联合查询了：
1、查询每张订单的订单号、价格、对应的客户姓名以及客户年龄

```mysql
mysql> select o.Number,o.Price,c.Name,c.Age
    -> From T_Orders o
    -> Left join T_Customers c
    -> On o.CustomerId=c.Id;
+--------+-------+------+------+
| Number | Price | Name | Age  |
+--------+-------+------+------+
| K001   |   100 | TOM  |   21 |
| K002   |   200 | TOM  |   21 |
| T003   |   300 | TOM  |   21 |
| T002   |   100 | TOM  |   21 |
| N002   |   100 | MIKE |   24 |
| N003   |   500 | JACK |   30 |
| T001   |   300 | TOM  |   25 |
+--------+-------+------+------+
7 rows in set (0.08 sec)
```
2、添加where语句(显示价格>=150元的订单)

```mysql
mysql> select o.Number,o.Price,o.CustomerId,c.Name,c.Age
    -> from t_orders o
    -> left join t_customers c on o.CustomerId = c.Id
    -> where o.price >=150;
+--------+-------+------------+------+------+
| Number | Price | CustomerId | Name | Age  |
+--------+-------+------------+------+------+
| K002   |   200 |          1 | TOM  |   21 |
| T003   |   300 |          1 | TOM  |   21 |
| N003   |   500 |          3 | JACK |   30 |
| T001   |   300 |          4 | TOM  |   25 |
+--------+-------+------------+------+------+
4 rows in set (0.06 sec)
```
3、多表join连接查询

```mysql
mysql> select o.Number 订单号 ,o.Price 价格,c.Name 客户姓名,c.Age 客户年
    -> t.Name 订单类型 from t_orders o
    -> left join T_customers c on o.customerId=c.Id
    -> left join T_OrderTypes t on o.TypeId=t.Id;
+--------+------+----------+----------+----------+
| 订单号 | 价格 | 客户姓名 | 客户年龄 | 订单类型 |
+--------+------+----------+----------+----------+
| K001   |  100 | TOM      |       21 | 现货订单 |
| K002   |  200 | TOM      |       21 | 现货订单 |
| T003   |  300 | TOM      |       21 | 预订订单 |
| T002   |  100 | TOM      |       21 | 现货订单 |
| N002   |  100 | MIKE     |       24 | 预订订单 |
| N003   |  500 | JACK     |       30 | 内部     |
| T001   |  300 | TOM      |       25 | 预购订单 |
+--------+------+----------+----------+----------+
7 rows in set (0.12 sec)
```


由此可见 多表查询的魅力！！！。

9.外键约束

```mysql
create table T_Orders(
Id int auto_increment primary key,
Number varchar(20) not null default '',
Price int not null default 0,
CustomerId int, foreign key(CustomerId) references T_customers(Id) on del
ete restrict on update cascade,
TypeId int, foreign key(TypeId) references T_OrderTypes(Id) on delete res
trict on update cascade);
on delete restrict: 也就是限制删除(当你的外键被使用是  一旦被引用则不被删除，防止产生数据紊乱）
on update cascade（当更新时候，索引也相应的进行更新）


```


待学习的知识点：  触发器，约束，子查询，引擎（存储过程），除了left join的其他join 。。。

补充（来自如鹏）：1SQL语句中字符串一般用单引号。
2简单的插入数据的SQL语句：INSERT INTO T_Persons(Id,Name,Age,Gender) VALUES(5,'Jim',20,1)
Insert语句可以省略表名后的列名，但是强烈不推荐
3.如果插入的行中有些字段的值不确定，那么Insert的时候不指定那些列即可。不“允许为空”的列在插入时不能省略
4，自动递增/自增(Auto Increment)：字段自增可以避免并发等问题，不要程序员代码控制自增。用自增字段在Insert的时候不用指定值。修改表结构的方法【设计表】


    1.MySQL的语言中没有top修饰，不过可以使用limit来操作。
        2.MySQL和SQL Server 均支持as。as可以命名一个列或者表为其他名称。
        3.Distinct可以用户排除重复，若在Distinct后面用逗号分隔了好几个列，那表示要将这些列联合起来去重复。
        4.Order by 列名 asc/desc  其中asc为默认的从小到大，desc为从大大小，实际项目中，很多表的数据显示都需要从后面到前面。
        5.聚合函数在各个数据库中均受到支持。AVG、MAX、SUM、COUNT。同一个语句中，多个聚合函数可以一起使用。
        6.聚合函数多和Group by一起使用，如果没有Group by那么聚合的就是整个表的所有指定列的数据。
        7.模糊查询不是使用= 而是使用 like结合百分号 %
        8.使用count聚合函数可以查找符合条件的某一列是否存在。
        9.原则∶当使用了分组函数Group by后，select只能是聚合函数或者Group by的列。虽然MySQL不报错，但是仍然不推荐这么使用。
        10.对于第9点，虽然select 的只能是Group by的但是聚合函数仍然可以聚合其他列。
        11.Having和Where区别，Having搭配Group by使用，是对分组后的数据进行筛选。而Where是在分组前就行筛选。
        12.当多表查询的时候，不推荐单纯的使用*
        13.对于多表查询，一定要注意使用别名。
        14.Inner  Join 就是Join。
        15.All  Some Any中Some和Any等价的。
        16.多表查询使用On来连接，多为=来连接。
        17.Join和Union区别，可以形象的记忆为 Join是横着的连接，Union是上下竖着的连接。
        18.Left Join 是一定显示左边的，没显示的使用null来补位，Right Join是右边的表一定显示，左边的表某列不存在使用Null来补位。
        19.当子查询的结果不是一个的时候，主查询又在使用> < =表达式的时候，All 所有  Any/Some的作用就发挥了。
        20.从原理上来讲，多张表联合起来的查询是∶若A,B两张表，A表的第一列依次扫描B表的每一列，提取符号条件的。类似2个嵌套的for循环那样。
        21.当子查询的结果不是1列的时候，是不能使用 > =< 这些运算符的。
        22.使用Union，Intersect，Except运算符的时候，目标列表需要有相同的数目。
        23.聚合函数一般搭配分组函数一起使用，若不分组，那么就按照where的来，如果where没有就是所有的列。
        24.在使用in的语句中，一般都是可以使用Exists，且Exits会走索性相对来说快点，但是不一定。练习的时候，2个都要写出来。
        25.当Exits Where 的时候，该SQL语句运行顺序是这样的∶先运行Exits前面的，然后将每一列带入where中进行查询匹配，如果找到了就输出这一列。反之不输出。
        26.Exits函数的返回只是true或者fanlse。
        27.like或者not like利用 %进行模糊查询。
        28.逗号连接的2个∶  from A，B   |||   order by a asc, b desc
        29.聚合函数不应该出现在Where中，如果Where要出现，那么将其单独搞成子查询的模式。

