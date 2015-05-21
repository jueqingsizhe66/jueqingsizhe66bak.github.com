---
layout: post
title: "JSDOM14-19节笔记代码April1th"
date: 2015-05-11 14:58:44 +0800
comments: true
categories: JSDom
---

<!--more-->
续先前的JSDOM1-15:http://www.rupeng.com/forum/thread-45297-1-1.html

21:图片移动，脱离文档流的案例

脱离文档流： 对象.style.position='absolute';
新建了一个图片控件，并让其在页面中随鼠标移动。

```html
<Html>
<Head>
<Title> Let bird fly</Title>
<script type="text/javascript">

onload=function()
{
/*
document.onmousemove=function()
{
var imObj = document.getElementById('im');
//脱离文档流

imObj.style.position='absolute';
imObj.style.left=window.event.clientX+'px';
imObj.style.top=window.event.clientY+'px';
};
*/
document.getElementById('ak').onmouseover=function()
{
if(!document.getElementById('im1'))
{
var imgObj=document.createElement('img');
imgObj='im1';
imgObj.src='bird.png';
//脱离文档流
imgObj.style.position='absolute';
imgObj.style.left=this.offsetLeft+'px';
imgObj.style.top=this.offsetTop+this.offsetHeight+'px';
}
};
};
</script>

</Head>

<Body>
<img id="im" src="bird.png"/>

<A id="ak" href="http://www.baidu.com">百度</A>
</Body>
</Html>
```
22： 四个案例




案例1：一个列表框进行动态属性的赋值 并获取点击对象的值
                  注意：setAttribute and gettAttribute的用法
                           循环添加所有表格的点击事件
案例2：产生一个登陆界面
                  注意：innerText和innerHTML的配合使用 ，设置控件的内容
                           为了增加用户的体验感觉checkbox一般设置对象.style.cursor=pointer
                            createElement 和appendChild的配合使用
案例3: 小图变大图
                   注意：JSON数据库的创建
                             循环的显示小图片
                             图片的onmouseover and onmouseout事件的响应
                              层的style.display=none;的意思是不显示该层
案例4:   搜索框
                   注意：onfocus  and onblur(鼠标焦点离开的事件）
                            一般会使用设置为gray，文本框为'请输入搜索内容'; 使用的时候设置为black，并且文本框为空。


```html
<Html>
  <Head>
    <Title> When click the button ,then generate the div</Title>
    <script type="text/javascript">
      // 学会添加层
      //数据库 包含照片信息  其中左边的key是小照片，右边是大照片信息
       var datasPic = {
        "mv/1-1.jpg": ["mv/1.jpg", "小丽", "163cm"],
        "mv/2-1.jpg": ["mv/2.jpg", "小红", "165cm"],
        "mv/3-1.jpg": ["mv/3.jpg", "小花", "150cm"]
        };


        //案例1：一个列表框进行动态属性的赋值 并获取点击对象的值
      onload=function()
      {
        //点击列表 产生分数来！！
        var tds=document.getElementById('nameRank').getElementsByTagName('td');
        for(var i=0;i<tds.length;i++)
        {
          tds[i].setAttribute('score',(i+1)*20);
          tds[i].onclick=function()
          {
            alert(this.getAttribute('score'));
          };
        }

        //案例2：产生一个登陆界面
        document.getElementById('generate').onclick=function()
        {
          //创建一个层
          var dvObj=document.createElement('div');
          dvObj.id='dv';
          dvObj.style.width='300px';
          dvObj.style.height='200px';
          dvObj.style.marginBottom='200px';
          dvObj.style.border='1px solid yellow';
          document.body.appendChild(dvObj);

          //c创建两个P标签
          var P1= document.createElement('p');
          //本想着添加标签来着
          P1.innerText='用户名:';
          var name=document.createElement('input');
          name.type='text';
          P1.appendChild(name);

          //第二个标签
          var P2 = document.createElement('p');
          //妙用innerHTML保持对齐！
          P2.innerHTML='密 码:';
          var pwd=document.createElement('input');
          pwd.type='password';
          P2.appendChild(pwd);

          //复选框

          var chk = document.createElement('input');
          chk.type = 'checkbox';
          chk.id = 'chkPwd';
          var labelChk = document.createElement('label');
          labelChk.innerText = '记住我的登录状态';
          labelChk.setAttribute('for', chk.id); //设置属性
          labelChk.style.cursor = 'pointer';//设置鼠标的样式 编程手的样式
          var pObj3 = document.createElement('p');
          pObj3.appendChild(chk);
          pObj3.appendChild(labelChk);

          //登陆和取消按钮
          var btnLogin = document.createElement('input');
          btnLogin.type = 'submit';
          btnLogin.value = '登录';
          btnLogin.marginRight='10px'
          var btnEsc = document.createElement('input');
          btnEsc.type = 'button';
          btnEsc.value = '取消';
          var pObj4 = document.createElement('p');
          pObj4.appendChild(btnLogin);
          pObj4.appendChild(btnEsc);



          //添加控件
          dvObj.appendChild(P1);
          dvObj.appendChild(P2);
          dvObj.appendChild(pObj3);
          dvObj.appendChild(pObj4);

        }


      //案例3: 小图变大图
      //0 先有了json数据库
      //1 也可以先加载小图

      //2
        var dvSObj=document.getElementById('dvSmall');
        //先把小图给显示出来
        for(var key in datasPic)
          {
            var imObj=document.createElement('img');
            imObj.src=key;
            imObj.setAttribute('userKey',key);
            imObj.style.marginRight='10px';//目的是添加右间距/
            dvSObj.appendChild(imObj);
            //添加小图的鼠标的到来事件
            imObj.onmouseover=function()
            {
              var arrs=datasPic[this.getAttribute('userKey')];
              document.getElementById('dvBig').style.display='block';
              document.getElementById('imBig').src=arrs[0];
              document.getElementById('spName').innerText=arrs[1];
              document.getElementById('spHeight').innerText=arrs[2];
            };
            //添加小图的鼠标的 离开事件
            imObj.onmouseout=function()
            {
              document.getElementById('dvBig').style.display='none';
            };
          }

          //案例4:   搜索框
          //获得焦点的事件
          document.getElementById('searchValue').onfocus=function()
          {
            if(this.style.color=='gray'&& this.value=='请输入搜索词')
              {
                this.style.color='black';
                this.value='';
                
              }
          }
          //失去焦点的事件
          document.getElementById('searchValue').onblur=function()
          {
            if(this.value=='')
              {
                this.style.color='gray';
                this.value='请搜索搜索关键词';
              }
          }

      };

    </script>
  </Head>

  <Body>
    <Table border="1" id="nameRank">
      <Tr>
        <Td>第一名</Td>
        <Td>第二名</Td>
        <Td>第三名</Td>
        <Td>第四名</Td>
        <Td>第五名</Td>
      </Tr>
      </Table>

      <Input type="button" id="generate" value="登陆栏"/>

      <Div id="dvSmall">

      </Div>

      <!--display:none 表示不显示 也就是刚开始不会显示div层-->
      <Div id="dvBig" style="background-Color:orange; border:1px solid yellow; width:200px;display:none;">
          大头像:<br/>
          <Img id="imBig" src="#" alt="如果没有图片 You will see me"/>
          姓名:<Span id="spName"></Span><br/>
          升高:<Span id="spHeight"></Span><br/>

      </Div>

      <Div id="search">
        <Input type='text' value="请输入搜索词" id="searchValue" style="color:gray"/>
        </Div>
  </Body>
</Html>
```
23 表格的提交的两种触发方式





submit按钮的click()
form的submit()
区别：
            调用submit按钮的点击事件click();
            这种情况下 如果onsubmit被赋值了，比如
        //处理click的提交事件  type=subomit的click()事件会触发 onsubmit监听器
        if(document.getElementById('searchValue').value=='Please input the search value')
          {
            document.getElementById('fm').onsubmit=function()
            {
              return false;
            };
复制代码

            那么submit按钮 失去作用，即使再输入内容 也没用！！
            document.getElementById('sm').click(); 也就是click()的调用需要经过onsubmit的匿名函数过程。
            submit()不会触发 onsubmit的匿名函数，即使文本框没有内容也直接提交。
            直接达到高层的表单form，进行提交，



```html
<Html>
  <Head>
    <Title> Test onclick() and onsubmit() </Title>
    <script type="text/javascript">
      onload=function()
      {
        document.getElementById('searchValue').onfocus=function()
        {
           if(this.style.color=='gray'&&this.value=='Please input the search value') 
             {
               this.value='';
               this.style.color='black';
             }
        }
        document.getElementById('searchValue').onblur=function()
        {
            if(this.value=='')
              {
                this.value='Please input the search value';
                this.style.color='gray';
              }
        }

        //处理click的提交事件  type=subomit的click()事件会触发 onsubmit监听器
        if(document.getElementById('searchValue').value=='Please input the search value')
          {
            document.getElementById('fm').onsubmit=function()
            {
              return false;
            };
          }
          document.getElementById('dv').onclick=function()
          {
            //点击层的 时候调用submit的点击事件
            //这种情况下 即使再输入内容 也没用！！
        //    document.getElementById('sm').click();
            //submit不会触发 onsubmit的匿名函数
            //直接达到高层的表单form，进行提交，
            document.getElementById('fm').submit();
          }
          document.getElementById('sm').onclick=function()
          {
            //this.click();
            document.getElementById('sm').click();
          }

      }
    </script>
  </Head>
    
  <Body>
    <Form action="http://www.baidu.com" method="get" id="fm">
      <Input type="text" name="name" value="Please input the search value" id="searchValue" style="width:300px;height:20px;color:gray;
      border:1px solid red"/> 
      <Input type="submit" name="name" value="baidu" id="sm"/>
      <Div id="dv" style="width:300px;height:200px;background-color:green;">
        </Div>
      </Form>
  </Body>
</Html>
```


24：正则表达式



       如果有了// 单句注释符，那么new RegExp('')就可以不用了
       注意1： test函数类似于java的match函数
                   exec函数类似于java的matches函数。

三个功能： 1：找到六位数字
                 2：确认文本框的内容是否为邮箱，若是为红色
                 3：替换操作，支持链式替换 *.replace().replace....


```html
<Html>
  <Head>
    <Title> Test Regex Expression </Title>
    <script type="text/javascript">
      var testString='12345';

      //1 网页字节数较多 不推荐
      var reg= new RegExp('\\d{5}');
      alert(reg.test(testString)); 
      //2 网页字节数较少 推荐  增加体验性

      var reg=/\d{5}/; //  //类似.Net的 @（）可以直接使用正则表达式 而不需要考虑具体的语言
      alert(reg.test(testString));

      
    </script>

    <script type="text/javascript">

      //获得所有匹配的内容
      var msg='昌平:102206,平和:363708,中国电信:10000';
      var reg=/\d{6}/g;
      var result;
      while(result=reg.exec(msg))
        {
          alert(result);
        }
    </script>



    <script type="text/javascript">
      onload=function()
      {
        document.getElementById('email').onblur=function()
        {
          // di第一个错误  A-Z 写成了a-z
          // 第二个错误  var reg =  写成了 var=reg=
          var reg=/^[\d\w_.-]+@[\d\w]+([.][a-zA-Z]+){1,2}$/;
          //var reg=/^[0-9a-zA-Z_.-]+@[0-9a-zA-Z]+([.][a-zA-Z]+){1,2}$/;
          if(reg.test(this.value))
            {
              this.style.backgroundColor='red';
              alert('邮箱');
            }
        }
      }
    </script>

    <script type="text/javascript">
      
      var msg = '   去掉左右空格    ';
      //注意// 不需要用 单引号括起来
      msg = msg.replace(/^\s+/,'').replace(/\s+$/,'');
      alert('==='+msg+'===');
    </script>
  </Head>

  <Body>
      <Input type="text" id="email" name="" value=""/>
  </Body>
</Html>
```

25:测试键盘的keyCode

      注意如果不放在onload中调用控件没有效果，为undefined
```html
<Html>
  <Head>
    <Title>Test Tab Shift </Title>
    <script type="text/javascript">
      //window.event.keyCode=

      //如果不放在onload中是没有效果
      onload=function()
      {
        var texts=document.getElementsByName('name');
        for(var i=0;i<texts.length;i++)
        {
          texts[i].onkeydown=function()
          {
            //alert(window.event.keyCode);
            //让enete键具备 tab的工呢过
            if(window.event.keyCode==13)
              {
                //IE可以实现
                window.event.keyCode=9;
                //google如何实现
              }
          };
        }
      };
      
    </script>
  </Head>

  <Body>
    <input type="text" name="name" value=""/>
    <input type="text" name="name" value=""/>
    <input type="text" name="name" value=""/>
    <input type="text" name="name" value=""/>
    <input type="text" name="name" value=""/>
    <input type="text" name="name" value=""/>
    <input type="text" name="name" value=""/>
    <input type="text" name="name" value=""/>
  </Body>
<Html>
```
26:测试密码的强弱




密码强弱等级协定：
         1：初始的密码等级为0
         2：当只出现数字或者只出现字母或者只出现特殊字符则密码等级加1
         3： 如果数字、字母、特殊字符 三者有两种出现 则再加上1   
         4： 如果都出现  在原先基础上再加1 
         5： 如果位数小于6 － 1  所有最少应该是0


  第一：获取文本框的内容

第二：进行函数的级别验证
第三：设置表格的背景颜色

注意1   if(msg.match(/[^\d\w]/)) //既不是数字也不是字母
注意2  在进行 新的级别的设置时候需要循环把表格背景颜色设置为white



```javascript
for(var i=0;i<tds.length;i++)
             {
               tds[i].style.backgroundColor='white';
             }
```
具体实验如下:

```html
<Html>
  <Head>
    <Title> Test the strong week and medium of the password </Title>
    <script type="text/javascript">

      /*
         1：初始的密码等级为0
         2：当只出现数字或者只出现字母或者只出现特殊字符则密码等级加1
         3： 如果数字、字母、特殊字符 三者有两种出现 则再加上1   
         4： 如果都出现  在原先基础上再加1 
         5： 如果位数小于6 － 1  所有最少应该是0
      */

     function getPasswordLevel(msg)
     {
       var level=0;
       if(msg.match(/\d/))
         {
           level++; //如果匹配数字则加上1
         }
        if(msg.match(/\w/))
          {
            level++; //如果也找到字母
          }
        if(msg.match(/[^\d\w]/)) //既不是数字也不是字母
           {
            level++;
           }
        if(msg.length<=6)
          {
            level--;
          }
          return level;
     }

     onload=function()
     {
       document.getElementById('password').onkeyup=function()
       {
         var tds=document.getElementById('tr1').getElementsByTagName('td');
         if(this.value.length>0) //value的值的长度
           {
             for(var i=0;i<tds.length;i++)
             {
               tds[i].style.backgroundColor='white';
             }
              var r =getPasswordLevel(this.value); 
              if(r <=1)
                {
                  tds[0].style.backgroundColor='red';
                }
              else if(r ==2)
                {
                  tds[0].style.backgroundColor='yellow';
                  tds[1].style.backgroundColor='yellow';
                }
              else if(r ==3)
                {
                  tds[0].style.backgroundColor='blue';
                  tds[1].style.backgroundColor='blue';
                  tds[2].style.backgroundColor='blue';
                }

           }
       }
     }
    </script>
  </Head>
  <Body>

    <Input type="text" id="password" value="" name="">
    <Table id="tb">
      <Tr id="tr1">
        <Td>弱</Td>
        <Td>中</Td>
        <Td>强</Td>
      </Tr>
      </Table>
  </Body>
</Html>
```
27:百度搜索




功能1： 输入一个字可以出现智能提示板（暂时是指调用JSON的数据  而不是数据库的内容）
功能2：可以获取智能提示板的内容，并鼠标扫过的时候出现红色 离开为白色  点击则把该对象内容附到搜索框中


注意点: 能力校验（暂时不太清楚 userAgent)




具体实现如下：

```html
<Html>
  <Head>
    <Title>Test Search by Baidu </Title>
  </Head>
  <style type="text/css">
    #main
    {
      width:250px;
      height:200px;
      margin-left:200px;
      margin-top:300px;
      background-color:green;
    }
  </style>

  <script type="text/javascript">
    onload=function()
    {
      var keyWords = {
          "小明": ["小明是谁", "小明很机智", "小明滚出去"],
          "地主": ["斗地主", "地主还有没有", "地主就是土豪"],
          "挖": ["挖掘机", "挖掘机技术那加强", "挖土豆"],
          "我": ["我爱北京", "我爱北京天安门", "我是苍"],
          "苍":["苍天大地","苍老师是大家的","苍天有井独自空"],
          "平":["平凡的世界","平白无故","平常"]
      };

      function funShow()
      {
        //如果在数据库中存在的话  则创建一个层
        if(keyWords[this.value])
          {
            //create the div
            if(document.getElementById('dv'))
              {
                //不是 直接'dv'
                document.body.removeChild(document.getElementById('dv'));
              }
            //开始创建
              var divObj=document.createElement('div');
              divObj.id='dv';
              divObj.width='300px';
              //不要少了  脱离文档流，才可以重新设置坐标
              divObj.style.position='absolute';
              divObj.style.border='1px solid purple';
              divObj.style.left=this.offsetLeft+'px';
              //divObj.style.left=tihs.offsetLeft+'px';
              divObj.style.top=this.offsetHeight+this.offsetTop+'px';
              document.body.appendChild(divObj);

              for(var i=0; i< keyWords[this.value].length;i++)
              {
                var pObj=document.createElement('p');
                pObj.innerHTML=keyWords[this.value][i];
                pObj.style.margin='5px';
                divObj.appendChild(pObj);
                pObj.onmouseover=function()
                {
                  this.style.backgroundColor='red';
                  this.style.cursor='pointer';
                }
                pObj.onmouseout=function()
                {
                  this.style.backgroundColor='';
                };
                //增加了点击的事件
                pObj.onclick=function()
                {
                  //alert(this.value);
                  document.getElementById('searchValue').value=this.innerText;
                }
              }
          }
        }
            //firefox下检测状态改变只能用oninput,且需要用addEventListener来注册事件。
            if (/msie/i.test(navigator.userAgent))    //ie浏览器
            {
                document.getElementById('searchValue').onpropertychange = funShow;
            }
            else {//非ie浏览器，比如Firefox
                document.getElementById('searchValue').addEventListener("input", funShow, false);
            }


    }
  </script>

  <Body>
    <Div id="main" >
      <Input type="text" name="" id="searchValue" value=""/>
      <Input type="button" name="" id="tijiao" value="Baidu It"/>
    </Div>
  </Body>
</Html>
```


28：JS常用习惯




多用js内部的函数
少用全局变量
利用数据结构来定义对象
尽可能使用switch代替else-if语句
减少字节数，增加用户体验



29：FireFox(FF)和IE的一些不同点

IE:innerText    FF:textContent
IE:srcElement(事件源）   FF：target
IE:attachEvent   FF:addEventListener   来绑定事件
http://www.360doc.com/content/09/0319/12/16915_2855107.shtml


JQuery是一个好东西，主要目的是为了统一不同浏览器使用DOM的不同点，屏蔽了不友好的功能！ 这也是下阶段的学习目标。



