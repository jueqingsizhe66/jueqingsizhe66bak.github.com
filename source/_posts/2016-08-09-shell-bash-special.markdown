---
layout: post
title: "Shell bash special"
date: 2016-08-09 13:59:15 +0800
comments: true
categories: Linux
---

想写一篇关于linux shell bash表达式的特殊形式（当然首先得说明他和其他语言的共性）,
下面是一些不错的总结，我想通过阅读他们可以加深你对bash语言的理解,特别是一些注意点。
<!--more-->

<h2 id='for'>for循环空格</h2>

for循环会遍历空格划分的条目
``` sh
#!/bin/bash
for X in red green blue
do
	echo $X
done
```

注意，如果某一项含有空格，必须要用引号引起来，例子如下：
``` sh
#!/bin/bash
colour1="red"
colour2="light blue"
colour3="dark green"
for X in "$colour1" $colour2" $colour3"
do
	echo $X
done
```

<h2 id='replace'>双引号和单引号</h2>

如果变量的内容存在着空格最好是使用单引号或者双引号括起来，在这种情况下，两种符号的作用一致。
不同的是，双引号内容里面如果存在需要转义的部分会进行转义或者变量展开，而单引号则完全完全把所有内容当做字符输出。

所以在很多情况下用双引号来包括一行命令，是一个不错的点子。

``` sh
#!/bin/bash
echo -n '$USER=' # -n选项表示阻止echo换行
echo "$USER"
echo "\$USER=$USER"  # 该命令等价于上面的两行命令
```

输出内容：
```
$USER=canbetter
$USER=canbetter
```

其中第一行的$USER就是`echo -n '$USER='` 输出的结果。

结论是： 双引号比较灵活，可以得到变量引用值（也就是变量替换），但是比较容易犯错。
直觉来说，单引号输出的内容就是你看到的内容，而单引号不一定。

但是下面这种情况非双引号不可！
``` sh
#!/bin/bash
X=""
if [ -n $X ]; then 	# -n 用来检查变量是否非空
	echo "the variable X is not the empty string"
fi

out:

the variable X is not the empty string

为何？这是因为shell将$X展开为空字符串，表达式[-n]返回真值（因为改表达式没有提供参数）。再看这个脚本：
#!/bin/bash
X=""
if [ -n "$X" ]; then 	# -n 用来检查变量是否非空
         echo "the variable X is not the empty string"
fi
在这个例子中，表达式展开为[ -n “”]，由于引号中内容为空，因此该表达式返回false值
```

所以，双引号也有他不可替代的作用。
<h2 id='replace'>变量替换</h2>
Bash shell有个非常好用的特性叫做命令替换。允许我们将一个命令的输出当做另一个命令的输入。
有两种命令替换的方式:

+ 小括号扩展
+ 反撇号扩展

比如:
``` sh
files = "$(ls)"
filesA = `ls`
echo $files,$filesA
```

大括号扩展被用于变量保护，比如
``` sh
	
#!/bin/bash
X=ABC
echo "${X}abc" ---> correct
echo "$Xabc"   ---> error
```
因为Xabc并没有该变量定义，只有X定义的变量，所以得用`{}`包括起来X变量.


<h2 id='test'>Test表达式</h2>

<font color="red">条件判断中的命令几乎都是test命令</font>。test根据测试条件通过或失败来返回true或false(更准确的说是返回0或非0值)
下图是test操作符的快速查询列表。当然这个列表并不全面，但记下这些就足够平常使用了（如果还需要了解其他操作符，可以查看man手册）。
``` sh
operator 	produces true if… 	                                                                number of operands
-n 	        operand non zero length 	                                                                1
-z 	        operand has zero length 	                                                                1
-d 	        there exists a directory whose name is operand 	                                            1
-f 	        there exists a file whose name is operand 	                                                1
-eq 	    the operands are integers and they are equal 	                                            2
-neq 	    the opposite of -eq 	                                                                    2
= 	        the operands are equal (as strings) 	                                                    2
!= 	        opposite of = 	                                                                            2
-lt 	    operand1 is strictly less than operand2 (both operands should be integers) 	                2
-gt 	    operand1 is strictly greater than operand2 (both operands should be integers) 	            2
-ge 	    operand1 is greater than or equal to operand2 (both operands should be integers) 	        2
-le 	    operand1 is less than or equal to operand2 (both operands should be integers) 	            2

```

``` sh
表达式                        功能
[ -f "somefile" ]        判断是否是一个文件
[ -x "/bin/ls" ]         判断/bin/ls是否存在并有可执行权限
[ -n "$var" ]            判断$var变量是否有值
[ "$a" = "$b" ]          判断$a和$b是否相等
-r file　　　　　        用户可读为真
-w file　　　　　        用户可写为真
-x file　　　　　        用户可执行为真
-f file　　　　　        文件为正规文件为真
-d file　　　　　        文件为目录为真
-c file　　　　　        文件为字符特殊文件为真
-b file　　　　　        文件为块特殊文件为真
-s file　　　　　        文件大小非0时为真
-t file　　　　　        当文件描述符(默认为1)指定的设备为终端时为真

```
Test命令的格式为

+ 操作数< 空格 >操作符< 空格 >操作数
+ 操作符< 空格 >操作数
<font color="green">这里特别说明必须要有这些空格</font>，
因为shell将没有空格的第一串字符视为一个操作符（以-开头）或者操作数

并且如果是[]须在两边也加上空格

+ [<空格>操作数<空格>操作符<空格>操作数<空格>]
+ [<空格>操作符<空格>操作数<空格>]

所以看上去应该是

``` sh
if [ 1 == 2 ]; then echo "hello" fi
if test 1 == 2 ; then echo "hello" fi
```

结论:
``` sh
[ operand1 operator operand2 ]
test operand1 operator operand2
```

而<font color="red">其实在变量赋值的时候，则必须没有空格</font>，这也是需要注意的地方

<h2 id="if-else">if-else 表达式</h2>

正确写法1:

``` sh
if [ 1 == 2 ]
    then echo "yes"
    else echo "no"
    fi
```

正确写法2：

``` sh
if [ 1 == 2 ];then echo "yes"
    else echo "no"
    fi
```

<h2 id="find"> find 表达式</h2>
find命令中跟[Test表达式](#test)有点类似的地方，就是他们判断的标准，逻辑上有点类似，具体可以参考

+ [Find][2]
+ [15条find命令][3]
+ [Shell编程－－第2章 使用find和xargs 推荐比较全面][4]
+ [find命令使用经验][5]

本文参考

+ [Bash快速入门指南][1]
+ [Peteris Krumins][6]
+ [substack][7]


[1]:http://blog.jobbole.com/85183/ 
[2]:http://www.cnblogs.com/serendipity/articles/2133385.html 
[3]:http://bbs.linuxtone.org/thread-3387-1-1.html 
[4]:http://bbs.linuxtone.org/thread-1425-1-1.html 
[5]:http://bbs.linuxtone.org/thread-1604-1-1.html 
[6]:http://www.catonmat.net/ 
[7]:substack.net
