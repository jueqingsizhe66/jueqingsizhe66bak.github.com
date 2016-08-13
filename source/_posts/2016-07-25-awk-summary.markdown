---
layout: post
title: "awk summary"
date: 2016-07-25 13:38:21 +0800
comments: true
categories: Linux
---
awk拓展学习

1. [awk insight](#1)
2. [awk separtor](#2)
3. [awk next的作用](#3)
4. [awk语法补充](#4)
5. [熟悉awk的比较运算符](#5)
6. [常用的字符串分割split](#6)
7. [另外一个直觉 awk是关联数组](#7)
8. [awk查看ip连接数 之调用ubuntu系统命令](#8)
9. [awk内置函数的介绍](#9)
10. [print导出到shell](#10)
11. [awk也可以做数值结算](#11)
12. [awk后缀文件执行（区分于单行命令模式）](#12)
13. [并行命令行 处理大数据文件](#13) 


<!--more-->

<h2 id="1">1. awk insight</h2>

awk默认所有文件都按照一定的分隔符划分成类似于![excel表的风格][11]

```
必须有一张没有杂质的表(awk对于数据的本质认识：表），这样我就可以进行分析了。
然后在一张没有杂质的文件中，写入计算的算子，针对每一行做计算
然后把它输入到你的统计表
最后再把它打印出来
```

也可以参考[excel的天下第一表][8]

从直觉上来讲，任何的awk程序都可以划分为命令模式(使用 awk -F'' '{}' 或者[awk 'BEGIN{}{}END{}'][12]组合风格）
)和文档模式（也就是使用awk -f *.awk 处理文件）

<hr/>

<h2 id="2">2. awk separtor</h2>

所有的分割应该都分为输入分割符和输出分隔符。
而分隔符又应该分为行分隔符和列分隔符，比如常用的行分隔符\n,列分隔符空格。

在awk中的几个常见的分隔符如下

```
# 输入

列分隔符（也叫做字段分隔符）:  FS (也可以使用但行命令的-F代替）
行分隔符                    :  RS

# 输出
列分隔符（也叫做字段分隔符）:  OFS
行分隔符                    :  ORS


```

示例部分：

```
# 字段分隔符 以-F打头，下面的分隔符为commas逗号 单行命令模式之-F
cat    test.csv  | awk -F"," '{print $2,$3}'  

# 单行命令模式之BEGIN
awk 'BEGIN{FS=",";OFS=";" }{ print $1,$2}'


# 比较有趣的一个例子 ，统计各省车牌号数 并排序,uniq -c去除重复 并统计个数 
awk -F ',' '{print substr($2,1,1)}' 2013-07-01.csv|sort|uniq -c
 102418 川
 261066 鄂
    601 颚
     39 贵
 789109 沪
    233 吉
    407 冀
  68218 津
    404 晋
1380934 京
  52852 辽
   2382 鲁
  44183 闽
  89263 陕
 184979 苏
    546 湘
     30 于
 108413 渝
     10 豫
 176184 粤
 273243 浙

```

另外几个比较常用到的字段

```
NF,表示awk开始执行程序后所读取的当前行字段数.（使用频率中）
NR,表示awk开始执行程序后所读取的数据行数.（使用频率高）
FNR,与NR功用类似,不同的是awk每打开一个新文件,FNR便从0重新累计.（使用频率中）
$0	当前记录（作为单个变量）（使用频率高）
$1~$n	当前记录的第n个字段，字段间由FS分隔（使用频率高）
ARGC	命令行参数个数（使用频率低）
ARGV	命令行参数数组（使用频率低）
FILENAME	当前输入文件的名字（使用频率低）
OFMT	数字的输出格式 %.6g（使用频率低）
SUBSEP	\034  在spilit的时候可能用到（使用频率低）
```
<hr/>
<h2 id="3">3. awk next的作用</h2>

[awk next的作用][13]


```
# awk '$4 <= 20 { printf "%s\t%s\n", $0,"*" ; next; } $4 > 20 { print $0 ;} ' food_list.txt

```

输入行用$4 <= 20 { printf "%s\t%s\n", $0,"*" ; next ; }命令打印以后，next命令将跳过第二个$4 > 20 { print $0 ;}表达式，继续判断下一个输入行，而不是浪费时间继续判断一下是不是当前输入行还大于 20。
next命令在编写高效的命令脚本时候是非常重要的，它可以提高脚本速度。本系列的下一部分我们将来学习如何使用 awk 来处理标准输入（STDIN）。

<hr/>

<h2 id="4"> 4. awk语法补充</h2>

a. awk两种for
b. awk的if-else

参考链接如下：

+ [awk code-practice][2] 
+ [一个不错的awk指南,短而精悍][5]
+ [awk faq][7]
+ [use Awk and Regular Expressions to Filter Text or String in Files][9] 过滤的目的是简化和美观。
+ [一个awk源码库][10]
+ [awk 复合表达式 ][12]


<hr/>

<h2 id="5" >5. 熟悉awk的比较运算符</h2>

使用场合:
```
expression { actions; }
```

参考[原文链接][3]


<hr/>
<h2 id="6">6. 常用的字符串分割split</h2>

```
awk 'BEGIN{info="it is a test";split(info,tA," ");for(k in tA){print k,tA[k];}}'
```

for…in 因为数组是关联数组，默认是无序的。所以通过for…in 得到是无序的数组。如果需要得到有序数组，需要通过下标获得。

```

awk 'BEGIN{info="it is a test";tlen=split(info,tA," ");for(k=1;k<=tlen;k++){print k,tA[k];}}' 
```

注意：数组下标是从1开始，与c数组不一样。


<hr/>

<h2 id="7">7. 另外一个直觉 awk是关联数组</h2>
关联数组是指类似于hashmap（键值对）的风格。

```
删除键值
awk 'BEGIN{tB["a"]="a1";tB["b"]="b1";delete tB["a"];for(k in tB){print k,tB[k];}}'          
打印键值
awk 'BEGIN{tB["a"]="a1";tB["b"]="b1";if( "c" in tB){print "ok";};for(k in tB){print k,tB[k];}}'  
```

<hr/>
<h2 id="8">8. awk查看ip连接数 之调用ubuntu系统命令</h2>

```
awk 'BEGIN{
    while("netstat -an"|getline){
        if( $5 ~ /[1-255]/)
        {
            split($5,t1,":");
            tarr[t1[1]]++;
        }
    }
    for(k in tarr)
        {
            print k,tarr[k] | "sort -r -n -k2";  #调用系统sort命令 通过管道传递
        }
};'
```

说明:
	通过管道，发送到外部程序“sort”排序，-r 从大到小，-n 按照数字排序，-k2 以第2列排序。通过将数据丢给第3方的sort命令，	所有问题变得非常简单。如果以key值排序 –k2 变成 -k1即可

也可以引入单行命令的变量：
```
awk -vcmd="sort -k3,3n <file" 'BEGIN{while(cmd|getline){print $0 FS (i?$3-i:"");i=$3}}
```

<hr/>

<h2 id = "9">9. awk内置函数的介绍</h2>
内置函数详细参考[linux awk 内置函数详细介绍][4].

常用几个内置函数：

```
? length(string) 求出string 有几个字符。

? match ( string,regexp ) 在字符串string 中寻找符合regexp 的最长、最靠左边的子字

符串。返回值是regexp 在string 的开始位置，即index值。match 函数将会设置系统变量

RSTART 等于index的值，系统变量RLENGTH 等于符合的字符个数。如果不符合，则会

设置R S TA RT 为0、RLENGTH 为- 1。

? sprintf ( format，expression1，. . . ) 和printf 类似，但是sprintf 并不显示，而是返回字

? sub ( regexp，replacement，target ) 在字符串target 中寻找符合regexp 的最长、最靠左的地方，以字串replacement 代替最左边的regexp。

·gsub(regexp，replacement，target)与前面的sub类似。在字符串target中寻找符合regexp的所有地方，以字符串replacement代替所有的regexp。例如：

?substr(string，start，length)返回字符串string的子字符串，这个子字符串的长度为

length，从第start个位置开始。

? tolower(string) 将字符串string的大写字母改为小写字母。

? toupper(string) 将字符串s t r i n g的小写字母改为大写字母。

? close(filename) 将输入或输出的文件filename 关闭。

? system(command) 此函数允许用户执行操作系统的指令，执行完毕后将回到gawk程

```

<hr/>

<h2 id="10">10. print导出到shell</h2>

把当前文件夹下的文件 复制到其他地方！ 也可以使用awk内部管道。
```
awk 'BEGIN{for(i=400;i<=2000;i+=20}{print "cp "i".png ./findDir"}'|sh
#完美的解决了问题！  把当前文件夹下的文件 复制到其他地方！
```
<hr/>

<h2 id="11">11. awk也可以做数值结算</h2>

[awk 数值计算][6]

<hr/>
<h2 id="12">12. awk后缀文件执行（区分于单行命令模式）</h2>


+ shell运行指令:

```
awk -f address.awk  address.txt

```

+ ?.awk文件内容


```
usage :  awk -f address.awk address.txt   {必须写在 BEGIN的同一行！
BEGIN{
   FS="\n"
   RS=""
    }

{
   print $1 "," $2 ","  $3
    }

```

+ address.txt文件内容

```
Jimmy the Weasel
100 Pleasant Drive
San Francisco, CA 12345

Big Tony
200 Incognito Ave.
Suburbia, WA 67890

```
address.txt的内容：（因为RS以空格划分行，所以空行的存在也是必须的）


<hr/>

<h2 id="13">13. 并行命令行 处理大数据文件 </h2>


+ BZIP2
bzip2是比gzip更好的压缩工具，但它很慢！别折腾了，我们有办法解决这问题。

以前的做法：

```
cat bigfile.bin | bzip2 --best > compressedfile.bz2
```
现在这样：
```
cat bigfile.bin | parallel --pipe --recend '' -k bzip2 --best > compressedfile.bz2
```

+ Grep
如果你有一个非常大的文本文件，以前你可能会这样：
```
grep pattern bigfile.txt
```

现在你可以这样：
```
cat bigfile.txt | parallel  --pipe grep 'pattern'
```
或者这样
```
cat bigfile.txt | parallel --block 10M --pipe grep 'pattern'
```

这第二种用法使用了 –block 10M参数，这是说每个内核处理1千万行——你可以用这个参数来调整每个CUP内核处理多少行数据。


+ AWK
下面是一个用awk命令计算一个非常大的数据文件的例子。

常规用法
```

cat rands20M.txt | awk '{s+=$1} END {print s}'
```
现在这样：
```

cat rands20M.txt | parallel --pipe awk \'{s+=\$1} END {print s}\' | awk '{s+=$1} END {print s}'

```
这个有点复杂：parallel命令中的–pipe参数将cat输出分成多个块分派给awk调用，形成了很多子计算操作。这些子计算经过第二个管道进入了同一个awk命令，从而输出最终结果。第一个awk有三个反斜杠，这是GNU parallel调用awk的需要。

+ WC
想要最快的速度计算一个文件的行数吗？

传统做法：
```
wc -l bigfile.txt
```
+ 现在你应该这样：
```
cat bigfile.txt | parallel  --pipe wc -l | awk '{s+=$1} END {print s}'
```
非常的巧妙，先使用parallel命令‘mapping’出大量的wc -l调用，形成子计算，最后通过管道发送给awk进行汇总。

+ SED
想在一个巨大的文件里使用sed命令做大量的替换操作吗？

常规做法：
```
sed s^old^new^g bigfile.txt
```
现在你可以：

```
cat bigfile.txt | parallel --pipe sed s\^old\^new\^g
```

然后你可以使用管道把输出存储到指定的文件里。

[July算法新书][1]


[1]:https://github.com/julycoding/The-Art-Of-Programming-By-July
[2]:https://github.com/chunyang-wen/code-practice/tree/master/Shell
[3]:http://www.tecmint.com/comparison-operators-in-awk/
[4]:http://www.cnblogs.com/chengmo/archive/2010/10/08/1845913.html
[5]:http://www.pement.org/awk/awk1line.txt
[6]:http://www.ibm.com/developerworks/cn/linux/l-cn-awkinwork/
[7]:http://www.faqs.org/faqs/computer-lang/awk/faq/
[8]:http://blog.sina.com.cn/u/1426389024
[9]:http://www.tecmint.com/use-linux-awk-command-to-filter-text-string-in-files/
[10]:http://dada.perl.it/shootout/gawk_allsrc.html
[11]:/images/linux/awk/awkEssential.png
[12]:http://www.tecmint.com/combine-multiple-expressions-in-awk/
[13]: http://www.tecmint.com/use-next-command-with-awk-in-linux/
[14]:http://www.aqee.net/use-multiple-cpu-cores-with-your-linux-commands/ 
