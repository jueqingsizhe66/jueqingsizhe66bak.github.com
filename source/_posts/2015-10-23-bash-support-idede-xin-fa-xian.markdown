---
layout: post
title: "Bash Support IDE的新发现"
date: 2015-10-23 00:28:00 +0800
comments: true
categories: Linux
---

[Bash Support][1]实现一个vim的bash IDE工具。好处式可以较快捷的
输入命令和编程以及调试(bashdb)。
<!--more-->

## 支持的几种后缀文件
1. c 的.c  c++ 的.cc  .cpp  cc
2. perl 的.pl
3. awk的.awk
4. shell的.sh
5. vim的.vim文件

## 查看系统的各种template
 
 一定要记住的一个命令 \ntl,通过它可以知道各个template系统文件
 都在干什么，以及如何使用那些快捷命令，比如\ckc \css \cfu \cc \cs  \se \sf 等。

## 几个常见的Macro

1. |FILENAME|
2. |AUTHOR|
3. |DATE|
4. |TIME|
5. |EMAIL|
6. |ORGANIZATION|
7. |AUTHORREF|

## css 的程序几个大的部分的注释

当你敲入\css会在vim的下端出现一个输入栏，有下面几个选项

1.	'GLOBAL DECLARATIONS'     : 'GLOBAL DECLARATIONS',
2.	'COMMAND LINE PROCESSING' : 'COMMAND LINE PROCESSING',
3.	'SANITY CHECKS'           : 'SANITY CHECKS',
4.	'FUNCTION DEFINITIONS'    : 'FUNCTION DEFINITIONS',
5.	'TRAPS'                   : 'TRAPS',
6.	'MAIN SCRIPT'             : 'MAIN SCRIPT',
7.	'STATISTICS AND CLEAN-UP' : 'STATISTICS AND CLEAN-UP',
  
  *技巧可以通过 G然后TAB就可以直接补全了。*
  之所以说他特别好是因为，根据[《Pro Bash Programming》][4], 提出的关于
  写更好的shell script的建议，如下所示：

0. Comments  文件开头的注释，以及单行注释、函数注释、块注释
1. Initialization of variables  变量声明
2. Function definitions  函数定义
3.  Runtime configuration (parse options, read configuration file, and so on) 通过getopt解析脚本选项和运行所需的配置文件的导入
4. Sanity check (are all values reasonable?)   一些判断、检查
5. Command Run  程序运行
6. Process information (calculate, slice and dice lines, I/O, and so on) 


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
compoundfile=$dict/Compounds

## Default is not to show compound words
compounds=

## Reular expression supplied on the command line
pattern=$1

## Function definitions

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
## parse command-line options, -c, -h, and -v
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
regex=$1
echo $(( $regex ))
## sanity check
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

******

## ckc的几种注释关键字

当你敲入\ckc会在vim的下端出现一个输入栏，有下面几个选项

1.	'bug'         : ':BUG:|DATE| |TIME|:|AUTHORREF|: <CURSOR>',
2.	'todo'        : ':TODO:|DATE| |TIME|:|AUTHORREF|: <CURSOR>',
3.	'tricky'      : ':TRICKY:|DATE| |TIME|:|AUTHORREF|: <CURSOR>',
4.	'warning'     : ':WARNING:|DATE| |TIME|:|AUTHORREF|: <CURSOR>',
5.	'workaround'  : ':WORKAROUND:|DATE| |TIME|:|AUTHORREF|: <CURSOR>',
6.	'new keyword' : ':<CURSOR>:|DATE| |TIME|:|AUTHORREF|: {+COMMENT+}',


进一步的帮助参考[Bash Style Guide][2]
[1]:http://www.vim.org/scripts/script.php?script_id=365 
[2]:https://lug.fh-swf.de/vim/vim-bash/StyleGuideShell.en.pdf 
[3]: /images/linux/bash-keymap.png
[4]:http://www.apress.com/9781430219972/ 
