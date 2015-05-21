---
layout: post
title: "HTML1-17节笔记代码March22nd"
date: 2015-05-11 14:58:42 +0800
comments: true
categories: HTML
---
<!--more-->


1：HTML学习大纲：


<!--Html 标签是一个最大的标签，包含底下的所有标签-->

     <!--Head标签 主要包含  <Titlt   Meta  Style标签  用来充当网页的修饰部分-->

           <!--网页标题Title: 指定浏览器标题栏显示的内容 -->

         <!--Base  针对连接的全局设置-->
         <!--网页属性Meta的设置 ： 可提供有关页面的原信息-->

         <!--Link提供连接文件的css和js文件的导入-->
         <!--定义样式-->

   <!--Body标签，作为网页内容的主体部分-->

        <!--Div 盒子标签 用来存标签-->
              <!--H1标签 样式标题1-->
              <!--Form  :action 指定的服务器地址处理表单-->
                    <!--1：段落标签  P-->
                         <!--Span标签 一个小控件  不断行  P标签会断行-->
                         <!--Input标签，包含多种表单控件，比如radio  checkbox    password  text 等10种-->
             <!--Ul无序列表Unorder list-->
                   <!--Li 列表行-->
             <!--Ol有序列表 OrderList--> 
                   <!--Li 列表行-->
             <!--Dl定义列表-->
                   <!--Dt列表行-->
         
            <!--3： 图像标签Img alt值不能超过1024个字符-->
            <!--4：表格标签Table-->
                           <!--Caption 表格标题-->
                           <!--Thead表头-->
                                    <!--Th-->
                           <!--TBody 表内容-->
                                    <!--Tr行数据-->
                                               <!--Td列标签-->
                           <!--TFoot表尾-->
            <!--5:超链接A实验-->
          <!--6:IFrame框架的测试-->
          <!-- 7: Form表单的测试-->


实验阶段：
<!DOCTYPE html  >
<!--Html 标签是一个最大的标签，包含底下的所有标签-->
<Html>
  <!--Head标签 主要包含  <Titlt   Meta  Style标签  用来充当网页的修饰部分-->
  <Head>
    <!--网页标题: 指定浏览器标题栏显示的内容 -->
   

<Title> 电子名片自动绑定系统</Title>


    <!--网页属性的设置 ： 可提供有关页面的原信息-->
    <!--
      content属性  定义与http-equiv 或name属相相关的元信息
      http-equiv属性: 把content属性关联到HTTP头部
      name属性：把content属性关联到一个名称

    -->
    <!--base  针对连接的全局设置-->
    <!--
     target 指定连接全局打开方式
     href  指定全局前缀比如  www.baiud.com  但是不可能要求所有连接以 www.baidu.com打开！
    -->

<Base target="_blank">



    <!-- 通过name content对   对应  key-value 键值对   http-equav 针对不同的类型 类似于Input控件 的type存在10种类型-->

<Meta name="Generator" content="vim"/>
    <Meta name="Author" content="yezhaoliang"/>
    <Meta name="Keywords" content="name picture"/>
    <Meta name="Description" content=""/>


    <!--做优化的时候可能需要用到keywords 优化搜索-->

<Meta http-equiv="keywords" name="DNS" content="直接数值模拟"/>


    <!-- content=3 表示3s 之后刷新出  163.com网页-->
    <!---  太烦了  3s 刷新  注释掉
<Meta http-equiv="refresh"  content="3;url=http://www.163.com"/>
    --> 

<Link type="text/css" rel="stylesheet" href="">
    <Link type="javascript" rel="stylesheet" href="">


    <!--
     Link 控件
       rel属性  ：目标文档与当前文档的关系
       type属性 ： 文档类型
    -->
      <!--设background:url(b.jpg);置背景颜色 和文字字体-->
    <!--定义样式-->

<Style type="text/css">
      body{font-family:"微软雅黑"}
      .box{width:600px; margin:0 auto; padding-top:100px;}
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



  </Head>

  <!--Body标签，作为网页内容的主体部分-->
  <Body>
    <!--输入区域  start-->
    <!--Div 盒子标签 用来存标签-->
    <Div class="box">
      <!--H1标签 样式标题1-->

<H1>
        电子名片自动绑定系统
      </H1>


      <!--1：段落标签  P-->

      <!--
      Form  :action 指定的服务器地址处理表单
      method Form表单提交方式    默认get
               Post
      -->
      <!--<Form action="www.163.com" method="get">-->
      <Form action="http://www.163.com" method="get">
      <P>
        <!--Span标签 一个小控件  不断行  P标签会断行-->
        <!--Input-->

<span>姓名： </span> <input type="text" class="input_box"/>  
        <span>公司： </span> <input type="text" class="input_box"/>


      </P>
      <P>

<span>职务： </span> <input type="text" class="input_box"/>
        <span>手机： </span> <input type="text" class="input_box"/>


      </P>
      <!--
      Input控件的type值共有10个值
        type="text"    <-----文本按钮
        type="submit"  <-----提交按钮
        type="hidden"    <----隐藏字段  给服务器读取的，客户端不现实的，隐藏照样提交！也有name-value 对
        type="radio"    <-----单选框
        type="password" <-----密码框
        type="checkbox" <-----复选框
        type="reset"    <-----重置按钮
        type="button"   <-----按钮
        type="file"     <-----文件上传
        type="image"    <-----图像
        type="text"
        type="text"
      -->

<Input type="submit" value="生成名片" class="input_btn"/>
      <Input type="button" value="按钮" class="input_btn"/>
      <Input type="image" value="图像" class="input_btn"/>
      <Input type="file" value="文件上传" class="input_btn"/>
      <Input type="reset" value="重置按钮" class="input_btn"/>
      <Input type="checkbox" value="复选框" class="input_btn"/>
      <Input type="radio" value="单选框" class="input_btn"/>


      <!--新加入的部分，另外一种使用radio控件-->
      <!--
        注意：
            Input的  name  value  其实就是key=value的组合！！hashmap的过程！
            观察发现所有的表单控件都是基于HashMap的过程
            也就是所有的Input控件都有name value的属性，充当表单信息，这很重要
      -->

<Input type="radio" name="gender" value="男"/>男<Input type="radio" name="gender" value="女">女</br>
      <Input type="password" value="密码框" class="input_btn"/>
      <Input type="hidden" value="隐藏字段" class="input_btn"/>


      </Form>
    </Div>
        <!--2：列表实验-->
    <Div>
      <!--无序列表-->

<Ul>
        <Li>流体</Li>
        <Li>结构</Li>
        <Li>耦合</Li>
      </Ul>


      <!--有序列表-->

<Ol>
        <Li>Ansys</Li>
        <Li>Fluent</Li>
        <Li>ICEM</Li>
      </Ol>



<A name="double1">定位标记</A>


      <!--定义列表-->

  <Dl>
        <Dt>网格</Dt>
          <Dd>ICEM</Dd>
          <Dd>Gambit</Dd>
          <Dt>求解器</Dt>
          <Dd>Fluent</Dd>
          <Dd>Numeca</Dd>
          <Dd>Abaqus</Dd>
      </Dl>



    </Div>
    <!--3： 图像标签 alt值不能超过1024个字符-->

<Div>
      <img src="b.jpg" border="3px" title="书签" alt="资源找不到时候，我就出现了",width=500px,height=200px/>
    </Div>


    <!--4：表格标签-->
    <!-- Table属性：cellpadding 单元格内容与边框的距离
                    cellspacing 单元格与相邻单元格之间的距离 
          Td常用属性：
                    colspan:合并单元格
                    rowspan：合并同列单元格
          每个Table可以有一个表头Thead、表尾TFoot和一个或多个标题TBody
          TBody的分行下载是一个网页优化的一个方法
    -->

<Div>
      
      <Table border=2px; width=300px; cellpadding=1px; cellspacing=1px;>
        <Caption>关于表格的实验</Caption>
        <Thead>
          <Th>姓名</Th> <!--对表格的第一行或者第一列进行格式化 c粗体剧中显示-->
          <Th>年龄</Th>
        </Thead>

        <TBody>
          <Tr> <!--行标签-->
            <Td>张三</Td><Td>20</Td>  <!--单元格标签-->
          </Tr>
          <Tr> <!--行标签-->
            <Td>李四</Td><Td>30</Td>  <!--单元格标签-->
          </Tr>
        </TBody>
      </Table>
    </Div>



    <!--5:超链接实验-->
    <!--
      _blank  在一个新的网页标签打开链接
      _parent 在父级窗口中打开
      _self   在自身页面中打开链接（默认）
      _top    在整个浏览器的最顶端(前端)开始打开链接

      定位标记+超链接
      <A name="标记名字"> 标记在某一个位置</A>
      找到他
      <A href="#标记名字">返回标记位置</A>

      注意： 1 定位标记要和超链接结合使用才有效  标记名字用#找到
             2 在网页过长才有效果 
    -->

<Div>
      <P><A href="http://www.163.com" target="_blank">网易</A></P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <P>..</P>
          <A href="#double1">去寻找标记</A>
    </Div>



    <!--6:IFrame框架的测试-->
    <!--
      在网页上嵌入一个框，显示另一个html,txt等文件信息
    -->

  <Div>
      <IFrame src="student.txt">
        如果你看到这段文字，说明您的浏览器不支持IFrame标签
        </IFrame>
    </Div>


    <!-- 7: Form表单的测试-->
    <!--
      Form是最常用的控件，主要用于采集和提交用户输入的信息
      和服务器进行交互
    -->
    <Div>
    <!--关于Input测试已在前面提及-->
    <!--Select 其实就是Combobox-->

<Select name="city">
      <Option value="1">北京</Option>
      <Option value="2">福建</Option>
      <Option value="3">广州</Option>
    </Select>



    <!-- 更好的办法是使用CSS的height & width替换掉rows cols-->

<Textarea rows="3" cols="20"></Textarea>



    <!--Radio type 和 Label配合使用 -->
    <!--Label标签做了一个字段的包装，这样用户点击字段 也可以选择！！！新的知识点啊！-->

<Label for="male"> Male</Label>
    <Input type="radio" name="sex" id="male"/>
    <Label for="female"> Female</Label>
    <Input type="radio" name="sex" id="female"/>




    </Div>

    <!--这么有趣  HTML也有块级元素的说法-->
    <!--
     1: 块级元素 通常以新行开始和结束
     比如   <H1>  <P> <Ul>  <Table>   <Tr>  <Div>
     2：内联元素 通常不以新行开始
     <Td> <B> <A> <Img>  <Span>
      3: Div   and   Span  主要用于文档的样式属性的区域设置
    -->
  </Body>

</Html>



<!--Framset 用于设置框架的大小比例-->

<!--Frame用于加载网页文件-->



<Html>
  <!--Framset 用于设置框架的大小比例-->
  <!--
    frameset常用属性：
       cols=20%,*   垂直切割画面，左右两个画面
       Rows="120,*"  横向切割，上下分开
       frameborder    0 and 1. 0 表示不要边框 
       border="0"  边框厚度
       bordercolor='"#99"
       framespace="5" 框架与框架间保留的空白距离
  -->

<Frameset rows="20%,*">


    <!--Frame用于加载网页文件-->
    <!--
    frame常用属性
    src="*.html" 每个frame框对应一个网页档案
    name="top" 设置这个窗的名称
    scrolling=auto|yes|no
    noresize="noresize"   表示框框不可以拖拽！！

    -->
    <!--
    Frameset = Noframes+frame-->

<Noframes>
      <Div>
        当你的浏览器不支持Frame时候会显示这段文字，一般很少。 
      </Div>
    </Noframes>



  <Frame src="rupeng.html">



<Frameset cols = "25%,75%">
      <Frame src="plan.html">
      <Frame src="student.txt">
    </Frameset>


  </Frameset>
</Html>

Frame Name的使用

```html
<Html>
  <Head>
    <Meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <Title> 我喜欢的样式 </Title>
  </Head>
  <Frameset rows="12%,*" frameborder="1" border="1px" bordercolor="#FF0000">
    <Frame src="top.html" scrolling="no" noresize="noresize"/>
    <FrameSet cols="20%,*">
      <!-- name的作用是让left.html的A连接的target属性 可以选择以name名字打开！！
            这是一个全新的知识点，妙-->
      <Frame src="left.html" name="left"/>
      <Frame src="right.html" name="right"/>
      </Frameset><Noframes></Noframes>
</Html>
```

