---
layout: post
title: "使用Ydiff工具生成文件差异比较文件"
date: 2016-02-22 19:24:53 +0800
comments: true
categories: Linux
---

Ydiff是一个不错的lisp语法分析工具([王垠编制][1]).下面是该工具简单用途和使用说明。
<!--more-->

1. 用途
主要是js，cpp，和lisp的分析，现在也有[python][2],[ruby][2]的分析工具。

2. 使用说明

编译的时候仅仅在项目下载之后，make一下即可生成对应的diff-cpp,diff-js,diff-lisp分析工具.
文件分为单文件和文件夹两种。



## 单文件使用说明

``` sh
 diff-lisp *.scm *1.scm 
```


## 文件夹使用方式


保存下面源代码为process-lisp.sh
``` sh

dir1=let-lang
dir2=letrec-lang
for i in `ls ${dir1}/*.scm`
do
    export TestVari=$i
    /home/happycamp-of-lisp/wangying/ydiff/diff-lisp $i ${dir2}/${TestVari##*\/}
done


```

当然也可以改为
``` sh

dir1=let-lang
dir2=letrec-lang
for i in `ls ${dir1}/*.scm`
do
    /home/happycamp-of-lisp/wangying/ydiff/diff-lisp $i ${dir2}/${i##*\/}
done


```
只要修改对应的文件夹dir1,dir2,就可以分析对应文件夹的scm所有文件,然后运用下面命令行，即可生成
对应的html文件,方便阅读

```
bash process-lisp.sh
```

## 数组引入进一步修改处理程序

数组的shell操作参考[如下][4],

+ 定义的时候通过小括号包裹
+ 调用的时候通过${}

注意 [数据运算][5]和判断(**注意只有在双括号的小于号才是小于号**)的两个中括号
```
seq 1 $((${#array1[@]}-1))

```

测试过很多错误的情况，比如

```
array1=(`ls -l|grep ^d|awk '{print $9}'`); 
echo ${array1[@]:0:${#array1[@]}-1};
export length1=${!array1[@]}; 
for i in ${array1[@]:1:length1-1}; do 
    for j in ${array1[@]:$i:length1}; do 
        echo $i,$j;
    done;
done;

```
原因在于,${!}操作并不是把它当作数组形式，所以改为数组即可，使用seq。
```
length1=${!array1[@]}
echo $length1 
    0 1 2 3

echo ${#length1[@]}
    1

```

正确的过程(**注意$i+1**)
```
array1=(`ls -l|grep ^d|awk '{print $9}'`);  
for i in `seq 1 $((${#array1[@]}-1))`; do 
    for j in ${array1[@]:$i+1:${#array1[@]}}; do 
        echo ${array1[$i]},$j;
    done;
done;

```

结果,满足组合公式
```
1,letrec-lang
1,lexaddr-lang
1,proc-lang
2,lexaddr-lang
2,proc-lang
3,proc-lang

```


## 批处理解析scm文件

当文件夹下存在如下文件下，则可以进一步利用[程序](#pi)进行分析，注意拷贝nav.js和diff.css进行渲染
```
ls┌─[root][canbetter-N53SM][±][master ?:10 ✗][2.2.1][/home/happycamp-of-lisp/EOPL2014/DF-eopl/chapter3/proc-lang/ds-rep]
└─➞ ls
compiled  data-structures.scm  drscheme-init.scm  environments.scm  interp.scm  lang.scm  tests.scm  top.scm

```

<h3 id="pi">批处理程序</h3>
```
array1=(`ls -l|grep ^d|awk '{print $9}'`); 
# 遍历所有文件夹 除了最后一个
for i in `seq 1 $((${#array1[@]}-1))`; do 
    #遍历从i之后的文件
    for j in ${array1[@]:$i:${#array1[@]}}; do 
        #echo ${array1[$i]},$j;
        dir1=${array1[$i]};
        dir2=$j;
        mkdir ${dir1}-${dir2}
        # 选取对应文件夹下的scm文件
        for k in `ls ${dir1}/*.scm`
        do
            #export TestVari=$i
            /home/happycamp-of-lisp/wangying/ydiff/diff-lisp $k ${dir2}/${k##*\/} 
        done

        mv *.html ${dir1}-${dir2}
        cp ./nav.js ./diff.css ${dir1}-${dir2}
    done;
done;


```

然后可以在terminal运行
```
python -m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 ...

```
在浏览器输入
    http://localhost:8000/ 


### 有问题的数组下标

数组下表是从0开始的！
``` sh

array1=(`ls -l|grep ^d|awk '{print $9}'`); 
# 遍历所有文件夹 除了最后一个
for i in `seq 0 $((${#array1[@]}-1))`; do 
    #遍历从i之后的文件
    for j in ${array1[@]:$i+1:${#array1[@]}}; do 
        #echo ${array1[$i]},$j;
        dir1=${array1[$i]};
        dir2=$j;
        mkdir ${dir1}-${dir2}
        # 选取对应文件夹下的scm文件
        for k in `ls ${dir1}/*.scm`
        do
            #export TestVari=$i
            /home/happycamp-of-lisp/wangying/ydiff/diff-lisp $k ${dir2}/${k##*\/} 
        done

        mv *.html ${dir1}-${dir2}
        cp ./nav.js ./diff.css ${dir1}-${dir2}
    done;
done;

mkdir final-result
for i in `seq 0 $((${#array1[@]}-1))`; do 
    #遍历从i之后的文件
    for j in ${array1[@]:$i+1:${#array1[@]}}; do 
        #echo ${array1[$i]},$j;
        dir1=${array1[$i]};
        dir2=$j;
        mv ${dir1}-${dir2} final-result
    done;
done;

```

这样就完成了文件夹下所有的文件对比操作，当然得确保该文件夹下存在scm文件。

## 进一步修改YDiff？

[1]: https://github.com/yinwang0/ydiff 
[2]: https://github.com/yinwang0/pysonar2 
[3]: https://github.com/yinwang0/rubysonar 
[4]: http://blog.csdn.net/liufei_learning/article/details/8000570 
[5]: http://blog.csdn.net/flowingflying/article/details/5146160 
