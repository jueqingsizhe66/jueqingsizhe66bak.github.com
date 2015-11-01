---
layout: post
title: "Shell_Programming_with_fewer_bugs"
date: 2015-08-09 14:37:30 +0800
comments: true
categories: Linux
---

shell的强大之处在于短小精悍，对于IO处理相当方便，当然也可以兼杂小型的数值计算（很少）。
shell脚本在编写过程中也会产生很多bug。本文借鉴《Pro-Bash-Programming》一书所提倡的：

1. comment  写上开头注释
2. Initialization of variables 变量初始化
3. Function definitions  函数定义
4. Runtime configuration 解析选项，读取配置文件等
5. Sanity Check  经过Runtime配置之后，可能修改某些值，比如shift操作，所以需要判断变量的合理性
6. Process information   最后才是计算、IO等

通过这中编写框架，可以减少错误。

<!--more-->


a. 首先保存最后的shell代码到一个文件wfe.sh
b. 其次下载所需要的字典文件
```
  mkdir ~/words
  cd ~/words
 wget http://cfaj.freeshell.org/wordfinder/Compounds
 wget http://cfaj.freeshell.org/wordfinder/singlewords

```
c. 运行脚本

``` sh

1. 获取帮助
  bash wfe.sh -h
2. 获取版本信息
  bash wfe.sh -v
3.单个单词查找  
  bash wfe.sh bro
4. 复合单词内查找（包含单个单词）
  bash wfe.sh -c bro 
```



d. 源码文件：
``` sh

#:    Title: wfe - List words ending with PATTERN
#: Synopsis: wfe [-c|-h|-v] REGEX
#:     Date: 2009-04-13
#:  Version: 1.0
#:   Author: Chris F.A. Johnson
#:  Options: -c - Include compound words
#:           -h - Print usage information
#:           -v - Print version number

set -x
export PS4='+ $LINENO : '

## 变量初始化
## Script metadata
scriptname=${0##*/}
description="List words ending with REGEX"
usage="$scriptname [-c|-h|-v] REGEX"
date_of_creation=2009-04-13
version=1.0
author="Chris F.A. Johnson"

## File locations
dict=$HOME/words
wordfile=$dict/singlewords
compoundfile=$dict/Compounds  # 源代码作者在这边设下陷阱1  con-->com

## Default is not to show compound words
compounds=

## Reular expression supplied on the command line
pattern=$1

## Function definitions 函数定义 

#提倡使用这样编写函数@ Description @Usage：
die() #@ DESCRIPTION: print error message and exit with supplied return code
{     #@ USAGE: die STATUS [MESSAGE]
  error=$1
  shift
  [ -n "$*" ] printf "%s\n" "$*" >&2
  exit "$error"
}

usage() #@ DESCRIPTION: print usage information
{       #@ USAGE: usage
        #@ REQUIRES: variable defined: $scriptname
  printf "%s - %s\n" "$scriptname" "$description"
  printf "USAGE: %s\n" "$usage"
}

version() #@ DESCRIPTION: print version information
{          #@ USAGE: version
           #@ REQUIRES: variables defined: $scriptname, $author and $version
  printf "%s version %s\n" "$scriptname" "$version"
  printf "by %s, %d\n" "$author" "${date_of_creation%%-*}"
  #printf "by %s, %d\n" "$author" "${date_of_creation%%-*"   指的那边错误不是真正的错误！一般需要往上找
}

## parse command-line options, -c, -h, and -v 解析

echo $(( $OPTIND ))
while getopts chv var
do
   case $var in
     c) compounds=$compoundfile ;;
     h) usage; exit ;;
     v) version; exit ;;
   esac
done
shift $(( $OPTIND - 1 ))  ## move out one parameter and then position to the right by one step
                          ## 右移操作
regex=$1           ## 设下陷阱2 没有注册regex变量，却直接调用$regex
echo $(( $regex ))

## sanity check   设下陷阱3 没有进行错误检查

if [ -z "$pattern" ]
then 
    {
        echo "Search term missing"
        usage
    } >&2
    exit 1
fi


## Search $wordfile and $compounds if it is defined
{
   cat "$wordfile"
   if [ -n "$compounds" ]  # not null  -n ~ -z   n :nonzero  z:zero
   then
      cut -f1 "$compounds"
   fi
#}| grep -i ".$regex$" |sort -fu   #errror
}| grep -i ".$regex$" |sort -fu 

## Case-insensitive sort; remove duplicates


```


