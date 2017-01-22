---
layout: post
title: "bash special annotation"
date: 2016-09-25 23:15:01 +0800
comments: true
categories: Linux
---

关于shell的summary参看[Linux Shell Summary][1]

以下是关于bash的一些比较特殊的地方。
<!--more-->

## 1. bash数组再理解

`declare -a`表示声明一个数组，类似于 `declare -i` 声明一个整数和`declare -x `声明一个环境变量。

<font color="red">bash数组定义时候使用小括号来赋值，而在引用数组的时候反而使用中括号（不同于其他编程语言)</font>

### 定义数组

``` sh
bash2=("34","5","5","4")
```

### 引用数组

``` sh
echo ${array[n]}
遍历数组：
filename=(`ls`)
for var in ${filename[@]};do
echo $var
done

```

### 添加数组

``` sh
bash1+="45"; #赋值两边不能有空格
```

## 2. bash变量截取和变量替换


<font color="red">bash使用井号代表开头(这和正则表达式有点不同，使用caret) 
而结尾则使用百分号(不同于正则表达式的dollar符号$)。并且在当使用两个重叠的井号，表示最大长度（从头开始的最大删除都删掉）
而使用两个百分号则表达反方向的最大长度删除（从尾到头删除匹配字符串）

1. \#表示正向最短匹配，##表示正向最大匹配
2. %表示反向最短匹配，%%表示反向最大匹配

而这一点也体现在变量替换当中，


```
子串替换：
${string/substring/replacement}
使用$replacement来替换第一个匹配的$substring.

${string//substring/replacement}
使用$replacement来替换所有匹配的$substring.

${string/#substring/replacement}
如果$substring匹配$string的开头部分, 那么就用$replacement来替换$substring.

${string/%substring/replacement}
如果$substring匹配$string的结尾部分, 那么就用$replacement来替换$substring.

子串提取的方法主要有：直接到指定位置求子串，字符匹配求子串。
${string:position}
在$string中从位置$position开始提取子串.

如果$string是"*"或者"@", 那么将会提取从位置$position开始的位置参数.[1]

${string:position:length}
在$string中从位置$position开始提取$length长度的子串.

```

1. 除号表示最短替换，双除号表示最长替换
2. 匹配项第一个位置出现#井号表示开头部分
3. 匹配项最后一个位置出现%百分号表示结尾部分


## 3. bash接受键盘操作

用一个read命令即可(类似于matlab的input命令)
通常的风格是 `read -p "prompt for reminding" variable-name`

``` sh
接收键盘输入：
    read [选项] [变量名]
    选项：
        -p "提示信息"：在等待read输入时，输出提示信息
        -t 秒数：read命令会一直等待用户输入，使用此选项可以指定等待时间
        -n 字符数：read命令只接受指定的字符数，就会执行
        -s：隐藏输入的数据，适用于机密信息的输入

read.sh：
#!/bin/bash

read -p "please input your name:" -t 30 name
echo $name

read -p "please input your passwd:" -s passwd
echo -e "\n"
echo $passwd

read -p "please input your sex [M/F]:" sex
echo -e "\n"
echo $sex

```

## 4. tac反向

cat在bash中是打印文本信息的作用，而[tac][2]则是反向输出文件流 

``` sh
cat old_file|awk '{print NR,$0}'|sort -r -n|awk '{print $2}'

```

1. 添加每行的行号
2. 逆向排列
3. 输出每行


## 5. if当中也可以使用双括号进行算术判断

if并不是支持正则表达式？还是只有变量支持？
``` sh
root at javazhao-N53SM [13:26:46ä��午] in /MyBashWork/clean on git:develop?
$ if [ -f "All.sh" ] ;then echo "hei"; else echo "no" ;fi;

hei

root at javazhao-N53SM [13:26:52ä��午] in /MyBashWork/clean on git:develop?
$ if [ -f "*.sh" ] ;then echo "hei"; else echo "no" ;fi;

no


但是对于变量是可以的
[root@bj_manager ~]# a=123a;if [[ $a =~ ^[0-9]+$ ]]; then echo ok; fi
[root@bj_manager ~]# a=123;if [[ $a =~ ^[0-9]+$ ]]; then echo ok; fi  
```

<font color="red">实际情况是只有在双中括号的时候才支持正则表达式</font>。<font color="green">一般是使用双中括号进行文件判断，目录判断，
文件大小判断等</font>

if另外一个特殊地方就是使用双括号执行算术比较

```
708@708-PC MINGW64 /e/plGraphViz (master)
  if (( 1<2 )) ;then echo "zero";   echo "a=2"; echo "a=3" ;else echo "no"; fi;
zero
a=2
a=3


708@708-PC MINGW64 /e/dot(1)/testByYe
$   if [[ 1<2 ]] ;then echo "zero";   echo "a=2"; echo "a=3" ;else echo "no"; fi;
bash: unexpected token 284 in conditional command
bash: syntax error near `1<'

```

而bash也一般是使用`$((...))` 进行算术的运算.

[1]:http://jueqingsizhe66.github.io/blog/2016/08/06/linux-shell-summary/#kernel 
[2]:http://bbs.chinaunix.net/thread-250407-1-1.html 
