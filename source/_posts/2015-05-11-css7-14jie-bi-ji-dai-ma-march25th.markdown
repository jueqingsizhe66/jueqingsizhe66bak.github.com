---
layout: post
title: "css7-14节笔记代码March25th"
date: 2015-05-11 14:58:42 +0800
comments: true
categories: JavaBasic
---
<!--more-->

   <Link type="text/css" rel="stylesheet" href="">
    <Link type="javascript" rel="stylesheet" href="">
    <!--
     Link 控件
       rel属性  ：目标文档与当前文档的关系
       type属性 ： 文档类型
    -->
      <!--设background:url(b.jpg);置背景颜色 和文字字体-->
    <!--定义样式-->
    <!--
    学习的网站 ！
http://www.w3school.com.cn/css/index.asp(用到的时候记得查红色关键字即可)
找颜色：http://www.114la.com/other/rgb.htm
       背景控件的修饰
           注意background相关信息即可！
                  background-color
                  background-image
                  background-position

        文本控件的修饰
            一般注意text相关信息
                   text-align
                   text-width
                   text-shadow等
                   text-indent:2em     1em =当前文本的一个字符的像素大小 （一个公式）
                            如果font-size=24px(默认是14px)  也就是此时1em=24px
                            这就是em的用法！！注意了！！
                    text-decoration:underline
                                    none
                                    overline
                                    line-through
                                    blink
        字体控件的修饰
             一般注意font开头
                  font-ize
                  font-family:"微软雅黑"
                  font-style:italic   对应htmlde <I>标签控件
                  font-weigth:
                           100-900
                            border
                            lighter
                  font-variant:small-cap

        链接控件（A标签）的修饰（伪类选择器）
              四种状态   :link      
                         :visited
                         :hover
                             a:hover{text-decoration:underline; color:red}
                         :active
                      提示 1：a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。
                提示 2：a:active 必须被置于 a:hover 之后，才是有效的。

        列表控件的修饰
             list-style-image:url{img/a.gif}
             list-style-type:square
             list-style:none

        表格控件的修饰
            表格作用：数据的格式化
            table{border-space: width:50px}
            tr{}
            td{}

        框模型（盒子模型）：

       Opera 浏览器先使用 ctrl+u 再使用ctrl+alt+i  打开computed的窗口，观察盒子模型（注意使用浏览器的开发者功能）
        IE用F12
           Margin是什么？
            元素的最外层，光晕部分 
                   Margin-top
                   margin: 1px 2px 3px 4px
           Border是什么?
             元素的中间层 夹在margin和padding之间
                  border:1px solid #3333

           Padding 是什么？
              元素的内层，包裹着元素
                   Padding-top:
                   padding: 1px 2px 3px 4px

        轮廓
           元素的border和margin之间！起着一种强调的作用！！ 起着元素突出作用！(元素其实就是控件）
          outline:red  2px dash;
          input:focus{border:green 1px solid}  当点击文本框！！！则显示为绿色！这是很好的技能！！！！
                                               一定要多写几遍！！！！不错！！

    所以其实在一个元素的外部有四重的包裹由内至外：  padding-> border-->outline-->margin  注意你所设置的位置

综上所述： 所以在标签中一般有Id属性  ，也可能有Class属性
                一般规律1是：（必须掌握）
                            Style的内嵌入的修饰  >   Id选择器>  Class选择器 > 标签选择器
                   公式2：  selectors{属性名：属性值}   对应HTML的 标签<属性名=属性值>       CSS是用selectors来说标签， CSS使用冒号，HTML使用等号
                     CSS+HTML使用得好在乎经验和功夫，非一天两天。


CSS基本选择器：
标签选择器
id选择器   #id_name
class选择器    .classname
CSS拓展选择器：
派生选择器  h1 a{}   指的是只要h1里面包含着a标签既满足该条件
子元素选择器（直接）  h1>a{}  这个a必须是在h1的下一层  不能是下下层（这边的分层是指谁包裹谁的意思）
属性选择器   a[href]{color:red}
组合选择器   h1,h2,h3,h4{color:blue}（注意提取共性）
伪选择器  主要针对于连接标签   （可用于控制界面的效果）
                     ：linked       ：visited      :hover         :actived       :focus(获得焦点的时候）


两种外联式方式：
```css
<link rel="stylesheet" type="text/css" href="*.css"/>    常用               
@import url("*.css");  必须写到<style></style>标签中
第一种是界面直接加载全部，第二种是在页面加载完毕，然后在加载css文件，也就是刚开始没有样式，了解即可。

    -->

<Style type="text/css">
      body{font-family:"微软雅黑"}
      .box{width:600px; margin:0 auto; padding-top:100px;background-color:black}
      .box 
      H1{text-align:center text-shadow:2px 5px 5px 
      rgba(0,0,2,1);color:white; }
      .box 
      p{background:#2c2c29;border-radius:10px;border:1px solid #222220;
      line-height:38px;margin-right:5px;float:left;width=600px;}
      .box 
      span{float:left;display:block;text-align:right;background:#373733
      ;width:70px;height:40px;color:#ffffff;border-right:1px solid #222220;}
      .input_box{width:167px;padding-left:10px;height:38px;border:0px;background:
      #2c2c29;float:left;color:#ffffff}
      .input_btn{width:500px;height:40px;margin-top:15px;line-height:40px;
      background:#009900;border:1px solid #0099900;border-radius:10px;
      cursor:pointer;}
      .input_btn:hover{background:#00CD00}

    </Style>
```



1.DOM（文档对象模型）是HTML和XML的应用程序

接口（API)所谓文档对象模型，其实就是对网页

HTML中的各种元素的一种内部的表示，例如HTML

中的头、段落、列表、风格、ID等，所有的元素

都能通过DOM来访问
document （计算机）文档
DOM就是Html页面的模型，将每个标签都做为一个

对象，JavaScript通过调用DOM中的属性、方法

就可以对网页中的文本框、层等元素进行编程控

制。

2.所有变量，方法，元素等都属于window对象。

都可以用window调用页面中定义的变量和方法都

是window的

3.CSS+JavaScript+DOM=DHtml

4.onload 事件会在页面或图像加载完成后立即发

生
5.window对象的方法
window.alert(‘大家好！’);//弹出警告对话

框
window.confirm(‘确定要删除吗？’);//确定

、取消对话框，返回true或false;
window.navigate(url);//将网页重新导航到

url,支持IE、Opera11.6。并不推荐,有些浏览器

不行,
建议使用window.location.href=‘url’;//支

持大多数浏览器
6.计时器
1>var steId=setTimeout(function(){
  alert('这是一次性的')
  },1000);
  clearTimeout(setId);//清除计时器
2>var setId=setInterval(function(){
        alert('神州行，我看行');
},1000);

//clearInterval(setId);//清除计时器
7.onload（页面加载后触发
onunload（页面卸载后触发）
onbeforeunload（页面卸载前触发）

8.window.location对象：
window.location.href=‘’;//重新导航到新页

面,可以取值，也可以赋值。
window.location.reload();//刷新当前页
对象的事件，所有元素的事件都可以通过event属

性取到相关信息。//兼容IEwindow.event是IE下

非常重要的属性，用来获得发生事件时的信息，

事件不局限于window、Chrome，不兼容FF（用

event参数）。
window.event.altKey属性，bool类型，表示事

件发生时是否按下了alt键。类似的还有

ctrlKey,shiftKey。
clientX、clientY 发生事件时鼠标在客户区的

坐标；(页面左上角)不支持火狐
screenX、screenY 发生事件时鼠标在屏幕上的

坐标；（相对于屏幕）
offsetX、offsetY 发生事件时鼠标相对于事件

源（比如点击按钮时触发onclick）的坐标。
```html 
onload=function(){

    document.getElementById

('dv').onmousedown=function(){

      if(window.event.altKey){
            alert('按下了alt键');

       }else if(window.event.shiftKey){
            alert('按下了shift键');

       }else if(window.event.ctrlKey){
            alert('按下了ctrl键');

        }else{
            alert('只是按下了鼠标了');
        }
        alert(window.event.button);

    };
};
```
9.赋值和粘贴的控制
```html
<script type="text/javascript">
   </script>

    <script type="text/javascript">

        onload=function(){
         
document.body.oncopy=function(){

                setTimeout(function(){
                  var tt= 
clipboardData.getData('text')+'文本出自哪

里';                    

clipboardData.setData('text',tt);
                },100);

            };
        };

    </script>
```
10.tag(标签)
11.获取元素的三种方式
getElementById(), （非常常用），根据元素的

Id获得对象，网页中id不能重复。也可以直接通

过元素的id来引用元素，但是有有效范围、
getElementsByName()，根据元素的name获得对

象，由于页面中元素的name可以重复，比如多个

RadioButton的name一样，因此

getElementsByName返回值是对象数组。
getElementsByTagName()，获得指定标签名称的

元素数组，比如getElementsByTagName(“input

”)可以获得所有的<input>标签。*表示所有标签
12.三个练习题，必须熟练
1>每隔一秒获得当前时间
  onload=function(){


            setInterval(function(){

                document.getElementById

('p1').innerText=new Date

().toLocaleTimeString();

            },1000);
        };
13.事件冒泡
