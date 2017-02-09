---
layout: post
title: "perl读取mysql数据库"
date: 2017-02-08 17:25:49 +0800
comments: true
categories: perl
---

perl中常见的文件打开关闭有open和IO模块等，而有时候涉及大量的
数据保存和打开的时候，就得借助数据库的使用，提供缓存的功能，并
使得保存的数据占用量最少，访问更快捷。下面就perl的mysql数据库访问
做一个简要的介绍。
<!--more-->

<h2 id="perldbi">1. 数据库访问原理</h2>

1. 加载对应数据库驱动
2. 获得相应的连接
3. 准备需要的查询数据 删除数据 更新数据
4. 执行sql语句
5. 获得相应的结果

<h2 id="perldbi">2. perl DBI mysql实现</h2>

What is DBI?
    "The DBI is the standard database interface module for Perl. 
    It defines a set of methods, variables and conventions that provide a 
    consistent database interface independent of the actual database being used."
    -- Tim Bunce

1. 安装perl dbi
2. 安装perl dbd:mysql驱动
3. 加载数据库驱动dsn
4. 从dsn获取相应的链接connect方法
5. 执行相应的sql语句
6. 注意，处理完毕最好做一个关闭操作(<font color="red">**这是一个技术问题，也是德行问题和习惯问题，编程的很多技术都是需要打开和关闭两个过程配对呈现**</font>)

具体可以参考

+ [perl dbi官网手册][3]
+ [Perl与数据库DBI快速入门][7]
+ [A short guid of the dbi programming][6]
+ [perl DBI 总结][1]
+ [perl dbi链接mysql中文乱码][2]
+ [使用Perl DBI操作MySQL的一些建议][4]
+ [perl快速入门][8]<font color="red">** 给出了所有的技术语言都可以由main函数引出很多其他的, 把技术当成你的习惯**</font>
+ [一本perl参考书: Programming_the_Perl_DBI ][9]
该建议使用binmode解决乱码问题，
``` perl
use utf8;
binmode(STDOUT, ':encoding(utf8)');
binmode(STDIN, ':encoding(utf8)');
binmode(STDERR, ':encoding(utf8)');
```
+ [perl DBI使用详解][5]

也可以直接在命令行使用`perldoc dbi` 查看dbi文档信息

<h2 id="perltest"> 3. 技术解决方案</h2>

feiji.txt:

```
汽车在高速行驶时，根据空气动力学原理，在行驶过程中会遇到空气阻力，围绕汽车重心同时产生纵向、侧向和垂直上升的三个方向的空气动力量，其中纵向为空气阻力。
为了有效地减少并克服汽车高速行驶时空气阻力的影响，人们设计使用了汽车尾翼，其作用就是使空气对汽车产生第四种作用力，即产生较大的对地面的附着力，它能抵消一部分升力，有效控制汽车上浮，使风阻系数相应减小，使汽车能紧贴在道路地面行驶，从而提高行驶的稳定性能。

工作原理
汽车尾翼作用
汽车尾翼的作用，就是在汽车高速行驶时，使空气阻力形成一个向下的压力，尽量抵消升力，有效控制气流下压力，使风阻系数相应减小，增加汽车的高速行驶稳定性；由于尾翼能降低汽车的空气阻力，因此高速汽车加装尾翼对于节省燃油也有一定的帮助；同时也使汽车的外形更加美观，起到一定的装饰作用。
汽车尾翼分类
玻璃钢尾翼：这类尾翼造型多样，有鸭舌状的、机翼状的，也有直板式的．比较好做造型，不过玻璃钢材质比较脆，韧性和刚性都较差，价格比较便宜。
铝合金尾翼：这类尾翼导流和散热效果不错，而且价格适中，不过重量要比其他材质的尾翼稍重些。
碳纤维尾翼：碳纤维尾翼刚性和耐久性都非常好．不仅重量轻而且也是最美观的一种尾翼．现在广泛被F1赛车采用不过价格比较昂贵。[1] 

```

source code for perl:

``` perl
#!/usr/bin/env perl 
#===============================================================================
#
#         FILE: testdbi.pl
#
#        USAGE: ./testdbi.pl  
#
#  DESCRIPTION: 
#
#      OPTIONS: ---
# REQUIREMENTS: ---
#         BUGS: ---
#        NOTES: ---
#    Author:  Ye Zhaoliang
# ORGANIZATION: 
#      VERSION: 1.0
#      CREATED: 2016/12/31 1:44:24
#     REVISION: ---
#===============================================================================

use strict;
use warnings;
use utf8;

use CGI::Carp "fatalsToBrowser";
use strict;
use warnings;
use DBI;
use CGI qw (:standard escapeHTML escape);
my ($driver_name, $db_name, $db_host, $db_sock, $db_port, $db_user, $db_pswd, $dsn);
$driver_name = 'mysql';
$db_name = 'mysql'; # database name
$db_host = 'localhost';
$db_sock = '/tmp/mysql.sock';
$db_port = '3306';
$db_user = 'root';
$db_pswd = 'root';
# data source
$dsn = "dbi:mysql:database=${db_name};hostname=${db_host};mysql_socket=${db_sock};port=${db_port}";
# ... set up connection to database (not shown) ...
# connect mysql database
my $dbh = DBI -> connect ($dsn, $db_user, $db_pswd,
{ RaiseError => 1, PrintError => 0 });
# put out initial part of page
my $title = "$db_name Database Browser";
# html header and start_html h1
print header ();
print start_html (-titlk => $title, -bgcolor => "white");
print h1 ($title);
# parameters to look for in URL
my $tbl_name = param ("tbl_name");
my $sort_col = param ("sort_col");
# If $tbl_name has no value, display a clickable list of tables.
# Otherwise, display contents of the given table. $sort_col, if
# set, indicates which column to sort by.

!defined ($tbl_name) ? display_table_names ($dbh, $db_name) : display_table_contents ($dbh, $tbl_name, $sort_col);
print end_html ();
sub display_table_names
{
my ($dbh, $db_name) = @_;

## html p paragraph
print p ("Select a table by clicking on its name:");
# retrieve reference to single-column array of table names
my $ary_ref = $dbh -> selectcol_arrayref (qq{ SHOW TABLES FROM $db_name });
# Construct a bullet list using the ul() (unordered list) and
# li() (list item) functions. Each item is a hyperlink that
# re-invokes the script to display a particular table.
my @item;
foreach my $tbl_name (@{$ary_ref})
{
my $url = sprintf ("%s?tbl_name=%s", url (), escape ($tbl_name));
my $link = a ({-href => $url}, escapeHTML ($tbl_name));
push (@item, li ($link));
}
## list control
print ul (@item);
}
sub display_table_contents
{
my ($dbh, $tbl_name, $sort_col) = @_;
my @rows;
my @cells;
# if sort column not specified, use first column
$sort_col = "1" if !defined ($sort_col);
# present a link that returns user to table list page
# p control widget
print p (a ({-href => url ()}, "Show Table List"));
print p (strong ("Contents of $tbl_name table:"));
## select from database
my $sth = $dbh -> prepare (qq{
SELECT * FROM $tbl_name ORDER BY $sort_col
LIMIT 200
});
$sth -> execute ();
# Use the names of the columns in the database table as the
# headings in an HTML table. Make each name a hyperlink that
# causes the script to be reinvoked to redisplay the table,
# sorted by the named column.
foreach my $col_name (@{$sth -> {NAME}})
{
my $url = sprintf ("%s?tbl_name=%s;sort_col=%s",
url (),
escape ($tbl_name),
escape ($col_name));
my $link = a ({-href => $url}, escapeHTML ($col_name));
push (@cells, th ($link));
}
push (@rows, Tr (@cells));
# display table rows
while (my @ary = $sth -> fetchrow_array ())
{
@cells = ();
foreach my $val (@ary)
{
# display value if non-empty, else display non-breaking space
if (defined ($val) && $val ne "")
{
$val = escapeHTML ($val);
}
else
{
$val = " ";
}
push (@cells, td ($val));
}
push (@rows, Tr (@cells));
}
# display table with a border
print table ({-border => "1"}, @rows);
}

```

[1]: http://www.cnblogs.com/homezzm/archive/2011/07/22/2113618.html
[2]: http://blog.sina.com.cn/s/blog_54dd80920102v8f5.html 
[3]:http://dbi.perl.org/ 
[4]:http://www.jb51.net/article/65791.htm 
[5]:http://blog.csdn.net/zzq900503/article/details/14454963 
[6]:http://www.perl.com/pub/1999/10/DBI.html 
[7]:http://log4think.com/perl_and_dbi/ 
[8]:http://log4think.com/perl_fast_tutorial/ 
[9]:http://vdisk.weibo.com/s/ah8r1yUxop3dO 
