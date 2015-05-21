---
layout: post
title: "jQuery10-18节笔记代码April5th"
date: 2015-05-11 14:58:44 +0800
comments: true
categories: jQuery
---
<!--more-->
http://www.rupeng.com/forum/thread-45422-1-1.html jquery1-9
1：移除元素 
$('div').empty()  清空层中的元素
$('div').remove('.cls')   清空层中运用cls样式的元素
$('div').remove()  自我删除
2: select 下拉框的练习
单选移到另一个框
全选移到另一个框
option:selected表示已选中的元素
:selected表示所有选中的下列框元素，前提是select设置了 multiple="multiple"

案例1：（单选和全选）

```html
<Html>
  <Head>
    <Title>Test select and option </Title>
    <script type="text/javascript" src="jquery-2.1.3.js" ></script>
    <script type="text/javascript">
      $(function(){
        $('#btnLeft').click(function(){
          $('#sel2 option:selected').appendTo('#sel1');
          //$('#sel1 option:selected').appendTo($('#sel2'));
        });
        $('#btnRight').click(function(){
          $('#sel1 option:selected').appendTo('#sel2');
        });

        $('#btnAllRight').click(function(){
          $('#sel1 option').appendTo('#sel2');
        });

        $('#btnAllLeft').click(function(){
          $('#sel2 option').appendTo('#sel1');
        });

      })
    </script>
  </Head>


  <Body>
    <Select id="sel1" multiple="multiple" style="float:left; width:400px;height:100px;">
      <!--Multipel can show all the contains in the selector-->
      <Option > delete</Option>
      <Option > create</Option>
      <Option > check</Option>
      <Option > update</Option>
      <Option > select</Option>
    </Select>
    <!--In the div, the width:50px  then inside the div
    is 50px width,so it change the line!
    And because the float:left ,so it will try to align
    along the left-->
    <Div style="float:left;width:50px">
      <Input type="button" id="btnLeft" value="<" style="width:50px"/>
      <Input type="button" id="btnRight" value=">" "width:50px"/>
      <Input type="button" id="btnAllLeft" value="<<" "width:50px"/>
      <Input type="button" id="btnAllRight" value=">>" "width:50px"/>

    </Div>
    <Button type="button" id="btnLeft1" value="To the left">"To the left"</Button>
    <Button type="button" id="btnRight1" value="To the right"/>"To the right"</Button>
    <Button type="button" id="btnAllLeft1" value="All To the left"/>"All To the left"</Button>
    <Button type="button" id="btnAllRight1" value="All To the left"/>"All To the left"</Button>
    <!--Because of the float:left,so selector will focuse to
        take occupy of the left position-->
    <Select id="sel2" multiple="multiple" style="float:left; width:400px;height:100px;">
      <!--<Select id="sel2" multiple="multiple">-->
    </Select>


  </Body>

</Html>
```
案例2：计算器

使用switch ...case 判断选中的计算器操作符
注意：输入参数的有效性的判断。

```html
<Html>
  <Head>
    <Title>Test Calculator with the basic operator</Title>
    <script type="text/javascript" src="jquery-2.1.3.js" ></script>
    <script type="text/javascript">
      $(function(){
        $('#equal').click(function(){
          var leftvalue=$('#one').val();
          var rightvalue=$('#two').val();
          if(leftvalue=='')
          {
            console.log('The empty of the one!;');
            return;
          }
          if(rightvalue=='')
          {
            console.log('The empty of the two!;');
            return;
          }
          var result=0.0;
          //alert($('#operator :selected').val());
          //alert(typeof($('#operator :selected').val()));
          switch($('#operator :selected').val())
          {
            case '+':
              result=parseFloat(leftvalue)+parseFloat(rightvalue);
              break;
              case '-':
result=parseFloat(leftvalue)-parseFloat(rightvalue);
              break;
             case '*':
result=parseFloat(leftvalue)*parseFloat(rightvalue);
              break;
             case '/':
result=parseFloat(leftvalue)/parseFloat(rightvalue);
              break;

              default:
                break;
          }
          $('#result').val(result);
        })
      })
    </script>

  <Body>
    <Input type="text" id="one"/>
    <Select id="operator" style=" width:40px;height:30px;">
      <Option>+</Option>
      <Option>-</Option>
      <Option>*</Option>
      <Option>/</Option>
    </Select>
    <Input type="text" id="two"/>
    <Input type="button" id="equal"value="="/>
    <Input type="text" id="result"/>
  </Body>
</Html>
```

4: checkbox的练习

:disabled 属性的练习

案例1： 软件安装时需要提醒用户阅读协议，一般是5s，然后才可以点击同意。

注意：计时器setInterval和清除计时器clearInterval() .  回顾： setTimeout 和 clearTimeout

```html
<Html>
  <Head>
    <Title>Test Time machine </Title>
    <script type="text/javascript" src="jquery-2.1.3.js" ></script>
    <script type="text/javascript">
      $(function(){
        var i =5;
        var timeMeter=setInterval(function(){
          if(i>0)
          {
            i--;
            $('#span1').text('Please read the agreement carefully('+i+'s)');
          }else
          {
            //forget clear The time meter
            clearInterval(timeMeter);
            $('#span1').text('Agree');
            $('#chk').attr('disabled',false);
          }
        },1000);
      })
    </script>
  </Head>

  <Body>
    <Input type="checkbox" id="chk" disabled="disabled"><Span id="span1">Please read the agreement carefully(5s)</Span>
  </Body>

</Html>

            $('#span1').text('Agree');
            $('#chk').attr('disabled',false);
          }
        },1000);
      })
    </script>
  </Head>

  <Body>
    <Input type="checkbox" id="chk" disabled="disabled"><Span id="span1">Please read the agreement carefully(5s)</Span>
  </Body>

</Html>
```
5:事件冒泡

取消事件冒泡的方法 
arguments[0].stopPropagation 或者
function(e){e.stopPropagation();}




附加上图片的脱离文档流操作和一次事件

但是one在jquery-1.8.3.min.js 能够试验通过 ，在2.1.3.js新版本则不行。

```html
<Html>
  <Head>
    <Title>Test select and option </Title>
    <script type="text/javascript" src="jquery-2.1.3.js" ></script>
    <script type="text/javascript">
      $(function(){
        $('#btn').click({'name':'xiaomin'},function(e){
          alert(e.data.name) ;
        });
        $('#dv').click(function(){
          alert($(this).attr('id'));
        });

        $('#ps').click(function(){
          alert($(this).attr('id'));
        });
        $('#sp1').click(function(){
          alert($(this).attr('id'));
          //如果加上这句话，事件就不会冒泡了
          //可不敢露了一个s
          //$(argument[0]).stopPropagation();//取消底层的事件冒泡
          arguments[0].stopPropagation();//取消底层的事件冒泡
          //方法2  function(e){e.stoppropagation();}
        });


        //Test Image
        $(document).mouseover(function(e){
          //1 脱离文档流
          //2 给定坐标
          $('img').css('position','absolute').css({"left":e.pageX,"top":e.pageY});
        });

        //一次事件
        $('#btnOnce').one(function(){
          alert('烟花总是灿烂，但是只开一次');
        });
      });
    </script>

    </Head>

  <Body>
    <Input type="button" name="name" value="getData" id="btn"/>
    <Div id="dv" style="width:300px; height:200px; border:1px solid red;">
      <P id="ps" >
        <Span id="sp1" style="width:200px; height:100px; border:1px solid yellow"> This is zone of span</Span>
      </P>
    </Div>

    <Input type="button" id="btnOnce" value="onceEvent"/>
    
    <Img src="2.png"/>
  </Body>

</Html>
```

7: replaceAll  and replaceWith   



repalceAll  把左边的标签替换包含右边文本的标签

replaceWith 把右边的标签替换左边文本的标签

        $('#btn').click({'name':'xiaomin'},function(e){
          alert(e.data.name) ;




7.1  标签包裹

1 wrap   每个p标签各自被一个 font包裹
2 wrapAll  所有p标签被一个font 包裹
3 wrapInner 每个P标签内部都有意个font标签


7.2 百度搜索框

focus   blur

以及removeClass的操作



7.3 RadioButton 的选择 :checked 

通过attr('checked',true);进行设置




```html
<Html>
  <Head>
    <Title></Title>
    <script type="text/javascript" src="jquery-2.1.3.js">
    </script>

    <script type="text/javascript">
      $(function(){
        //with all label
        $('#btn1').click(function(){
          $('br').replaceWith('<Hr/>')
        });

        //replaceAll with not all labels
        $('#btn2').click(function(){
          $('<br/>').replaceAll('hr');
        });

        $('#btn3').click(function(){
          //1wrap   每个p标签各自被一个 font包裹
          //2wrapAll  所有p标签被一个font 包裹
          //3wrapInner 每个P标签内部都有意个font标签
          //$('div p').wrap('<Font color="red" size="8" ></Font>');
          //$('div p').wrapAll('<Font color="red" size="8" ></Font>');
          $('div p').wrapInner('<Font color="red" size="8" ></Font>');
        });

        //你可以通过浏览器的探测功能，观察每一个过程html代码结构的
        //变化
        //下面代码适应所有的html文件！！！
        //只有body里面有东西都是可以的
        // 回顾： 聚焦控件联系    其实还包括focus and blur的联系
        /// 另外这些知识点统归为jQuery的时间编程
        /*
        $('body *').mouseover(function(){
          $(this).addClass('cls');
        }).mouseout(function(){
          $(this).removeClass('cls');
        });
        */


        //focus and blur
        //回顾了 事件编程 包括click  mouseover  mouseout hover悬停事件等

        // 百度搜索框的jquery实现
        //var contents=$(this).val();
        $('#baidu001').focus(function(){

          if($(this).val() =='Please input some value')
          {
            // 作用是如果当前的值为这个，那我就证明需要我输入什么
            //于是我就removeClass 然后开始写上值
            $(this).val('').removeClass('baidu');
          }
        }).blur(function(){
          if($(this).val().length==0)
          {
            //如果用户没有填写任何东西，就主动给他附上值
            $(this).val('Please input some value').addClass('baidu');
          }
        });

        $('#btnRadio').click(function(){
//          $(':radio[value=1]').attr('checked',true);
//          $('input[value=1]').attr('checked',true);
//          $('input[type=radio][value=1]').attr('checked',true);
            //$(':radio').val(['1','2','6']);  必须一一对应才会被选中？
            //$(':radio').val(['1','2','3']);
          $(':radio').val(['4','2','3']);// 哪个不对应上 则不选择哪个

          //也就是radiobutton是按照alue值来进行区分的


        })

      });
    </script>
    <style type="text/css">
      .cls
      {
        background-color:red;
      }
      .baidu
      {
        background-color:gray;
      }
      div
      {
        width:300px;
        height:200px;
        background-color:green;
      }
    </style>

  </Head>
  <Body>
    <Input type="button" id="btn1" value="with"/>
    <Input type="button" id="btn2" value="All"/>

    <P>Good morning 1</P>
    <Br/>
    <P>Good morning 2</P>
    <Br/>
    <P>Good morning 3</P>
    <Br/>
    <P>Good morning 4</P>
    <Br/>

    <Input type="button" id="btn3" value="packet with one font"/>
    <Div id="dv">
      <P> Good Afternoon</P>
      <P> Good Afternoon</P>
      <P> Good Afternoon</P>
      <P> Good Afternoon</P>
      <P> Good Afternoon</P>
    </Div>
    <Input type="text" id="baidu001" class="baidu" value="Please input some value">

    <Input type="button" id="btnRadio" value="selectTheRadio"/>
    <Input type="radio" name="1" value="1"> Man
    <Input type="radio" name="2" value="2"> Woman
    <Input type="radio" name="3" value="3"> Secret
  </Body>
</Html>
```

8:checkbox事件bind



属性选择器

attr 和prop的区别和联系：


```javascript
attr: function( elem, name, value, pass ) { 
var ret, hooks, notxml, 
nType = elem.nodeType; 
// don't get/set attributes on text, comment and attribute nodes 
if ( !elem || nType === 3 || nType === 8 || nType === 2 ) { 
return; 
} 
if ( pass && jQuery.isFunction( jQuery.fn[ name ] ) ) { 
return jQuery( elem )[ name ]( value ); 
} 
// Fallback to prop when attributes are not supported 
if ( typeof elem.getAttribute === "undefined" ) { 
return jQuery.prop( elem, name, value ); 
} 
notxml = nType !== 1 || !jQuery.isXMLDoc( elem ); 
// All attributes are lowercase 
// Grab necessary hook if one is defined 
if ( notxml ) { 
name = name.toLowerCase(); 
hooks = jQuery.attrHooks[ name ] || ( rboolean.test( name ) ? boolHook : nodeHook ); 
} 
if ( value !== undefined ) { 
if ( value === null ) { 
jQuery.removeAttr( elem, name ); 
return; 
} else if ( hooks && "set" in hooks && notxml && (ret = hooks.set( elem, value, name )) !== undefined ) { 
return ret; 
} else { 
elem.setAttribute( name, value + "" ); 
return value; 
} 
} else if ( hooks && "get" in hooks && notxml && (ret = hooks.get( elem, name )) !== null ) { 
return ret; 
} else { 
ret = elem.getAttribute( name ); 
// Non-existent attributes return null, we normalize to undefined 
return ret === null ? 
undefined : 
ret; 
} 
}

prop:

```javascript
prop: function( elem, name, value ) { 
var ret, hooks, notxml, 
nType = elem.nodeType; 
// don't get/set properties on text, comment and attribute nodes 
if ( !elem || nType === 3 || nType === 8 || nType === 2 ) { 
return; 
} 
notxml = nType !== 1 || !jQuery.isXMLDoc( elem ); 
if ( notxml ) { 
// Fix name and attach hooks 
name = jQuery.propFix[ name ] || name; 
hooks = jQuery.propHooks[ name ]; 
} 
if ( value !== undefined ) { 
if ( hooks && "set" in hooks && (ret = hooks.set( elem, value, name )) !== undefined ) { 
return ret; 
} else { 
return ( elem[ name ] = value ); 
} 
} else { 
if ( hooks && "get" in hooks && (ret = hooks.get( elem, name )) !== null ) { 
return ret; 
} else { 
return elem[ name ]; 
} 
} 
}
```



attr方法里面，最关键的两行代码，elem.setAttribute( name, value + “” )和ret = elem.getAttribute( name )，很明显的看出来，使用的DOM的API setAttribute和getAttribute方法操作的属性元素节点。而prop方法里面，最关键的两行代码，return ( elem[ name ] = value )和return elem[ name ]，你可以理解成这样document.getElementById(el)[name] = value，这是转化成JS对象的一个属性。(Perfect的确这样，有两种方法一种是[] 另外一种是get(0))


分别做了文本框和checkbox的测试：
1：在遇到要获取或设置checked,selected,readonly和disabled等属性时，用prop方法显然更好
2：需要true or false的时候 最好使用 prop ，因为他返回的是true or false
3：其他情况再考虑attr, checkbox返回的是checked

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style type="text/css">

        .cls
        {
            width: 300px;
            height: 200px;
            background-color: red;
        }
    </style>
    <script type="text/javascript"src="jquery-2.1.3.js"></script>
    <script type="text/javascript">

$(function(){

    $('#btn').click(function(){

        //$('div').empty();//清空层中的元素
        //$('div').remove();//层没了,自杀


        $('div').remove('.cls');//移除应用了cls样式的层;
    });
    var el=$('#atr1');
    console.log('attr :'+el.attr('style')); //undefined 
    console.log('prop() :'+el.prop('style')); //CSSStyleDeclaration对象 
    console.log('prop() :'+document.getElementById('atr1').style); //CSSStyleDeclaration对象 
    //attr 定义 只能attr('')去掉用  attr是一种jQuery的对象操作
    //prop 定义 只能prop('')去掉用  prop是一种JSDom的对象操作
  el.attr('abc','111');
console.log('attr el:abc--------'+el.attr('abc'));
console.log('prop el:abc--------'+el.prop('abc'));
     el.prop('def','2222');
console.log('attr el:def--------'+el.attr('def'));
console.log('prop el:def--------'+el.prop('def'));

// prop显示的checked之类的是  true or false 而不是attr的undefined or checked
var ek=$('#chk1');
console.log('attr el:checked--------'+el.attr('checked'));
console.log('prop el:checked--------'+el.prop('checked'));
console.log('attr el:disabled--------'+el.attr('disabled'));
console.log('prop() 的方法叫合理些');
console.log('prop el:disabled--------'+el.prop('disabled'));



});
    </script>
</head>
<body>
<input type="button" name="name" value="删除元素" id="btn"/>
<div class="cls">

    <input type="text" name="name" value="文本框" id="atr1"/>
    <input type="checkbox" name="name" value="ceshi" id="chk1" checked="checked"/>
</div>
</body>
</html>
```


checkbox的测试案例：
```html
<Html>
  <Head>
    <Title> </Title>
    <Meta charset="UTF-8">
    <script type="text/javascript" src="jquery-2.1.3.js"></script>
    <script type="text/javascript" >
      $(function(){
        $('#btnAll').click(function(){
          //可以但是不好用
          //$('#dv :checkbox').attr('checked','checked');
          //$('#dv :checkbox').attr('checked',true);
          //$('div :checkbox').attr('checked',true);
          $('div :checkbox').prop('checked',true);

        });
      $('#btnNot').click(function(){
        //1
          //$('#dv :checkbox').removeAttr('checked');
        //2
          //$('#dv :checkbox').attr('checked',false);
          //$('div :checkbox').attr('checked',false);
          $('div :checkbox').prop('checked',false);
        });
      $('#btnInverse').click(function(){
        //$('#dv :checkbox').each(function(){
        $('div :checkbox').each(function(){
          //错误1
          //$(this).setAttribute('checked',!$(this)['checked']);
          //不行
          //$(this).setAttribute('checked',!$(this).attr('checked'));
          //思路对了！！但是就是不行！！是不是跟编码有关系
          //$(this).attr('checked',!$(this).attr('checked'));
          $(this).prop('checked',!$(this).prop('checked'));
        });
      });

      });
    </script>
    <style type="text/css">
      /*
      div
      {
        margin:5px;
        background-color:green;
      }
      */
    </style>
  </Head>


  <Body>
    <Div id="btnDv">
      <Input type="button" value="allSelected" id="btnAll"/>
      <Input type="button" value="allNotSelected" id="btnNot"/>
      <Input type="button" value="allSelectedInverse" id="btnInverse"/>
    </Div>

    <Div id="dv">
      <Input type="checkbox" name="name" value="1" id="btnEat"/>eating
      <Input type="checkbox"  name="name" value="2" id="btnSleep"/>sleeping
      <Input type="checkbox"  name="name" value="3" id="btnPlay"/>playing
    </Div>
  </Body>

</Html>
```
最终通过prop解决了属性问题。attr在2.1.3.js是有问题的


另外2.1.3.js还存在toggle的点击切换问题，在1.8.3测试中未发现问题，类似先前的one点击事件

TestBind.html: 用于测试toggle

但是在2.1.3无法切换，在1.8.3可以

本想通过下述代码 实现toggle，却发现无法返回到first当中
```javascript
    $('#btn').click()
     {
       alert('first');
       $(this).unbind().bind('click',function(){
         alert('second');
         $(this).unbind().bind('click',function(){
           alert('third');
      
         });
       });
```





9: 文本输入框的测试

1:如果文本框没有输入文本，那么鼠标离开变成红色


```html
<Html>
  <Head>
    <Title>Test select and option </Title>
    <script type="text/javascript" src="jquery-2.1.3.js" ></script>
    <script type="text/javascript">
      $(function(){
        $('input[type=text]').blur(function(){
          if($(this).val().length==0)
          {
            $(this).css('backgroundColor','red');
          }else
          {
            $(this).css('backgroundColor','');
          }
        });
      });
    </script>
    <style type="text/css">
      div
      {
        width:100px;
        float:left;
      }
      input
      {
        width:100px;
        margin:10px;
      }
    </style>

  </Head>
  <Body>
      
  <Div id="dv" >
    <Input type="text"/>
    <Input type="text"/>
    <Input type="text"/>
    <Input type="text"/>
    <Input type="text"/>
  </Div>
  </Body>
</Html>

```


10:获取鼠标的点击

左键：1
中键：2
右键：3
e.which 返回其值

注意：js中使用 backgroundColor:pink     css中使用background-color:pink;
```html
<Html>
  <Head>
    <Title>Test select and option </Title>
    <script type="text/javascript" src="jquery-2.1.3.js" ></script>
    <script type="text/javascript">
      $(function(){

        $('#countMouse').click(function(e){
          //获得鼠标敲击的值
          //左键 1
          //中键 2
          //右键 3
          alert(e.which);
        });
    });
    </script>

  </Head>

  <Body>

    <!--可不敢再犯这种backgroundColor:pink的错误-->
<Div id="countMouse" style="width:300px;height:200px;background-color:pink">
    </Div>
  </Body>

</Html>
```
11.  隐藏和显示


1:  标签显示和隐藏
    show
     hide
    toggle  在2.1.3中不行

2:slide 幻灯片
     slideDown
    slideUp
   slideToggle

3: Fade 渐进
      FadeIn
     FadeOut
      FadeToggle


1000 代表1000ms  
一般分为三等 slow  normal   fast  ，默认是normal
```html
<Html>
  <Head>
    <Meta charset="utf-8">
    <script type="text/javascript" src="jquery-2.1.3.js"></script>

    <script type="text/javascript">
      $(function(){
        $('#btnShow').click(function(){
         // $('div').show(1000);
         //slide 分为三个 
         $('div').slideDown(1000);
         //$('div').fadeIn(1000);
        });
        $('#btnHide').click(function(){
         // $('div').hide(1000);
          //在1.8.3.min效果一样

          $('div').slideUp(1000);

         //Fade in out toggle 都可以实现
         //$('div').fadeOut(1000);
        });
        $('#btnToggle').click(function(){
          $('div').slideToggle(1000);
          //$('div').fadeToggle(1000);
        });

      });
    </script>
  </Head>
  <Body>
      <Input type="button" id="btnShow" value="show"/>
      <Input type="button" id="btnHide" value="hide"/>
      <Input type="button" id="btnToggle" value="toggle"/>
      <Div id="dv" style="width:400px;height:400px;background-color:pink">
      </Div>
  </Body>
</Html>
```

12.NBA东西部篮球队 

a.通过unbind().bind()来绑定已经解绑的标签事件
b.attr 包含很多属性 比如style（主要是一些背景颜色  border   width heigth 等）

功能是：
a.鼠标经过时候变沉红色 其他的为白色背景
b.鼠标点击后则把该只球队剔除，并放在退赛球队中

bug: 
   东部球队在剔除后，分配到outEast中，mouseover的鼠标移动事件无法实现，然而西部却是可以。。。。

```html
<Html>
  <Head>
    <Meta charset="utf-8">
    <script type="text/javascript" src="jquery-2.1.3.js"></script>

    <script type="text/javascript">
      $(function(){
        $('#east li').mouseover(function(){
          $(this).css('backgroundColor','red').siblings().css('backgroundColor','');
          //alert($(this).style);
        }).click(function()
           {
             //$(this).removeAttr('style').unbind().appendTo('#outerEast');
             //$(this).attr('backgroundColor','blue').appendTo('#outerEast');
             //$(this).css('backgroundColor','blue').unbind().appendTo('#outerEast');
             //$(this).attr('backgroundColor','blue').unbind().appendTo('#outerEast');
             //可不敢犯这个错误，如果不加end()的断链操作，会把所有的siblings()元素
             //提交到下边哐当中！
             //$(this).css('backgroundColor','blue').siblings().css('backgroundColor','').end().unbind().appendTo('#outerEast');
             //在appendTo的事件发生之后（发展的角度来看）
             //但是发现不行，于是留下一个疑问

             //现在明白了既然unbind就得需要用bind()来解决
             //$(this).unbind().appendTo('#outerEast').attr('onmouseover','').mouseover(function(){
             $(this).css('backgroundColor','blue').siblings().css('backgroundColor','').end().unbind().appendTo('#outerEast').bind('mouseover',function(){
               //错误阿！！！居然把所有的其他都给功能禁用了！
             //$(this).css('backgroundColor','blue').siblings().css('backgroundColor','').unbind().end().appendTo('#outerEast').bind('mouseover',function(){
               $(this).css('backgroundcolor','blue').siblings().css('backgroundColor','');
             });
           });
        $('#west li').mouseover(function(){
          $(this).css('backgroundColor','red').siblings().css('backgroundColor','');
        }).click(function()
           {
             //可不敢再犯这个错误
             //attr不止一个属性，不仅仅是style
             //而css只是设置界面属性
             //$(this).removeAttr('style').unbind().appendTo('#outeWest');

             //可行阿！！！试验了一个小时   bind之后再到之后的地方unbind即可
             //$(this).unbind().appendTo('#outerWest').bind('mouseover',function(){
             //好处是直接显示蓝色
             $(this).css('backgroundColor','blue').siblings().css('backgroundColor','').end().unbind().appendTo('#outerWest').bind('mouseover',function(){
              $(this).css('backgroundColor','blue').siblings().css('backgroundColor','');
             });
             //$(this).attr('backgroundColor','blue').unbind().appendTo('#outerWest');
             //attr只会保存着第一个backgroundcolor的值！
           });

           //增加一个功能，可以返回到原先的竞争队列中
           // 先清空一下试试
         //$('#outerEast li').click(function(){

         /*
         $('#outerEast li').attr('onclick','').click(function(){
           $(this).css('backgroundColor','blue').bind('click',function(){
            $(this).css('backgroundColor','red').appendTo($('#east'));
           });
         });

         $('#outerEast li').attr('onclick','').click(function(){
           $(this).css('backgroundColor','blue').bind('click',function(){
            $(this).css('backgroundColor','red').appendTo($('#east'));
           });
         });
         */

//如果onclick事件原先有值，要先清空，再用click( 
//为什么？
      });
    </script>

  </Head>

  <Body>
    <H2>The basketball team</H2>
    <Ol id="east">
      <H3>The East Basketbal Teams</H3>
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
      <H3>The West Basketbal Teams</H3>
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

    <H2>The basketball team which have been out of the game.</H2>

    <Hr color="red"/>
    <H3>The team of the west</H3>
    <Ol id="outerWest">

    </Ol>

    <Hr color="red"/>
    <H3>The team of the east</H3>
    <Ol id="outerEast">

    </Ol>

  </Body>
</Html>
```



12:逐帧变化Animate

        //animate的作用其实就是动态的改变效果
        // 比如从某个点 移动某个点
        //     慢慢的拉伸长度  高度  模仿人的动作，并可以通过设置
        //     ms数 


会飞的鸟：
功能： 1 先让他从(0,0)移动到 （200，50）  1s移动时间
       2 再让其从(200,50)   移动到(700,150)  2s移动时间(可以使用+= 注意了)

```html
<Html>
  <Head>
    <Title>Test select and option </Title>
    <script type="text/javascript" src="jquery-2.1.3.js" ></script>
    <script type="text/javascript">
      $(function(){
        //animate的作用其实就是动态的改变效果
        // 比如从某个点 移动某个点
        //     慢慢的拉伸长度  高度  模仿人的动作，并可以通过设置
        //     ms数 
        $('#btn1').click(function(){
          //按照 默认的正常速度 延长高度到300px
          $('#box').animate({height:"300px"});
        });
        $('#fly').click(function(){
          //1000ms 从(0 0)  运行到200 50 的位置 时间1s
          //可不敢犯这个错误
          //$('#bird').animate('{"left":"200px","top":"500px"}',1000);
          $('#bird').animate({"left":"200px","top":"50px"},1000);
          // 加一个变小飞退的效果
          $('#bird').animate({"left":"+=500px","top":"+=100px","width":"40px","height":"30px"},2000);
        })
      })
    </script>
  </Head>


  <Body>
    <Input type="button" id="btn1" value="animate"/>
    <Div id="box" style="width:300px;height:200px ;background-color:red"> 
    </Div>
    <Input type="button" value="let me fly" id="fly">
    <Img src="2.png" style="position:absolute;" id="bird"/>
  </Body>

</Html>
```

13: Cookie



        //突然明白  cookie只不过是一堆hashmap  临时存储在浏览器当中哦 


定义：

$.cookie('uName', $('#YourName').val());


使用：



$.cookie('uName')



作用：

当刷新页面仍然会保存uName的值！



需要加载jquery.cookie.js 插件


```html
<Html>
  <Head>
    <Title>Test select and option </Title>
    <script type="text/javascript" src="jquery-2.1.3.js" ></script>
    <!--$.cookie有效必须加载 jquery.cookie.js-->
    <script type="text/javascript" src="jquery.cookie.js" ></script>
    <script type="text/javascript">
      $(function(){
        $('#btn').click(function(){
            $.cookie('uName', $('#YourName').val());
            alert('I browser have record your name');
        });
        //突然明白  cookie只不过是一堆hashmap  临时存储在浏览器当中哦 
        if($.cookie('uName'))
        {
            $('span').text('welcome  Mr/Mrs '+$.cookie('uName')+' coming back');
        }else
        {
          $('span').text('welcome you !Little Bird . ')
        }
      });

    </script>

  </Head>

  <Body>
      <Input type="text" name="name" value="" id="YourName"/>
      <INput type="button" name="name" value="record" id="btn"/>
      <Span></Span>
  </Body>


</Html>

```

14:jqZoom局部放大器

注意点1：jqZoom在2.1.3测试失败！  但在1.8.3.min.js测试通过

注意点2: 要    
  连接上css否则效果不好：<LINK rel=stylesheet  type=text/css href="jquery.jqzoom.css" />

注意点3： jqzoom()失效写的



注意点4： 大图片是放在一个<A class=MYCLASS href="大图片"> <img src="小图片"/></A> 


```html
<Html>
  <Head>
    <Title>Test select and option </Title>
    <script type="text/javascript" src="jquery-1.8.3.min.js" ></script>
    <script type="text/javascript" src="jquery.jqzoom-core.js" ></script>
    <!-- 可不敢犯这个错误
    <script type="text/css" src="jquery.jqzoom.css" ></script>
    -->
    <LINK rel=stylesheet  type=text/css href="jquery.jqzoom.css" />
    <script type="text/javascript">
      $(function(){
      //$(document).ready(function(){
      //可不敢写错函数 z不可以大写
        //$('.MYCLASS').jqZoom();
        $('.MYCLASS').jqzoom();
            /*
          {
                zoomType: 'standard',
                lens:true,
                zoomWidth: 300,
                zoomHeight:450,
                xOffset:90,
                yOffset:30,
                preloadImages: true,
                alwaysOn:false
          });
              */
      });
    </script>

  </Head>

  <Body>
    <!--可不敢把 </A>放在 IMg标签外了-->
    <A class=MYCLASS  title=MYTITLE href="triumph_big1.jpg">
      <Img title="IMAGE TITLE" src="triumph_small1.jpg">
</A>
  </Body>


</Html>

```



15 qq微博案例

模拟微博140个字的提交案例

1： html部分
    加载weibo.css
    加载weibo.js
    加载logo
    加载textarea文本框
    加载附件区
    加载按钮
2： css部分
    no-repeat 不进行平铺

3： js部分

  3.1需要着重掌握 拓展的写法
  3.2通过脱离文档流  来加在朋友列表和表情列表
  3.3 学习广播的 追踪显示部分图片的功能 配合 background   background-position
      以及坐标进行设置  当然背景框是有大小限制的 no-repeat不平铺  利用css进
      如果超出多少个字  则变红色警告

  3.4 有三个地方 Please write down the topic  为什么？  1: 为了添加Topic 点击事件需要 2：在朋友列表和表情列表
            第二个是因为当没有只有#Please write..# 则使用val()进行替换

        但是当textarea原先有内容时候，如果直接使用val（新内容）则会覆盖原先的旧内容 ，所以需要进行判断
  3.5 selectRange的作用就是选择掉从第一个字符到最后一个字符，但是我的测试结果变成了选择
  3.6 朋友列表，表情列表 都是通过建立一个新的层来设置
        注意添加完close 标签后，在添加 朋友列表的时候需要在style属性中添加clear:both来清除 float:right的属性
3.7  表情层中 显示的文件夹路径得设置正确
              注意 1： userFaces[key]在 表情上面的title
               2： 小手功能
              //     3： 点击功能  单机事件  鼠标mouseover的事件 会显示文字信息



weibo.html:

```html
<Html>
  <Head>
    <Title>Test select and option </Title>
    <Link rel=stylesheet href="weibo.css" type="text/css">
    <script type="text/javascript" src="jquery-2.1.3.js" ></script>
    <script type="text/javascript" src="weibo.js">
    </script>

  </Head>

  <Body>

    <Img id="logo" src="weiboPic/b3_100901.png" alt="log"/>
    <!--让评论居中-->
    <Center>
      <Div id="Weibo">
        <Div id="WL">
          <Div id="Talking">
            <H2><A>Weibo Title: communication</A></H2>
            <Textarea id="Msg"></Textarea>
            <Div id="Attach">
              <A href="javascript:void();" class="NewTopic">Topic</A>
              <A href="javascript:void();" class="Friend">Friends</A>
              <A href="javascript:void();" class="Face">Face</A>
              <A href="javascript:void();" class="Pic">Picture</A>
              <A href="javascript:void();" class="Video">Video</A>
            </Div>
            <Div id="Send">
              <Input type="button" class="sendBtn" value=""/>
              <Span class="CountTxt">You can still write<Em>140</Em>words</Span>
            </Div>

        </Div>
        <!--
        <Div id="WR">

        </Div>
        -->
      </Div> 
    </Center>
  </Body>

</Html>
```



weibo.css:
```css
        body
        {
            margin: 0px;
            background: url('weiboPic/wrapBg.jpg') no-repeat #EBF1F1;
        }
        #logo
        {
            margin: 30px 0 0 300px;
        }
        #Weibo
        {
            width: 800px;
            height: 200px;
            border: 1px solid #000;
        }
        /* WL: weiboLeft*/
        #WL
        {
            width: 590px;
            height: 100%;
            background: #fff;
            float: left;
        }
        /* WR: weiboLeft */
        #WR
        {
            background: #CCEBF4;
            width: 210px;
            height: 100%;
            float: right;
        }

        #Talking
        {
            text-align: left;
            margin: 0 0 0 25px;
        }
        /*增加文本框的大小*/
        #Msg
        {
            width: 540px;
            height: 80px;
            overflow: hidden;
            font-family: Tahoma, Arial;
            font-size: 14px;
            border: 1px solid gray;
        }
        #Talking H2
        {
            text-align: left;
            padding: 0px;
            margin: 0px;
            font: normal normal normal 18px/29px 'MicroSoft YaHei' , SimHei;
        }
        #Attach
        {
            width: 540px;
        }
        #Attach a
        {
            color: #000;
            text-decoration: none;
            font-size: 14px;
        }
        .NewTopic, .Friend, .Face, .Pic, .Video
        {
            background-position: -170px -33px;
            display: inline-block;
            height: 16px;
            padding-left: 18px;
        }
        .CountTxt
        {
            color: #999;
            float: right;
            line-height: 33px;
            margin: 0 15px 0 0;
        }
        .CountTxt em
        {
            font-family: Georgia, Tahoma, Arial;
            font-size: 26px;
            position: relative;
            top: -5px;
            vertical-align: middle;
        }
        .sendBtn
        {
            float: right;
            margin: 0 20px 0 0;
            padding: 0px;
            background: url(weiboPic/bg1.png) -117px -165px no-repeat;
            line-height: 33px;
            margin-left: 14px;
            height: 30px;
            width: 112px;
            border: 0px;
            cursor: pointer;
        }
        #btnCloFri
        {
            cursor: pointer;
        }
        
```


weibo.js:
//1。扩展jQuery
//需要着重掌握 拓展的写法
//通过脱离文档流  来加在朋友列表和表情列表
//学习广播的 追踪显示部分图片的功能 配合 background   background-position
//   以及坐标进行设置  当然背景框是有大小限制的 no-repeat不平铺  利用css进行
//   切换
//如果超出多少个字  则变红色警告
```javascript
        $.fn.selectRange = function (start, end) 
        {
          //please write down the topic 高亮显示
          //转化为 js对象
            var curObj = $(this).get(0);
            if (!curObj)
              return;
            else if (curObj.setSelectionRange) 
            {
                curObj.focus(); curObj.setSelectionRange(start, end);
            } /* WebKit */
            else if (curObj.createTextRange) 
            {
                var range = curObj.createTextRange();
                range.collapse(true);
                range.moveEnd('character', end);
                range.moveStart('character', start);
                range.select();
            } /* IE */
            else if (curObj.selectionStart) 
            {
                curObj.selectionStart = start;
                curObj.selectionEnd = end;
            }
        };
        //====上面的代码是扩展的

    $(function()
    {

        //按钮高亮显示
        $('#Send .sendBtn').mouseover(function(){
          $(this).css('backgroundPosition','0 -195px');
        }).mouseout(function(){
            $(this).css('backgroundPosition','-117px -165px');
//-117px -165px
        });
        //计算文本框还能输入多少个字
        $('#Msg').change(function(){
            //还能输入多少个字
           var len= 140- $(this).val().length;
           if(len>=0){
                $('#Send .CountTxt').html('You can still write down<em>'+len+'</em>words');
            }else{
              //设置 变成高亮的红色
                $('#Send .CountTxt').html('Already exceed <em><font color="red">'+Math.abs(len)+'</font></em>words');
            }

        });
        setInterval(function(){
            $('#Msg').change();
        },500);


     //显示话题
        ////有三个地方 Please write down the topic  为什么？  1: 为了添加Topic 点击事件需要 2：在朋友列表和表情列表
        //为什么？
        $('#Attach .NewTopic').click(function(){

            if($('#Msg').val().length==0){
              //selectRange的作用就是去除掉 从第一个字符到最后一个字符
              //已方便我们输入话题
                $('#Msg').val('#Please write down the topic#').selectRange(1,28);
            }
        });
//显示朋友
        $('#Attach .Friend').click(function(){
            var friendsList = ['Foleide', 'Risse', 'LiLei','HanMeiMei','Tom','Turkkey','John'];
            if($('#dvF').length>0){
                $('#dvF').remove();
            }
            //增加了一个 朋友层 用于选择！！
            var dvFriends=$('<div id="dvF" style="width:100px;border:1px solid blue;background-color:white;position: absolute;"></div>').appendTo($('body'));
            //设置完层的脱离文档流 后，设置他的偏移
            var dvX=$(this).offset().left+'px';//层距离左侧的像素 注意 height() 有圆括号包裹
            var dvY=$(this).offset().top+$(this).height()+'px';
            //设置他的上和左的坐标
            dvFriends.css({"left":dvX,"top":dvY});
            //添加一个关闭按钮
            //float:right 向右浮动
            $('<span style="background-color: gray;cursor: pointer; float: right;">Close</span>').click(function(){
                $(this).parent().remove();
            }).appendTo(dvFriends);

            //显示朋友列表
            //clear:both 清除掉 span所具有的float效果！！
            //    规律： 如果同一级的元素有浮动的，就需要用clear:both来清除
            //否则 ulobj会跟close处在同一行当中
            //list-style-type:square
           var ulObj= $('<Ul style="clear: both; list-style-type: square; margin: 0;padding: 0;"></Ul>').appendTo(dvFriends);
            for(var i=0;i<friendsList.length;i++){
              //设置移到的鼠标为手型
               $('<Li style="margin-bottom: 5px; cursor: pointer;">@'+friendsList[i]+'</Li>').mouseover(function(){
                 //鼠标进入时候的高亮显示
                   $(this).css('backgroundColor','yellow');

                   //鼠标离开又得恢复过来
               }).mouseout(function(){
                   $(this).css('backgroundColor','');
               }).click(function(){
                 //不太明白这边的写法 ok
                 //如果用户没有输入任何内容 则替换掉原先的内容
                   if($('#Msg').val()=='##'){
                       $('#Msg').val($(this).text());
                   }else{
                     //如果用户已经输入了，则必须保证用户的数据不被覆盖
                       $('#Msg').val($('#Msg').val()+$(this).text());
                   }


               }).appendTo(ulObj);
            }
        });

        //显示表情
        var userFaces = { '0.gif': '微笑', '1.gif': '撇嘴', '2.gif': '色', '3.gif': '发呆', '4.gif': '得意', '5.gif': '流泪', '6.gif': '害羞', '7.gif': '闭嘴', '8.gif': '睡', '9.gif': '大哭', '10.gif': '尴尬', '11.gif': '发怒', '12.gif': '调皮', '13.gif': '呲牙', '14.gif': '惊讶', '15.gif': '难过', '16.gif': '酷', '17.gif': '冷汗', '18.gif': '抓狂', '19.gif': '吐', '20.gif': '偷笑', '21.gif': '可爱', '22.gif': '白眼', '23.gif': '傲慢', '24.gif': '饥饿', '25.gif': '困', '26.gif': '惊恐', '27.gif': '流汗', '28.gif': '憨笑', '29.gif': '大兵', '30.gif': '奋斗', '31.gif': '咒骂', '32.gif': '疑问', '33.gif': '嘘', '34.gif': '晕', '35.gif': '折磨', '36.gif': '衰', '37.gif': '骷髅', '38.gif': '敲打', '39.gif': '再见', '40.gif': '擦汗', '41.gif': '抠鼻', '42.gif': '鼓掌', '43.gif': '糗大了', '44.gif': '坏笑', '45.gif': '左哼哼', '46.gif': '右哼哼', '47.gif': '哈欠', '48.gif': '鄙视', '49.gif': '委屈', '50.gif': '快哭了', '51.gif': '阴险', '52.gif': '亲亲', '53.gif': '吓', '54.gif': '可怜', '55.gif': '菜刀', '56.gif': '西瓜', '57.gif': '啤酒', '58.gif': '篮球 ', '59.gif': '乒乓', '60.gif': '咖啡', '61.gif': '饭', '62.gif': '猪头', '63.gif': '玫瑰', '64.gif': '凋谢', '65.gif': '示爱', '66.gif': '爱心', '67.gif': '心碎', '68.gif': '蛋糕', '69.gif': '闪电', '70.gif': '炸弹', '71.gif': '刀', '72.gif': '足球', '73.gif': '瓢虫', '74.gif': '便便', '75.gif': '月亮', '76.gif': '太阳', '77.gif': '礼物', '78.gif': '拥抱', '79.gif': '强', '80.gif': '弱', '81.gif': '握手', '82.gif': '胜利', '83.gif': '抱拳', '84.gif': '勾引', '85.gif': '拳头', '86.gif': '差劲', '87.gif': '爱你', '88.gif': 'NO', '89.gif': 'OK', '90.gif': '爱情', '91.gif': '飞吻', '92.gif': '跳跳', '93.gif': '发抖', '94.gif': '怄火', '95.gif': '转圈', '96.gif': '磕头', '97.gif': '回头', '98.gif': '跳绳', '99.gif': '挥手', '100.gif': '激动', '101.gif': '街舞', '102.gif': '献吻', '103.gif': '左太极', '104.gif': '右太极', '105.gif': '淡定', '106.gif': '晕', '107.gif': '不满', '108.gif': '睡觉', '109.gif': '小调皮', '110.gif': '咒骂', '111.gif': '发怒', '112.gif': '偷笑', '113.gif': '微笑', '114.gif': '震惊', '115.gif': '囧' };
        $('#Attach .Face').click(function(){

            if($('#dvfaceImg').length>0){
                $('#dvfaceImg').remove();
            }
           var dvFace= $('<div id="dvfaceImg" style="width:370px;border:1px solid blue;background-color: white;position: absolute;"></div>').appendTo($('body'));
           //和朋友列表相同的操作过程！！！
            var dvXX=$(this).offset().left-100+'px';
            var dvYY=$(this).offset().top+$(this).height()+'px';
            //最终设置他的坐标
            dvFace.css({"left":dvXX,"top":dvYY});
            //显示 表情  关闭
            $('<Span style="float: left;">Expression</Span>').appendTo(dvFace);
            $('<Span style="float: right;cursor: pointer; background-color: gray;">close</Span>').click(function(){

                $(this).parent().remove();//删除层
            }).appendTo(dvFace);

            //显示表情
           var dvfaceimage= $('<div style="clear: both;"></div>').appendTo(dvFace);
            for(var key in userFaces){
              //显示的文件夹路径得设置正确
              //注意 1： userFaces[key]在 表情上面的title
              //     2： 小手功能
              //     3： 点击功能  单机事件
                $('<Img src="weiboPic/faces/'+key+'" title="'+userFaces[key]+'" />').mouseover(function(){
                    $(this).css('cursor','pointer');

                }).click(function(){
                  //不清楚 这边为什么这样写  同样的 在friend也是如此
                    //if($('#Msg').val()=='#Please write down the topic#'){
                    if($('#Msg').val()=='##'){
                        $('#Msg').val( '['+$(this).attr('title')+']');
                    }else{
                        $('#Msg').val($('#Msg').val()+'['+$(this).attr('title')+']');
                    }


                }).appendTo(dvfaceimage);
            }
        });

    });

```

