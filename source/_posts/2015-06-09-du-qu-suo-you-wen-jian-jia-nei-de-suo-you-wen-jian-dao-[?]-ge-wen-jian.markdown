---
layout: post
title: "读取所有文件夹内的所有文件到一个文件"
date: 2015-06-09 17:40:11 +0800
comments: true
categories: Linux
---

问题：
我想着每次都需要进入文件夹(cd)，然后再vim读取有点不方便。

解决方法：
何不用shell直接便利，然后生成一个单一文件即可。

解决步骤：
1：读取所有文件夹
2：利用cat打开所有文件，并追加到summary.org
3：为了美观增加了文件名头。

<!--more-->

``` sh
#!/bin/bash - 
#===============================================================================
#
#          FILE: browser.sh
# 
#         USAGE: ./browser.sh 
# 
#   DESCRIPTION: 
# 
#       OPTIONS: ---
#  REQUIREMENTS: ---
#          BUGS: ---
#         NOTES: ---
#        AUTHOR: Ye Zhaoliang (), zhaoturkkey@163.com
#  shANIZATION: YZL
#       CREATED: 2015年06月09日 14:14
#      REVISION:  ---
#===============================================================================

generateChapter()
{
    for var in `seq 1 20`
    do
        if [[ var -lt 10 ]] # < cannot . Error
        then
            var="0$var"
            echo "Chapter$var"
        else
            echo "Chapter$var"
        fi

    done
}


for cap in `generateChapter`
do
    for file in "$cap"/*
    do

        echo "--------------------<<<<<<<<<"$file">>>>>>>>>--------------------" >>summary.sh
        cat $file >> summary.sh
        echo "--------------------<<<<<<<<<"$file">>>>>>>>>--------------------" >> summary.sh
    done
done

```

