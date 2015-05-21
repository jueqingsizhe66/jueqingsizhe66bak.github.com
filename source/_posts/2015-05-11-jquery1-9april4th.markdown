---
layout: post
title: "JQuery1-9April4th"
date: 2015-05-11 14:58:44 +0800
comments: true
categories: jQuery
---
<!--more-->

1: jquery的加载顺序

IE中会有不同点
结论：jquery中使用$(function(){})即可！其他知道即可
```html
<Html>
  <Head>
    <script type="text/javascript" src="jquery-2.1.3.js">
    </script>
    <script type="text/javascript">

      //javascript
      //第四个加载
      //在IE中居然无法加载！！！
      //需要等待页面完全加载完毕才会触发
      onload=function()
      {
        //console的作用就是在浏览器的控制台 打印出结果
        console.log('javascript\'s onload');
        alert('javascript window.onload js ok 4');
      };
      //window object change into jQuery object
      //第五个加载
      $(window).load(function(){
        console.log('window onload');
        alert('$(window).load No 5');
      });

//第二个加载
      $(function()
        {
          console.log('dom ready='+$('#span').html());
          alert('$(function(){}niming hanshu! 2');
        });


      //Dom object change into Jquery Object
      // 很是奇怪 第一个加载
      //可以多次触发
      $(document).ready(function(){
        console.log('document ready')
        alert('$(document).ready Ok 1');
      });

      // THe most common  and most convininet
      //jQuery(function(){})
           //第三个加载  跟第二个一样的  按照前后顺序进行布置
      jQuery(function()
       {
        console.log('dom ready='+$('#span').html());
        alert(' jQuery(function(){}niming hanshu 3!')
       });

       function init()
       {
         console.log('body init');
       }

    </script>

  </Head>
  <Body onload="init()">
    
    <Span id="span"> Rupeng!</Span>
  </Body>
</Html>
```
2：Map 和each函数的使用
map：用于修改
each：用于遍历

map:  value index
each:  key  value
```
<Html>
  <Head>
    <script type="text/javascript" src="jquery-2.1.3.js"></script>
    <script type="text/javascript">
      
       var arr=[1,2,4,5,6];
       $.map(arr,function()
             {
               alert('element'+arguments[1]+'=='+arguments[0]);
             });
        //namely  ,we can write below
        $.map(arr,function(ele,index)
              {
                alert('element'+index+'=='+ele);
              });

        //return a  new array ==2*old_array
              var newArray=$.map(arr,function(ele,index)
               {
                 return ele*2;
               })

        //$.map is similar to lisp's (map ...) 
        // so we can get the two properties of array with $.map function

        //print new array
        alert(newArray);   


        var newArray2=$.map(arr,function(ele,index){
          return index>2?ele*2:ele;
        })

        alert(newArray2);


    </script>
    <script type="text/javascript">
      var dic={"name":"xiaoming","age":3,"gender":"1"};
      //each 主要用于遍历    map主要用于修改
      $.each(dic,function(key,value)
             {
               alert(key+'=='+value);
             })
    </script>
  </Head>
</Html>
```
3:jquery和jsDom的互转

1：jsDom -> jquery       把jsDom对象放在$()括号中即可
2： jquery -> jsDom
     两种方法
         a:  $().get(0)
         b: $()[0]
```html
<Html>
  <Head>
    <Title> Test the transformation between jQuery and Dom </Title>
    <script type="text/javascript" src="jquery-2.1.3.js"></script>

    <script type="text/javascript" >
    
    
      $(function(){

  /*
        document.getElementById('btn').onclick=function()
        {
          var dvObj=document.getElementById('dv');
          //方法1  Dom
          //dvObj.style.backgroundColor='black'; 
         
          //方法2 jQuery 方法
           //Dom object change into jQuery
//          var dvObjJquery=$(dvObj);
//          dvObjJquery.css('backgroundColor','yellow');

          // 把jquery对象转回到Dom对象
          var dvObjJquery=$(dvObj);
          //1  转回去dom的方法1
//          var dvObjAgain=dvObjJquery[0]; 
          //2  转回去dom的方法2
          var dvObjAgain=dvObjJquery.get(0); 
           dvObjAgain.style.backgroundColor='black'; 

        

        }
   */       

        //1  省略了  等号
        //2  省略了   Dom长长的函数名调用
        //2  取而代之的是简易的jQuery语法  利用链式法则
        $('#btn').click(function()
                        {
                          $('#dv').css('backgroundColor','purple');
                        });
      })
    </script>
  </Head>

  <Body>
    <Input type="button" id="btn" value="changeColor"/>
    <Div id="dv" style="width:300px; height:200px; background-color:green">
      hello
      </Div>
  </Body>
</Html>
```

4多重选择器
注意：中间不能有空格  否则没有反应 这是需要注意的
          text() 类似于jsDom的innerText()
          html()类似于 innerHtml()

```html
<Html>
  <Head>
    <script type="text/javascript" src="jquery-2.1.3.js"></script>

    <script type="text/javascript">
//      alert('hello')
      $(function()
        {
          $('#btn').click(function(){
//            alert('hello');
//            $('#cls').text('This is  a  layer');  // class selector


            //$('div#cls').css('backgroundColor','yellow');  // label with Id selector
//            $('div #cls').css('backgroundColor','yellow'); //中间不能有空格  否则没有反应 这是需要注意的
//   同样的标签加上lei也不是不能加上空格


            $('div#cls').text('InnerText方式添加');
            $('div#cls').html('<H1>InnerHTML方式添加</H1>');

            //标签选择器的隐式编程   隐式循环！ 这是一个jQuery的一个好处 implicit cycle
            $('p').text('Set all the p the value');
    
          });
        });
    </script>

    <style type="text/css">
       #cls
       {
         width:300px;
         height:200px;
         background-color:green;
       }
    </style>
  </Head>

  <Body>
    <input type="button" id="btn" value="change"/>
    <div id="cls">

    </div>
    <p class="name"></p>
    <p class="name"></p>
    <p class="name"></p>
    <p class="name"></p>
    <p class="name"></p>
    <p class="name"></p>
    <p class="name"></p>
    <p class="name"></p>
  </Body>
</Html>
```

5： 选择器  过滤器的使用
:first   first()
:last   last()
:eq()
:lt()
:gt()

注意：可不敢再把siblings()漏掉了！
```html
<html>
  <head>
    <title>  test  the start mark</title>
    <script type="text/javascript" src="jquery-2.1.3.js"></script>
    <script type="text/javascript">
      $(function(){
        //注意是tr  行
        $('#tb tr:first').css('backgroundColor','green').css('fontSize','50px');
        $('#tb tr:last').css('color','black');
        // 0-3之间  0 1 2 3
        $('#tb tr:gt(0):lt(3)').css('fontSize','25px');
        $('#tb tr:odd').css('backgroundColor','yellow');

        $('#tb tr').click(function(){
          //可不敢再犯这种错误了！ ！ siblings()  加上括号
          $(this).css('backgroundColor','red').siblings().css('backgroundColor','gray');
        })
      })
    </script>

    
   <style type="text/css">
      .cls
      {
        background-color:black;
      }
      td
      {
        border:1px solid red;
      }
    </style>
  </head>

  <body>
    <table id="tb" style="width:2\450px;height:20px;text-align:center; font-size:14px ;border:1px solid black;cursor:pointer">
      <tr>
        <td>Name</td>
        <td>Mark</td>
      </tr>
      <tr>
        <td>Chairman Mao</td>
        <td>89</td>
      </tr>
      <tr>
        <td>Chairman Hu</td>
        <td>79</td>
      </tr>
      <tr>
        <td>Chairman Deng</td>
        <td>99</td>
      </tr>
      <tr>
        <td>Chairman Xi</td>
        <td>98</td>
      </tr>
      <tr>
        <td>Chairman jiang</td>
        <td>76</td>
      </tr>

    </table>
  </body>
</html>
```
6：相对容器 和绝对容器

$('p div')  绝对容器  p的后代
$('div','p')  相对容器   所有在p标签中的 div
参考点不一样

注意
          //$('this').siblings.css('backgroundColor','');//可不敢再犯这个错误
          //this不需要加上单引号
          //为什么无法直接行t添加上 颜色？ 因为真正在表面显示的是td
          //$(this).siblings().css('backgroundColor','');

```html

<html>
  <head>
    <title>  test  the start mark</title>
    <script type="text/javascript" src="jquery-2.1.3.js"></script>
    <script type="text/javascript">
      $(function(){

        //绝对定位
        // $('div p')表示div下的p元素
        //相对定位
        //相对div元素的p标签
        //$('p','div') 表示在div层中的p元素

        $('#tb tr').click(function(){
          //$('this').siblings.css('backgroundColor','');//可不敢再犯这个错误
          //this不需要加上单引号
          //为什么无法直接行t添加上 颜色？ 因为真正在表面显示的是td
          //$(this).siblings().css('backgroundColor','');

          $('td',$(this).siblings()).css('backgroundColor','');
          $('td:even',$(this)).css('backgroundColor','red');
          $('td:odd',$(this)).css('backgroundColor','yellow');
        });

        //学习了.hover(,) 函数的使用方式
        //来的时候是第一个参数
        //去的时候是第二个参数
        $('tr').hover(function(){
          $(this).find('td').addClass('hover');
        },function(){
          $(this).find('td').removeClass('hover');
        });


        //l列的追踪
        $('td').hover(function(){
          var $index=$(this).index();
          $(this).addClass('hover');
          $('td:nth-child('+($index+1)+')').addClass('hover');
        },function(){
          $('#tb tr').children().removeClass('hover');
        });
        alert('Total rows:'+$('#tb tr').length);


      })
    </script>

    
   <style type="text/css">
     .hover
     {
       color:#fff;
       background-color:gray;
     }
      .cls
      {
        background-color:black;
      }
      td
      {
        border:1px solid red;
      }
    </style>
  </head>

  <body>
    <table id="tb" style="width:2\450px;height:20px;text-align:center; font-size:14px ;border:1px solid black;cursor:pointer">
      <tr>
        <td>First</td>
        <td>Second</td>
        <td>Third</td>
        <td>Fourth</td>
        <td>Fifth</td>
        <td>Sixth</td>
        <td>Seventh</td>
        <td>eighth</td>
        <td>nineth</td>
        <td>tenth</td>
      </tr>
      <tr>
        <td>First</td>
        <td>Second</td>
        <td>Third</td>
        <td>Fourth</td>
        <td>Fifth</td>
        <td>Sixth</td>
        <td>Seventh</td>
        <td>eighth</td>
        <td>nineth</td>
        <td>tenth</td>
      </tr>
      <tr>
        <td>First</td>
        <td>Second</td>
        <td>Third</td>
        <td>Fourth</td>
        <td>Fifth</td>
        <td>Sixth</td>
        <td>Seventh</td>
        <td>eighth</td>
        <td>nineth</td>
        <td>tenth</td>
      </tr>
      <tr>
        <td>First</td>
        <td>Second</td>
        <td>Third</td>
        <td>Fourth</td>
        <td>Fifth</td>
        <td>Sixth</td>
        <td>Seventh</td>
        <td>eighth</td>
        <td>nineth</td>
        <td>tenth</td>
      </tr>

    </table>
  </body>
</html>
```



一个有趣的分页


奇巧淫技

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
    <style type="text/css">
   .page
   {
       margin:5px;
   }
   .hover
   {
       background-color:#00f;
       color:#fff;
       cursor:hand;
   }
    </style>

     <script src="jquery-2.1.3.js" type="text/javascript"></script>
     <script type="text/javascript">

         $(document).ready(function () {
           //实际的效果是在先前的表格中进行隐藏和显示的小伎俩！ 奇巧淫技
           //get the rows
             var $rows = $('table tbody tr').length;
          // get the show lines  页面行数
             var $pagesize = 2;
          //总页数
             var $pagecount = Math.ceil($rows / $pagesize);
          //创建一个层
             var $div = $('<div id="pages"></div>');
          //在层中加入page
             for (var i = 0; i < $pagecount; i++) {
               //把创建的东西加入到层中
                 $('<span class="page">' + (i + 1) + '</span>').appendTo($div);
             }
             $div.appendTo('table');

             //appendd the hover
             //设置页面的数字的悬浮效果
             $('.page').hover(function () {
                 $(this).addClass('hover');
             }, function () {
                 $(this).removeClass('hover');
             });

             //增加了页面行的点击隐藏的事件
             $('table tbody tr').click(function(){
               $(this).hide();

             })
             //隐藏所有行
             $('table tbody tr').hide();
             //获得所有的tr行对象
             var tr = $('table tbody tr');
             for (var i = 0; i < $pagecount - 1; i++) {
                 $(tr[i]).show();
             }

             //设置页面的点击事件 效果
             $('span').click(function () {
                 $('table tbody tr').hide();
                 for (i = ($(this).text() - 1) * $pagesize; i <= $(this).text() * $pagesize - 1; i++) {
                     $(tr[i]).show();
                 }
             })
         });
     </script>
</head>
<body>
<table border="1" width="200px">
<thead>
<tr>
<th>ID</th><th>Name</th><th>Mark</th>
</tr>
</thead>
<tbody>
<tr><td>100</td><td>Steven</td><td>100</td></tr>
<tr><td>101</td><td>Mike</td><td>70</td></tr>
<tr><td>102</td><td>Robot</td><td>80</td></tr>
<tr><td>103</td><td>Perry</td><td>100</td></tr>
<tr><td>104</td><td>Lion</td><td>90</td></tr>
<tr><td>105</td><td>Andy</td><td>85</td></tr>
</tbody>

</table>
</body>
</html>
```





一个小练习： 东西部篮球队

注意：
       //可不敢再犯这个错误   1  逗号写成了句话   2： siblings后直接街上css的参数
          //$(this).css('backgroundColor'.'red').siblings()'backgroundColor','');

          //由此错误可得到一个结论
          // jQuery分成两个部分一个是获得Dom对象  紧接着是设置CSS样式！
          // 也就是js其实就是把这两个部分按照链条一样一对一对的串起来了
          //一个Dom一个Css  然后再一个Dom 一个CSS


```html
<Html>
  <Head>
    <Meta charset="utf-8">
    <script type="text/javascript" src="jquery-2.1.3.js"></script>

    <script type="text/javascript">
      $(function(){
        $('ol li').mouseover(function(){
          //可不敢再犯这个错误   1  逗号写成了句话   2： siblings后直接街上css的参数
          //$(this).css('backgroundColor'.'red').siblings()'backgroundColor','');

          //由此错误可得到一个结论
          // jQuery分成两个部分一个是获得Dom对象  紧接着是设置CSS样式！
          // 也就是js其实就是把这两个部分按照链条一样一对一对的串起来了
          //一个Dom一个Css  然后再一个Dom 一个CSS
          $(this).css('backgroundColor','red').siblings().css('backgroundColor','');
        });
        $('ol li').click(function()
           {

             //第一种写法
//             $(this).prevAll().css('backgroundColor','blue');
//             $(this).nextAll().css('backgroundColor','yellow');

             //第二种写法
             //end的作用就是从前面所有 又跳回到$(this)的作用！！关键点是end() 这样才可以帮助我们就你行链式编程
             $(this).prevAll().css('backgroundColor','blue').end().nextAll().css('backgroundColor','yellow');
           })
      });
    </script>

  </Head>

  <Body>
    <Ol id="east">
      <Li>亚特兰大 老鹰</Li>
      <Li>芝加哥 公牛</Li>
      <Li>波士顿 凯尔特人</Li>
      <Li>夏洛特 黄蜂</Li>
      <Li>克利夫兰 骑士</Li>
      <Li>布鲁克林 篮网</Li>
      <Li>迈阿密 热火</Li>
      <Li>底特律 活塞</Li>
      <Li>纽约 尼克斯</Li>
      <Li>奥兰多 魔术</Li>
      <Li>印第安纳 步行者</Li>
      <Li>费城 76人</Li>
      <Li>华盛顿 奇才</Li>
      <Li>密尔沃基 雄鹿</Li>
      <Li>多伦多 猛龙</Li>
    </Ol>
    <Ol id="west">
      <Li>金州 勇士</Li>
      <Li>丹佛 掘金</Li>
      <Li>达拉斯 小牛</Li>
      <Li>洛杉矶 快船</Li>
      <Li>明尼苏达 森林狼</Li>
      <Li>休斯顿 火箭</Li>
      <Li>洛杉矶 湖人</Li>
      <Li>奥克兰陈马成 雷霆</Li>
      <Li>孟菲斯 灰熊</Li>
      <Li>菲尼克斯 太阳</Li>
      <Li>波特兰 开拓者</Li>
      <Li>新奥尔良 鹈鹕</Li>
      <Li>萨克拉门托 国王</Li>
      <Li>犹他 爵士</Li>
      <Li>圣安东尼奥 马刺</Li>
    </Ol>
    <Ul id="other">
      <Li>步行者</Li>
      <Li>山猫</Li>
    </Ul>
  </Body>
</Html>
```
7：过滤器的练习

注意： 
   even和odd选择器     index从0开始。

```html
<html>
  <head>
    <title>  test  the start mark</title>
    <script type="text/javascript" src="jquery-2.1.3.js"></script>
    <script type="text/javascript">
      $(function(){
        $('#btnFirst').click(function(){
          $('div:first').css('backgroundColor','green').siblings().css('backgroundColor','blue');
        });
        $('#btnFirst2').click(function(){
          $('div').first().css('backgroundColor','red').siblings().css('backgroundColor','blue');
        });
        $('#Last').click(function(){
          $('div:last').css('backgroundColor','purple').siblings().css('backgroundColor','blue');
        });
        $('#Last2').click(function(){
          $('div').last().css('backgroundColor','red').siblings().css('backgroundColor','blue');
        });

        $('#eq2').click(function(){
          $('div:eq(2)').css('backgroundColor','black').siblings().css('backgroundColor','blue');
        });
        $('#lt2').click(function(){
          //范围性内容 不能用siblings
          //$('div:lt(2)').css('backgroundColor','black').siblings().css('backgroundColor','blue');
          $('div:lt(2)').css('backgroundColor','black');
        });
        $('#gt2').click(function(){
        // $('div:gt(2)').css('backgroundColor','red').siblings().css('backgroundColor','blue');
          $('div:gt(2)').css('backgroundColor','red');
        });
        //下标从0开始  index 所以表面上even是第一个元素 
        $('#even2').click(function(){
         // $('div:even').css('backgroundColor','red').siblings().css('backgroundColor','blue');
          $('div:even').css('backgroundColor','red');
        });
        $('#odd2').click(function(){
          //$('div:odd').css('backgroundColor','red').siblings().css('backgroundColor','blue');
          $('div:odd').css('backgroundColor','red');
        });
        // 当然也是可以组合起来  $('div:gt(3):lt(2)')
        // $('div:not(.cls)')
        $('#olivenot').click(function(){
          //必须加上. 句话才是有效的
          $('div:not(.cls)').css('backgroundColor','yellow');
        });
      $('#header1').click(function(){
        $('h1,h2,h3,h4,h5,h6').css('backgroundColor','gray');
      });
      $('#header2').click(function(){
        $(':header').css('backgroundColor','yellow');
      });



      });
    </script>
    <style type="text/css">
      div
      {
        width:150px;
        height:30px;
        margin-bottom:10px;
        background-color:blue;
      }
      td
      {
        border:1px solid red;
      }
      .cls
      {
           background-color:olive;
      }
    </style>
  </head>


  <body>
    <Input type="button" id="btnFirst" value="divColorToGreenFirst"/>
    <Input type="button" id="btnFirst2" value="divColorToGreenFirst2"/>
    <Input type="button" id="Last" value="divColorToGreenLast"/>
    <Input type="button" id="Last2" value="divColorToGreenLast2"/>
    <Input type="button" id="gt2" value="divColorToGreenGreater2"/>
    <Input type="button" id="eq2" value="divColorToGreenEqual2"/>
    <Input type="button" id="lt2" value="divColorToGreenLess2"/>
    <Input type="button" id="even2" value="divColorToGreenEven"/>
    <Input type="button" id="odd2" value="divColorToGreenOdd"/>
    <Input type="button" id="olivenot" value="not olive"/>
    <Input type="button" id="header1" value="header1"/>
    <Input type="button" id="header2" value="header2"/>
    <Div></Div>
    <Div></Div>
    <Div class="cls"></Div>
    <Div></Div>
    <Div></Div>

    <H1>Header 1</H1>
    <H2>Header 2</H2>
    <H3>Header 3</H3>
    <H4>Header 4</H4>
    <H5>Header 5</H5>
    <H6>Header 6</H6>
      
  </body>
</html>
```


一个有趣的slideup and down 小案例：


```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
    <style type="text/css">
    </style>
     <script src="jquery-2.1.3.js" type="text/javascript"></script>
     <script type="text/javascript">
         $(document).ready(function () {
             $('#message1').css({ 'backgroundColor': '#f00', 'text-align': 'center','width':'200px' }).hide();
             $("#message2").css({ 'backgroundColor': 'gray', 'text-align': 'center','width': '200px' }).click(function () {
                 $(this).slideUp();
                 $("#message1").slideDown();
             });
         });
     </script>
</head>
<body>
<p id="message1">Welome to My Blog</p>

<p id="message2">Welcome to My Facebook</p>
</body>
</html>
```


8： 评分和addClass

.end()断链的使用，之所以要断链也是因为返回来的是集合，而不是原先的对象
注意：
      //测试开关灯
      //隐含着规律是：jquery获得的都是集合的元素
    //做判断一定得是 $(function(){})里面中

五角星是直接输入，而不是通过图片

```html
<html>
  <head>
    <title>  test  the start mark</title>
    <script type="text/javascript" src="jquery-2.1.3.js"></script>
    <script type="text/javascript">

      // 测试评分
      $(function(){
        $('#tb td').mouseover(function(){
            //$(this).prevall().css('backgroundcolor','red');
            $(this).text('○').prevall().text('○').end().nextall().text('☆');
        });
        $('#tb td').mouseout(function()
        {
          $('#tb td').text('☆');
        });
      });
    </script>

    
    <script type="text/javascript">
      //测试开关灯
      //隐含着规律是：jquery获得的都是集合的元素
    //做判断一定得是 $(function(){})里面中
    $(function(){
      if($('#btn').length>0)
        {
          alert('存在id=#tb的标签');
        }else
        {
          alert('不存在id=#tb的标签');
        }
        $('#btnopen').click(function(){
          $('body').addclass('cls');
        });
        $('#btnclose').click(function(){
          $('body').removeclass('cls');
        });
        $('#btncloseopen').click(function(){
          if($('body').hasclass('cls'))
            {
                $('body').removeclass('cls');
            }else
            {
                $('body').addclass('cls');

            }
        });
        $('#btncloseopen2').click(function(){
            $('body').toggleclass('cls');
        });

    });


    </script>
    <style type="text/css">
      .cls
      {
        background-color:black;
      }
      td
      {
        border:1px solid red;
      }
    </style>
  </head>

  <body>
    <table id="tb" style="width:250px;height:20px;text-align:center; font-size:20px ;border:1px solid black;cursor:pointer">
      <tr>
        <td>☆</td>
        <td>☆</td>
        <td>☆</td>
        <td>☆</td>
        <td>☆</td>
      </tr>
    </table>
    <input type='button' name="hello" value="hello" id="btn"/>
    <input type='button' name="open" value="turn on the light" id="btnopen"/>
    <input type='button' name="close" value="turn off the light" id="btnclose"/>
    <input type='button' name="openclose" value="turn off|on the light" id="btncloseopen"/>
    <input type='button' name="openclose2" value="turn off|on the light2" id="btncloseopen2"/>
  </body>
</html>
```



两个综合实验：

case1: 数组 练习join()
append()
sort()
splice()
hover()悬停事件
concat()


```html
<Html>
  <Head>
    <Title>  test  the start mark</Title>
    <script type="text/javascript" src="jquery-2.1.3.js"></script>

    <!--1 touppercase-->
    <style type="text/css">
      .hover
      {
          background-color:red;
          font-size:24px;
      }
    </style>
    <script type="text/javascript">
      $(function(){
        //核心：touppercase
        // Good!  Let the array become beautiful
        var namePostion=$('#names'); 
        var Member=['basket','foot','tennis','running','pingpong','net'];
        // map() 迭代数组中每个元素  并为每个元素分别调用一次回调函数
        Member =$.map(Member,function(value,index){
          return(index+1+'.'+value.toUpperCase());
        });
        $.each(Member,function(index,value){
          namePostion.append($('<Li class="sport">'+value+'</Li>'));
        });

        //使用了悬停事件
        $('.sport').hover(function(){
           //   alert('ok');
              $(this).addClass('hover');
        },function(){
              $(this).removeClass('hover');
           //   alert('byel');
        });

      })
    </script>
    <!--2 grep-->
    <style type="text/css">
      #All
      {
        background-color:green;

      }
      #Part
      {
        background-color:gray;
      }
    </style>
    <script type="text/javascript">
      $(function(){
        var Member=['basket','foot','tennis','running','pingpong','net'];
        //一定要注意在p和#All不要加入空格
        // join通过<br>来划分数组中的元素
        $('p#All').append(Member.join('<br/>'));
        //grep() 分析数组的所有元素，把不想要的元素过滤掉。
        Member = $.grep(Member,function(value){
          return value.length > 6;
        });
        $('p#Part').append(Member.join('<br/>'));
      });


    </script>

    <!--3 join sort-->

    <style type="text/css">
      
      .All
      {
        background-color:red;
      }
      .Part
      {
        background-color:gray;
      }
      .AllNumber
      {
        background-color:blue;
      }
      .PartNumber
      {
        background-color:lime;
      }
    </style>



    <script type="text/javascript">
      $(function(){
        var Member=['basket','foot','tennis','running','pingpong','net'];
        //重复的一个练习
        $('p.All').append(Member.join('<Br>'));
        $('p.Part').append(Member.sort().join('</Br>'));

        var Number1 = [12,43,2,34,6,34];
        $('p.AllNumber').append(Number1.join('<Br/>'));

        // a b 两个值 进行比较  大的那个放在后排
        Number1 =  Number1.sort(function(a,b){
          return a-b;
        });
        $('p.partNumber').append(Number1.join('<Br/>'));
        
      });

    </script>

    <!--4 splice-->
    <style type="text/css">
     
      .Part1
      {
        background-color:lime;
      }

      .Remain
      {
        background-color:red;
      }
    </style>



    <script type="text/javascript">

      $(function(){
        var Member=['basket','foot','tennis','running','pingpong','net'];
        //第一个参数为索引开始   第二个参数为删除的个数     splice作用返回前两个成员到Filter数组
        var Filter=Member.splice(0,2);
        $('p.Part1').append(Filter.join('<Br/>'));
        $('p.Remain').append(Member.join('<Br/>'));
      })

    </script>

    <!--5  concat -->
    <style type="text/css">
    
      .First
      {
        background-color:lime;
      }
      .Last
      {
        background-color:gray;
      }
      .Whole
      {
        background-color:red;
      }
    </style>



    <script type="text/javascript">
      $(function(){
        var first=['basket','foot','tennis'];
        var last=['running','pingpong','net'];
        $('p.First').append(first.join('<Br/>'));
        $('p.Last').append(last.join('<Br/>'));
        var whole=first.concat(last);
        $('p.Whole').append(whole.join('<Br/>'));
      });

    </script>


    <!--6  Json sort-->
    <style type="text/css">
      .All1
      {
        background-color:green;
      }

    
    </style>



    <script type="text/javascript">
      $(function(){
        var students=[{Name:'Qianqian',Role:'Administrator',Age:'45'},
          {Name:'Lilei',Role:'Programmer',Age:'15'},
          {Name:'Dongyuliang',Role:'Seller',Age:'25'},
          {Name:'LiFeng',Role:'manager',Age:'25'},
          {Name:'zhaoli',Role:'Architector',Age:'35'}
        ];

        //有点类似 Java中的treeset的作用  需要设置一个比较器
        students=students.sort(function(studnetA,studentB){
          if(studnetA.name>studentB.name){
            return 1;
          }else if(studnetA.name < studentB.name)
          {
            return -1;
          }else
          {
            return 0;
          }
        });
        $.each(students,function(index,value){
          $('p.All1').append('<Tr><Td>>'+value.Name+'</Td><Td>'+value.Role+'</Td><Td>'+value.Age+'</Td></Tr>');
        });
      });

    </script>

  </Head>

  <Body>
    <p id="names"></P>

    <hr/>
    <H3> Member Name</H3>
    <P id="All"></P>
    <H3> Part Name</H3>
    <P id="Part"></P>
    <Hr/>

    <H3>Not sort string array</H3>
    <P class="All"></P>
    <H3> sort string array</H3>
    <P class="Part"></P>
    <H3>Not sort number</H3>
    <P class="AllNumber"></P>
    <H3> sort number</H3>
    <P class="PartNumber"></P>

    <Hr/>
    <H3>get the  Filer array</H3>
    <P class="Part1"></P>
    <H3>get the remain array</H3>
    <P class="Remain"></P>

    <Hr/>
    <H3>Get the First part</H3>
    <P class="First"></P>
    <H3>Get the Last part</H3>
    <P class="Last"></P>
    <H3>Get the Whole part</H3>
    <P class="Whole"></P>


    <Hr>
    <H3>Member Info</H3>
    <P class="All1"></P>


  </Body>

</Html>
```


case2: 事件触发


bind()
unbind()
trigger()
多类的设置 class="  "

```html
<Html>
  <Head>
    <Title>  test  the common jQuery event </Title>
    <script type="text/javascript" src="jquery-2.1.3.js"></script>


    <!--1  trigger   bind  unbind-->
    <style type="text/css">
      .Red
      {
          color:red;
      }
      .Green
      {
        color:green;
      }
      .unbind
      {
        color:lime
      }
      .buttons
      {
        width:150px;
        height:20px;
        margin:5px;
        border:2px solid;
        float:left;
        font-weight:bold;
        background-color:gray;
      }
      .Down
      {
        color:green;
      }
      .Up
      {
        color:red;
      }
      .Over
      {
        color:lime;
      }
      .Mouses
      {
        width:150px;
        height:20px;
        margin:5px;
        border:2px solid;
        float:left;
        font-weight:bold;
        background-color:gray;
      }

    </style>
    <script type="text/javascript">
      //可不敢再犯这个错误
      //$('.button').bind('click',function(){
    $(function(){
       $('.buttons').bind('click',function(){
        alert('You have clicked '+$(this).text()); //innerText()
        // trigger without click !!!  automated;
      //可不敢再犯这个错误
        //$(.Green).trigger('click');
       // $('.Green').trigger('click');

       // unbindf test
        $('.Unbind').bind('click',function(){
          alert('You have clicked the'+$(this).text()+'<Br/>After that the button will unbind the click event!');
          $('.Unbind').unbind('click');

        })
      });
       
      //mouse event
      $('.Mouses').bind('mousedown',function(){
          alert('your mouse have been pressed down'+$(this).text());
      });

      $('.Mouses').bind('mouseup',function(){
          alert('your mouse have been pressed up'+$(this).text());
      });

      $('.Mouses').bind('mouseover',function(){
          alert('your mouse have been pressed over'+$(this).text());
      });


      //focus test
      $('#btn').focus(function(){
        alert('ol');
        $('#btn').html('<Font color="red"><Strong>Focus</Strong></Font>');
      });
      $('#btn').blur(function(){
        alert('fuck');
        $('#btn').html('<Font color="gray"><Italic>Focus</Italic></Font>');
      });

       
    });
    </script>



  </Head>

  <Body>
    <H3>Test bind and trigger</H3>
    <!--Multiple classes !!!　Take Note-->
    <Span class="Red buttons">Red</Span>
    <Span class="Green buttons">Green</Span>
    <Span class="Unbind buttons">Unbind it</Span>
    <Br/>
    <hr/>
    <Br/>
    <H3>Test bind and trigger</H3>
    <!--Multiple classes !!!　Take Note-->
    <Span class="Mouses Down">Mouse Down</Span>
    <Span class="Mouses Up">Mouse Up</Span>
    <Span class="Mouses Over">Mouse over</Span>

    <Br/>
    <hr/>
    <Br/>
    <H3>Test focus and blur</H3>
    <!--     Input cannot  html  -->
    <Input type="text" name="mouse" id="btn" values="focus"/>

    </Div>
    


  </Body>

</Html>
```


