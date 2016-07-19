---
layout: post
title: "program-as-data"
date: 2016-03-30 13:23:21 +0800
comments: true
categories: scheme
---

[A program is like an essay. The first version is a draft, and drafts demand editing.][1]
Program is also can be seen as data.

1. 直觉认识proc,提取body。
2. 对比识别。
3. 加壳。
4. 三行表格，n列迭代。
<!--more-->

``` scheme

(value-of (proc (var body)) env)
 = (value-of (proc-exp (procedure (var body env)) val) env)

 = (value-of body ([var=val] env))
```
函数其实也是expression，它可以被consume也可以被produce.凭着这个脑中的
印象是否可以帮助你继续理解abstration和程序即数据的思想。

所有的函数和数据犹如花生的壳和仁的关系。

![peanut][2]

只有加壳就相当于是创建一层抽象，把类似的东西包裹起来，或者也可以换着一种思路
（每个壳里面都包着类似的花生仁，只不过可能存在些许不同）


### Two similar functions
``` scheme
; Los -> Boolean
; does l contain "dog"
(define (contains-dog? l)
  (cond
    [(empty? l) #false]
    [else
     (or
       (string=? (first l) "dog")
       (contains-dog?
         (rest l)))]))

	
; Los -> Boolean
; does l contain "cat"
(define (contains-cat? l)
  (cond
    [(empty? l) #false]
    [else
     (or
       (string=? (first l) "cat")
       (contains-cat?
         (rest l)))]))
```


### 加壳

加上一个函数皮，并封上一层。

``` scheme
; String Los -> Boolean
; determines whether l contains the string s
(define (contains? s l)
  (cond
    [(empty? l) #false]
    [else (or (string=? (first l) s)
              (contains? s (rest l)))]))
```

然后我们就可以类似的改写了
``` scheme
; Los -> Boolean
; does l contain "dog"
(define (contains-dog? l)
  (contains? "dog" l))
	
	
; Los -> Boolean
; does l contain "cat"
(define (contains-cat? l)
  (contains? "cat" l))
```

这个典型的过程就是函数抽象。进一步可以参考[HowToDesginProgram][1]和[TheLittleScheme][3].
Note: 你需要解析的其实是花生仁，但是你不得不先把壳打开或者通过另外一种方式，比如红外线等技术把它识别出来。

也就是说进一步归纳的话，你首先得recognise识别出来，然后才能进行解析（提取其中的蛋白质、脂肪、热量等）。

![mybaby][4]

保护它的壳，给它提供营养，防止它受到感染和伤害。

[表格转换链接][5].

<table>
      <tr>
                <td>index</td>
                <td>0</td>
                <td>1</td>
                <td>2</td>
                <td>…</td>
      </tr>
      <tr>
                <td>M</td>
                <td>a+S</td>
                <td>a+1*W+S</td>
                <td>a+2*W+S</td>
                <td>…</td>
      </tr>
      <tr>
                <td>f at M</td>
                <td>f(a+S)</td>
                <td>f(a+1*W+S)</td>
                <td>f(a+2*W+S)</td>
                <td>…</td>
      </tr>
      <tr>
                <td>Area</td>
                <td>W*f(a+S)</td>
                <td>W*f(a+1*W+S)</td>
                <td>W*f(a+2*W+S)</td>
                <td>…</td>
      </tr>
      <tr>
                <td>residual</td>
                <td>60.00%</td>
                <td>50.00%</td>
                <td>40.00%</td>
                <td>…</td>
      </tr>
      <tr>
                <td></td>
      </tr>
</table>

慢工才能出细活，通过三行表格n列迭代的形式(也可以进一步加大计算量变成n行表格n列迭代)理解递归迭代的过程,在一定的时间，进行一系列的列计算，并让时间推进，直到满足你想要的结果。

[1]:http://www.ccs.neu.edu/home/matthias/HtDP2e/part_three.html 
[2]: /images/lisp/peanut.jpeg
[3]:http://www.ccs.neu.edu/home/matthias/BTLS/ 
[4]: /images/lisp/baby.png
[5]:http://pressbin.com/tools/excel_to_html_table/index.html 
