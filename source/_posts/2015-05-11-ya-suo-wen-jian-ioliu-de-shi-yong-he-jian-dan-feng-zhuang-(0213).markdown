---
layout: post
title: "压缩文件IO流的使用和简单封装（0213）"
date: 2015-05-11 14:58:47 +0800
comments: true
categories: JavaBasic
---
<!--more-->

分为三个部分：
第一部分 ，先是试用了zipinputstream的用法
第二部分， 是进一步试用了压缩流zipoutputStream和加密的方法
第三部分 ， 则是对上述过程封装为一个OOZip类

功能简述：  分为普通的压缩和加密的压缩，对应的解压缩 。普通的压缩：文件和文件夹都可以。 加密的压缩：文件和文件夹都可以，当加密的时候，则生成的zip文件里面的文件打开时乱码，必须用对应的unzipCrypto方法进行解压才有效。当然不会像winzip会提醒你输入密码，可以让你打开，只不过打开的是乱码。

1： 先从com.rupeng.gtk4j挖出了zipInputStream的用法，用于解压缩，测试只能针对zip文件。
于是就有了初始版本的解压缩流：
```java
import java.io.Closeable;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;
import java.io.StringWriter;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;

/**
* @author    叶昭良
* @time      2015年2月12日下午1:19:18
* @version   TestZipInputStream V1.0
*/
public class TestZipInputStream
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                //unZipShared("e:\\test\\test.zip");
                //unZipShared("e:\\test\\test1.zip");
                //rar暂时无法解压出来，但是zip文件是可以的
                unZipShared("e:\\test\\test1.zip","e:\\testOutput");
        }
        
        /**
         *   解压缩 zip文件，只能限制为rar
         * @param zipName        待解压的zip文件
         * @param outputfolder   解压zip文件到outputFoler文件夹下
         */
        public static void unZipShared(String zipName,String outputfolder)
        {
                File gtkDir = new File(outputfolder);// *.dll放的文件夹
                if (!gtkDir.exists())
                {
                        gtkDir.mkdirs();
                }
                InputStream inStream = null;
                try
                {
                        inStream = new  FileInputStream(zipName);
                } catch (FileNotFoundException e1)
                {
                        // TODO Auto-generated catch block
                        e1.printStackTrace();
                }
                //InputStream inStream = Utils.class.getResourceAsStream("/gtkshare.zip");
                if (inStream == null)
                {
                        throw new UnsatisfiedLinkError("没找到"+zipName);
                }
                try
                {                        
                        unZip(inStream, gtkDir.toString());
                        System.out.println(gtkDir.toString());
                } catch (IOException e)
                {
                        System.err.println("解压缩gtkshare.zip失败" + toFullString(e));
                }
        }

        /**
         * 把streamToZip这个zip文件流解压到硬盘的destDir文件夹，支持多级目录
         * @param streamToZip
         * @param destDir
         * @throws IOException
         */
        public static void unZip(InputStream streamToZip,String destDir)throws IOException
        {  
                ZipInputStream zipStream = new ZipInputStream(streamToZip);
                try
                {
                        ZipEntry zipEntry = null;
                        //通过zipEntry方式支持多级目录
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
        /**
         *     从zip文件包中拷贝文件
         * @param inStream       zipEntry的某个文件
         * @param outStream      输出的某个文件流
         * @throws IOException
         */
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
        /**
         *    摘自rupeng.gtk4j   不明白具体的作用    非主要问题
         * @param throwable
         * @return
         */
        static String toFullString(Throwable throwable)
        {
                StringWriter sw = null;
                PrintWriter pw = null;
                try
                {
                        sw = new StringWriter();
                        pw = new PrintWriter(sw);
                        throwable.printStackTrace(pw);
                        return sw.toString();
                } finally
                {
                        close(sw);
                        close(pw);
                }
        }
        /**
         *     让文件流安静的关闭
         * @param closeable   关闭接口
         */
        static void close(Closeable closeable)
        {
                if (closeable != null)
                {
                        try
                        {
                                closeable.close();
                        } catch (IOException e)
                        {

                        }
                }
        }


}
```



2：后来想着有解压缩，必然也是有着压缩，于是就摆了一下，参考了一篇百度知道文章
    2.1 首先加入了zip的方法
    2.2 改进了zip方法的文件压缩流的写入过程，利用buffersize
    2.3 常使用了文章中的加密过程，添加了加密压缩和加密解压缩的过程
    2.4 想着实用命令流来进一步实现 压缩和解压缩的调用，后来弃用，改用封装一个OOZip类来实现

于是就有了下面的升级版的压缩和解压缩的程序：
```java
import java.io.Closeable;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;
import java.io.StringWriter;
import java.security.InvalidAlgorithmParameterException;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.Random;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;
import java.util.zip.ZipOutputStream;

import javax.crypto.BadPaddingException;
import javax.crypto.Cipher;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.NoSuchPaddingException;
import javax.crypto.SecretKey;
import javax.crypto.SecretKeyFactory;
import javax.crypto.interfaces.PBEKey;
import javax.crypto.spec.PBEKeySpec;
import javax.crypto.spec.PBEParameterSpec;
import javax.crypto.spec.SecretKeySpec;


/**
* @author    叶昭良
* @time      2015年2月12日下午5:22:31
* @version   TestZipInputStreamUpdate V1.0  增加了压缩 
*                              V2.0   ZipShared加入了 zosTemp.close(); 修复了 压缩的bug，删掉则无法压缩
*                              V3.0   升级了ZipShared 使用了copy函数。
*                              V4.0   增加了加密压缩 和解加密压缩
*/
public class TestZipInputStreamUpdate
{

        
        /**
         * @param args
         */
        private static final String ALGORITHM = "PBEWithMD5AndDES";
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                TestZipInputStreamUpdate tisu = new TestZipInputStreamUpdate();
                //不需要再次加入zip文件后缀了
                //tisu.ZipShared("e://test1222bak","e://test1222");
                
                tisu.ZipSharedCrypto("e://test1222bak", "e://test1222passwdByZhao.zip", "123456");
                tisu.unzipCrypto("e://test1222passwdByZhao.zip","c://laoliang","123456");
        //        tisu.unzipCrypto("e://test1222passwdByZhao.zip","c://laoliang","1234565");
                
                /*
                 *  你正在进入e:\test1222bak文件夹
                        你正在进入e:\test1222bak\test1222文件夹
                        你正在压缩a1.zip
                        你正在压缩test123.txt
                        你正在压缩test124.txt
                        
                        通过这个实验总结了：所有操作系统内部的文件都是文件，无论是普通的文件
                        还是文件夹文件，还是管道文件，还是索引文件，还是设备文件，本质上都是
                        文件，只不过是在文件的头上面增加了一些特殊的标记，比如说你需要在文件夹
                        的路径增加一个\路径标志 反斜杠的道理是一样的。
                 */
                
                /*  这是一个命令流的使用方式：：
                 *         if(args.length==2){ 
            String name = args[1]; 
            Zip zip = new Zip(); 

            if(args[0].equals("-zip")) 
            {
                    zipname = args[2];
                    zip.doZip(name); 
            }
                
            else if(args[0].equals("-unzip")) 
            {
                    outputfolder = args[2];
                     zip.unZip(name);          
            }     
        } 
        else{ 
            System.out.println("Usage:"); 
            System.out.println("压缩:java Zip -zip directoryName  zipname"); 
            System.out.println("解压:java Zip -unzip fileName.zip outputfolder"); 
            throw new Exception("Arguments error!"); 
        } 
                 */
        }
        public void ZipShared(String fileinput,String zos)
        {
                try
                {
                        //加入"zip"后缀！
                        ZipOutputStream zosTemp = new ZipOutputStream(new FileOutputStream(zos+"zip"));
                        File fApple = new File(fileinput);
                        ZipShared(fApple,zosTemp,"");
                        try
                        {
                                /// 为什么加入这个就可以？？？？
                                //  不加入这有异常？？？？why   Tell me 
                                zosTemp.close();
                        } catch (IOException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }
                } catch (FileNotFoundException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                
        }
        public  void ZipShared(File fileinput,ZipOutputStream zos,String base)
        {
                //ZipOutputStream zos = new ZipOutputStream(new FileOutputStream(outputZipName));
                
                //File fileinput = new File(outputZipName);
if(fileinput.isDirectory())
{
        System.out.println("你正在进入"+fileinput+"文件夹");
        File[] fBanana = fileinput.listFiles();
        try
        {
                //传进一个文件夹标志
                zos.putNextEntry(new ZipEntry(base+"/"));
                //让base加上一个/
                base = base.length()==0?"":base+"/";
                for(int i = 0; i < fBanana.length; i++)
                {
                        ZipShared(fBanana[i],zos,base+fBanana[i].getName());
                }
        } catch (IOException e)
        {
                // TODO Auto-generated catch block
                System.out.println("压缩文件夹失败"+e.getMessage());
        }
        
}else
{
        try
        {
                zos.putNextEntry(new ZipEntry(base));
                FileInputStream fis = new FileInputStream(fileinput);
                //改进写入的方式
                /*int b;
                while((b = fis.read())!= -1)
                {
                        zos.write(b); 
                        //效率很定不高  每一个字符  进行一次缓冲
                        //zos.flush();
                }*/
                copy(fis,zos); //利用汝鹏版的copy函数
                System.out.println("你正在压缩"+fileinput.getName());
                //fis.close();
                
                //zos.close();
        } catch (IOException e)
        {
                // TODO Auto-generated catch block
                System.out.println("压缩文件失败"+e.getMessage());
        } 
        //为什么加入则错误
        /*finally
        {
                try
                {
                        zos.close();
                }catch(IOException e)
                {
                        System.out.println("打开流错误！");
                }
        }*/
                }
        }
        public  void ZipSharedCrypto(String fileinput,String zosFile,String pwd)
        {
                
                try
                {        File f1 = new File(fileinput);
                //采用和ZipShared一样的FileOutputStream
                        ZipOutputStream zos  = null;
                        zos = new ZipOutputStream(new FileOutputStream(zosFile));
                        ZipSharedCrypto(f1,zos,"", pwd);
                        try
                        {
                                zos.close();
                        } catch (IOException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }
                } catch (FileNotFoundException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                System.out.println("成了");
        }
        public  void ZipSharedCrypto(File fileinput,ZipOutputStream zos,String base,String pwd)
        {
if(fileinput.isDirectory())
{
        System.out.println("你正在进入"+fileinput+"文件夹");
File[] fApples = fileinput.listFiles();
//因为ZipEntry的isDirectory()方法中，目录以"/"结尾
try
{        
        zos.putNextEntry(new ZipEntry(base+"/"));
        base = base.length()==0?"":(base+"/");
        //一种比较新型的方式循环读写东西。
        for(File ftemp:fApples)
        {
                ZipSharedCrypto(ftemp,zos,base+ftemp.getName(),pwd);
        }
}catch(IOException e)
{
        System.out.println("文件夹加密压缩失败");
        }
}else
{        
        
        try
        {
                zos.putNextEntry(new ZipEntry(base));
                FileInputStream fis = new FileInputStream(fileinput);
                System.out.println("你正在开始加密压缩"+fileinput+"文件");
        PBEKeySpec keySpec = new PBEKeySpec(pwd.toCharArray());
        SecretKeyFactory keyFactory = null;
        try
        {
                keyFactory = SecretKeyFactory.getInstance(ALGORITHM);
        } catch (NoSuchAlgorithmException e)
        {
                // TODO Auto-generated catch block
                e.printStackTrace();
        }
        SecretKey passwordKey = keyFactory.generateSecret(keySpec);
        //生成一个炸弹 进行加密
        byte[] bomb =  new byte[8];
        Random rnd = new Random();
        rnd.nextBytes(bomb);
        int iterations = 100;
        PBEParameterSpec parameterSpec = new PBEParameterSpec(bomb, iterations);
        Cipher cipher = null;
        try
        {
                cipher = Cipher.getInstance(ALGORITHM);
        } catch (NoSuchAlgorithmException
                        | NoSuchPaddingException e)
        {
                // TODO Auto-generated catch block
                e.printStackTrace();
        }
        cipher.init(Cipher.ENCRYPT_MODE, passwordKey,parameterSpec);
        //往输出流 添加炸弹
        zos.write(bomb);
        
        //添加加密的主文件内容  1KB缓存区
        byte[] inputBuffer = new byte[1024];
        int bytesRead = 0;
        //如果没有读到信息则为-1
        while((bytesRead = fis.read(inputBuffer))!= -1)
        {
                //每个缓冲区 进行加密写入
                byte[] output = cipher.update(inputBuffer);
                if(output != null)
                {
                        zos.write(output);
                }
        }
        
        //加密结束语
        byte[] outputFinal =null;
        try
        {
                outputFinal = cipher.doFinal();
        } catch (IllegalBlockSizeException | BadPaddingException e)
        {
                // TODO Auto-generated catch block
                e.printStackTrace();
        }
        if(outputFinal != null)
        {
                zos.write(outputFinal);
        }
        
/*        fis.close();
        zos.flush();
        zos.close();*/
        
        
}catch(InvalidKeySpecException | InvalidKeyException | InvalidAlgorithmParameterException | IOException e)
{
        System.out.println("加密失败");
        }
}
        }
        /**
         *   解压缩 zip文件，只能限制为rar
         * @param zipName        待解压的zip文件
         * @param outputfolder   解压zip文件到outputFoler文件夹下
         */
        public static void unZipShared(String zipName,String outputfolder)
        {
                File gtkDir = new File(outputfolder);// *.dll放的文件夹
                //指定的目录不存在  则创建之
                if (!gtkDir.exists())
                {
                        gtkDir.mkdirs();
                }
                InputStream inStream = null;
                try
                {
                        inStream = new  FileInputStream(zipName);
                } catch (FileNotFoundException e1)
                {
                        // TODO Auto-generated catch block
                        e1.printStackTrace();
                }
                //InputStream inStream = Utils.class.getResourceAsStream("/gtkshare.zip");
                if (inStream == null)
                {
                        throw new UnsatisfiedLinkError("没找到"+zipName);
                }
                try
                {                        
                        unZip(inStream, gtkDir.toString());
                        System.out.println(gtkDir.toString());
                } catch (IOException e)
                {
                        System.err.println("解压缩gtkshare.zip失败" + toFullString(e));
                }
        }

        /**
         * 把streamToZip这个zip文件流解压到硬盘的destDir文件夹，支持多级目录
         * @param streamToZip
         * @param destDir
         * @throws IOException
         */
        public static void unZip(InputStream streamToZip,String destDir)throws IOException
        {  
                ZipInputStream zipStream = new ZipInputStream(streamToZip);
                try
                {
                        ZipEntry zipEntry = null;
                        //通过zipEntry方式支持多级目录
        while((zipEntry=zipStream.getNextEntry())!=null)
        {
                if(zipEntry.isDirectory())
                {
                        System.out.println("你正在创建文件夹文件 比较特殊");
                        File dir = new File(destDir,zipEntry.getName());
                        //如果指定的目录不存在 则创建之
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
                        System.out.println("你正在解压缩压缩"+zipEntry.getName());
                }
        }
}
finally
{
        close(zipStream);
}
                
    }
        /**
         *     从zip文件包中拷贝文件 ，按照0.5MB的缓冲写入文件(默认方式）
         * @param inStream       zipEntry的某个文件
         * @param outStream      输出的某个文件流
         * @throws IOException
         */
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
        /**
         *    摘自rupeng.gtk4j   不明白具体的作用    非主要问题
         * @param throwable
         * @return
         */
        static String toFullString(Throwable throwable)
        {
                StringWriter sw = null;
                PrintWriter pw = null;
                try
                {
                        sw = new StringWriter();
                        pw = new PrintWriter(sw);
                        throwable.printStackTrace(pw);
                        return sw.toString();
                } finally
                {
                        close(sw);
                        close(pw);
                }
        }
        /**
         *     让文件流安静的关闭
         * @param closeable   关闭接口
         */
        static void close(Closeable closeable)
        {
                if (closeable != null)
                {
                        try
                        {
                                closeable.close();
                        } catch (IOException e)
                        {

                        }
                }
        }
        
        // 加密解压缩

/**
         * 功能描述：将压缩文件解压到指定的文件目录下
         * @param zipFileName      压缩文件名称(带路径)
         * @param outputDirectory  指定解压目录
         * @return
         * @throws Exception
         */
        public  void unzipCrypto(String zipFileName, String outputDirectory, String pwd)
        {
                ZipInputStream inputStream;
                try
                {
                        inputStream = new ZipInputStream(new FileInputStream(zipFileName));
                        unzipCrypto(inputStream, outputDirectory, pwd);
                } catch (Exception e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                
        }


        public  void unzipCrypto(ZipInputStream inputStream, String outputDirectory, String pwd) throws Exception
        {
                ZipEntry zipEntry = null;
                FileOutputStream outputStream = null;
                try{
                        while ((zipEntry = inputStream.getNextEntry()) != null) 
                        {
if (zipEntry.isDirectory())
{
        System.out.println("你正在进入"+zipEntry.getName()+"文件夹");
        String name = zipEntry.getName();
        name = name.substring(0, name.length() - 1);
        File file = new File(outputDirectory + File.separator + name);
        file.mkdir();
} 
else 
{
        File file = new File(outputDirectory + File.separator + zipEntry.getName());
        file.createNewFile();
        outputStream = new FileOutputStream(file);
        System.out.println("你正在解压缩"+file.getName()+"文件");
        PBEKeySpec keySpec = new PBEKeySpec(pwd.toCharArray());
    SecretKeyFactory keyFactory = SecretKeyFactory.getInstance(ALGORITHM);
    SecretKey passwordKey = keyFactory.generateSecret(keySpec);
    byte[] salt = new byte[8];
    inputStream.read(salt);
    int iterations = 100;
    PBEParameterSpec parameterSpec = new PBEParameterSpec(salt, iterations);
    Cipher cipher = Cipher.getInstance(ALGORITHM);
    cipher.init(Cipher.DECRYPT_MODE, passwordKey, parameterSpec);
    byte[] input = new byte[1024];
    int bytesRead;
    while ((bytesRead = inputStream.read(input)) != -1) 
    {
            byte[] output = cipher.update(input, 0, bytesRead);
            if (output != null)
            {
                    outputStream.write(output);
            }
    }
    byte[] output = cipher.doFinal();
    if (output != null)
    {
            outputStream.write(output);
    }
/*                                    outputStream.flush();
                                    outputStream.close();*/
        
}
                        }
                        //inputStream.close();
                }
                catch(IOException ex)
                {
                        throw new Exception("解压读取文件失败");
                }
                catch(Exception ex)
                {
                        throw new Exception("解压文件密码不正确");
                }
/*                finally
                {
                        inputStream.close();
                        outputStream.flush();
                    outputStream.close();
                }*/
        }

}
```



3：进一步 实现先前的想法，封装一个OOZip类，
    基本思想：
         1：抽取常用变量，比如bufferSize 缓冲区大小     buf缓冲区； 本想着加入压缩和解压缩的流变量，后来删掉了
         2：定义构造函数， 设置缓冲区大小
         3：复制之前的方法，并利用buffersize和buf改写copy函数和 文件的复制的函数
         4：进行简单地压缩和解压缩文件夹      加密压缩和解加密压缩文件夹   的测试。 并找到了压缩单个文件的bug
         5：bug修复


  完整代码如下：
```java
import java.io.Closeable;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;
import java.io.StringWriter;
import java.security.InvalidAlgorithmParameterException;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.Random;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;
import java.util.zip.ZipOutputStream;
import javax.crypto.BadPaddingException;
import javax.crypto.Cipher;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.NoSuchPaddingException;
import javax.crypto.SecretKey;
import javax.crypto.SecretKeyFactory;
import javax.crypto.spec.PBEKeySpec;
import javax.crypto.spec.PBEParameterSpec;

/**
* @author    叶昭良
* @time      2015年2月12日下午9:51:11
* @version   OOZip    用于压缩和解压缩
*                   V1.0  增加了压缩 
*                   V2.0   ZipShared加入了 zosTemp.close(); 修复了 压缩的bug
*                   V3.0   改进了构造函数 利用具有缓冲区的压缩
*                   V4.0   升级了ZipShared 使用了copy函数。
*                   V5.0   增加了加密压缩 和解加密压缩
*                   V6.0   改用了面向对象方式修改了一番
*                   V7.0   加入了一些压缩和解压缩完成的的console标记
*                   V8.0   修复了单个文件无法加密的bug   new ZipEntry(base) 改为
*                    new ZipEntry(base+fileinput.getName())，未添入到zipentry的缘故
*                    而若是文件夹遍历的时候则是有加入文件名的标记！所以在单个文件的时候也需要加入文件名的
*                    标记
*                   V9.0   若有中文问题，可以进一步采用import org.apache.tools.zip.* 的zip包！ 更好的支持中文
*                          具体参看http://blog.csdn.net/liu149339750/article/details/7887701  
*                                 http://szhnet.iteye.com/blog/199059 ，这个链接当中提供了ant版本 
*                          当然此版本，不需要org.apache.tool.zip包也不需要ant包  附录了ant版本的代码，的确看起来
*                          是简单的。
*/
public class OOZip
{

/**
*  这几个私有变量的控制，主要体现在针对具体的文件的复制过程中
*/
        private int bufSize ; //压缩和解压缩会用到。 一次从压缩文件zip读取多少文件信息
                                                                 //或者一次写入多少文件信息到压缩流
        private byte[] buf;   //写入或者写出压缩流的字节数组
        private int readBytes = 0;  //实际写入或者写出文件流的大小。
        private static final String ALGORITHM = "PBEWithMD5AndDES";
        
        //构造函数的定义
   public OOZip(){ 
           // 1025*512  //设置输入输出流的缓冲区的大小 ，统一设置
        this(1024*512); 
    } 

    public OOZip(int bufSize){ 
        this.bufSize = bufSize; 
        this.buf = new byte[this.bufSize]; 
    } 
        
        //常用的类中 内部函数
        
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                OOZip oz = new OOZip();
                //单个文件加密和非加密测试暂时失败
                oz.ZipShared("e://student.txt", "e://laoliang"); 
                //oz.ZipSharedCrypto("e://student.txt", "e://laoliang","1234");
                //输入文件夹路径则加密和非加密测试通过
                //oz.ZipShared("e://test1222bak", "e://laoliang");  //已测试通过
                //oz.ZipSharedCrypto("e://test1222bak", "e://laoliang", "123456");
                
                //解压缩，不用输入.zip后缀，只需要输入文件名
                //oz.unzipCrypto("e://laoliang", "c://laozi","123456");
                //测试成功
        }

        /**
         *     压缩名字为fileinput变量内容的文件夹
         * @param fileinput   文件夹名字
         * @param zos         zip文件夹名字
         */
        public void ZipShared(String fileinput,String zos)
        {
                try
                {
                        //加入"zip"后缀！
                        ZipOutputStream zosTemp = new ZipOutputStream(new FileOutputStream(zos+".zip"));
                        File fApple = new File(fileinput);
                        ZipShared(fApple,zosTemp,"");
                        try
                        {
                                /// 为什么加入这个就可以？？？？
                                //  不加入这有异常？？？？why   Tell me 
                                zosTemp.close();
                        } catch (IOException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }
                } catch (FileNotFoundException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                System.out.println("success to create"+zos+".zip");
        }
        /**
         *     压缩文件夹的中间函数
         * @param fileinput    输入函数的文件对象
         * @param zos          压缩输出流对象
         * @param base         一般是"" 表示子目录的作用，在递归目录的时候会用到，在文件夹递归中，涉及到改变；
         */
        public  void ZipShared(File fileinput,ZipOutputStream zos,String base)
        {
if(fileinput.isDirectory())
{
        System.out.println("你正在进入"+fileinput+"文件夹");
        File[] fBanana = fileinput.listFiles();
        try
        {
                //传进一个文件夹标志
                zos.putNextEntry(new ZipEntry(base+"/"));
                //让base加上一个/
                base = base.length()==0?"":base+"/";
                for(int i = 0; i < fBanana.length; i++)
                {
                        ZipShared(fBanana[i],zos,base+fBanana[i].getName());
                }
        } catch (IOException e)
        {
                System.out.println("压缩文件夹失败"+e.getMessage());
        }
        
}else
{
        try
        {
                zos.putNextEntry(new ZipEntry(base+fileinput.getName()));
                FileInputStream fis = new FileInputStream(fileinput);
                //改进写入的方式
                /*int b;
                while((b = fis.read())!= -1)
                {
                        zos.write(b); 
                        //效率很定不高  每一个字符  进行一次缓冲
                        //zos.flush();
                }*/
                copy(fis,zos); //利用汝鹏版的copy函数
                System.out.println("你正在压缩"+fileinput.getName());
        } catch (IOException e)
        {
                System.out.println("压缩文件失败"+e.getMessage());
        } 
}
        }
        /**
         *     加密压缩文件夹
         * @param fileinput    文件夹字符串
         * @param zosFile      压缩字符串名字
         * @param pwd          加密的密码
         */
        public  void ZipSharedCrypto(String fileinput,String zosFile,String pwd)
        {
                
                try
                {        
                        File f1 = new File(fileinput);
                        //采用和ZipShared一样的FileOutputStream
                        ZipOutputStream zos  = null;
                        zos = new ZipOutputStream(new FileOutputStream(zosFile+".zip"));
                        ZipSharedCrypto(f1,zos,"", pwd);
                        try
                        {
                                zos.close();
                        } catch (IOException e)
                        {
                                e.printStackTrace();
                        }
                } catch (FileNotFoundException e)
                {
                        System.out.println("未找到文件"+e.getMessage());
                }
                System.out.println("success to create crypto "+zosFile+".zip");
        }
        /**
         *          加密压缩文件夹     
         * @param fileinput     压缩文件夹的File对象
         * @param zos           zip压缩输出流ZipOutputStream
         * @param base          一般是""
         * @param pwd           压缩的密码
         */
        public  void ZipSharedCrypto(File fileinput,ZipOutputStream zos,String base,String pwd)
        {
if(fileinput.isDirectory())
{
        System.out.println("你正在进入"+fileinput+"文件夹");
        File[] fApples = fileinput.listFiles();
        //因为ZipEntry的isDirectory()方法中，目录以"/"结尾
        try
        {        
                zos.putNextEntry(new ZipEntry(base+"/"));
                base = base.length()==0?"":(base+"/");
                //一种比较新型的方式循环读写东西。
                for(File ftemp:fApples)
                {
                        ZipSharedCrypto(ftemp,zos,base+ftemp.getName(),pwd);
                }
        }catch(IOException e)
        {
                System.out.println("文件夹加密压缩失败");
        }
}else
{        
        
        try
        {
                zos.putNextEntry(new ZipEntry(base));
                FileInputStream fis = new FileInputStream(fileinput);
                System.out.println("你正在开始加密压缩"+fileinput+"文件");
                //加密过程的开始
                PBEKeySpec keySpec = new PBEKeySpec(pwd.toCharArray());
                SecretKeyFactory keyFactory = null;
                try
                {
                        keyFactory = SecretKeyFactory.getInstance(ALGORITHM);
                } catch (NoSuchAlgorithmException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                SecretKey passwordKey = keyFactory.generateSecret(keySpec);
                //生成一个炸弹 进行加密
                byte[] bomb =  new byte[8];
                Random rnd = new Random();
                rnd.nextBytes(bomb);
                int iterations = 100;
                PBEParameterSpec parameterSpec = new PBEParameterSpec(bomb, iterations);
                Cipher cipher = null;
                try
                {
                        cipher = Cipher.getInstance(ALGORITHM);
                } catch (NoSuchAlgorithmException
                                | NoSuchPaddingException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                cipher.init(Cipher.ENCRYPT_MODE, passwordKey,parameterSpec);
                //往输出流 添加炸弹
                zos.write(bomb);
                
                //添加加密的主文件内容  1KB缓存区
                //byte[] inputBuffer = new byte[1024];
                //int bytesRead = 0;
                //如果没有读到信息则为-1
                
                //改用 readBytes  buf在类头定义的私有变量，进行统一的buffer缓存区大小的控制
                while((this.readBytes = fis.read(this.buf))!= -1)
                {
                        //每个缓冲区 进行加密写入
                        byte[] output = cipher.update(this.buf);
                        if(output != null)
                        {
                                zos.write(output);
                        }
                }
                
                //加密结束语-------------加密结束
                byte[] outputFinal =null;
                try
                {
                        outputFinal = cipher.doFinal();
                } catch (IllegalBlockSizeException | BadPaddingException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                if(outputFinal != null)
                {
                        zos.write(outputFinal);
                }        
        }catch(InvalidKeySpecException | InvalidKeyException | InvalidAlgorithmParameterException | IOException e)
        {
                System.out.println("加密失败");
        }
}
        }
        /**
         *   解压缩 zip文件，只能限制为zip ,rar无法进行，利用如鹏版的
         * @param zipName        待解压的zip文件
         * @param outputfolder   解压zip文件到outputFoler文件夹下
         */
        public void unzip(String zipName,String outputfolder)
        {
                File gtkDir = new File(outputfolder);// *.dll放的文件夹
                //指定的目录不存在  则创建之
                if (!gtkDir.exists())
                {
                        gtkDir.mkdirs();
                }
                InputStream inStream = null;
                try
                {
                        //不用输入.zip后缀，只需要输入文件名
                        inStream = new  FileInputStream(zipName+".zip");
                } catch (FileNotFoundException e1)
                {
                        // TODO Auto-generated catch block
                        e1.printStackTrace();
                }
                //InputStream inStream = Utils.class.getResourceAsStream("/gtkshare.zip");
                if (inStream == null)
                {
                        throw new UnsatisfiedLinkError("没找到"+zipName);
                }
                try
                {                        
                        unzip(inStream, gtkDir.toString());
                        //System.out.println(gtkDir.toString());
                        System.out.println("成功解压缩"+zipName+".zip"+"文件 到"+outputfolder+"文件夹下");
                } catch (IOException e)
                {
                        System.err.println("解压缩"+zipName+".zip失败" + toFullString(e));
                }
        }

        /**
         * 把streamToZip这个zip文件流解压到硬盘的destDir文件夹，支持多级目录
         * @param streamToZip
         * @param destDir
         * @throws IOException
         */
        public  void unzip(InputStream streamToZip,String destDir)throws IOException
        {  
                ZipInputStream zipStream = new ZipInputStream(streamToZip);
try
{
        ZipEntry zipEntry = null;
        //通过zipEntry方式支持多级目录
while((zipEntry=zipStream.getNextEntry())!=null)
{
        if(zipEntry.isDirectory())
        {
                System.out.println("你正在创建文件夹文件 比较特殊");
                File dir = new File(destDir,zipEntry.getName());
                //如果指定的目录不存在 则创建之
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
                System.out.println("你正在解压缩压缩"+zipEntry.getName());
        }
}
                }
                finally
                {
                        close(zipStream);
                }
                
    }
        /**
         *     从zip文件包中拷贝文件 ，按照0.5MB的缓冲写入文件(默认方式）
         * @param inStream       zipEntry的某个文件
         * @param outStream      输出的某个文件流
         * @throws IOException
         */
        public void copy(InputStream inStream, OutputStream outStream)
                        throws IOException
        {
                //byte[] buffer = new byte[512 * 1024];// 0.5MB 的缓冲区
                //int len;
                while ((this.readBytes = inStream.read(this.buf)) >= 0)
                {
                        outStream.write(this.buf, 0, this.readBytes);
                }
        }
        /**
         *    摘自rupeng.gtk4j   不明白具体的作用    非主要问题
         * @param throwable
         * @return
         */
        public String toFullString(Throwable throwable)
        {
                StringWriter sw = null;
                PrintWriter pw = null;
                try
                {
                        sw = new StringWriter();
                        pw = new PrintWriter(sw);
                        throwable.printStackTrace(pw);
                        return sw.toString();
                } finally
                {
                        close(sw);
                        close(pw);
                }
        }
        /**
         *     让文件流安静的关闭
         * @param closeable   关闭接口
         */
        public void close(Closeable closeable)
        {
                if (closeable != null)
                {
                        try
                        {
                                closeable.close();
                        } catch (IOException e)
                        {

                        }
                }
        }
        
        // 加密解压缩

/**
         * 功能描述：将压缩文件解压到指定的文件目录下
         * @param zipFileName      压缩文件名称(带路径)
         * @param outputDirectory  指定解压目录
         * @return
         * @throws Exception
         */
        public  void unzipCrypto(String zipFileName, String outputDirectory, String pwd)
        {
                ZipInputStream inputStream;
                File outputDir;
                try
                {
                        //不用输入.zip后缀，只需要输入文件名
                        inputStream = new ZipInputStream(new FileInputStream(zipFileName+".zip"));
                        outputDir = new File(outputDirectory);
                        unzipCrypto(inputStream, outputDir, pwd);
                } catch (Exception e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                System.out.println("成功解压缩"+zipFileName+".zip"+"文件 到"+outputDirectory+"文件夹下");
                
        }


        public void unzipCrypto(ZipInputStream inputStream, File outputDir, String pwd) throws Exception
        {
                ZipEntry zipEntry = null;
                FileOutputStream outputStream = null;
                try{
while ((zipEntry = inputStream.getNextEntry()) != null) 
{
        //如果是文件夹  则遍历
        if (zipEntry.isDirectory())
        {
                System.out.println("你正在进入"+zipEntry.getName()+"文件夹");
                String name = zipEntry.getName();
                name = name.substring(0, name.length() - 1);
                File file = new File(outputDir + File.separator + name);
                file.mkdir();
        } 
        //对单个普通文件进行处理
        else 
        {
                File file = new File(outputDir + File.separator + zipEntry.getName());
                file.createNewFile();
                outputStream = new FileOutputStream(file);
                System.out.println("你正在解压缩"+file.getName()+"文件");
                //解压加密过程的开始
                PBEKeySpec keySpec = new PBEKeySpec(pwd.toCharArray());
            SecretKeyFactory keyFactory = SecretKeyFactory.getInstance(ALGORITHM);
            SecretKey passwordKey = keyFactory.generateSecret(keySpec);
            //准备排除加密的炸弹头
            byte[] apple = new byte[8];
            //在zip输入流添加read
            inputStream.read(apple);
            int iterations = 100;
            PBEParameterSpec parameterSpec = new PBEParameterSpec(apple, iterations);
            Cipher cipher = Cipher.getInstance(ALGORITHM);
            cipher.init(Cipher.DECRYPT_MODE, passwordKey, parameterSpec);
            //byte[] input = new byte[1024];
            //int bytesRead;
            //利用全局的私有变量this.buf  this.readBytes ,已在类开头定义，统一控制
            while ((this.readBytes = inputStream.read(this.buf)) != -1) 
            {
                    byte[] output = cipher.update(this.buf, 0, this.readBytes);
                    if (output != null)
                    {
                            outputStream.write(output);
                    }
            }
            byte[] output = cipher.doFinal();
            if (output != null)
            {
                    outputStream.write(output);
            }
                
        }
}
                }
                catch(IOException ex)
                {
                        throw new Exception("解压读取文件失败");
                }
                catch(Exception ex)
                {
                        throw new Exception("解压文件密码不正确");
                }
        }

}
/*
*  附录ant版本的压缩实现http://szhnet.iteye.com/blog/199059
*  可以加入某些文件和删除某些文件
*  package net.szh.zip;

import java.io.File;

import org.apache.tools.ant.Project;
import org.apache.tools.ant.taskdefs.Zip;
import org.apache.tools.ant.types.FileSet;

public class ZipCompressorByAnt {

        private File zipFile;

        public ZipCompressorByAnt(String pathName) {
                zipFile = new File(pathName);
        }
        
        public void compress(String srcPathName) {
                File srcdir = new File(srcPathName);
                if (!srcdir.exists())
                        throw new RuntimeException(srcPathName + "不存在！");
                
                Project prj = new Project();
                Zip zip = new Zip();
                zip.setProject(prj);
                zip.setDestFile(zipFile);
                FileSet fileSet = new FileSet();
                fileSet.setProject(prj);
                fileSet.setDir(srcdir);
                //fileSet.setIncludes("**//*.java"); 包括哪些文件或文件夹 eg:zip.setIncludes("*.java");
                //fileSet.setExcludes(...); 排除哪些文件或文件夹
                zip.addFileset(fileSet);
                
                zip.execute();
        }
}

ant版本的使用：
package net.szh.zip;

public class TestZip {
        public static void main(String[] args) {                
                ZipCompressorByAnt zca = new ZipCompressorByAnt("E:\\szhzipant.zip");
                zca.compress("E:\\test");
        }
}
*  
*/
```

