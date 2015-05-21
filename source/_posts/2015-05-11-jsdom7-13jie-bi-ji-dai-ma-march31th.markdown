---
layout: post
title: "JSDOM7-13节笔记代码March31th"
date: 2015-05-11 14:58:44 +0800
comments: true
categories: JSDom
---
<!--more-->
连接到前文JavaScript 7-11节笔记代码

8: 注册和调用的不同：
注册
  Onclick=function(){}; //匿名函数的注册行为
  Onclick=ff;
调用
   Onclick=ff();


9:计时器
setInterval反复性计时器（随着一定时间之后 再跳出来）


```javascript
Var setId=setInterval(function()
{
Alert(‘你名函数’);
}，1000);
clearInterval(setId);
setTimeout一次性计时器：
Var setID= setTimeOut(function()
{
Alert(‘这是一次性计时器’);
},1000);
clearTimeOut(setId);
```

计时器小案例1：


```javascript
onload=function()
{
        var i = 10;
        setIntervar(function()
            {
               i--;
               var btnObj=document.getElementById('btn');//获得btn对象
               btnObj.value=i;//设置该对象的value值即可！
            },1000);

}
```


经这么一写：



规律：所有的标签都可以有id-value对 或者name-value对象 或者class-value对
          完全可以通过document对象获得所有的标签元素的id，name,class进行对应标签内容的设置



计时器小案例2：
//跑马灯效果

```javascript
setIntervale(function()
    {
        var tt=document.title;
        document.title = tt.substring(1)+tt.substr(0,1); //1-尾部  + 0-1的字符
},1000 );
```



10 页面加载事件（3个）
前面接触最多的就是onload ,另外还有onunload,onbeforeload
Onload:页面加载后触发
Onunload 页面退出后触发
Onbeforeunload: 页面关闭之前触发（发表帖子 和   你确定退出？）


11 刷新后，返回的页面
回顾window.location.href(‘www.baidu.com’); 类似于window.navigaete(‘’)

12 Document.write的两个作用
动态的创建控件(但是只能在页面加载时候动态创建  ) 但是有一个缺点：在IE中会删掉之前的控件标签，在源代码浏览时候，发现原先的标签都没有了
镶嵌广告



1.打开 news.baidu.com/newscode  
输入关键字获得新闻的代码
2.复制代码
代码包含两个部分：
样式部分：

```css
<style type=text/css> div{font-size:12px;font-family:arial}.baidu{font-size:14px;line-height:24px;font-family:arial} a,a:link{color:#0000cc;}
.baidu span{color:#6f6f6f;font-size:12px} a.more{color:#008000;}a.blk{color:#000;font-weight:bold;}</style>
```
Js脚本部分：

```html
<script language="JavaScript" type="text/JavaScript" src="http://news.baidu.com/ns?word=title%3A%E9%A3%8E%E5%8A%9B%E5%8F%91%E7%94%B5&tn=newsfcu&from=news&cl=2&rn=5&ct=0"></script>

```

说明src其实就是document.write的实现！！但是为什么？原理暂时不知道


查询src里面内容的方式：
1：notepad
2：open-?黏贴进 src的地址
3： 就可以在记事本看到src所代表的新闻内容，他其实就是document.write()的内容！为了保证百度更新新闻的时候他也能更新新闻，所以采用这种document.write()来实现


```html
<Html>
  <Head>
    <script type="text/javascript">
      onload=function()
      {
        //第一个功能动态加载控件标签  但是在IE中会使其他的控件消失
        //d当然这边加载 会把原先的页面的控件给隐藏掉！！
//        document.write('<Font color="red" size="6">Today is </font>');
//       document.write('<Input type="button" value="click me"\>');

      }
      </script>
<style type=text/css> div{font-size:12px;font-family:arial}.baidu{font-size:14px;line-height:24px;font-family:arial} a,a:link{color:#0000cc;}
.baidu span{color:#6f6f6f;font-size:12px} a.more{color:#008000;}a.blk{color:#000;font-weight:bold;}</style>

  </Head>

  <Body>

<script language="JavaScript" type="text/JavaScript" src="http://news.baidu.com/ns?word=title%3A%E9%A3%8E%E5%8A%9B%E5%8F%91%E7%94%B5&tn=newsfcu&from=news&cl=2&rn=5&ct=0">
</script>
  </Body>
</Html>
```




13 获得页面元素的三张方法
Document.getElementById(‘btn’)---->最常用的方式
Document.getElementByName(‘fds’) ---->name=”fds”
Document.getElementByTagName(‘Input’)---->标签名




14获取坐标点
顶级对象window包含一个比较重要的对象event可以由此获得当前对象、页面、屏幕

1：当前对象作为参考点 -->offsetX|Y
2：当前页面作为参考点 --->clientX|Y
3：：当前屏幕作为参考点-->screenX|Y

```html
<Html>
  <Head>
    <Title>hellow</Title>
    <script type="text/javascript">
//      alert(document.getElementById('btn').titleLabel.text)
     // alert(window.event.button);
      alert(screen.width+','+screen.height);
      onload=function()
      {
        document.getElementById('ak').onclick=function()
        {
          alert(new Date().toLocaleTimeString());
          window.event.returnValue=false;//IE Opera  google支持  火狐不支持
        }
        document.getElementById('dv').onmousemove=function()
        {
          //鲁棒性最好的参数
          if(arguments.length!=0)
            {
              //火狐 google  IE不行
              document.getElementById('ipClient').value=arguments[0].clientX+','+arguments[0].clientY;
            }else
            {

              document.getElementById('ipClient').value=event.clientX+','+event.clientY;
            }
          //也可以另外一种鲁棒性的优化
          //能力检测方式！ 
          if(window.event)//判断如果存在则是IE不存在则是火狐  window.event是IE下的重要的属性
            {

              document.getElementById('ipClient2').value=event.clientX+','+event.clientY;
            }else
            {
              document.getElementById('ipClient2').value=arguments[0].clientX+','+arguments[0].clientY;
            }
          //火弧+google可以通过  IE不行
          

          //下面代码google和IE支持
          //相对于页面的左上角
            document.title=event.clientX+','+event.clientY;
          //相对于屏幕的左上角
            document.getElementById('btn').value=event.screenX+','+event.screenY;            
            document.getElementById('ip1').value=event.screenX+','+event.screenY;            
          //相对于当前对象的左上角（this)对象

            document.getElementById('ip').value=event.offsetX+','+event.offsetY;            
        }
  
      }
    </script>
  </Head>
  

  <Body>
      <Div id="dv" color="red">
      <button type="button" id="btn" class="button">
          屏幕值
      </button>
      <Input type="button" id="ipClient" value="页面左上角偏移"/>
      <Input type="button" id="ipClient2" value="页面左上角偏移"/>
      <Input type="button" id="ip" value="对象的左上角偏移"/>
      <Input type="button" id="ip1" value="屏幕的偏移坐标"/>
        hello
      </Div>
      <Div>
        <A id="ak" href="www.baidu.com">百度</A>
        </Div>
  </Body>
</Html>
```




15：剪切板clipboard

当在网页复制的时候或者ctrl+C的时候，会激活body的oncopy函数。
当在网页黏贴的时候或者ctrl+V的时候，会激活body的onpaste函数。
        由此可以模拟页面的进制复制和黏贴的案例，具体如代码所示：

```html
<Html>
  <Head>
    <Title>Test copy  clipdata</Title>
    <script type='text/javascript'>
      // 一复制则激活oncopy函数
/*
      onload=function()
      {
        document.body.oncopy=function()
        {
          alert('不让复制');
          return false;
        };
        document.getElementById('txt').onpaste=function()
        {
          alert('不能黏贴');
          return false
        };

      };
*/
      onload=function()
      {
        document.body.oncopy=function()
        {
          setTimeout(function()
                      {
                        //IE支持！！  opera 火狐不支持
//出现此错误是因为 window.clipboardData 为 IE 专有，其他浏览器均没有此对象。
                        if(window.clipboardData)
                          {
                            var dataFromClip=clipboardData.getData('text')+'你这个拷贝党';
                              alert(dataFromClip);
                              clipboardData.setData('text',  dataFromClip);
                          }else
                            {
                              alert('Opera不知道如何做了  Opera        无操作剪贴板的对象。');
                             // window.location=dataFromClip;
                              alert('<Strong>复制</Strong>成功！')
                              return false;
                            }

                      },10);
       };

       };
      document.onclick=function()
      {

        document.write('hello');
      }


    </script>
  </Head>

  <Body>
    <Input type="text" id="txt" value="" />
    good weather!
  </Body>
</Html>
```




16：this和事件源srcElement
     this只记录当前对象的id
     alert(window.event.srcElement.id) 只记录最初加载完body第一次点击的id；

而srcElement其实是可以和事件冒泡的规律相结合的。
事件冒泡是指内层事件的触发，会由内至外的触发外层事件。
为了只是显示最内层的事件，可以用srcElement.id,比如：



注意点：
    如果text/javscript 则 不会去检查 fucntion的错误！如果写成text/javascript
    则客户端浏览器会去检查 js的语法。
一般规律：
   一定要注意 style里面用的是 background-color  而 在js中用的是  backgroundColor 两者是不同的写法
    并且这个是一个基本的规律！ js中去除了破折号  然后大写后一个单词首字母。

```html
<Html>
  <Head>
    <Title>事件冒泡程序</Title>
    <script type="text/javascript">
      // 如果text/javscript 则 不会去检查 fucntion的错误！如果写成text/javascript
      //则客户端浏览器会去检查 js的语法！
      onload=function()
      {
         document.getElementById('div1').onclick=function()
        {
        //  alert(this.id);
        alert(window.event.srcElement.id);
        };
        document.getElementById('p1').onclick=function()
        {
          alert(this.id);
        };
        document.getElementById('str1').onclick=function()
        {
          alert(this.id);
          //opera IE可以！
          window.event.cancelBubble=true;
          // 火狐e.stopPropagation();
        };
      };
    </script>
  </Head>

  <Body>
    <!--一定要注意 style里面用的是 background-color  而 在js中用的是  backgroundColor 两者是不同的写法
    并且这个是一个基本的规律！ js中去除了破折号  然后大写后一个单词首字母-->
    <Div id="div1" style="width:300px;height:200px;background-color:red;cursor:pointer;">
      <!--<Div id="div1" style="width:300px;height:200px;backgroundColor=red;cursor:pointer;"> 一定要注意不要写成=号形式 style里面写成json形式-->
        
        <!--<P id= "p1" style="width:100px;height:100px;background-color:blue;cursor:pointer;">这是第二层文字 -->
          <P id= "p1" style="width:100px;height:100px;background-color:blue;cursor:pointer;">
        <Strong id="str1">这是第三层文字</Strong> 
      </P>
    </Div>
  </Body>
</Html>

```





17：innerText和innerhtml

注意1：innerhtml 不要写成innerHtml 否则是undefined
注意2：  var btnObjAgree=document.getElementsByName('agree');
             var timeControl=document.getElementById('time');
            正因为Id没有s 所以他必然是唯一的， 而name TagName都是可以重复 所以他的Element是复数的
案例1：四个按钮 分别显示 设置innerText 设置innerhtml  获取innerText 获取innerhtml
     注意：
        //1   在添加文本时候 Text模式是直接添加无效果  而html是有效果 
        //2   在获取文本时候  Text模式是直接获得文本   而html是包含着标签信息
       另外//在firefox不行!!firefox and  Ie is a pair of enemy
          //In Firefox , You should use the statement below
          // dvObj.contentText=''




具体如下所示：

```html
<Html>

  <Head>
    <Title>Test  InnerText And InnerHtml IN fireFox and IE ! </Title>
    <style type="text/css">
      div
      {
        width:300px;
        height:200px;
        background-color:green;
        // 在style中使用 json形式的数据结构
        // 在js中使用  =形式的数据结构
      }
      </style>
    <script type="text/javascript">
      onload=function()
      {
        var dvObj=document.getElementById('dv');
        document.getElementById('btn1').onclick=function()
        {
          //显示原始的内容  不带上效果
          //Ie  google可以
          dvObj.innerText='<A href="http://www.163.com">网易</A>';
          //在firefox不行!!firefox and  Ie is a pair of enemy
          //In Firefox , You should use the statement below
          // dvObj.contentText=''
        }

        //1   在添加文本时候 Text模式是直接添加无效果  而html是有效果 
        //2   在获取文本时候  Text模式是直接获得文本   而html是包含着标签信息
        document.getElementById('btn2').onclick=function()
        {
          //直接添加上效果
          //document.getElementById('dv').innerHtml='<A href="www.163.com">网易</A>';
          //注意大小写 否则不能显示出 结果
          document.getElementById('dv').innerhtml='<A href="http://www.163.com">网易</A>';
        }
        document.getElementById('btn3').onclick=function()
        {
          alert(dvObj.innerText);
        }
        document.getElementById('btn4').onclick=function()
        {
          alert(dvObj.innerhtml);
          //alert(dvObj.innerHtml);
        }


      };
    </script>
  </Head>

  <Body>
    <input type="button" value="innerText insert" id="btn1"/>
    <input type="button" value="innerHtml insert" id="btn2"/>
    <input type="button" value="innerText get" id="btn3"/>
    <input type="button" value="innerHtml get" id="btn4"/>
    <Div id="dv">
    </Div>
  </Body>
</Html>

```


案例2：   innerText和计时器联合的一个小程序
             功能需求： 起先按钮是灰色，显示“请仔细阅读协议(5)”  并随时间递减，
                              当回归到0时，则按钮变成可选状态，并其值设置为“同意”。



              实现如下：
```html
<Html>
  <Head>
    <Title> Three small practices</Title>
    <script type="text/javascript">

        onload=function()
        {
          var btnObjs=document.getElementsByName("name");
          for(var i =0; i< btnObjs.length;i++)
          {
            //你再点击之前得把所有的其他颜色都给他恢复成默认
            //这点很重要 ！！ 否则你会发现之前点过的按钮依然是红色的！没有
            //恢复回默认的状态
            btnObjs.onclick=function()
            //对这个网页的逻辑不是特别清楚！ 应该是点击之后才触发的事件
            //而不是你想着我让他点的时候  他就触发！！必须有一个介质！
            //这个介质说大点叫做计算机，说小点叫做onclick
            {
             for(var j=0; j< btnObjs.length;j++)
              {
                btnObjs[j].style.backgroundColor='';
              }

                this.style.backgroundColor='red';
            }

          }

          var sec=5;
          //不要少了一个s   间接说明 name可以有多个
          //var btnObjAgree=document.getElementsByName('agree');
          // 返回的是一个name数组！！！一定得注意 单写上btnobjagree.value不起作用
          var btnObjAgree=document.getElementsByName('agree');

          //不要多了一个s   间接说明Id只能有一个
          //var btnObjAgree=document.getElementById('btn');
          var setId= setInterval(function()
                     {
                        if(sec> 0)
                          {
                            sec--;
                            btnObjAgree[0].value='请仔细阅读下列协议('+sec+')';

                          }else
                          {
                            btnObjAgree[0].value='同意';
                            btnObjAgree[0].disabled=false;
                            clearInterval(setId);
                          }

                      },1000);

                      setInterval(function(){
                            var timeControl=document.getElementById('time');
                            //timeControl.innerText=new Date().toLocalTimeString();//别写错了Locale而不是Local
                            timeControl.innerText=new Date().toLocaleTimeString();

                      },1000);
          };
    </script>
  </Head>

  <Body>
    <Input type="button" name="name" value="小明"/>
    <Input type="button" name="name" value="小明"/>
    <Input type="button" name="name" value="小明"/>
    <Input type="button" name="name" value="小明"/>
    <Input type="button" name="name" value="小明"/>
    <Input type="button" name="name" value="小明"/>
    <Input type="button" name="name" value="小明"/>
    <Input type="button" name="name" value="小明"/>
    <Input type="button" id="btn" name="agree" value="请仔细阅读下列协议(5)" disabled="disabled"/>

    <P id="time"></P>
  </Body>
</Html>


```





案例3：通过一个JSON数据结构，解析到一个表格中，其中表格分为两列，第一列是网址名字(innerText)，第二列是网址的连接（innerhtml）

注意 ：
1:再进行能力测试的时候，typeof不能写成typeOf
2:  //如果不加入下面一行代码则无法显示！！！甚至调试不出来
          document.body.appendChild(tableObj);
3: JSON数据结构使用逗号进行分隔，不同于在<style type="text/css"> 中使用分号进行分隔；

```html
<Html>
  <Head>
    <Title> Create JSon database and insert them into the table with innerText and Innerhtml</Title>
    <script type="text/javascript">


      onload=function()
      {
        document.getElementById('create').onclick=function()
        {
        /*
        var dic={"如鹏":"http://www.rupeng.com"; 
                "百度":"http://www.baidu.com";
                "网易":"http://www.163.com";
                "搜狗":"http://www.sougou.com";
                "新浪":"http://www.sina.com.cn";
                "github":"http://www.github.com";
        };
        */

       //用逗号隔开  而不是分号
      var dic={"如鹏":"http://www.rupeng.com", 
                "百度":"http://www.baidu.com",
                "网易":"http://www.163.com",
                "搜狗":"http://www.sougou.com",
                "新浪":"http://www.sina.com.cn",
                "github":"http://www.github.com"
        };

        var tableObj = document.createElement('table');
        tableObj.border='1 ';
        //tableObj.border='1 dashed red';
        for(var key in dic)
          {
            //create line or record
            var trObj=document.createElement('tr');
            ///create column
            var tdObj1=document.createElement('td');

            // for the safety between firefox and Ie,so talent test
            //if(typeOf(tdObj1.innerText)=='string')    typeOf应改为typeof
            if(typeof(tdObj1.innerText)=='string')
            {
              tdObj1.innerText=key;
            }else
            {
              td1.textContent=key;
            }
            var tdObj2=document.createElement('td');
            tdObj2.innerhtml='<A href=+'+dic[key]+'>'+key+'</A>';

            //add tdObj1 and tdObj2 to the line or record
            trObj.appendChild(tdObj1);
            trObj.appendChild(tdObj2);
            //add the trObj to the table
            tableObj.appendChild(trObj);


          }
          //如果不加入下面一行代码则无法显示！！！甚至调试不出来
          document.body.appendChild(tableObj);
        }     
      }
    </script>
  </Head>

  <Body>
    <Input type="button" value="create list" id="create"/>
  </Body>
</Html>
```








18：createElement 动态创建标签（无刷新评论的案例，即直接添加评论内容到页面的表格中）
      在之前我们知道只在页面加载的时候可以用document.write()来动态加载控件，那么如何在页面使用的过程中动态加载呢？ 
具体方法是利用 createElement("标签名") ，最终 会返回一个标签的对象。

案例1：  创建一个层  并在层中添加按钮   插入按钮  和删除所有按钮
注意1： 给层中加一个id的作用是为了可以供document对象进行调用
注意2：firstChild and lastChild是层的属性  区分于   removeChild()  appendChild()  insertBefore()等方法

```html

<Html>
  <Head>
    <Title>  Create Element dynamic </Title>
    <script type="text/javascript">
      onload=function()
      {
        document.getElementById('btn').onclick=function()
        {
          var divObj=document.createElement('div');

          divObj.style.width='400px';
          divObj.id='dv'; //用于解决在层中添加标签的方式
          divObj.style.height='200px';
          divObj.style.backgroundColor='red';
          divObj.style.border='1px solid yellow';

          document.body.appendChild(divObj);
          
        }
        var count=0;
        document.getElementById('btnadd').onclick=function()
        {
          var inputObj=document.createElement('input');
          inputObj.type='button';
          inputObj.value='xiaoMing'+count;
          count++;
          // divObj.appendChild(inputObj);//不起作用
          //解决办法  给divObj加一个Id
          document.getElementById('dv').appendChild(inputObj);

        }

        document.getElementById('btnInsert').onclick=function()
        {
          var inputInObj=document.createElement('input');
         inputInObj.type='button';
         inputInObj.value='xiaoming'+count;
         count++;
         var objDivD= document.getElementById('dv');
         objDivD.insertBefore(inputInObj,objDivD.firstChild); //必须先找到层中的第一个元素！ 然后插入新的元速
          
        }

        document.getElementById('btnDelete').onclick=function()
        {
         var objDivD= document.getElementById('dv');
         //firstChild 是层的第一个元素的属性
         //lastChild 是层的最后一个元素的属性

         while(objDivD.firstChild)
           {
             objDivD.removeChild(objDivD.firstChild);
           }
         
        }
      }
    </script>
  </Head>

  <Body>
      <Input type="button" id="btn" value="创建"/>
      <Input type="button" id="btnadd" value="层中创建"/>
      <Input type="button" id="btnInsert" value="层中插入"/>
      <Input type="button" id="btnDelete" value="删除层中所有元素"/>
  </Body>
</Html>
```







案例2： 提供一个评论的界面，包含昵称： 
                                                 以及评论内容框和一个input的提交框
            最终把结果展示到页面的下面表格控件中

注意：
       1: 在提交之后 ，进行name 和评论区域是否为空的判断
                        若评论内容为空，则把屏幕焦点移到评论区域，不进行提交 
                        若名称区域为空，则把屏幕焦点移到名称区域，不进行提交

       2：若没有问题，提交之后得清空name和评论区域的内容
       3：进行能力测试
            火狐和IE是两个不友好的朋友。
            typeof(tdObj1.innerText)=='string'



具体代码如下：
```html
<Html>
  <Head>
    <Title> Test comment submited without refresh 1</Title>
    <style type="text/css">
      #submitArea
      {
        width:300px;
        height:300px;
        background-color:blue;
      }
      #showArea
      {
        //这边使用分号进行隔开！！ 在js中定义json数据格式 一般用逗号隔开！！
        width:400px;
        height:500;
        background-color:green;
      }
      textarea
      {
        width:200px;
        height:200px;

      }
      table
      {
        border:1px dashed yellow;
        background-color:red;
      }
      td
      {
        border:1px solid black;
      }
    </style>
    <script type="text/javascript">
      onload=function()
      {
        document.getElementById('tijiao').onclick=function()
        {
        //获取昵称
          var names=document.getElementById('nicheng').value;
        //获取评论内容
          var content=document.getElementById('pinglun').value;
        //保存信息到一个table中
          var tableObj=document.getElementById('tb1');

          //创建一行记录
          var trObj=document.createElement('tr');
          //创建一列
          var tdObj1=document.createElement('td');
          var tdObj2=document.createElement('td');

          //设置值
/*
          tdObj1.value=names;
          tdObj2.value=content;
*/
          //忘记了使用innerText and innerhtml
          
          if(typeof(tdObj1.innerText)=='string')
          {
            tdObj1.innerText=names;
          }else
          {
            tdObj1.textContent=names;
          }
          if(typeof(tdObj2.innerText)=='string')
          {
            tdObj2.innerText=content;
          }else
          {
            tdObj2.textContent=content;
          }
                  //添加列到记录中
          trObj.appendChild(tdObj1);
          trObj.appendChild(tdObj2);

          tableObj.appendChild(trObj);

          names.value="";
          content.value="";

          document.getElementById('nicheng').value="";
          document.getElementById('nicheng').focus();
          document.getElementById('pinglun').value="";
                    

        }
      }
         
    </script>
  </Head>

  <Body>
    <Div id="submitArea">
      <Label>昵称:</Label><Input type="text" id="nicheng" value=""/>
      <Textarea id="pinglun" rows="15" cols="10"> </Textarea>
      <br/>
      <input type="submit" id="tijiao" value="提交"/>
    </Div>

    <Div id="showArea">
      <Table id="tb1">
        
      </Table>
    </Div>

  </Body>
</Html>
```





案例延展
案例3： 上面并没有进行名称区域和评论区域为空的判断
       另外的，也可以使用table.insertRow(-1)  并利用返回来的行对象创建列insertCell(-1)来创建
        一个好处是不需要像createElement，在结尾需要进行appendChild添加节点的错做。
注意点：列的值设置需要用innerText或者innerhtml进行设置（style部分保持一样）




具体如下：
```html
<Html>
  <Head>
    <Title> Test comment submited without refresh 2</Title>
    <style type="text/css">
      #submitArea
      {
        width:300px;
        height:300px;
        background-color:blue;
      }
      #showArea
      {
        //这边使用分号进行隔开！！ 在js中定义json数据格式 一般用逗号隔开！！
        width:400px;
        height:500;
        background-color:green;
      }
      textarea
      {
        width:200px;
        height:200px;

      }
      table
      {
        border:1px dashed yellow;
        background-color:red;
      }
      td
      {
        border:1px solid black;
      }
    </style>
    <script type="text/javascript">
      onload=function()
      {
        document.getElementById('tijiao').onclick=function()
        {
        //获取昵称
          var names=document.getElementById('nicheng').value;
        //获取评论内容


        //增加一个输入参数值的有效性 判断！
             if(names=="")
              {
               // alert(names+typeof(names));
                alert('请输入昵称内容');
                document.getElementById('nicheng').focus();
                return ;
              }else
              {
                var content=document.getElementById('pinglun').value;

                //alert(content+typeof(content));
                  if(content=="")
                  {
                    alert('请输入评论内容');
                    document.getElementById('pinglun').focus();
                    return false;
                  }


              }


        //保存信息到一个table中
          var tableObj=document.getElementById('tb1');

          //不需要CreateElement之后还需要 appendChild的操作
          //创建一行记录
          var trObj=tableObj.insertRow(-1);
          //创建一列
          var tdObj1=trObj.insertCell(-1);
          var tdObj2=trObj.insertCell(-1);

          //设置值
/*
          tdObj1.value=names;
          tdObj2.value=content;
*/
          //忘记了使用innerText and innerhtml
          
          if(typeof(tdObj1.innerText)=='string')
          {
             tdObj1.innerText=names;
          }else
          {
             tdObj1.textContent=names;
          }
          if(typeof(tdObj2.innerText)=='string')
          {
            tdObj2.innerText=content;
          }else
          {
            
            tdObj2.textContent=content;
          }
                  //添加列到记录中
          //names.value="";
          //content.value="";

          document.getElementById('nicheng').value="";
          document.getElementById('nicheng').focus();
          document.getElementById('pinglun').value="";
                    

        }
      }
         
    </script>
  </Head>

  <Body>
    <Div id="submitArea">
      <Label>昵称:</Label><Input type="text" id="nicheng" value=""/>
      <Textarea id="pinglun" rows="15" cols="10"> </Textarea>
      <br/>
      <input type="submit" id="tijiao" value="提交"/>
    </Div>

    <Div id="showArea">
      <Table id="tb1">
        
      </Table>
    </Div>

  </Body>
</Html>
```


19：一个综合的小案例：
   功能： 1点击按钮，则奇数行为红色，偶数行蓝色
              2鼠标移动时候，对应的对象变红色，其他的默认颜色：


```html
<Html>
  <Head>
    <Title> change the table's even and odd line's color</Title>
    <script type="text/javascript">
      onload=function()
      {
        //获得点击的按钮
        document.getElementById('changeColor').onclick=function()
        {
          //如何获得表格的长度？？？
          var tds=document.getElementById('tb1').getElementsByTagName('td');
          for(var i=0;i<tds.length;i++)
          {
            //当为奇数的时候  其实也可以用css解决方式
            if(i%2==0)
              {
                tds.style.backgroundColor='red';
              }else
              {
                tds.style.backgroundColor='blue';
              }
          }
        };

        //进行鼠标移动变色的事件
          var tds1=document.getElementById('tb1').getElementsByTagName('td');
          for(var j=0;j<tds1.length;j++)
          {
            tds1[j].onmousemove=function()
            {
              for(var k=0; k<tds1.length;k++)
              {
                  tds1[k].style.backgroundColor='';  
              }  
              this.style.backgroundColor='red';
            }
          }
      };
    </script>
  </Head>

  <Body>
    <Input type="button" id="changeColor" value="change color of the table"/>

    <Table id="tb1">
      <tr><td>第一行</td></tr>
      <tr><td>第二行</td></tr>
      <tr><td>第三行</td></tr>
      <tr><td>第四行</td></tr>
      <tr><td>第五行</td></tr>
      <tr><td>第六行</td></tr>
    </Table>
  </Body>
</Html>
```

小结： 学会了添加事件，认识了事件冒泡过程，理解了document.write的两个作用（document.writeln暂时不管 实际上只有<br>才是真正的网页换行）
          并对innerText和innerHtml进行区分，结合createElement可以动态的创建标签，弥补了document.write的不足之处（只在网页加载的时候创建），
           再次利用JSon数据结构来动态显示数据，我们会发现所有的html代码中犹如一颗颗树，可以利用三种调用标签的方式来调用所有的元素，可是我们
           总是需要写那么长的代码，可以简化？有。




   20：最后通过一个较典型的案例来结尾：
          功能1: 把JSON数据结构进行table侠士
                    功能2：设置复选框的全选
                     功能3：设置title的走马灯效果
```html
<Html>
  <Head>
    <Title>
        Tree! Tree ! Everywhere fills with trees.
    </Title>


    <script type="text/javascript">
      var de = document.documentElement;
      alert(de.tagName);

      var head=document.getElementsByTagName("Head");
      var body=document.getElementsByTagName("Body");

      alert(head);
     // alert(head.firstChild.nodeName);
      alert(body);

      var imgObj=document.getElementsByTagName("img");
      var imgObj1=document.getElementsByTagName("34");
      alert(imgObj.alt);
      alert(imgObj1.alt);

      //用于把JSON的数据结构 生成一个列表
      function createTable()
      {
        var div=document.getElementById("news");
        var table=document.createElement("table");
        table.border=1;
        //创建一个JSON数据结构，用于存储网络地址d
        var newsBlog={"如鹏网站":"http://www.rupeng.com",
          "网易博客":"http://www.163.com",
          "sina网站":"http://www.sina.com"};//可以进行无限的添加
        for(var name in newsBlog)
          {
            //创建行列结构
            var tr = document.createElement("tr"); //创建一个tr对象
            var td1 = document.createElement("td"); //创建一个第一列td1
            var td2 = document.createElement("td"); //创建一个第一列td1

            //设置行列属性
            td1.innerText=name;
            td2.innerhtml="<a href="+newsBlog[name]+">"+newsBlog[name]+"</A>";

            //添加节点
            tr.appendChild(td1);
            tr.appendChild(td2);
            //添加行到表中
            table.appendChild(tr);

          }
          //div层添加table节点
          div.appendChild(table);
          table.style.listStyle="none";
          table.style.color="blue";
      }

      //设置全选
      function setCheckedAll()
      {
        //获取所有name="setall"的标签   注意多一个s
        var allBtns= document.getElementsByName("setall");
        //var allBtns= document.getElementByName("setall");
        //获取id=”all"的标签节点
        var setOpenBtn = document.getElementById("all");
        for(var i = 0; i < allBtns.length; i++)
        {
          allBtns.checked = setOpenBtn.checked;
          allBtns.onclick = function()
          {
            var b = true;
            //为什么要进行一个循环
            //因为  再进行单选时候 得清楚所有之前的checkbox的状态！
            for(var i = 0 ; i< allBtns.length; i++)
            {
              if(!allBtns.checked)
                {
                  b = false;
                  break;
                }
                setOpenBtn.checked = b;
            }
          }
        }

      }

      //设置标签title的走马灯效果
      function scroll()
      {
        var title=document.title;
        var first=title.charAt(0);
        var last =title.substring(1,title.length);
        document.title=last+first;
      }
      setInterval("scroll()",500);

      /*
      鼠标事件
      onclick
      ondbclick
      onkeydown
      onkeyup
      onkeypress
      onmousedown
      onmousemove
      onmouseout
      onmouseover
      onmouseup
       
       
      */

    </script>
  </Head>

  <Body>
    <Div value="全选区">
      <Input type="checkbox" name="setall" />
      <Input type="checkbox" name="setall" />
      <Input type="checkbox" name="setall" />
      <Input type="checkbox" name="setall" />
      <Input type="checkbox" name="setall" />
      <Input type="checkbox" name="setall" />
      <Input type="checkbox" name="" id="all"/>
    </Div>
    <Div id ="news">

    </Div>
    <Input type="button" value="生成列表"/>
    <H1>Tree ! Everything can become trees.</H1>
    <P>Html代码层层嵌套像一颗颗<Em>树</Em>一样。</P>
    <Div>一层一层的数<Img id="34" src="" alt="如果没有图片会显示我"/></Div>
  </Body>
</Html>
```


上面为止是到第JSDom15的内容。

