---
layout: post
title: "javascript闭包训练"
date: 2015-05-11 14:58:43 +0800
comments: true
categories: JavaScript 
---
<!--more-->
javascript  闭包训练

闭包其实就是保存着内部定义函数的一张表，该表保存着函数头、函数变量（变量当前状态的值）、函数实现。

利用  一个outFun 和一个innerFun进行实验《Learning jQuery 4th edition》

case 1:  闭包传递，内部函数可以被传递到外头

```javascript
      $(function(){
        //函数被封在一个闭包中 可以像数据一样传来传去
        var globalInner;
        function outFun()
        {

          console.log('Inside the outFun');
          function innerFun()
          {
            console.log('inside the innerFun of the OutFun');
          }
          globalInner=innerFun;
          console.log('innerFun():');
          innerFun();
          console.log('return Begin')
          return innerFun;
        }

        console.log('outFun:')
        outFun();
        console.log('globalInner:');
        globalInner();
        console.log('Test outFun return');
        outFun()();

      }) ;
```
复制代码
测试结果：

outFun:
TestCloser.html:12 Inside the outFun
TestCloser.html:18 innerFun():
TestCloser.html:15 inside the innerFun of the OutFun
TestCloser.html:20 return Begin
TestCloser.html:26 globalInner:
TestCloser.html:15 inside the innerFun of the OutFun
TestCloser.html:28 Test outFun return
TestCloser.html:12 Inside the outFun
TestCloser.html:18 innerFun():
TestCloser.html:15 inside the innerFun of the OutFun
TestCloser.html:20 return Begin
TestCloser.html:15 inside the innerFun of the OutFun
复制代码



case 2:  内部变量 和外部变量的区别
```javascript
      $(function(){
        console.log('测试InnerFunPig的局部变量');
        //局部变量是存在的
        //alert('hhell');
        function outFunPig()
        {

          function innerFunPig()
          {
            //局部变量 重新赋值
            var innerVar=1;
            console.log('innerVar:'+innerVar);
            innerVar++;

          }
          return innerFunPig;
        }

        var out1= outFunPig();
        out1();
        out1();
        var out2= outFunPig();
        out2();
        out2();

        console.log('测试outFunCat的全局变量');
        //测试全局变量
        var globalVar=1;
        function outFunCat()
        {

          function innerFunCat()
          {
            //局部变量 重新赋值
            console.log('innerVar:'+ globalVar);
            globalVar++;

          }
          return innerFunCat;
        }

        var out3= outFunCat();
        out3();
        out3();
        var out4= outFunCat();
        out4();
        out4();

        console.log('测试outFunDog的内部变量');
        //测试半全局变量
        function outFunDog()
        {

          //半全局变量
          var semiGlobalVar=1;
          function innerFunDog()
          {
            //局部变量 重新赋值
            console.log('innerVar:'+ semiGlobalVar );
             semiGlobalVar++;

          }
          return innerFunDog;
        }

        var out5= outFunDog();
        out5();
        out5();
        var out6= outFunDog();
        out6();
        out6();



      });
```
测试结果2：
测试InnerFunPig的局部变量
4TestCloser.html:44 innerVar:1
TestCloser.html:58 测试outFunCat的全局变量
TestCloser.html:67 innerVar:1
TestCloser.html:67 innerVar:2
TestCloser.html:67 innerVar:3
TestCloser.html:67 innerVar:4
TestCloser.html:81 测试outFunDog的内部变量
TestCloser.html:91 innerVar:1
TestCloser.html:91 innerVar:2
TestCloser.html:91 innerVar:1
TestCloser.html:91 innerVar:2
复制代码

