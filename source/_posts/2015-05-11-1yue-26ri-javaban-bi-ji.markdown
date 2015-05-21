---
layout: post
title: "1月26日Java班笔记"
date: 2015-05-11 14:58:40 +0800
comments: true
categories: JavaBasic
---
<!--more-->

进一步针对com.rupeng.gtk.GTK主类进行了分析
第一部分：


```java
public class GTK
{

        static
        {
                Utils.unZipShared();
                Utils.loadGtkLibs();
                Utils.loadDll("gtk4j.dll");
        }
.....
        /**
         * 初始化gtk环境
         */
        public static native void gtk_init();//
```

在GTK类中包含主要两个部分，一个是静态代码段Static部分，主要用于做前期工作，产生dll文件以及解压缩，文件流的复制等
另一部分主要是静态代码的声明；

针对第一部分静态代码段需要分为三个部分 unZipShare()部分、loadGtkLibs和loadDll，他们三个分别是Utils类的主要静态方法。
```java
Utils.unZipShare():
        public static void unZipShared()
        {
                File gtkDir = new File(System.getProperty("user.dir"), "gtk");// *.dll放的文件夹
                if (!gtkDir.exists())
                {
                        gtkDir.mkdirs();
                }
                InputStream inStream = Utils.class.getResourceAsStream("/gtkshare.zip");
                if (inStream == null)
                {
                        throw new UnsatisfiedLinkError("没找到gtkshare.zip");
                }
                try
                {                        
                        unZip(inStream, gtkDir.toString());
                } catch (IOException e)
                {
                        System.err.println("解压缩gtkshare.zip失败" + toFullString(e));
                }
        }
```

这段代码的主要功能是：
1.利用File(System.getProperties("User.dir","gtk")创建主要文件夹gtk，用于存放dll文件
2.利用Utils类的反射（不清楚，和之前的Properties的当前类TestProperties.class.getClassLoader().getResourceAsStream()的效果一样） 都是获取一个输入流。在这边获取的是一个当前文件夹下的gtkshare.zip输入流附上Properties的语法，当做复习
```java
inStream = TestProperties.class.getClassLoader().getResourceAsStream("b.properties");
                // 用于提取项目文件中的文件  默认如果没有包名字  就只写项目里面的文件即可！
            System.out.println(inStream.getClass());
                Properties prop  = new Properties();//修正图片中存在的错误  TestProperties是一个当前类
类似于图片中的Utils类！ 有所不同的是class之后还需要调用getClassLoader 。 当前的dll载入，rupeng的
包是采用System.load而Properities是采用prop.load(inStream);的方式。
```

3.最后通过unZip方式打开，为此还需要进一步研究unZip代码
```java
        public static void unZip(InputStream streamToZip,String destDir)throws IOException
        {  
                ZipInputStream zipStream = new ZipInputStream(streamToZip);
                try
                {
                        ZipEntry zipEntry = null;
                        while((zipEntry=zipStream.getNextEntry())!=null)
                        {
                                if(zipEntry.isDirectory())
                                {
                                        File dir = new File(destDir,zipEntry.getName());
                                        if(!dir.exists())
                                        {
                                                dir.mkdirs();
                                        }
                                }
                                else
                                {
                                        FileOutputStream fileOutStream = new FileOutputStream(new File(destDir,zipEntry.getName()));
                                        try
                                        {
                                                copy(zipStream, fileOutStream);
                                        }
                                        finally
                                        {
                                                close(fileOutStream);
                                        }
                                }
                        }
                }
                finally
                {
                        close(zipStream);
                }
                
    }  
```


这段代码的主要作用是
1：把一个输入的压缩流zipStream 利用压缩流特有的ZipEntry（类似容器当中的Iterator ）逐个从流中解压缩，
      1.1 如果该流文件夹，则创建文件夹
      1.2 如果该流是文件夹 ，则利用文件输出流（FileOutputStream），把解压缩流的信息存到输出流中。
所以也就是说：解压缩流的文件的结构也是和为解压缩时候的结构是一样的，既有文件夹文件，又有普通文件（另外文件File是一个比较宽泛的定义，File定义的对象既可以是文件夹文件对戏那个，也可以是普通文件，或者管道文件，索引文件，设备文件 ，即File可以代表文件夹、普通文件、管道文件、索引文件、设备文件等。)

为了进一步研究Utils.unZip(),我们需要进一步研究Utils.copy()：
```java
        static void copy(InputStream inStream, OutputStream outStream)
                        throws IOException
        {
                byte[] buffer = new byte[512 * 1024];// 0.5MB 的缓冲区
                int len;
                while ((len = inStream.read(buffer)) >= 0)
                {
                        outStream.write(buffer, 0, len);
                }
        }
```


说明: 如鹏网先通过建立0.5MB的缓冲区，用于载入输入的压缩流，当达到0.5MB的时候，再利用文件输出流写出压缩流信息到先前创建的gtk/bin目录下。 
我想着是不是可以用BufferedInputStream和BufferedOutputStream,于是修改代码如下：
```java
        static void copy(InputStream inStream, OutputStream outStream)
                        throws IOException
        {
BufferedInputStream bis = null;
BufferedOutputStream bos = null;
try
{
bis = new BufferedInputStream(inStream);
bos = new BufferedOutputStream(outStream);

                byte[] buffer = new byte[512 * 1024];// 0.5MB 的缓冲区(rupeng采用的策略)
                int len;
       
                while ((len = bis.read(buffer)) >= 0) //最后一个返回的是-1
                {
                        bos.write(buffer, 0, len);
                }
}catch(IOException e)
{   System.out.println(“读取错误”);
}        
        }
```
并未做测试。只是想着为什么不用缓冲流？当然两者的效果一样
于是我做了下面的测试 针对BufferedInputStream BufferedOutputStream 以及FileInputstream ,FileOutputStream（不是如鹏gtk4j的ZipInputStream 和FileOutputStream）

测试代码：
```java
/**
* 
*/

/**
* @author 叶昭良
*
*/
import java.io.*;
import java.util.Arrays;
public class TestBufferedInputStream
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                long t1 = System.currentTimeMillis();
                TestBufferedInputStream();
                //System.out.printf("BufferedInputStream总共用时:"+Long.toString((System.currentTimeMillis() - t1)));
                System.out.println("BufferedInputStream总共用时:"+(System.currentTimeMillis() - t1));
                //System.out.printf("BufferedInputStream总共用时:%ld",(System.currentTimeMillis() - t1));
                
                
                long t2 = System.currentTimeMillis();
                TestFileInputStream();
                //System.out.printf("FileInputStream总共用时:"+Long.toString((System.currentTimeMillis() - t2)));
                System.out.println("FileInputStream总共用时:"+(System.currentTimeMillis() - t2));
                //System.out.printf("FileInputStream总共用时:%ld",(System.currentTimeMillis() - t2));
                
        }
        public static void TestBufferedInputStream()        
        {
                // TODO 自动生成的方法存根
                BufferedInputStream bis = null;
                BufferedOutputStream  bos = null;
                byte[] fileToFile = new byte[512*1024];
                int len = 0;
                try
                {
                        bis = new BufferedInputStream(new FileInputStream("E:\\测试目录\\MindManager.exe"));
                        bos = new BufferedOutputStream(new FileOutputStream("E:\\测试目录\\Mind1.exe"));
                        
                        len  = bis.read(fileToFile);
                        while(-1 != len)
                        {
                                //System.out.printf("%s",new String(fileToFile));
                                //System.out.printf("%s",Arrays.toString(fileToFile));
                                bos.write(fileToFile,0,len);
                                bos.flush();
                                len = bis.read(fileToFile);
                                
                        }
                }catch(Exception e)
                {
                        e.printStackTrace();
                }
                finally
                {
                        try
                        {
                                bis.close();
                                bos.close();
                        }catch(Exception e)
                        {
                                e.printStackTrace();
                        }
                }
        }
        
        
        public static void TestFileInputStream()        
        {
                // TODO 自动生成的方法存根
                FileInputStream fis = null;
                FileOutputStream  fos = null;
                byte[] fileToFile = new byte[512*1024];
                int len = 0;
                try
                {
                        fis = new FileInputStream("E:\\测试目录\\MindManager.exe");
                        fos = new FileOutputStream("E:\\测试目录\\Mind2.exe");
                        
                        len  = fis.read(fileToFile);
                        while(-1 != len)
                        {
                                //System.out.printf("%s",fileToFile);
                                //System.out.printf("%s",Arrays.toString(fileToFile));
                                fos.write(fileToFile,0,len);
                                fos.flush();
                                len = fis.read(fileToFile);
                                
                        }
                }catch(Exception e)
                {
                        e.printStackTrace();
                }
                finally
                {
                        try
                        {
                                fis.close();
                                fos.close();
                        }catch(Exception e)
                        {
                                e.printStackTrace();
                        }
                }
        }

}
```

基本上保持上下的代码的一致性，除了输入流和输出流的不一样，并比较时间

说明：测试对象 MindManage.exe 大概是126MB ，
测试结果：第一阶段测试：
第一次：
BufferedInputStream总共用时:301
FileInputStream总共用时:369
第二次再运行:
BufferedInputStream总共用时:296
FileInputStream总共用时:319
第三次
BufferedInputStream总共用时:280
FileInputStream总共用时:278
第四次
BufferedInputStream总共用时:337
FileInputStream总共用时:305

分析发现，前两次基本上是Buffered方式较好，后两种基本上是File较好，这有可能是eclipse已经帮你载入了缓存的原因，于是删掉测试目录下的Mind1.exe Mind2.exe

进行第二次测试阶段：
第一次：
BufferedInputStream总共用时:251
FileInputStream总共用时:261

第二次：
BufferedInputStream总共用时:286
FileInputStream总共用时:303

第三次： 报错，提示Mind1.exe被占用，问题不是主要的，于是再次删掉Mind1.exe Mind2.exe

第三阶段测试：
第一次：
BufferedInputStream总共用时:304
FileInputStream总共用时:431
第二次：
又提醒Mind1.exe被占用，估计还未写完，但其实文件的大小已经是和MindMange.exe相等。
第三次：
BufferedInputStream总共用时:299   ，变长了
FileInputStream总共用时:284


经过三个阶段测试发现: 如果单以第一次为标准的话，Buffered的方式是比FileInputStream的方式快一些。

回到主题，已经从Utils.unZipShared()---->Utils.unZip()---->Utils.copy()这条线走通了一遍，基本上完成的是
1.创建主目录gtk ,并解压缩gtkshare.zip,并调用unzip
2.创建gtk底下的子目录bin和share以及其他，并导入其中的一些dll文件（暂时不清楚具体是哪个dll)(导入的方式是通过文件流的复制过程，当然其实也是可以用IOUtils.copy(inStream,outStream）的方式


第二部分：
```java
Utils.loadGtkLibs();
        static void loadGtkLibs()
        {
                // 如果这些dll放到user.dir根目录下，则只要在gtk4j.dll加载之前把libffi-6.dll等这些被依赖的放到user.dir根目录下即可
                // 但是如果放到其他目录下（放到根目录下在项目里看到一堆dll文件，太难看，所以放到gtkbin下），就要注意加载顺序
                // 要先加载被依赖的dll。使用DependencyWalker可以看dll的依赖关系

                loadDll("libffi-6.dll");
                loadDll("libpixman-1-0.dll");
                loadDll("zlib1.dll");
                loadDll("liblzma-5.dll");
                loadDll("libiconv-2.dll");
                loadDll("pthreadGC2.dll");

                loadDll("libxml2-2.dll");// 依赖于libiconv-2.dll、liblzma-5.dll

                loadDll("libpng15-15.dll");// 依赖于zlib1.dll
。。。。
}
```


说明： 原来这个函数主要是用于载入存在于gtk/bin目录下的大多数的dll文件 ，但是不包含gtk4j.dll (这是在第三步的时候单独实现，为什么分开不明白？）

为此进一步追踪了loadDll的实现：
```java
        static void loadDll(String dllName)
        {
                // 把dll拷贝到项目根目录下
                File gtkBinDir = new File(System.getProperty("user.dir"), "gtk/bin");// *.dll放的文件夹
                if (!gtkBinDir.exists())
                {
                        gtkBinDir.mkdirs();
                }
                File destDllFile = new File(gtkBinDir, dllName);

                // dll位于项目的lib目录下，并且把lib设定为“源码文件夹”，这样这些dll就会生成到jar的根目录下了
                InputStream inStream = Utils.class.getResourceAsStream("/" + dllName);
                if (inStream == null)
                {
                        throw new UnsatisfiedLinkError("没找到" + dllName);
                }

                // 如果dll已经存在，则尝试删除，再拷贝新的，这样保证能自动升级
                if (destDllFile.exists())
                {
                        // 如果文件已经存在，则不覆盖，提高运行效率，如果需要升级，则需要手动清理dll
                        System.out.println(dllName + "已经存在，默认加载。如果需要采用新版本dll，请先删除文件夹"
                                        + gtkBinDir + "下所有的*.dll");
                } else
                {
                        // 拷贝来新的
                        FileOutputStream fileOutStream = null;
                        try
                        {
                                fileOutStream = new FileOutputStream(destDllFile);
                                copy(inStream, fileOutStream);
                        } catch (IOException e)
                        {
                                throw new RuntimeException("拷贝" + dllName + "失败", e);
                        } finally
                        {
                                close(inStream);
                                close(fileOutStream);
                        }
                }
                System.load(destDllFile.toString());
        }
```


说明：正如源代码的注释拷贝dll文件到gtk/bin目录下。
1：先创建一个主目录gtk/bin 
2:  根据gtk/bin这个file对象和dll文件名字创建一个File对象，并判断是否存在，如果不存在则复制到gtk/bin中，如果存在，则会发现一句我们比较熟悉的“已经存在，默认加载。如果需要采用新版本dll，请先删除文件夹"+ gtkBinDir + "下所有的*.dll”
3.拷贝dll文件方式  基本上也是利用FileOutputStream 和copy函数
4.载入dll文件到系统当中，通过System.load(*.dll);


到这里为止，基本上完成了GTK.class和Utils.class中主要使用到的类的实现和调用学习。

后来经赵帅提醒原来GTK里面其实是有很多的native的静态类的声明，但是并没有对应的实现，那么实现到底去哪里？ 当然这个是dll载入和JNI接口的问题（而之所以用dll动态链接库载入文件，是因为原先的gtk源代码是通过c语言编写，在java中可以利用jni接口方便的调用c语言产生的动态链接库的实现，而具体的调用规范可以见下面的五个步骤），为此我用最简单的hello、hi程序进行模拟。

主要分为五步（感谢帅帅）：
1.建立一个hello.java模仿GTK class的方式 建立了静态代码快 导入待生成的dll文件，然后创建了一个类对象调用dll实现的方法  javac hello.java 生成一个java.class 
2.利用javah -jni hello 生成hello.h ，这个hello.h是关键，提示hello.c的函数头的书写
3.利用hello.h内部的函数头，新建hello.c文件 ，并按照函数头的形式实现三个函数  
4.利用 gcc -shared hello.c -o hello.dll的形式生成dll 文件 
5.java hello


分解第一步：创建了hello.java
```java
public class hello
{
    public native void sayHello(); // 3个native函数声明，表明需要有3个c语言的实现
    public native void sayHi();
    public native int max(int a ,int  b);
    static // 静态代码块
    {
        System.loadLibrary("hello");
    }

    public static void main(String[] args)
    {
        new hello().sayHello();
        new hello().sayHi();
        int  a = new hello().max(3,5);
        System.out.println("the maximum value of 3 and 5 is "+a);
    }
}
```

上面的过程基本上和GTK.class有点像，
1.三个非静态函数的声明（这边不用静态 ，如果是静态也可以直接加上一个native），
2.然后是一个静态代码快载入dll文件，
3.紧接着是main函数调用当前对象的方法，实现java调用c语言的实现

编译1：
javac hello.java
复制代码

编译2：
javah -jni  hello
复制代码

在编译2中生成了hello.h ,打开了，文件内容为：
```c
/* DO NOT EDIT THIS FILE - it is machine generated */
#include <jni.h>
/* Header for class hello */

#ifndef _Included_hello
#define _Included_hello
#ifdef __cplusplus
extern "C" {
#endif
/*
* Class:     hello
* Method:    sayHello
* Signature: ()V
*/
JNIEXPORT void JNICALL Java_hello_sayHello
  (JNIEnv *, jobject);

/*
* Class:     hello
* Method:    sayHi
* Signature: ()V
*/
JNIEXPORT void JNICALL Java_hello_sayHi
  (JNIEnv *, jobject);

/*
* Class:     hello
* Method:    max
* Signature: (II)I
*/
JNIEXPORT jint JNICALL Java_hello_max
  (JNIEnv *, jobject, jint, jint);

#ifdef __cplusplus
}
#endif
#endif
```


上面的c语言编译指令，暂时不管，因为我们关心的是函数的声明，把他们一一拷贝到hello.c
当然首先你得用记事本新建一个hello.c文件，然后一一拷贝上面的三个函数头声明，并在里面写上实现（当时我也是蒙着写着那些实现，猜的），注意导入jni.h(我是直接拷贝%JAVA_HOME%/include/jni.h到当前目录下) 和
hello.h 以及stdio.h（printf函数的声明）
```c
#include <jni.h>
#include "hello.h"
#include <stdio.h>
JNIEXPORT void JNICALL Java_hello_sayHello(JNIEnv * env, jclass obj)
{
    printf("say Hello to you\n");
}

JNIEXPORT void JNICALL Java_hello_sayHi(JNIEnv * env, jclass obj)
{
    printf("say hi to you\n");
}

JNIEXPORT jint JNICALL Java_hello_max(JNIEnv * env, jclass obj, jint a , jint b)
{
    int  max =  a;
    if(max < b)
    {
        return b;
    }else
    {
        return  a;
    }
}
```


于是我们得到了c语言的实现，并进一步整理为一个dll文件
编译3：
```makefile
gcc -I. -LD --add-stdcall-alias -shared hello.c -o hello.dll
```



再用gcc进行编译成java可以使用的dll文件时候一定要加入--add-stdcall-alias,否则报UnsatisfiedLinkError  ,如果查过loadDLL的实现会发现，
```java
                if (inStream == null)
                {
                        throw new UnsatisfiedLinkError("没找到" + dllName);
                }
```


这个异常的出现和编译指令--add-stdcall-alias惊人的对应，一个产生，一个消去。


完成了编译3之后，我们就得到了hello.dll(动态链接库是什么，就是你在运行时候java会导入的函数实现，供java进行c语言函数调用，当然这个过程不用管，只需要hello.dll在当前的项目文件夹下即可）

运行：
```java
java hello
E:\testDLL\testJavaDll>java hello
say Hello to you
say hi to you
the maximum value of 3 and 5 is 5
```



可见已经连接成功。

另外如果大家觉得编译过程麻烦，我也附上一个makefile
```makefile
JC=javac
JH=javah
JHOption=-jni
JRun=java
CC=gcc
# I had put the jni.h into current directory
DLLOPTION  = -I.  -LD -Wl,--add-stdcall-alias -shared
CFLAGS = -c


src = hello.java
srctemp = hello.c
srcClass = $(basename $(src))
# generate  the hello.java
# generate hello.h
#NOw you need to write your hello.c according to hello.h

Dll1 =$(addsuffix .dll,$(basename $(src))) 
HeadTarget:
        $(JC)  $(src)
        $(JH)  $(JHOption) $(srcClass) 

AppTarget:
        $(CC) $(DLLOPTION) -o $(Dll1) $(srctemp)
        $(JRun) $(srcClass)

.PHONY:clean
clean:
        del $(Dll1) /s
        del $(srcClass).class /s
```


先是运行：
1：make  HeadTarget 
      会得到hello.h,然后得去编写hello.c文件
2:  make AppTarget
      会得到hello.dll文件，并运行java hello

前提你得安装gnu make.

附上loadDLL的实现：

![loadDLL的注释](/images/java/4.png)
![hello.c hello.java Makefile](/images/java/5.gif)
![unZipShare](/images/java/5.png)
![unZip的注释](/images/java/6.gif)





认识界面！
控制台程序
界面程序（GUI）
网站程序（HTML+JavaScript+JSP）
手机程序（Android）

学GTK不是主要目的
学工具是低层次，学思想才是王道，后期学习javaee的ssh的架构思想。

GTK的学习目的是深刻理解面向对象的意义，而不是学会GTK的使用
名字中没有gtk的都是通用的java知识。

案例1：带有彻底关闭对话框功能的简单对话框，并设置对话框的名字为“冰封”

```java
/**
* @author 叶昭良
*
*/
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

public class TestGtk
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
        GTK.gtk_init(); // initialize
        int window  = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL); //创建一个新窗口的顶级标        识，在c语言也叫做文件句柄，这边应该叫做窗口句柄。 句柄其实就是一串唯一标识一个资源的数字
        // 设置窗口的名字，通过调用GTK类的静态方法
        GTK.gtk_window_set_title(window,"冰封");// 
        // 设置窗口的显示位置
        GTK.gtk_window_set_position(window,GTK.GTK_WIN_POS_CENTER_ALWAYS);
        //每一个空间都得show出来
        GTK.gtk_widget_show(window); // show it
        //事件的创建，当点击删除按钮时候 摧毁对话框
//IGCallBack中的代码块是“回调”的，在发生“destroy”事件的时候才执行。回调是一个操作系统的机制
//同时解决无法彻底关闭GTK对话框
//解释：在唯一标识的当前窗口，绑定一个destroy的事件，当发生destroy信号的时候，则调用
//一个匿名类，匿名类是一个回调类，JVM负责执行内部的内代码
        GTK.g_signal_connect(window,"destroy",new IGCallBack()
                {
                    @Override
                    public void execute(int instance,int eventData,Object object)
                    {
                        GTK.gtk_main_quit();
                    }
                },null);
        //创建一个box用于盛放控件
        int box = GTK.gtk_box_new(GTK.GTK_ORIENTATION_HORIZONTAL,0);
        // 在gtk_container_add只能盛放一个容器或者控件
        GTK.gtk_container_add(window,box);
        //显示box
        GTK.gtk_widget_show(box);
        //GTK的主循环程序 gtk_main();如果不添加则闪退。
        GTK.gtk_main();
//一般在GTK.gtk_main()之后不能有程序
        }

}
```

运行效果为：

通过本例：
1.可以学习com.rupeng.gtk4j.GTK和IGCallback的简答使用
2.GTK的静态方法库的调用
3.以及彻底关闭时候涉及到的对于特殊的匿名类的创建和回调函数的使用。
4.并归纳出简单的GTK制作流程为：

       a. GTK.gtk_init();
       b. 中间的额外过程
       c. GTK.gtk_widget_show(window) 以及GTK.gtk_widget_show(其他控件)
       d.   GTK.g_signal_connect(window,"destroy",new IGCallBack()
                {
                    @Override
                    public void execute(int instance,int eventData,Object object)
                    {
                        GTK.gtk_main_quit();
                    }
                },null);   来关闭窗口
     e:  GTK.gtk_container_add（window,box) 并show一下。
     f:   GTK.gtk_main();   启动循环线程
![GTK的冰封对话框](/images/java/1.png)



在使用com.rupeng.gtk4j.Utils遇到如下问题：
import com.rupeng.gtk4j.Utils;  一直倒不进去(这也说明一点 我们在使用com.rupeng.gtk4j的时候，不需要使用Utils这个类)
于是查了一下Utils类的实现
class Utils
{
   ...
}
复制代码

原来不是公有类，并且不处于同一个包下，相互之间不能访问。默认的Utils是 default修饰符（default限制在同包内（同一山寨）），对于我们使用者需要管两个问题1：可否继承？ 2：可否访问   不管他们如何进行分包。如果站在更高的高度，就需要有座山雕的风范，统一东北18座山寨（假设座山雕拥有此技能，山寨之间如何访问，如何继承由他说了算）。

在使用类的时候，得问自己，“哪个山寨的？” “可否继承？” “可否访问”（只有public支持不同包的访问（访问是指直接可以new ...），protected是在不同包的情况允许继承(j继承是指可以extends)）

大部分情况我们是在不同包下进行调用，所以一般是public和protected。  
小部分情况下 我们在自己建包的时候，使用 default或者private


在复习了一下包的机制，找到了一张好的总结图：
![包的控制机制](/images/java/包的控制机制.png)

