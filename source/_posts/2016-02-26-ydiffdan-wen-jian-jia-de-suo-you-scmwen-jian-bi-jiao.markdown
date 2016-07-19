---
layout: post
title: "Ydiff单文件夹的所有scm文件比较"
date: 2016-02-26 18:29:08 +0800
comments: true
categories: Linux
---

关于YDIff处理已经在[使用Ydiff工具生成文件差异比较文件][1]中提及，但是只是比较了两个文件夹之间的文件，
而如果是较为简单的单个文件的所有文件的比较？类似思路如下所示，
<!--more-->

一定要注意数组使用的是小括号包括起来，否则程序有问题。


``` sh

#遍历当前目录下的所有文件 并进行比较 最终结果存入final-result
#array1=`ls *.scm`
array1=(`ls *.scm`)  # 必须加上括号
mkdir final-result
for i in `seq 0 $((${#array1[@]}-1))`; do 
    #遍历从i之后的文件
    for j in ${array1[@]:$i+1:${#array1[@]}}; do 
        #echo ${array1[$i]},$j;
        dir1=${array1[$i]};
        dir2=$j;
        # 选取对应文件夹下的scm文件
            #export TestVari=$i
            /home/happycamp-of-lisp/wangying/ydiff/diff-lisp $dir1 ${dir2}

    done;
done;
        mv *.html final-result
        cp ./nav.js ./diff.css final-result


```

ls会找出当前文件夹下的所有scm文件，然后利用bash中的数组操作，使用双重
循环进行遍历即可。
[1]:http://jueqingsizhe66.github.io/blog/2016/02/22/shi-yong-ydiffgong-ju-sheng-cheng-wen-jian-chai-yi-bi-jiao-wen-jian/ 
