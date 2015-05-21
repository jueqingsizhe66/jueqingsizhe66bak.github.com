---
layout: post
title: "JavaScript 7-11节笔记代码March28th"
date: 2015-05-11 14:58:43 +0800
comments: true
categories: JavaScript
---
<!--more-->
function串讲笔记

1：function是什么？
     JS的function类似于java和.Net的方法

2：对比JS版本和Java版本的两个变量的求和，得出三个不同点。
JS版本：
```html
function sum(num1,num2)
{
    var ret = num1+num2;
    return ret;
}
```
Java版本：
```html
public int sum(int num1,int num2)
{
   int ret= num1+num2;
  return ret;
}
```

相同点：
函数名字
参数列表
变量声明
返回值
不同点：
JS的函数声明没有修饰符
JS的函数声明没有返回值
JS的函数声明的参数列表没有变量类型


由这三个不同点我们可以提出三个问题
为什么没有修饰符？
为什么没有返回值类型？
为什么参数列表没有变量类型？


3：JS为什么没有修饰符？
Js不是面向对象的语言，虽然有对象但不是真正的对象。

Js的对象是模拟出来的，没有封装的概念，所以不需要使用修饰符（修饰符只是针对于对象）

4：JS为什么没有返回值类型和参数列表变量类型？
这两个问题是一样的道理，都是弱类型的原因
5：由参数列表引入一个arguments对象，what is it?
  在开始了解arguments对象之前，必须说明函数的四种定义方式，如下所示：
5.1最一般的函数定义方法：
有function
有函数名称sum
有参数列表 num1,num2
可有可无的return语句
```html
function sum(num1,num2)
{
    var ret = num1+num2;
    return ret
    }
```
5.2最常见的匿名类函数调用方法：
有function
没有函数名称
有变量泪飚 num1,num2
可有可无的return语句
通过一个var sum函数类型变量获得该类型，并使得sum变量具有可调用的属性。
```html
var sum = function(num1,num2)
{
    var ret = num1+num2;
    return ret
    }
```
规律：页面中的js代码执行之前，浏览器会先扫描全部的js代码，遇到一个函数声明，就把此函数的声明加到全局域，从上到下扫描，若函数名称一样，后声明的函数会覆盖掉前面。
5.3较特殊的匿名类函数的声明和使用：
两个括号()()
第一个括号是匿名类函数的声明
第二个括号是匿名类的函数实参
```html
(function(num1,num2)
{
   var ret=num1+num2;
   return ret;
})(3,2);
```
特点：只会执行一次。

5.4较少用的Function类产生函数对象

```html
var sum = new Function();形式
var sum = Function("n1","n2","alert(n1+n2); var a=3;var b=4;alert(a+b);")

sum(3,3);//和其他函数声明的调用一样。
```

这也是为什么匿名函数可以赋值给一个变量的原因，因为Function类可以产生对象，对象可以赋值给一个变量（对象只是一个引用）。
特点：效率低效。


上面的五种函数声明方式都涉及到参数列表，而JS使用arguments可以获得任意长度的参数列表。
比如：
```javascript
var sum = function()
{
    var ret=0;
   for(var i=0;i<arguments.length;i++)
  {
    ret = ret+arguments[i];  
   }
    return ret;
}

sum(1,2);
sum(1,5,68,98);
sum(4,45,7,8,3,23,7,9);
```

而正因为arguments的作用导致了js不能重载。


6：JS为什么不能重载？
存在arguments对象，导致参数列表可以任意长度。
弱类型决定了参数列表的变量不需要类型修饰。

7：函数调用的两种方式？
标签的事件
```javascript
<button id =”btn” onclick=”alert(4);var  a =3; var b=4; alert(a+b);”)
复制代码

Dom的window.onload=function(){}方式，通过document的方法获得标签地址
Window.onload=function()
{
Window.document.getElementById(“btn”).onclick=function()
{
    Show();
}
}
```

