---
layout: post
title: "JavaScript1-6节笔记代码March26th"
date: 2015-05-11 14:58:43 +0800
comments: true
categories: JavaScript
---

js 常见 Events
属性        当以下情况发生时，出现此事件        
onabort        图像加载被中断        元素失去焦点        
onchange        用户改变域的内容
onclick        鼠标点击某个对象
ondblclick        鼠标双击某个对象        
onerror        当加载文档或图像时发生某个错误        
onfocus        元素获得焦点        
onkeydown        某个键盘的键被按下        
onkeypress        某个键盘的键被按下或按住        
onkeyup        某个键盘的键被松开        
onload        某个页面或图像被完成加载        
onmousedown        某个鼠标按键被按下        
onmousemove        鼠标被移动        
onmouseout        鼠标从某元素移开        
onmouseover        鼠标被移到某元素之上        
onmouseup        某个鼠标按键被松开        
onreset        重置按钮被点击        
onresize        窗口或框架被调整尺寸        
onselect        文本被选定        
onsubmit        提交按钮被点击        
onunload        用户退出页面   
一、复习DOM
        1.对于JS来说，在Web中，最重要的应用之一就是DOM。
        2.Dom是针对HTML和XML操作的一个API。
        3.Dom将一个文档表示为一颗家族树（父，兄，子 节点）
        4.几个常用的API。
                getElementById 只能用document来使用，通过id查找一个节点。
                getElementsByTagName  返回的是一个数组，通过指定的标签名去寻找，不必作用于整个文档。
                hasChildNodes 调用的节点是否含有子节点。
        5.Dom中的节点有文本节点（Text），属性节点（Attribute），元素节点（Element）。
        6.以上3类节点中，都有如下属性：
                nodeName：只读，标识的是给定节点的名字，该节点为只读属性。对于文本节点返回的是 #text 的字符串。
                noteType：只读，返回值为int，标识节点的类型。元素节点 1，属性节点 2，文本节点 3。（从小到大的记忆顺序）
                nodeValue: 可读，可写。返回当前节点的值，属性节点返回属性的值，文本节点返回文本的值，元素节点啥都没有。
        7.Dom中常用方法：
                replaceChild()：返回值是一个指向新的或者旧的节点。（？）作用是吧一个父元素中的子节点替换为另外一个子节点。
                        ex:        var reference = element.replaceChild(newChild,oldChild);
                getAttribute(): 返回的是指定元素的给凌属性的值。
                        ex:        var attributeValue = element.getAttribute(attributeName);
                setAttribute(key,value)：为指定的元素添加某个属性，如果此属性已经存在就刷新，如果不存在就添加。
                        ex:      var para = document.createElement(“p”);           para.setAttribute(“id”,”fineprint”);
                createElement(eleName):通过指定的标签名字来创建一个元素节点。这个新创建的节点没实际添加到某个节点上。
                        ex:        var oP = document.createElement("p");
                createTextNode()：创建一个包含指定文本的文本节点。返回值为一个指向新建文本节点的指针。
                        ex: var oText=document.createTextNode("HEllo world");
                appendChild()：为指定的元素添加一个节点到最后一个节点后。
                insertBefore()：在指定的节点前面添加。
                        ex:        var reference = element.insertBefore(newNode,targetNode);
                        注：Dom没有提供insertAfter()方法：就是在插入节点的下一个节点之前添加，就是在指定节点的前面添加。
                removeChild()：在指定节点中的子节点删除一个节点。
                        注：一定要是父节点去调用然后括号里面是自己点。
                        ex： var message = document.getElementById(“fineprint”);
                                        var container = message.parentNode;
                                        container.removeChild(message);
        8.Dom中常用属性
                ChildNodes：返回的是指定父节点的所有子节点。
                        注：文本节点和属性节点，肯定不含有子节点了，如果调用这个属性，会返回空数组。
                firstChild:返回的是指定元素的第一个子节点。
                        ex:node.ChileNodes[0]和firstChild 是相同的。
                lastChild:和first想法，最后一个。
                nextSibling：指定节点的下一个子节点。
                previousSibling:指定节点的上一个子节点。
                parentNode：返回的是指定节点的父节点。
二、JavaScript练习总结
        1.window.onload=function(){...}表示的是在页面加载完成后便开始运行。
        2.node.insertBefor(new node,null)。这样调用就可以在指定的前面添加了!
        3.如果网页乱码，可以在<head></head>之间加上：  <meta http-equiv="content-type" content="text/html; charset=utf-8"/>
        4.特别说明：insertBefor这个方法。3个参数：  paraentNode.insertBefor(newNode,childNodeInParaNode);
                就是说哪个Node来调用insertBefore方法就是在那个Node的子节点中插入，其中第一个参数是要插入的节点，第二个参数是自己点中要在哪个指定的子元素前面。
                注：第二个节点可是是null，就表示在最后添加，同AppendChild。特别注意了。
        5.getElementByTagName("标签")，这个里面输入的是HTML的标签。
        6.对于元素节点的nodeName，其返回值永远是大小的标签。
        7.忘记nodeValue吧，使用value就够了，前面的需要区分下。
        8.value这个属性可读可写。
        9.对于使用firstChild和lastChild不是我们所见的第一个。这个特别注意了。
        10.通常来说firstNode和lastNode获取的是文本节点。
        11.对于文本节点，如果要设置他的值，需要使用nodeValue而不是value。比如说firstChild和lastChild获取的都是属于文本节点。
        12.为某一个节点添加 可以使用innderHtml也可以使用 创建一个TextNode然后添加到某个节点上去。
        13.对于replaceChild，parentNode.replaceChild(newChild，childInParane).这个方法的作用是父节点调用替换某个子节点，第一个节点一般都是新的节点，第二个节点
        14.几个不加括号的地方:
                window.onload=function myFunc(){...}
        15.在JS中，方法带了括号表示运行。
        16.
三、问题
        2.innerHtml和innerText的区别？
        3.window.onload=function(){} 在一个页面只能写一次。
        4.关于parenNode和parentElement区别？



2.innerHtml和innerText的区别？
从字面意义来讲 innerHtml操作标签内的html代码 , 而innerText操作标签内的文本 , 当标签内只有文本时这两个方法是一样的 , 区别就在标签内还要子标签时,写代码测试就知道了

3.window.onload=function(){} 在一个页面只能写一次。
这个是"重量级"的加载完成事件 , 只能注册一次,jQuery的ready()就很方便


4.关于parenNode和parentElement区别？
parentNode 是Node类的属性 , 而parentElement是Element类的属性 , 很多情况下可以通用,只是属于两个不同的体系,使用时尝试着看看效果就好



```html

<Html>
<Head>

<!--script 一定要小写-->
<script type="text/javascript">
//javascript内部注释使用//表示单行注释  /**/表示快注释
//javascript大小写敏感
//javascript可以用var表示所有数值类型，代替int,String,boolean等
//按F5刷新，进行调试
//变量和数组的定义
//变量定义


var name='字符串';
var age=39;

//数组定义
//一定要数组用  [] 进行定义，元素之间用逗号隔开   []吃过亏
//map数组用{} 定义 键值对之间用逗号隔开
var apples=['ap1','ap2'];
var arr={"name":'张三','age':45};

//打印alert
alert(name);
alert(age);
for(var i=0;i<apples.length;i++)
{
    alert(apples[i]);
}

for(var key in arr)
{
    alert(key+'==='+arr[key]);
}

//数值类型的转换
var n = 'fsf';
if(isNaN(n))
  {
    alert('不能使用');
  }else
    {
      alert('可以使用');
    }
var num='100'
var n = Number(num);
alert(n);

alert(parseFloat('100.3'));//转小数
alert(parseInt('100'))//转整数

//函数定义  及参数获取
// 在浏览器的源码窗口中 ，加入 断点，进行断点调试,单步调试(F8 快捷键 在Opera中）

function sum()
{
  var apple=0;
  //循环语句的使用
  //内置变量arguments的调用
  for(var i=0;i<arguments.length;i++)
  {
    apple+=arguments[i];
  }
  //不加入return 没有返回值 则sumofmoney=undefined
  return apple;
}

var sumOfMoney=sum(1,3,4,5,6,2);
alert('1+2+3+4+5+6+2=='+sumOfMoney);


//注意转义字符
alert("D:\\Program Files \\ok\\fd");
alert('D:\\Program Files \\ok\\fd');


//匿名方法是需要着重学习的
//1
var ff = function()
{
  alert('go home!');
}
ff();

//一个易错点
//btn.onclick=function(){} onclick后面不能家少年宫()
//2
(function(n1,n2)
{
   alert(n1+n2);
}
)(1,2);
//3
onload=function()
{
  document.getElementById('btn').onClick=function()
  {
    alert("You are clicking  the button")
  }
};



//字符串的常用方法
alert('I am from the God'.length);
alert('I am from the God'.charAt(3));
alert('I am from the God'.charAt(9));
alert('I am from the God'.substring(3,9));
alert('I am from the God'.indexOf('t'));
var  names = 'I am from the God'.split(' ');
for(var name in names)
  {
    alert(name+'='+names[name]);
  }
for(var i=0;i<names.length;i++)
{
  alert(names[i]);
}

//杨老师练习1
alert('杨1');
window.onload=function()
{
  var buttons=document.getElementsByName('button1');
  for(var i=0;  i < buttons.length;i++)
  {
    //alert(buttons[i]   onclick不能写成onClick  大小写敏感
    buttons[i].onclick=function()
    {
        alert(i);
    };
  }
}
//杨老师练习2
alert('杨2');
function aa()
{
  alert('aaa');
  return function(){alert('bbb');};
}
alert(aa);
alert(aa());
// alert(aa());打印是aaa,和function(){alert("bbb");}以为执行函数方法题内容,返回的是一个匿名函数,返回的是函数体字符串. 
alert(aa()());
//alert(aa()());打印的是aaa,bbb,undefined ,先执行aa()，函数体内容aaa，返回字符串function(){alert("bbb");}，再执行function(){alert("bbb");}()，
//这时该方法名是：function(){alert("bbb");}，先打印bbb，在打印undefined，因为该方法名未定义， 
//杨老师练习3
alert('杨3');
//每隔1s重复的执行  烦人，注释掉
//setInterval(aa,1000);
//setInterval(aa(),1000);
//杨老师练习4
alert('杨4');
//setInterval(alert('a'),1000); //每隔1s显示a
//setInterval(function(){alert("a");},1000);  //返回一个函数 因为未被执行，这个函数没有任何东西
//杨老师练习5
alert('杨5');
var s1="aaa";
var s2=new String("aaa");
alert(s1 instanceof Object); //false
alert(s2 instanceof Object);//true
alert(s1 instanceof String);//false
alert(s2 instanceof String);// true
alert(typeof(s1));  //string
alert(typeof(s2)); //object
//杨老师练习6
alert('杨6');
var x=1;
var y=0;
var z=0;
function add(n)
{
  n=n+1;
  return n;
}
y=add(x);
//js只会用最新的函数定义
function add(n)
{
  n=n+3;
  return n;
}

z=add(x);
alert('y='+y+'z='+z);//y-4  z=4  只用最新的
//杨老师练习7
alert('杨7');
var add1=function(n)
{
   n = n+1;
   return n;
}
y=add1(x);
add1 =function(n)
{
  n= n +3;
  return n;
}
z= add1(x);
alert('y='+y+' z='+z);
//杨老师练习8
alert('杨8');
var s1="aaa"; // -------------------->是String类型 非对象
var s2= new String('aaa');
alert(s1 instanceof Object);
alert(s2 instanceof Object);
alert(s1 instanceof String);
alert(s2 instanceof String);
alert(typeof(s1));
alert(typeof(s2));


//杨老师练习9
alert('杨9');
alert('易错点： btn.onclick=function(){}  onclick后面别跟()')

</script>

</Head>

<Body>
  <Div>
    <Input type="button" name="button1" value="按钮1"/>
    <Input type="button" name="button1" value="按钮2"/>
    <Input type="button" name="button1" value="按钮3"/>
    <Input type="button" name="button1" value="按钮4"/>
    <Input type="button" name="button1" value="按钮5"/>
  </Div>
  <Div>
    <Input type="button" name="name" value="click" id="btn"/>
  </Div>
  <Div>
    <Br/>
    <Br/>
    <Br/>
    <A href="javascript:alert(new Date().toLocaleTimeString());">显示时间</A>
    </Br>
    <A href="javascript:void(0);">显示时间</A>
    </Br>
    <Input type="button" name="name" value="显示时间",onclick="alert(new Date().toLocaleTimeString());"/>
  <Div>
</Body>
</Html>
```

为什么jquery.js有那么多的  
name : function(){}, 的写法？ 他们是什么意思？


JQuery.js中出现如下的书写方式：

success：function(data){}
类似于function success(data){}
不过是面向对象的写法，把这个方法当成一个prototype对象


引入JSON对象
JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式，易于阅读和编写，同时也易于机器解析和生成。{a:"a"}这就是一
个JSON数据。 JSON数据是用键值对的形式存储的。冒号（:）前面的是键，冒号后面的是值。JSON,的每一个值之间可以用分号（;）隔
开。大的类可以用{}大括号包围他其中的值，集合可以用[]中括号，包围值
"名称/值"对的集合 不同语言中，它被理解为对象(object)，记录(record)，结构(struct)，字典(dictionary)，哈希表(hash table)，键列表(keyed list)等
值的有序列表 多数语言中被理解为数组(array)


使用：
JSON以一种特定的字符串形式来表示 JavaScript 对象。如果将具有这样一种形式的字符串赋给任意一个 JavaScript 变量，那么该变

量会变成一个对象引用.
比如：
您可以使用以下JSON形式来表示User对象：
{"UserID":11, "Name":"tht", "Email":"18039010◎qq.com"};
然后如果把这一字符串赋予一个JavaScript变量，那么就可以直接使用对象的任一属性了。

```html
<Html>
  <Head>
    <script type="text/javascript">
        var User={"UserId":11,"Name":"Ye Zhaoliang","Email":"977962857@qq.com"};
        alert(User.Name);

        //利用JSon对象方式  定义两个User对象
        var User1={"UserId":12,"Name":{"FirstName":"Ye","LastName":"Zhaoliang"},"Email":"977962857@qq.com"};
        var User2={"UserId":13,"Name":{"FirstName":"Ye","LastName":"Wangliang"},"Email":"977962859@qq.com"};
        alert(User1.Name.LastName+" love you!");

        //用数组存储一个用户列表
        var Users=[User1,User2];
        alert(Users.length);
        for(var i=0;i<Users.length;i++)
        {
          alert(Users[i].UserId+" "+Users[i].Name.LastName);
        }
    </script>
  </Head>
  <Body>

  </Body>
</Html>
```


由此引出面向对象的JavaScript编程。

![./image/java/object.gif][JSON对象的格式描述，用逗号分隔开多个对象，一个对象用{}组合，每个对象的属性用:分隔，冒号前面表示属性 ...]

![./image/java/array.gif][用[]表达多个数组元素，元素之间不分类型，用逗号分隔元素]




1: JS中单引号和双引号有什么区别？为什么在字符串中用单引号括起来，而不是双引号？
           一方面因为Html代码的属性值一般是使用双引号。
           JSON的数据格式一般是使用双引号的。

           再进行字符串拼接的一般原则是： 单引号的内部可包含双引号（如若包含则加转义字符），双引号的内部可包含单引号（如若包含则加转义字符）
所以字符串如果用单引号修饰就可以内部引用Html的属性。
比如
<Div class="global-notify" id="global-message">
var str='<Div class="global-notify" id="global-message">'
复制代码
更多的是考虑到前端，会使用HTML的属性值，而Html的属性值都是用双引号包裹，所以如果再用单引号括起来不需要进行转义。
另外可能就是大家习惯。
    

2：Javascript也有事件编程

button中的onclick="js代码"
        
<input type=“button” onclick=“ js代码" />
复制代码

href超链接的特殊的调用事件的方法：
         
<a href=“javascript:js代码”>热点文字</a>
复制代码

          只有超链接的href中的JavaScript中才需要加“javascript:”，因为它不是事件，而是把”javascript:”看成像“http:”、“ftp:”、“thunder://”、“ed2k://”、“mailto:”一样的网络协议，交             由js解析引擎处理。只有href中，这是一个特例。


3：数据类型
基本数据类型：
Boolean
Number   ---->小数和整数
        ----parseInt('')
        ----parseFloat('')
String
Null  ---->
Undefined --->未定义(未初始化)
               var ul ;    --->ul is undefined
                    typeof(ul) ---->undefined
                    typeof(null)---->null

      == 非严格等于
      ===严格等于
         null==undefined ----->true
         null===undefined----->false
          为什么？
          ===先判断类型是否一致，再判断值
           null对象是有着特殊意义的值，此时变量的值是"已知状态"(这和mysql的null不太一样，mysql直接就是不知道，那肯定就是无法判断）
引用数据类型：
Object ---->函数
          ----> new Date()
          ----> 数组    var apple=['shanxi','yantai'];
          ---->null 


方法：
instanceof ----->属于某种类型？
typeof(object) ------->是什么类型的？


4：javascript中没有方法重载

所以在运行时候会事先确定好函数，一般是最靠近调用函数的函数定义
        function f1()
        {
          alert('ell');
        }
        function f1(name)
        {
          alert(name);
        }
        // f1();  得到undefined  ，直接选择最新的当做 f1的函数
        //这点和java很不一样
        f1('fd'); //得到fd
复制代码


5：String  Boolean  Number 等可以拓展其方法，使用类的静态属性prototype进行设置
比如：
String.prototype.IseMaile = function ()
复制代码

6：js文件引入javascript的好处是?      1:多文档共享  js
      2:减少网络流量

  使用方法 ： <Link src="*.js" type="text/javascript"


存在疑问：Dom  and Bom的区别？

Broser object model (BOM)
Document object model(Dom)
Bom模型有window对象  ，而Dom没有。

Bom对象结构：

Window  --- document  ------authors
                                  ------forms
                                  ------images
                                 -------links
                                 -------location ...
            ---- frames
            ---- history
            ---- location
            ---- navigator
            ---- screen
根节点是：window
      window对象是Bom模型的核心，包含6个对象，document,frames,history,location,navigator,screen对象

Dom对象结构：

document  ------authors
                ------forms
                ------images
                -------links
                -------location...


根节点是:document




联系：
```html
document.write('test');
window.document.write('test');
```

![./images/java/dom.png][某个dom的树状节点层次图（可以进行实例化]



14.this事件
事件中的this。除了可以使用event.srcElement

在事件响应函数中，this表示发生事件的控件。

只有在事件响应函数才能使用this获得发生事件

的控件，在事件响应函数调用的函数中不能使用

（这里的this表示window对象），如果要使用则

要将this传递给函数或者使用event.srcElement

。(*)this和event.srcElement的语义是不一样

的，this就是表示当前监听事件的这个对象，

event.srcElement是引发事件的对象：事件冒泡

。


14.动态创建元素
Element(元素)
document.write只能在页面加载过程中才能动态

创建。
可以调用document的createElement方法来创建

具有指定标签的DOM对象，然后通过调用某个元素

的appendChild();方法将新创建元素添加到相应

的元素下。//父元素对象.removeChild(子元素

对象);删除元素。
createElement(‘element’);创建一个节点
appendChild(node); 追加一个节点
removeChild(node);移除一个节点
replaceChild(new,old);替换一个节点
insertBefore(new,参照);把节点加到前面（插

到某个节点前面）
方法：
属性：
firstChild
lastChild

15.getElementsByTagName() 方法可返回带有指

定标签名的对象的集合。
16.IE中body的事件范围
IE中如果在body上添加onclick、onmousemove等

事件响应，那么如果页面没有满，则 “body 中

最后一个元素以下（横向不限制）” 的部分是无

法响应事件的，必须使用代码在document上监听

那些事件，比如document.onmousemove = 

MovePic
document.body.onmousedown=function(){}
document.onmousedown=function(){}
注意加文档定义与不加文档定义的也不一样。
如果为整个文档注册事件可以使用：

document.onxxxx事件。
17.onmouseover事件会在鼠标指针移动到指定的

对象上时触发事件发生
element.setAttribute()        把指定属性设置或更

改为指定值。
18. dvObj.style.width='300px';
   dvObj.style.height='200px';
   dvObj.style.border='1px solid red';
加''的是因为给变量赋值必须是一个正确的类型

。数字就是数字字符串就是字符串
19.<p> 标签定义段落。
border:1px solid green; 边框：1像素，实心

，绿色
20.form对象
document.getElementById(‘btn1’).click()

。搜索引擎的，智能提示，点击后相当于点击了

“搜索”按钮。
常用：click(),focus(),blur();//相当于通过

程序来触发元素的单击、获得焦点以及失去焦点

的事件。
form对象是表单的Dom对象。
方法：submit()提交表单，但是不会触发

onsubmit事件。
实现autopost，也就是焦点离开控件以后页面立

即提交，而不是只有提交submit按钮以后才提交

，当光标离开的时候触发onblur事件，在onblur

中调用form的submit方法。
在点击submit后form的onsubmit事件被触发，在

onsubmit中可以进行数据校验，如果数据有问题

，返回false即可取消提交

21.不同浏览器的差异
<form> 标签用于为用户输入创建 HTML 表单。

表单能够包含 input 元素，比如文本字段、复选

框、单选框、提交按钮等等。表单还可以包含 

menus、textarea、fieldset、legend 和 

label 元素。表单用于向服务器传输数据。
onsubmit 事件会在表单中的确认按钮(Submit)被点击时发生。
submit() 提交表单。 是form中的一个方法
onsubmit 在提交表单之前调用。是一个事件





1: js的for循环的var i = 0的再认识
经过var定义的是一个全局变量
```html
        //一个重要错误   var i =0 其实就是定义了一个全局变量i = 0
        //所有的js都可以调用
        for(var i=0;i<Users.length;i++)
        {
          alert(Users[i].UserId+" "+Users[i].Name.LastName);
        }
```
也就是for之后alert(i)的值是 Users.length

所以可以在for之前直接定义 var i = 0;即可

2：以二进制读取，10进制输出
```html
        alert(parseInt('100',8));//以8进制
        alert(parseInt('100',2));// 以2进制读取，并10进制显示
        alert(parseInt('100',10));// 以10进制 默认方式
        alert(parseInt('100',16));// 以16进制

```
3： onload在页面加载完后在加载的部分js
```html
        //很重要  onload  只在页面加载完控件才执行，所以确保id有效！！
        //否则可能报错
        onload=function()
        {
          document.getElementById('btn').onclick=function()
          {
            alert('hello');
          };
        }
```
4：Html的事件在学习

```html
      <Input type="button" name="time" value="show current time" onclick="alert(new Date().toLocaleTimeString());"/>
      <!--会跳转到最前面   #z只是一个页面最前面的一个锚-->
      <A href="#" onclick="alert(new Date().toLocaleTimeString());">百度</A>
      <br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<!--直接在当前位置打印出时间： javascript: 类似于http://  ftp://-->
<A href="javascript:alert(new Date().toLocaleTimeString());">网易</A>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<A href="javascript:void(0);" onclick="alert(new Date().toLocaleTimeString());">新浪</A>
<A href>
```

