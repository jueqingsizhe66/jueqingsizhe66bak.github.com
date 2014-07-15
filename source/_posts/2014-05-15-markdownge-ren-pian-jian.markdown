---
layout: post
title: "markdown个人偏见"
date: 2014-05-15 22:57:56 +0800
comments: true
categories: markdown
keyword: markdown  emphasize  category  link  picture
---

<!--more-->

###嵌套的列表###
1. 有序   数字 
2. 无序   * + 注意在写法上的增加缩进

###四个部分：###

* 重点
* 分类
* 链接
* 图片

> markdown基本上没有下划线，也没有颜色，因为hacker不需要？ 其实也需要！只不过更关注的是重点（着重  真正的问题 大小和重量）和分类（列表） 链接（源自哪边）  需要一些图片（也许颜色和下划线 可以这样引入） 还有更重要的是真实的代码快（这个地方特别讲究，所以每种语言有不同的标记，也许代码的颜色才是黑客真正的颜色），一般他们还特别注意版权（也就是引用快，所以还有> , >> etc的选项）其他的工作就由html标记，但是特别区域快的摆放，特别注意前后的空行.


+ **如果加入空行就会显示空行，如果不加入空行会把前后的数据叠加在一起，除非是加入标题。空行和加标题的一个效果类似，会使前后的数据用空行分开，另起一行！**
+ **在> 缩进的前方加入多余空格会造成*代码块*的效果**
+ **如果`<table>`标签没有`<td>`的加入没有表格的效果，还是一堆数据**

> [For any markup that is not covered by Markdown’s syntax, you simply use HTML itself][1]. There’s no need to preface it or delimit it to indicate that you’re switching from Markdown to HTML; you just use the tags.

> The only restrictions are that block-level HTML elements — e.g.`<div>`, `<table>`, `<pre>`, `<p>`,etc. — must be separated from surrounding content by blank lines, and the start and end tags of the block should not be indented** with tabs or spaces**. Markdown is smart enough not to add extra (unwanted) `<p>` tags around HTML block-level tags.

******

##[<u>一些常用的markdown没有的编辑操作：</u>][2]###

###下划线###

<u>代码:《u》下划线《/u》 </u>

中间标明:要加下划线的文字.

###删除线###
代码:《s》删除线《/s》

<s>中间标明:要加删除线的文字.</s>

###更改字体颜色：###

《font color="#value"》写上你想写的字《/font》
(其中value值在000000与FFFFFF(16位进制)之间修改#后面的数值，就可以改变字体的颜色。

###部分常用颜色代码：###

<table>
<tr><td><font color="#ff0000">#ff0000 红色的字喔！</td></font></tr>
<tr><td><font color="#ff8000">#ff8000 橙色的字喔！</td></font></tr>
<tr><td><font color="#ffff00">#ffff00 黄色的字喔！</td> </font></tr>
<tr><td><font color="#00ff00">#00ff00 绿色的字喔！</td></font></tr>
<tr><td><font color="#0080ff">#0080ff 蓝色的字喔！</td> </font></tr>
<tr><td><font color="#0000a0">#0000a0 靛色的字喔！</td></font></tr>
<tr><td><font color="#8000ff">#8000ff 紫色的字喔！</td> </font></tr>
<tr><td><font color="#000000">#000000 黑色的字喔！</td></font> </tr>
<tr><td><font color="#c0c0c0">#c0c0c0 灰色的字喔！</td></font></tr>
</table>

[1]: http://daringfireball.net/projects/markdown/syntax
[2]: http://hi.baidu.com/zolyfxughcbdkpq/item/85ea887c03081c2bd7a89cc1
