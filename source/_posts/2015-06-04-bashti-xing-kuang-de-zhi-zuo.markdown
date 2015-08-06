---
layout: post
title: "bash提醒框的制作"
date: 2015-06-04 21:13:33 +0800
comments: true
categories: Linux
---

一个elegant的提示窗口的制作。从\_repeat到升级的优化版本（采用三步优化），然后到后来组装成alert函数。
<!--more-->

``` sh
_repeat()
{
    #@ 使用方式： _repeat 要重复的字符串  重复次数
    #@  Usage : _repeat string number
    _REPEAT=  #set but empty
    echo $2
    while [[ ${#_REPEAT} -lt $2 ]]
    do
        _REPEAT=$_REPEAT$1
    done
}

```

``` sh
_repeat "hello" 30
echo $_REPEAT
```

``` sh
#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  _repeat1
#   DESCRIPTION:  优化版本
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
_repeat1()
{
    #@ 使用方式： _repeat 要重复的字符串  重复次数
    #@  Usage : _repeat string number
    _REPEAT1=$1  #set and not empty, or unlimit recycle
    while [[ ${#_REPEAT1} -lt $2 ]]
    do
        _REPEAT1=$_REPEAT1$_REPEAT1$_REPEAT1 ## 3次优化
    done
    _REPEAT1=${_REPEAT1:0:$2} ##Trim到我们需要的长度
}

repeat()
{
    _repeat "$@"
    printf "%s\n" "$_REPEAT1"
}

```


``` sh
_repeat1 "hello" 30
echo $_REPEAT1
repeat "hello 30"

```



``` sh
alert()
{
    _repeat1 "${2:-#}" $(( ${#1} + 8 )) ## ${2:-#} 如果$2未定义则使用#
    printf '\a%s\n' "$_REPEAT1" ## \a=BEL
    printf '%2.2s %s %2.2s\n' "$_REPEAT1" "$1" "$_REPEAT1" ## \a=BEL
    printf '%s\n' "$_REPEAT1"
}


alert "Do you really want to delete all the files?"

```

