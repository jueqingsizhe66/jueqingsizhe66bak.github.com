---
layout: post
title: "bash Array 和拓展的pattern match"
date: 2015-10-24 15:36:57 +0800
comments: true
categories: Linux
---

通过[Bash_It][1]的学习，第一次意识到Shell数组的不一样的写法。
并且也看到Pattern Match的一些用途。
<!--more-->

## 1 Array

### 1.1 Bash数组定义

通过(1,2,3)区分于fortran语言的(/1,2,3/).

bash1=(34,5,6,4)

bash2=("34","5","5","4")

### 1.2 Bash关联数组定义

通过 -A开关选项定义一个关联数组。

declare -A bash1

bash1["34"]="53"

bash1["56"]="45"

```
declare -A array
for subscript in a b c d e
     do
         array[$subscript]="$subscript $RANDOM"
     done

```

也可以在*命令行使用*

```
  a=($(ls))  #而不是a=$(ls)

  #这样就可以使用
  ${a[1]}
```

### 1.3 Bash数组添加数据

bash1+="54"

### 1.4 Bash数组显示数据和便利

```
##单个显示
printf "%s\n" "${array["c"]}"

##遍历
printf "%s\n" "${array[@]}"
printf "%s\n" "${array[*]}"

```

### 1.5 拓展的操作

\#表示长度的作用

1. ${#array[2]}  现实第二个数组元素的长度
2. ${#array[@]}   显示全部数组元素的长度
3. ${#array[\*]}  显示全部数组元素的长度
4. ${#array[@]:2:3}  获取第2个到3个的长度
5. ${!array[@]} !的作用是现实所有的key在数组当中。

进一步参考   

[pro_Bash_programming][7]第五章 array部分



******
## 2. PATTERN MATCH

```
?(pattern-list)   Matches zero or one occurrence of the given patterns
*(pattern-list)   Matches zero or more occurrences of the given patterns
+(pattern-list)   Matches one or more occurrences of the given patterns
@(pattern-list)   Matches one of the given patterns
!(pattern-list)   Matches anything except one of the given patterns
```

比如：
```
  $ ls +(ab|def)*+(.jpg|.gif)
```

进一步参考  ls使用[PATTERN-MATCH][6]

******

Linux基础资料参考 

[GNU官网][2]

[鸟哥论坛][3] 

[Linux_About][4] 

[CN_Blogs的一个收集][5]


## 3 事情流程

1. 认定你的事情
2. 做
3. 检验

拓展为:

1. 认清楚这件事情；
2. 分析与这件事情有关的一切的一切；
3. 制定做好这件事情的计划；
4. 实施计划；
5. 验证这件事情的结果；

[1]:http://jueqingsizhe66.github.io/blog/2015/10/23/bash-support-idede-xin-fa-xian/ 
[2]:http://www.gnu.org/ 
[3]:http://linux.vbird.org/linux_basic/ 
[4]:http://linux.about.com/ 
[5]:http://www.cnblogs.com/bluebbc/tag/linux 
[6]:http://www.linuxjournal.com/content/bash-extended-globbing 
[7]:http://www.apress.com/9781484201220 
