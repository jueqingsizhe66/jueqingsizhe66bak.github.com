---
layout: post
title: "AllFilesInFoldsToOneFile"
date: 2015-08-09 14:03:43 +0800
comments: true
categories: Linux
---

1. find的使用
2. 更新了注释
<!--more-->


``` sh
#!/bin/bash - 
#===============================================================================
#
#          FILE: browserNew.sh
# 
#         USAGE: ./browserNew.sh 
# 
#   DESCRIPTION: 
# 
#       OPTIONS: ---
#  REQUIREMENTS: ---
#          BUGS: ---
#         NOTES: ---
#        AUTHOR: Ye Zhaoliang (), zhaoturkkey@163.com
#  ORGANIZATION: YZL
#       CREATED: 2015年06月27日 15:19
#      REVISION:  ---
#===============================================================================

#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  generateChapter
#   DESCRIPTION:  针对不同额文件采用不同的处理方法，判断目录 和判断脚本是一个重要的操作
#                 但是更为重要的是 find获得文集拿的绝对路径(相对于当前文件)
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------

## Function definition 

generateChapter() # @Description : 对不同文件进行不同处理
                  # @usage       : generatechapter
{
    # 妙用find 得到当前目录的相对路径 不需要不断的进入目录
    for var in `find . -name "*"`
    do
        if [[ -d  $var ]] # < cannot . Error
        then
            echo "${currentDir}${var#.}是一个目录" # 使用#号来删除之前的点号
    # 为什么会想到使用 ${var##*/}因为var是一个相对当前文件的绝对路径，所以经过测试发现他和${0}等效如果截取到最大长度的/的话
        elif [[ "${var##*/}" == "${0}" ]] # 一定要注意等式两边有空格
        then
            echo "${currentDir}${var#.} ${0} 脚本文件不处理"
        else
            getFile ${currentDir}${var#.} # 这边需要去除到第一个点号,这是才得到的处理方法
        fi

    done
}



#---  FUNCTION  ----------------------------------------------------------------
#          NAME:  getFile
#   DESCRIPTION:  在每个文件的头部添加一些信息
#    PARAMETERS:  
#       RETURNS:  
#-------------------------------------------------------------------------------
getFile() #@ Description: 细节处理  包括产生开头 产生边框等
          #@ Usage      : getFile
{
    startString="# -------------------<<<<<<<<<"$1" started>>>>>>>>>-------------------#"
    endString="# --------------------<<<<<<<<<"$1" ended >>>>>>>>>-------------------#"
       #echo "" >>summary.sh
    #echo -e "\033[44;37m -------------------<<<<<<<<<"$1" started>>>>>>>>>-------------------\033[0m" >>summary.sh
    #echo  "# -------------------<<<<<<<<<"$1" started>>>>>>>>>-------------------" >>summary.sh
    echo $startString >> summary.sh
    echo "#`printf "%$(( ${#startString}-2 ))s" " "`#" >>summary.sh
    echo "#`printf "%$(( ${#startString}-2 ))s" " "`#" >>summary.sh
    echo "#`printf "%$(( ${#startString}-2 ))s" " "`#" >>summary.sh ### 不加上$反而不行
   # echo "#" >>summary.sh
   # echo "#" >>summary.sh
    sed '1,7d' $1 >> summary.sh
    echo "#`printf "%$(( ${#endString}-2 ))s" " "`#" >>summary.sh
    echo "#`printf "%$(( ${#endString}-2 ))s" " "`#" >>summary.sh
    echo "#`printf "%$(( ${#endString}-2 ))s" " "`#" >>summary.sh
    #echo "#" >>summary.sh
    #echo "#" >>summary.sh
    #echo -e "\033[43;37m --------------------<<<<<<<<<"$1" ended >>>>>>>>>-------------------\033[0m" >> summary.sh
    #echo "# --------------------<<<<<<<<<"$1" ended >>>>>>>>>-------------------" >> summary.sh
    echo $endString >> summary.sh
    echo "" >>summary.sh
    echo "" >>summary.sh
    echo "" >>summary.sh
    
}

## variable initial 
currentDir=`pwd`


## processing information 
# 如果存在 summary.sh 证明已经存在了，就先把删掉， 因为这里是
# 采用追加的方式，而不是写入。
if [[ -e ./summary.sh ]]
then 
    rm -rf ./summary.sh
fi
# 调用产生所有的文件夹内的数据到一个文件中
generateChapter


```
