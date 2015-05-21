---
layout: post
title: "测试外部DTD约束的有效性"
date: 2015-05-11 14:58:45 +0800
comments: true
categories: xml
---
<!--more-->

问题来源：
        我本想着查查网上有没有什么方法可以用来验证dtd，于是就搜到这篇，然后开始练习。遇到的问题还真不少，java IO有些忘了，就重新查（这是一点收获） 但是现在遇到的最大的问题是 db.parse()一直提示person.xml不存在，我已经在多个地方都存放着person.xml, 也知道fileInputStream读取的相对路径应该是java项目的跟路径，甚至我把文件名的全路径都给写上，但是报的错误就是  person.xml not  found



问题的产生项目文件：（我把所有的文件都放在TestDTD的包下面）
person.xml   :第一个测试dtd的xml文件
worker.xml   ：第二个测试dtd的xml文件
person.dtd   ：dtd的约束文件
ValidateDTD.java   :用于验证person.xml  和work.xml 是否正确的收到person.dtd的约束

所有源文件：
person.dtd:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!ELEMENT person (name,sex,birthday)*>
<!ELEMENT name (#PCDATA)>
<!ELEMENT sex (#PCDATA)>
<!ELEMENT birthday (#PCDATA)>
```
复制代码
person.xml:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE person SYSTEM "person.dtd">        
<person>
        <name>Tom</name>
        <sex>male</sex>
        <birthday>1990-01-07</birthday>
</person>
```
复制代码
worker.xml:(多了一条job字段，但是是不需要的)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE person SYSTEM "person.dtd">        
<person>
        <name>Tom</name>
        <sex>male</sex>
        <birthday>1990-01-07</birthday>
        <job>Painting</job>
</person>
```
复制代码


ValidateDTD.java:（已修正的ValidateDTD程序）
```java
package TestDTD;

import java.io.File;
import java.io.IOException;
import java.net.URL;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.xml.sax.SAXException;

/**
* @author 叶昭良
* @time 2015年4月26日上午3:48:53
* @version TestDTDValidataDTD V1.0 功能： 步骤1： 步骤2： 注意： 掌握： 思考1： 思考2： 回顾：
*/
public class ValidataDTD
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                // 最好是事先打印当前的路径实在哪里
                System.out.println(new File(".").getAbsolutePath());

                System.out.println(new File(".").getName());
                System.out.println(new File(".").getPath());
                System.out.println("开始测试person.xml");
                testPerson1();
                System.out.println("开始测试worker.xml");
                testWorker();
        }

        // 利用工厂类DocumentBuilderFactory来判断DTD书写的正确性
        public static void testPerson1()
        {
                try
                {
                        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
                        dbf.setValidating(true);
                        // 利用dbf建立一个documentBuilder对象

                        DocumentBuilder db = dbf.newDocumentBuilder();
                        // 错误1
                        // db.parse(new FileInputStream(new File(".").getAbsoluteFile()+
                        // "//TestDTD//person.xml"));
                        // 错误2
                        // InputStream is = ValidataDTD.class.getClassLoader()
                        // .getResourceAsStream("person.xml");
                        /**
                         * 最常用读取properties文件的方法 InputStream in =
                         * getClass().getResourceAsStream
                         * ("资源Name");这种方式要求properties文件和当前类在同一文件夹下面。如果在不同的包中，必须使用：
                         * InputStream ins = this.getClass().getResourceAsStream(
                         * "/cn/zhao/properties/testPropertiesPath2.properties");
                         */

                        // 错误3
                        // db.parse(new FileInputStream("./src/TestDTD/person.xml"));
                        // 错误4
                        // db.parse(new FileInputStream(
                        // "C:\\Users\\708\\git\\Day2015-4-14\\src\\TestDTD\\person.xml"));
                        // db.parse(new FileInputStream(new File("E://person.xml")));
                        URL fileURLPerson = ResourceAccessSecond.class.getClassLoader().getResource(
                                        "TestDTD/person.xml");

                        db.parse(new File(fileURLPerson.getFile()));
                } catch (ParserConfigurationException e)
                {
                        System.out.println("DocumentBuilder 建立失败");
                } catch (IOException e)
                {
                        System.out.println("person.xml文件不存在");
                } catch (SAXException e)
                { // TODO Auto-generated catch block
                        e.printStackTrace();
                }

        }

        // 利用工厂类DocumentBuilderFactory来判断DTD书写的正确性
        public static void testWorker()
        {
                try
                {
                        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
                        dbf.setValidating(true);
                        // 利用dbf建立一个documentBuilder对象

                        DocumentBuilder db = dbf.newDocumentBuilder();

                        URL fileURLPerson = ResourceAccessSecond.class.getClassLoader().getResource(
                                        "TestDTD/worker.xml");

                        db.parse(new File(fileURLPerson.getFile()));
                } catch (ParserConfigurationException e)
                {
                        System.out.println("DocumentBuilder 建立失败");
                } catch (IOException e)
                {
                        System.out.println("person.xml文件不存在");
                } catch (SAXException e)
                { // TODO Auto-generated catch block
                        e.printStackTrace();
                }

        }
}

```
复制代码



1： 查阅了 dtd的语法规范，也注意到了空格要把元素和圆括号分开写，检查基本上没有拼写错误，但是还是报了person.xml的
2： 查阅了FileInputStream的用法， 有三种参数  String，Files ,另外一种比较少用，也就是既可以直接使用文件名字符串，也可以使用
       new File(文件名字符串) 又或者第三种。 未找到解决办法
补充：    之前我们一直训练的是在项目中读取的是properties（然后利用prop.load(in)的方法）,而在非项目文件中读取的是全路径问题（比如e:/person.xml)，第一次想到如果我们用FileInputStream是否读取java项目中的文件。

补充2：  myeclipse并不会报警我在使用外部约束DTD的不存在的元素，自己又不知道xml解析器是否真的晓得dtd的文件的存在，于是就想着自己               测试一下
3：于是求教 Professor Du   ，给了我两个读取项目文件的视频
视频1： 用  当前类 ValidateDTD.class.getResource()方法 
http://v.polyv.net/uc/video/iframeview?vid=e46d36780492b6d4ec8ed55f4fb04583_e&playerwidth=100
          注意： bin目录表示类加载根目录（要知道我在说什么，打开window-> show view-->navigator   
                    你会发现navigator现实的bin文件夹和  src文件夹基本上是对应的，只不过是把java文件，都变成了
                     class文件，其他文件保持原样，这也就是bin根目录的所在地和他的文档结构）   在使用 此方法必须加上斜杠开头
视频2 ： 用 当前类 ValidateDTD.class.classLoader().getResource()方法
http://v.polyv.net/uc/video/iframeview?vid=e46d367804795fd7b389ade7541a9db7_e&playerwidth=100
         注意： 与上面方法不同的是，在加载bin目录的时候，不需要使用斜杠  直接写上包名或者文件名（按照文件结构，
                         把bin目录当做根目录即可）


视频一的测试源文件：
```java
package TestDTD;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.net.URL;

/*
* 打开navigator试图，才能知道我在讲什么 window-->show view--->navigator
* 可以看到bin和src基本上是java文件变成了class其他一一对应
* 也就是一个java文件一经编译， 变成class文件都会到bin文件（规律1）
* 另外的bin文件也叫做类加载根目录（下面可以进行测试）
* 其他的比如xml dtd  txt等文件 则是直接会在bin根目录下显示
* 
* 
* 所以我们的问题是：
*          如何动态加载项目的根目录下的文件（也就是我把文件放在TestDTD下，如果
*          TestDTD包的路径一直改变（体现动态）我如何能够加载得上？
*          
*          反着来思考
*        1： 我需要FileReader或者FileInputStream 等
*        2： 也许我还需要用BufferedReader  或者BufferedInputStream来缓冲加载文件
*        3： 我需要使用FileReader还需要一个File对象
*        4： 我需要File对象  我就同时需要知道资源的全路径
*        5： 资源全路径的问题，就是如何加载bin根目录的问题！ 也就是我已经知道java编译器是如何解析相对路径
*        6： 于是我只要按照上述思路让类加载器去加载文件即可！
*        
*        注意：/ 表示的bin根目录  所以你需要在包的资源文件的开头是斜杠/
*/

public class ResourceAccess {
        public static void main(String[] args) throws Exception {
                System.out.println(ResourceAccess.class.getResource(""));
                URL fileUrl = ResourceAccess.class.getResource("/TestDTD/person.dtd");
                System.out.println(fileUrl.getFile());

                // FileReader fr = new FileReader(new File(fileUrl));
                FileReader fr = new FileReader(new File(fileUrl.getFile()));
                BufferedReader br = new BufferedReader(fr);
                String line = br.readLine();
                System.out.println("Read by BufferedReader(FileReader(File....)))");
                System.out.println(line);
                System.out.println("YEs it works!");
        }

}
```
视频二的测试源文件：
```java
package TestDTD;

import java.net.URL;

/**
* @author 叶昭良
* @time 2015年4月26日下午1:25:32
* @version TestDTDResourceAccessSecond V1.0 功能： 步骤1： 步骤2： 注意： 掌握： 思考1： 思考2： 回顾：
*          第一种加载项目根目录资源的方式 类.class.getResource(FileName);
*/
public class ResourceAccessSecond
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                // 通过类加载器 classloader加载项目资源
                // 首先classloader指定的项目的路径是什么？做一个测试
                /*
                 * URL fileURL =
                 * ResourceAccessSecond.class.getClassLoader().getResource(""); //
                 * 默认是bin目录 // fd System.out.println(fileURL);
                 */
                // 加了斜杠反而不行！！！原來classloader类不同于class的方法！
                // 1 他不需要使用斜杠
                // 2 他多了一个功能 getResources()
                URL fileURLPerson = ResourceAccessSecond.class.getClassLoader().getResource(
                                "TestDTD/person.dtd");
                System.out.println(fileURLPerson);

        }

}
```



经过这样一倒腾：
就可以使用下面方法
```java
URL fileURLPerson = ResourceAccessSecond.class.getClassLoader().getResource(
"TestDTD/worker.xml");

db.parse(new File(fileURLPerson.getFile()));
```


测试结果：
开始测试person.xml
开始测试worker.xml
Warning: validation was turned on but an org.xml.sax.ErrorHandler was not
set, which is probably not what is desired. Parser will use a default
ErrorHandler to print the first 10 errors. Please call
the 'setErrorHandler' method to fix this.
Error: URI=file:/C:/Users/708/git/Day2015-4-14/bin/TestDTD/worker.xml Line=7: 必须声明元素类型 "job"。
Error: URI=file:/C:/Users/708/git/Day2015-4-14/bin/TestDTD/worker.xml Line=8: 元素类型为 "person" 的内容必须匹配 "(name,sex,birthday)*"
复制代码
可以发现worker加了job字段，的确是检测出来了。

问题总算解决了：
主要问题还是FileInputStream并没有读入文件的问题。

