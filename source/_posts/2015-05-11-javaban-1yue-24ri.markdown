---
layout: post
title: "Java班 1月24日"
date: 2015-05-11 14:58:43 +0800
comments: true
categories: JavaBasic
---

<!--more-->

第一题：合并图片
```java
/**
* @author 叶昭良
*
*/
import java.io.*;
public class TestCopyTwoFilesToOne implements AutoCloseable
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                File inputFileApple = new File("e://a.jpg");
                File inputFileBanana = new File("e://b.jpg");
                File inputFileOrange = new File("e://c.jpg");
                String temp = null;
                try
                (
                        //字节流
                        InputStream  fisApple= new FileInputStream(inputFileApple);
                        InputStream  fisBanana= new FileInputStream(inputFileBanana);
                        OutputStream  fosOrange= new FileOutputStream(inputFileOrange);
                        // 缓冲流（已通过字符流转换）
                        BufferedReader brApple =  new BufferedReader(new InputStreamReader(fisApple));
                        BufferedReader brBanana =  new BufferedReader(new InputStreamReader(fisBanana));
                        BufferedWriter brOrange =  new BufferedWriter(new OutputStreamWriter(fosOrange));
                )
                {
                        while((temp = brApple.readLine()) != null)
                        {        
                                brOrange.write(temp); // 写入第一个文件
                                brOrange.newLine();
                        }
                        while((temp = brBanana.readLine()) != null)
                        {        
                                brOrange.write(temp);//写入第二个文件信息
                                brOrange.newLine();
                        }
                }catch(IOException e)
                {
                        System.out.println("文件读取或者写入错误");
                }
        }

        @Override
        public void close() throws Exception
        {
                // TODO 自动生成的方法存根
                
        }

}
```


第一题合并图片第二种方法
```java
/**
* @author 叶昭良
*
*/
import java.io.*;

import org.apache.commons.io.FileUtils;
public class TestCopyTwoFilesToOne2
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                try
                {
                        // FileUtils暂时无法实现追加！
                        //FileUtils.copyFile(new File("e:/a.jpg"), new File("e://c.jpg"));
                        //FileUtils.copyFile(new File("e:/b.jpg"), new File("e://c.jpg"), true);
                        // 对于大文件可能有问题
                        //利用FileUtils的readFileToString方法读取a.jpg文件的数据，并保存在content中
                        String content = FileUtils.readFileToString(new File("e:/a.jpg"));

                        // 利用FileUtils的writeStringToFIle方法，借用content中间变量写入a.jpg文件信息到c.jpg
                        // 也可以利用FileUtils的copyFile方法，直接复制到c.jpg.
                        FileUtils.writeStringToFile(new File("e://c.jpg"), content);
                        // //利用FileUtils的readFileToString方法读取b.jpg文件的数据，并保存在content中
                        content = FileUtils.readFileToString(new File("e:/b.jpg"));
                        FileUtils.writeStringToFile(new File("e://c.jpg"), content,true);
                } catch (IOException e)
                {
                        // TODO 自动生成的 catch 块
                        System.out.println("写入文件错误！");
                }
        }

}
```
第二题：把文件夹下所有的txt  文件变为doc文件
```java
import java.io.File;

/**
* @author 叶昭良
*
*/
public class RenameDirectoryFiles
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                String roots = "e:";         //盘符
                String Testfile ="TestFile"; //文件夹
                String targetDir = roots+File.separator+Testfile;
                reNameUnderDirectory(new File(targetDir),".java",".txt"); //调用自定义方法
        }
        
        public static void reNameUnderDirectory(File dir,String from, String to)
        {
                if(null == dir)
                {
                        return; //为空则直接退出
                }
                if(dir.isDirectory())                // 通过路径读入,给定一个路径
                { 
                       //抵用File的listFiles方法获取文件夹下的所有File对象
                        File[] fileArray = dir.listFiles(); 
                        String banana = null;
                        if(null == fileArray)          // 判断是否为空
                        {
                                System.out.println("文件夹内文件为空  找个别的文件夹吧！");
                                return;
                        }
                        for(int i = 0 ; i< fileArray.length; i++)  // 遍历所有文件
                        {
                                if(fileArray[i].isFile())             // 判断是否是文件
                                {
                                        banana = fileArray[i].getName(); //获取文件对象的文件名
                                        if(banana.endsWith(from))        // 只是对以java文件结尾的进行修改
                                        {
                                                String newFilename= fileArray[i].getParent()+File.separator
                                                        +banana.replaceAll(from+"[        DISCUZ_CODE_1        ]quot;;, to); //只替换最后一个from表征的字符串
                                                fileArray[i].renameTo(new File(newFilename)); // 利用renameTo方法 修改文件夹的名字
                                        }
                                }else
                                {
                                        reNameUnderDirectory(fileArray[i],from,to); //递归调用该方法 实现遍历文件夹
                                }
                        }
                }else
                {
                        System.out.println("不是文件夹");
                }
        }
}
```


第三题：后缀修改（Directory walker):
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
public class TestCommonIODirectoryWalkRename extends DirectoryWalker
{

        /**
         * @param args
         */
        public TestCommonIODirectoryWalkRename() 
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
                String banana = file.getName();
                String newFilename= file.getParent()+File.separator+banana.replaceAll(".txt"+"$", ".doc");
                file.renameTo(new File(newFilename));
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
                TestCommonIODirectoryWalkRename dw = new TestCommonIODirectoryWalkRename() ;
                List l1 = dw.reName(new File("e://codes"));
                Iterator it = l1.iterator();
                while(it.hasNext())
                {
                        System.out.println(it.next());
                }
        }

}
```


第四题：读取文本文件，并统计数字字符的个数查阅了ASCII表信息：
```java
/**
* @author 叶昭良
*
*/
import java.io.*;
public class getDigtiNumberCount
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                // TODO 自动生成的方法存根

                String  temp = null;
                String roots = "e:";
                String filename = "student.txt";
                String filenameWithPathname=roots+File.separator+filename;
                int len = 0;
                try  // try_with_resources方法，不需要关闭需要closed的资源。
                (
                        BufferedReader br = new BufferedReader(new FileReader(filenameWithPathname));                           
                )
                {
                        int  i = 0;
                        int count = 0;
                        while((temp = br.readLine()) != null)
                        {
                                System.out.println("打印一行");
                                System.out.println(temp);
                                for(int  j = 0 ;  j  < temp.length(); j++)
                                {//进行数字字符的区间范围判断 
                                        if(temp.charAt(j) >= 48 && temp.charAt(j) <= 57)
                                        {
                                                count++;
                                        }
                                }
                                i++;
                        }
                        System.out.println(filenameWithPathname+"文件总

共有："+count+"个数字");
                }catch(IOException e)
                {
                        System.out.println("文件打开失败"）;
                }
        }

}
```

![ASCII码表数字字符的数值][/images/java/ASCII表.gif]


第四题： 读取文本文件的信息，输入某个人的名字，并输出对应某个人的成绩
文本文件：（保存为 student.txt)
张三 80
李四 90
王五 95
赵四 80
李丹江 64 
```java
/**
* @author 叶昭良
*
*/
import java.io.*;
import java.util.*;
public class GetMessageofNameFromFile
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                String  temp = null;
                int len = 0;
                try
                (
                        BufferedReader br = new BufferedReader(new FileReader("e:/student.txt"));                
                )
                {
                        while((temp = br.readLine()) != null)
                        {
                                len++;
                        }
                }catch(IOException e)
                {
                        System.out.println("读取文件失败");
                }
                String[] names = new String[len];
                //String[] banana = null;
                double[] chengji = new double[len] ;
                try
                (
                        BufferedReader br = new BufferedReader(new FileReader("e:/student.txt"));        
                )
                {
                        int  i = 0;
                        while((temp = br.readLine()) != null)
                        {
                                //System.out.println(temp);
                                String[] banana = temp.split(" ");
                                //System.out.println(banana[0]+":"+banana[1]);
                                // 读取文件的name信息
                                names[i] = banana[0];
                                //chengji[i]  = Double.parseDouble(banana[1].trim());
                                //读取文件的成绩信息
                                chengji[i]  = Double.parseDouble(banana[1]);
                                i++;
                        }
                }catch(IOException e)
                {
                        System.out.println("读取文件失败");
                }
                
                Scanner  sc = new Scanner(System.in);
        
                while(true)
                {        
                        System.out.println("请输入你要查询的名字");
                        String name = sc.nextLine();
                        //如果不输入姓名，直接回车则显示全部姓名！
                        if(name.isEmpty())
                        {
                                System.out.println("输入为空显示全部");
                                for(int i = 0; i < names.length ; i++)
                                {
                                        System.out.println(names[i]+"的成绩是:"+chengji[i]);        
                                }
                        }else
                        {
                                for(int i = 0; i < names.length ; i++)
                                {
                                        if(name.equalsIgnoreCase(names[i])) //不区分大小写
                                        {
                                                System.out.println(name+"的成绩是:"+chengji[i]);
                                        }
                                }
                        }
                }

        }

}
```
