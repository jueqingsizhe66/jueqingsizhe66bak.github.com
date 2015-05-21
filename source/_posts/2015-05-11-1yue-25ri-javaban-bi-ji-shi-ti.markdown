---
layout: post
title: "1月25日Java班笔记试题"
date: 2015-05-11 14:58:40 +0800
comments: true
categories: JavaBasic
---
<!--more-->


一、IO基础
        1.File在Java中，既可以表示文件，也可以表示目录。可以通过File对象来判定文件是否可读，可写，存在等。
        2.File对象的mkdirs()推荐使用这个方法，此方法可以创建多层次的目录。而mkdir()只能一层一层创建。
        3.File的createNewfile()是创建的文件。
        4.File的delete()删除的File的构造函数String的那最后的一级，如果是文件，可以删除。如果是目录，需要这个目录是空的才能够删除。
        5.File的listFiles()返回的是File构造函数指定路径下的所有File对象，包括了目录，也包括了文件。
        6.File的静态方法，listRoots()可以获取所有的盘符。
        7.OutputStream和InputStream都是字节流。
        8.以后对于流的关闭等操作，还是使用JDK1.6的形式。1.7的怕以后公司不用。
        9.NullPointerException是空指针异常。
        10.当通过InputStream的read()方法读取值的时候他是一个byte一个byte的读取的，所以对于汉字读取会出现乱码。
        11.java中数字是1个byte，汉字和汉字标点是2个byte。
        12.可以通过System.currentTimMillis()来获取1970年1月1日到现在的毫秒数。
        13.在Java中，字节流的读取推荐使用read方法，对于read方法，是把一个流的内容读取到byte[] 这种缓冲区中。且如果读取到了最后，返回值是-1.C#是0.所以如果使用do-while需要判定下！
        14.缓冲区也不是越大越好！一般来说1M就够了！1024*1024
        15.closeable为哪些可以关闭流的接口。所以封装finally中的close方法的时候，可以使用closeable的多态来表示完成。
        16.对于output流，不用写创建一个File。会自动覆盖。
        17.对于InputStream每次读取的是一个byte，就是一个byte一个byte的读，本质来说，其实是非常费时间的。
        18.FileInputStream在read到最后返回值是-1.如果不是-1的话，那么返回值为int。int代表的是ASCII(?)的char。所以强制转成char就可以获取汉字。
        19.InputStream在读取汉字的时候，内部是在做判定的，本质来说他也是一个byte一个byte的在读，但是没读取一个byte，他会判定是否是中文特征码，如果是那么会在读下一个byte。合起来返回一个char。
        20.为了规避乱码问题，用什么编码来写，就用什么编码来读。
        21.ANSI编码不是一种真正意义上的编码，而是根据不同的操作系统来设定不同的编码。就是default编码。
        22.关于字符串编码的设置是在InputStreamReader或者OutputStreamWriter中。
        23.在关闭流的时候，推荐从外到内依次关闭。
        24.BufferedReader读取到最后，返回时是null。
        25.获取默认编码的2个方法 System.getProperty("file.encoding")  还有一个是 Charset.defaultCharset().toString()

26. 文件夹的遍历  可以通过listFiles(), 如果是文件夹，则递归调用该定义方法
27.BufferedReader 需要一个Reader对象充当其参数，一般是InputStreamReader, 而InputStreamReader又需要一个InputStream（抽象方法不能new处对象）类的对象，一般采用InputStream的子类FileInputStream，类似的BufferedWriter
28.通过创建Properties对象可以载入项目的配置文件。一般有两种方法载入文件 ，1是 直接通过项目文件载入 调用当前类的getClass方法，并接着调用getResourceAsStream载入文件流  ，通过Properites对象的load方法载入 。（得写好b.properties的路径  com/rupeng/b.properties 表示在com.rupeng这个包下的b.properties文件） 2 直接通过FileInputStream创建一个文件流，然后再用Properties对象的load方法载入。两者区别未知，效果一样。




CommonsIO的学习
统一说明：
import org.apache.commons.io.IOUtils; // 加载类库 
import org.apache.commons.io.FilesUtils; // 加载类库 

1： IOUtils
案例1：从一个URL中读取字节并打印
主要代码：
```java
InputStream in = null;
                try
                {
                        in = new URL("http://www.rupeng.com").openStream();
                        System.out.println(IOUtils.toString(in,"utf-8"));
                } catch (MalformedURLException e)
                {
                        // TODO 自动生成的 catch 块
                        
                } catch (IOException e)
                {
                        // TODO 自动生成的 catch 块
                        e.printStackTrace();
                }finally
                {
                        IOUtils.closeQuietly(in);
                }
```

在大多数的运用领域，上述的CommonsIO操作是非常常见的，并且会节省很多的时间。对于工具代码，比如IOUtils,灵活性和速度
占据主要的作用。然而，在使用过程中也需要注意他的缺点，比如如果使用上述的技术读取一个1GB的大文件，将导致产生一个
1GB的字符串对象。

案例2:复制文件
主要代码
```java
                try
                {
                        fis = new FileInputStream("e://a.txt");
                        fos = new FileOutputStream("e://acopyByStream.txt");
                        IOUtils.copy(fis, fos); // 把e盘下的a.txt复制到e://的acopyByStream.txt
                }
```


2：FileUtils
案例1：复制url内容链接到文件
主要代码
```java
URL url1 = null;
                File newFile = new File("e://rupeng.html");
                try
                {
                        url1  = new URL("http://www.rupeng.com");
                        FileUtils.copyURLToFile(url1, newFile); //把如鹏网首页写入e盘下的rupeng.html
                }
```

案例2：复制文本、文件夹
主要代码：
```java
                // TODO 自动生成的方法存根
                File input = new File("e://a.txt");
                File output = new File("e://acopy.txt");
                File Input1 = new File("e://a");  //文件夹
                File Output1 = new File("e://TestFile");//文件夹
                String loveWord = "love you,my girl";
                
                try
                {
                        //FileUtils.cleanDirectory(readyToDeleteDir); // 保留文件夹
                        System.out.println("目录e://a的大小："+FileUtils.sizeOfDirectory(readyToDeleteDir)/1024.0+"KB");
                        //FileUtils.deleteDirectory(readyToDeleteDir); // 连文件夹多没有
                        FileUtils.copyFile(input, output);  //复制文件
                        FileUtils.copyDirectory(Input1, Output1); //复制文件夹
                        FileUtils.copyFileToDirectory(input, Output1); //复制文件到文件夹
                        FileUtils.writeStringToFile(output, loveWord, true); // 追加写入
                }
```

IOUtils是采用流方式进行复制，FileUtils把文本文件作为参数（省去中间流的创建，估计有）

3：Directory-walker
DirectoryWalker 是一个抽象类，无法直接创建对象。通过创建一个继承类，并实现类中的抽象方法。
案例1：递归删除某个文件夹下的文件
全部代码：
```java
/**
* @author 叶昭良
*
*/
import java.io.*;
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
import java.util.List;

import org.apache.commons.io.DirectoryWalker;
public class TestCommonIODiretoryWalk extends DirectoryWalker
{

        /**
         * @param args
         */
        public TestCommonIODiretoryWalk() 
        {
                     super();
    }
        public List reName(File startDirectory) 
        {
                List results = new ArrayList();
                try
                {
                        walk(startDirectory,results        );
                } catch (IOException e)
                {
                        // TODO 自动生成的 catch 块
                        e.printStackTrace();
                }
                return results;
        }
        
        protected boolean handleDirectory(File directory,int depth,Collection results)
        {
                //save.svn directories and then skip 不会删除.svn文件夹下的任何信息
                if("fasf".equals(directory.getName())) // 如果你想要保存某个文件夹
                                                        //就添加elseif即可！！当然文件架构还在
                {
                        directory.delete();
                        return false;
                }else
                {
                        return true;
                }
        }
        
        protected void handleFile(File file,int depth, Collection results)
        {
                file.delete();
                results.add(file);
        }
        // 使用Directory Walker来遍历一个目录并进行相应操作步骤如下
        // 1:创建一个继承DirectoryWalker的类，并在构造函数调用super()
        // 2:定义一个业务入口方法，比如clean，这个方法调用walk方法
        // 3:重写handleDiretory方法  提供相应的业务逻辑
        // 4:重写handleFile方法，提供相应处理逻辑
        
        // 其中最为关键的是walk方法，该方法制定入口路径，
        // 并提供一个用于接受处理结构的参数。
        // 在walk方法的内部：针对目录对象调用handleDirectory
        //                       文件对象调用handleFile
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                TestCommonIODiretoryWalk dw = new TestCommonIODiretoryWalk() ;
                List l1 = dw.reName(new File("e://b"));
                Iterator it = l1.iterator();
                while(it.hasNext())
                {
                        System.out.println(it.next());
                }
        }

}
```


案例2：修改一个文件加下的所有以txt结尾的修改为doc结尾的
主要代码：（基本上 框架类似于案例1  主要修改handleFile的子程序 其他地方不需要怎么修改）
```java
protected void handleFile(File file,int depth, Collection results)
        {
                String banana = file.getName();
                String newFilename= file.getParent()+File.separator+banana.replaceAll(".txt"+"[        DISCUZ_CODE_5        ]quot;, ".doc");
                file.renameTo(new File(newFilename));
                results.add(file);
        }
```


4.Filters
过滤器：过滤在这边的意思是指保留那些满足某个条件的文件(而不是取消）
Org.apache.commons.io.filefilter 包定义了一个接口（IOFileFilter）用于连接java.io.FileFilter和java.io.FilenameFilter.除了这个之外，包中提供了一系列可以使用的IOFileFilter接口的实现，包括允许你连接其他过滤器的实现。这些过滤器可以被用于列出文件或者在FileDialog
全部代码
```java
/**
* @author 叶昭良
*
*/
import java.io.File;
import java.io.FilenameFilter;

import org.apache.commons.io.filefilter.SuffixFileFilter;
import org.apache.commons.io.filefilter.IOFileFilter;
import org.apache.commons.io.filefilter.NotFileFilter;
import org.apache.commons.io.filefilter.OrFileFilter;
import org.apache.commons.io.filefilter.AndFileFilter;
import org.apache.commons.io.filefilter.DirectoryFileFilter;

import java.io.*;
public class TestCommonIOgetTxtFiles
{

        /**
         * @param args
         */        
public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                
                File rootDir = new File("e://");
                // 列出所有的java结尾的所有文件（当前目录）
                // SuffixFileFilter的使用，定义了一个FilenameFilter接口
                FilenameFilter fileFilter = new SuffixFileFilter(".java"); // 满足.java结尾的文件，这也正好符合过滤在当前情况下的定义。
                String[] txtFiles = rootDir.list(fileFilter);
                for(int i = 0 ; i < txtFiles.length; i++)
                {
                        System.out.println(rootDir+File.separator+txtFiles[i]);
                }
                // 列出所有的html和htm的文件（在当前文件目录下）
                // IOFileFilter、OrFileFilter、NotFileFilter、AndFileFilter的使用（and or 逻辑）
                IOFileFilter htmlFilter =   // 定义了一个IOFileFilter接口接口
                                new OrFileFilter(new SuffixFileFilter(".html"),new SuffixFileFilter(".htm"));  // 为什么有时候用IOFileFilter 而有时候用FilenameFilter?
                IOFileFilter notDirectory = new NotFileFilter(DirectoryFileFilter.INSTANCE);
                // 拒绝选择目录
                FilenameFilter fileFilter1= new AndFileFilter(htmlFilter,notDirectory); // 利用AndFileFilter连接两个过滤器接口
                File[] htmlfiles = rootDir.listFiles(fileFilter1);
                for(int i = 0; i< htmlfiles.length; i++)
                {
                        System.out.println(htmlfiles[i]);
                }
        }
```

代码说明：
接口是一个更高级别的类，所以可以定义类型变量。具体看filename.gif

案例2： 当前文件夹下的一天前的所有文件
AgeFileFilter : 通过一个cutoff截止时间来过滤文件，可以过滤出更新的 更旧的 或者就是某一个时间点的信息。
比如下面的  打印所有当前文件夹内的早于一天前创建的文件和文件夹
主要代码：
查看主要代码的图片。

案例3： 获取文件夹下的可读 可写   目录文件（普通文件类似） 空文件  隐藏文件
说明：最好是在打印时候(for 循环)加上异常的判断（NullPointerException)
主要代码：
```java
import org.apache.commons.io.filefilter.CanReadFileFilter;
import org.apache.commons.io.filefilter.CanWriteFileFilter;
import org.apache.commons.io.filefilter.DirectoryFileFilter;
import org.apache.commons.io.filefilter.EmptyFileFilter;
import org.apache.commons.io.filefilter.HiddenFileFilter;
                // TODO 自动生成的方法存根
                 File dirBanana = new File("E:\\测试目录");
                 String[] filesBanana = dirBanana.list(CanReadFileFilter.CAN_READ);
                 for(int i = 0; i < filesBanana.length; i++)
                 {
                         System.out.println("Banana可读文件:"+filesBanana[i]);
                 }
                 System.out.println("分割线--------------------------");
                 File dirOrange = new File("E:\\测试目录");
                 String[] filesOrange = dirOrange.list(CanReadFileFilter.READ_ONLY);
                 for(int i = 0; i < filesOrange.length; i++)
                 {
                         System.out.println("Orange只读文件:"+filesOrange[i]);
                 }
                 
                 System.out.println("分割线--------------------------");
                 File dirApple = new File("E:\\测试目录");
                 String[] filesApple = dirApple.list(CanReadFileFilter.CANNOT_READ);
                 if(filesApple.length == 0)
                 {
                          System.out.println("Apple未找到不可读文件");
                 }else
                 {
                         for(int i = 0; i < filesApple.length; i++)
                         {
                                 System.out.println("Apple不可读文件:"+filesApple[i]);
                         }
                 }
                 
                 
                 File dirEgg = new File("E:\\测试目录");
                 String[] filesEgg = dirEgg.list(CanWriteFileFilter.CAN_WRITE);
                 for(int i = 0 ; i < filesEgg.length; i++)
                 {
                         System.out.println("Egg 找到了可写文件:"+filesEgg[i]);
                 }
                 
                 File dirChichen = new File("E:\\测试目录");
                 String[] filesChichen = dirChichen.list(CanWriteFileFilter.CANNOT_WRITE);
                 for(int i = 0 ; i < filesChichen.length; i++)
                 {
                         System.out.println("Chichen 找到了不可写文件:"+filesChichen[i]);
                 }
                 
                 File dirDirectory = new File("E:\\");
                 String[] filesDirectory = dirDirectory.list(DirectoryFileFilter.INSTANCE);
                 for(int i = 0; i < filesDirectory.length; i++)
                 {
                         System.out.println("列出所有E盘下的文件夹名字:"+filesDirectory[i]);
                 }
                 
                 File dirEmpty = new File("E:\\");
                 String[] filesEmpty = dirEmpty.list(EmptyFileFilter.EMPTY);
                 for(int i = 0; i < filesEmpty.length; i++)
                 {
                         System.out.println("dirEmpty列出所有E盘下的空文件名字（包括空文件夹）:"+filesEmpty[i]);
                 }
                 
                 File dirNotEmpty = new File("E:\\");
                 String[] filesNotEmpty = dirNotEmpty.list(EmptyFileFilter.NOT_EMPTY);
                 for(int i = 0; i < filesNotEmpty.length; i++)
                 {
                         System.out.println("dirNotEmpty列出所有E盘下的非空文件名字（包括空文件夹）:"+filesNotEmpty[i]);
                 }
                 
                 File dirHidden = new File("E:\\测试目录");
                 String[] filesHidden = dirHidden.list(HiddenFileFilter.HIDDEN);
                 try
                 {
                         for(int i = 0; i < filesHidden.length; i++)
                         {
                                 System.out.println("dirHidden列出所有E盘下的隐藏名字（包括空文件夹）:"+filesHidden[i]);
                         }
                 }catch(NullPointerException e)
                 {
                         System.out.println("未找到任何隐藏文件 在测试目录下");
                 }
                 
                 File dirVisible = new File("E:\\测试目录");
                 String[] filesVisible = dirHidden.list(HiddenFileFilter.VISIBLE);
                 try
                 {
                         for(int i = 0; i < filesVisible.length; i++)
                         {
                                 System.out.println("dirHidden列出所有E盘下的隐藏名字（包括空文件夹）:"+filesHidden[i]);
                         }
                 }catch(NullPointerException e)
                 {
                         System.out.println("未找到任何隐藏文件 在测试目录下");
                 }

        }
```
文件过滤器的规律是: 都是以FileFilter结尾的 比如SuffixFileFilter   AndFileFilter,OrFileFilter, NotFileFilter,AgeFileFilter,CanWriteFileFilter,CanReadFileFilter,DirectoryFileFilter,FileFileFilter,NameFileFilter,WildcardFileFilter。。。
有些接口需要new出来一个对象，比如new  SuffixFileFilter(".txt")  ,new NameFileFilter("TestFile") 、new SizeFileFilter(1024*1024) 、new And,Or,NotFileFilter...等有些接口是直接使用，比如HiddenFileFilter.HIDDEN|VISIBLE   CanReadFileFilter.CANREAD|CANNOTREAD     CanWriteFileFilter.CANWRITE|CANNOTWRITE   EmptyFileFilter.EMPTY|NOTEMPTY   DirectoryFileFilter.INSTANCE   FileFileFilter.FILE等


5.Stream部分：
主要测试了CountingInputStream 、  CountingOutputStream、TeeOutputStream、TeeInputStream

CountingInputStream :统计输出流的字节个数（Counting InputStream），可用于网络收费
CountingOutputStream:测量输入流的统计，可用于网络收费
TeeOutputStream:需要输入两个OutputStreams，同时输出到两个硬盘文件
TeeOutputStream：一个inputStream 一个OutputStream,在读取InputStream的同时，也输出到硬盘上的另一个文件，充当备份。 


TeeInputStream:
CommonsIO刚刚接触，只是一些浅显的知识，跟大家分享了,可以通过比较传统的IO流的实现，
CommonsIO流的确为我们封装了很多有用而且快捷的类方法，当然前提是得对基本的IO流有一定的基础性认识。
有问题的，求指教。
![过滤器接口的说明](/images/java/filename.gif)
![打印所有当前文件夹内的早于一天前创建的文件和文件夹](/images/java/主要代码.gif)

另外的,WildcardFileFilter类似于RegexFileFilter可以模糊的定位当前文件夹下的所有符合模糊条件的文件，
比如
```java
WildcardFileFilter
         //列出所有的文件大小大于*test*.java*的文件
                 File dirWildcard = new File("E:\\测试目录");
                 String[] filesWildcard = dirWildcard.list(new WildcardFileFilter("*test*.java*")); //正则表达式
                 try
                 {
                         for(int i = 0; i < filesWildcard.length; i++)
                         {
                                 System.out.println("dirWildcard列出所有E盘下的隐藏名字（包括空文件夹）:"+filesWildcard[i]);
                         }
                 }catch(NullPointerException e)
                 {
                         System.out.println("未找到任何普通文件 在测试目录下");
                 }
```


RegexFileFilter:
```java
//列出所有的文件包含*test*.java的文件
                 File dirRegex = new File("E:\\测试目录");
                 FileFilter ff = new RegexFileFilter(".*test.*\\.java.*"); //正则表达式 不能写成*test*
                 File[] filesRegex = dirRegex.listFiles(ff);
                 try
                 {
                         for(int i = 0; i < filesRegex.length; i++)
                         {
                                 System.out.println("dirRegex列出所有E盘下的隐藏名字（包括空文件夹）:"+filesRegex[i]);
                         }
                 }catch(NullPointerException e)
                 {
                         System.out.println("未找到任何普通文件 在测试目录下");
                 }
```
说明：FileFilter ff = new RegexFileFilter(".*test.*\\.java");  ff可以作为listFiles的参数类型，但是RegexFileFilter这个FileFilter的子类产生的对象无法充当listFiles的参数，这边体现着多态的概念，而且是接口的多态。有一个问题：不明白为什么不能用RegexFileFilter 的对象？？？？ RegexFileFilter查阅源码的确是继承FileFilter和FilenameFilter的，难道是说多态一般只是使用在赋值语句，而在构造函数必须使用对应的类型   listFiles函数的声明为 public File[] listFiles(FileFilter filter){..}  或者public File[] listFiles(FilenameFilter filter){..} ）   求指教~-~


首先感谢杨老师的远控解决了我的一个困惑。
其实自己也应该猜到，分享一下讲解过程
public class RegexFileFilter extends AbstractFileFilter  
复制代码
RegexFileFilter继承了AbstractFileFilter

public abstract class AbstractFileFilter implements IOFileFilter
复制代码

AbstractFileFilter实现了IOFileFilter  接口可以实现多继承，于是进一步观看了IOFileFilter
public interface IOFileFilter extends FileFilter, FilenameFilter
复制代码

IOFileFilter又继承了FileFilter和FilenameFilter这两个接口，并且这两个接口已经是祖先级的接口

之前我一直用RegexFileFilter定义ff变量，传到listFiles()中死活报错，如下：
RegexFileFilter ff = new RegexFileFilter(".*test.*\\.java.*");
复制代码
原因是RegexFileFilter继承了两个接口FileFilter,FilenameFilter,而listFiles也是实现了两种方式的重载（可以进入listFiles类，利用神健组合ctrl+o观看到   listFiles(FileFilte)  listFiles(FileFilename))
所以如果ff不显示指定的话，listFiles怎么知道要重载哪个函数？

三种解决方法：
1：
```java
   File[] filesRegex = dirRegex.listFiles((FilenameFilter)ff);
```
2： 
```java             
   File[] filesRegex = dirRegex.listFiles((FileFilter)ff);
```

都是可以的，也可以用上面的多态的方法
3：
```java
   FileFilter ff = new RegexFileFilter(".*test.*\\.java");
```

问题的根源：重载+显示指定类型

----------------------------------------------------
学习了    /**  然后摁下 Enter键 在eclipse中自动加入注释段，并且 敲击@可以产生想要的注释字段。
较专业写法，代码的注释，必须学习
/**
         * 输入流中的内容到输出流去
         * @param input
         * @param out
         * @throws Exception
         */
