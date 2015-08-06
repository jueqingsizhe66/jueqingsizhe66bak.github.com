---
layout: post
title: "字符串关键字的出现频率统计"
date: 2015-06-14 01:50:24 +0800
comments: true
categories: Linux
---

有时候我们需要统计一篇论文当中某个字符串的出现频率。
试验了，grep，awk，sed，wc -l等，最终发现awk的RS
完美的解决了问题。
<!--more-->
[RS的用法](http://www.cnblogs.com/fhefh/archive/2011/11/16/2251656.html)
[统计次数](http://blog.163.com/facteur@126/blog/static/23208030201282085943698/)


关键字文件（可以不断的添加你要分析的字段):
``` sh
风力机
气动力
失谐
偏航
阵风
扭矩
功率
风切变
转速
风速
升力系数
阻力系数
升阻比


```


短短的几行shell语句解决了问题，优雅。
shell文件：

``` sh

#!/bin/bash - 
#===============================================================================
#
#          FILE: analysis.sh
# 
#         USAGE: ./analysis.sh 
# 
#   DESCRIPTION: 
# 
#       OPTIONS: ---
#  REQUIREMENTS: ---
#          BUGS: ---
#         NOTES: ---
#        AUTHOR: Ye Zhaoliang (), zhaoturkkey@163.com
#  ORGANIZATION: YZL
#       CREATED: 2015年06月14日 01:18
#      REVISION:  ---
#===============================================================================

while read lines
do
    awk -v RS="$lines" 'END{print RS,--NR}' qiu.org #减减表示减1
done < "analysis.data"

```


## 搜索内容

1. grep -n "内容关键字" 文件名路径
2. awk '{if($0~关键字){}}'

等。
3. sed '/关键字/p'


##  结果

``` sh
┌─[root][javazhao-N53SM][/paper]
└─➞ sh analysis.sh 
风力机 379
气动力 397
失谐 31
偏航 353
阵风 63
扭矩 171
功率 139
风切变 59
转速 172
风速 353
升力系数 27
阻力系数 13
升阻比 2

```
