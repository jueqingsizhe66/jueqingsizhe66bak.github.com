---
layout: post
title: "email邮件发送+OOGTK(0217)"
date: 2015-05-11 14:58:42 +0800
comments: true
categories: JavaBasic
---
<!--more-->
参考阅读：
[commons.email的邮件使用：][http://commons.apache.org/proper/commons-email/userguide.html]
[我的gtk的封装源代码： ][http://www.rupeng.com/forum/thread-44377-1-1.html]


今早起来想着是不是该去对IO流做个总结，于是打开了Apache 的CommonsIO页面，看了一遍，本想写的，后来看到了一个Commons email包！于是页浏览了一下userGuide and JavaDoc,发现不是特别难，于是就按照user guide 做了几个实验！ 刚开始前期遇到了一个比较重要的问题

```java
/**
* 
*/
package TestEmail;

/*import java.net.Authenticator;

import org.apache.commons.mail.DefaultAuthenticator;
import org.apache.commons.mail.Email;
import org.apache.commons.mail.SimpleEmail;*/
import org.apache.commons.mail.*;

/**
* @author    叶昭良
* @time      2015年2月17日上午11:10:04
* @version   TestEmailEmailText V1.0
*/
public class EmailText
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                Email email = new SimpleEmail();
                email.setHostName("smtp.163.com");
                email.setSmtpPort(25);
                email.setSubject("欢迎你");
                email.setAuthenticator(new DefaultAuthenticator("zhaoturkkey@163.com", "密码"));
                email.setSSLOnConnect(true);
                try
                {
                        email.setFrom("zhaoturkkey@163.com","Ye zhaoliang");
                        email.setMsg("This is a test mail ... :-)");
                        email.addTo("977962857@qq.com","Mr Ye");
                        email.send();
                } catch (EmailException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                email.setSubject("TestMail");

        }

}
```
email.setAuthenticator(new DefaultAuthenticator("zhaoturkkey@163.com", "密码")); 他是来自javaee的javax.email包（通过ctrl+鼠标点击，一路插查过去就知道了）！！一直报错！于是下载了javaee版本的eclipse并抽取处 javax.email包！已添加在附件中！只要加载到项目中就可以使用。


紧接着参考了带附件的邮件发送，也编写了相关程序，跟text差不多，只不过是采用了EmailAttachment 和MultiPartEmail 替换掉text版本的SimpleEmail类，就可以增加附件了，下面是我最后版本的源码，需要有OOGTK和com.rupeng.gtk4j.jar这两个类库即可       文本发送邮件是挺快的
       带附件的邮件发送相对较慢些

```java
/**
*   邮件发送器
*/
package TestEmail;

import java.io.File;

import org.apache.commons.mail.DefaultAuthenticator;
import org.apache.commons.mail.Email;
import org.apache.commons.mail.EmailAttachment;
import org.apache.commons.mail.EmailException;
import org.apache.commons.mail.MultiPartEmail;
import org.apache.commons.mail.SimpleEmail;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;


/**
* @author    叶昭良
* @time      2015年2月17日下午2:27:35
* @version   TestEmailMyEmailSend V1.0
*/
import GTKEncapsulate.*;
public class MyEmailSend
{

        /**
         * @param args
         */
        //控件的声明
        private static OOWindow ow = null;
        private static OOButton ob = null;
        private static OOButton obApple = null;
        private static OOButton obAttach = null;
        private static OOTextView otv  = null;
        private static OOGrid og = null;
        
        private static OOLabel olPort = null;
        private static OOLabel olMessage = null;
        private static OOLabel olFrom = null;
        private static OOLabel olTo = null;
        private static OOLabel olServer = null;
        private static OOLabel olPassword = null;
        private static OOLabel olSubject = null;
        private static OOLabel olFromName = null;
        private static OOLabel olToName = null;
        private static OOLabel olAttachName = null;
        private static OOLabel olAttachDescription = null;
        private static OOLabel olAttachSetName = null;
        
        private static OOEntry oePort = null;
        private static OOEntry oeFrom = null;
        private static OOEntry oeTo   = null;
        private static OOEntry oeServer = null;
        private static OOPassword opPassword = null;
        private static OOEntry oeSubject = null;
        private static OOEntry oeFromName = null;
        private static OOEntry oeToName = null;
        private static OOEntry oeAttachDescription = null;
        private static OOEntry oeAttachSetName = null;
        
        private static String[] filenames = null;
        
        //构造函数  初始化界面
        public MyEmailSend()
        {
                ow = new OOWindow();
                ow.setTitle("邮件发送器");
                ow.setExitAfterDestroy(true);
                
                olServer = new OOLabel("发送端服务器:");
                olPort = new OOLabel("服务器端口：");
                olMessage = new OOLabel("待发送信息：");
                olFrom = new OOLabel("发送方邮箱（163.com）：");
                olTo = new OOLabel("接收方邮箱:");
                olPassword = new OOLabel("发送方密码：");
                olSubject = new OOLabel("邮件主题：");
                olFromName = new OOLabel("你的名字：");
                olToName = new OOLabel("对方的名字：");
                olAttachName = new OOLabel("");
                olAttachDescription = new OOLabel("添加附件描述:");
                olAttachSetName = new OOLabel("设置附件名字:");
                
                //文本框创建
                oePort = new OOEntry();
                oePort.setText("25");
                oeFrom = new OOEntry();
                oeFrom.setText("zhaoturkkey@163.com");
                oeTo = new OOEntry();
                oeTo.setText("977962857@qq.com");
                oeServer = new OOEntry();
                oeServer.setText("smtp.163.com");
                opPassword = new OOPassword();
                oeSubject = new OOEntry();
                oeFromName = new OOEntry();
                oeFromName.setText("叶昭良");
                oeToName = new OOEntry();
                oeToName.setText("肖欣然");
                oeAttachDescription = new OOEntry();
                oeAttachSetName = new OOEntry();
                
                obAttach = new OOButton("添加附件");
                ob = new OOButton("仅文本发送");
                obApple = new OOButton("带附件发送");
                otv = new OOTextView();
                otv.setText("尊敬的"+oeToName.getText()+":\n");
                OOScrollBar osb = new OOScrollBar();
                osb.setWidgetSize(200, 200);
                osb.addView(otv);
                og = new OOGrid();
                
                //显示控件
                ow.show();
                ob.show();
                obApple.show();
                otv.show();
                osb.show();
                obAttach.show();
                
                olMessage.show();
                olPort.show();
                olPassword.show();
                olServer.show();
                olFrom.show();
                olTo.show();
                olSubject.show();
                olFromName.show();
                olToName.show();
                //olAttachName.show();
                
                oeFrom.show();
                oeTo.show();
                oeServer.show();
                oePort.show();
                oeSubject.show();
                opPassword.show();
                oeFromName.show();
                oeToName.show();
                
                og.show();
                //添加控件
                ow.add(og);
                og.add(olServer, 0,0);
                og.add(oeServer, 0, 1);
                
                og.add(olPort,0,2);
                og.add(oePort,0,3);
                
                og.add(olFrom,1,0);
                og.add(oeFrom,1,1);
        
                og.add(olPassword,1,2);
                og.add(opPassword,1,3);
                
                og.add(olTo,2,0);
                og.add(oeTo,2,1);
                og.add(olToName,2,2);
                og.add(oeToName,2,3);
                
                og.add(olSubject,3,0);
                og.add(oeSubject,3,1);
                og.add(olFromName,3,2);
                og.add(oeFromName,3,3);
                
                og.add(olMessage,4,0);
                og.add(osb,5,0,4,2);
                
                og.add(obAttach,7,0);
                og.add(olAttachName,7,1);
                
                og.add(olAttachDescription,8,0);
                og.add(oeAttachDescription,8,1);
                og.add(olAttachSetName,8,2);
                og.add(oeAttachSetName,8,3);
                
                og.add(ob, 9, 0);
                og.add(obApple,9,1);
                //文本监听
                ob.addClickedListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                Email email = new SimpleEmail();
                                email.setHostName(oeServer.getText());
                                email.setSmtpPort(Integer.parseInt(oePort.getText()));
                                email.setSubject(oeSubject.getText());
//                                email.setAuthenticator(new DefaultAuthenticator("zhaoturkkey@163.com", "457866zhao"));
                                email.setAuthenticator(new DefaultAuthenticator(oeFrom.getText(), opPassword.getText()));
                                email.setSSLOnConnect(true);
                                if(opPassword.getText().equalsIgnoreCase("")) 
                                {
                                        OOMessageDialog om = new OOMessageDialog("错误");
                                        om.showError("密码框不能为空！", "赶紧去填写");
                                        return;
                                }
                                //测试
/*                                System.out.println(oeServer.getText());
                                System.out.println(Integer.parseInt(oePort.getText()));
                                System.out.println(oeFrom.getText());
                                System.out.println(opPassword.getText());
                                System.out.println(oeSubject.getText());
                                System.out.println(otv.getText());*/
                                try
                                {
                                        email.setFrom(oeFrom.getText(),oeFromName.getText());
                                        email.setMsg(otv.getText());
                                        email.addTo(oeTo.getText(),oeToName.getText());
                                        email.send();
                                } catch (EmailException e)
                                {
                                        // TODO Auto-generated catch block
                                        System.out.println("发送邮件异常"+e.getMessage());
                                }
                        
                        }
                });
                //文本附件监听
                obApple.addClickedListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                 // Create the attachment
                                
                                if(opPassword.getText().equalsIgnoreCase("")) 
                                {
                                        OOMessageDialog om = new OOMessageDialog("错误");
                                        om.showError("密码框不能为空！", "赶紧去填写");
                                        return;
                                }
                                  EmailAttachment attachment = new EmailAttachment();
                                  //attachment.setPath("hunsha.jpg");
                                  //attachment.setPath("E:\\a1.zip");
                                  //attachment.setPath("E:\\1.jpg");
                                  if(oeAttachSetName.getText().equalsIgnoreCase(""))
                                  {
                                         OOMessageDialog om = new OOMessageDialog("错误");
                                        om.showError("你还没有添加附件呢！", "赶紧去添加");
                                        return;
                                  }
                                  attachment.setPath(filenames[0]);
                                  
                                  attachment.setDisposition(EmailAttachment.ATTACHMENT);
                                  attachment.setDescription(oeAttachDescription.getText());
                                  attachment.setName(oeAttachSetName.getText());
                                 
                                  // Create the email message
                                  MultiPartEmail email = new MultiPartEmail();
                                  email.setHostName(oeServer.getText());
                                  email.setSmtpPort(Integer.parseInt(oePort.getText()));
                                  email.setAuthenticator(new DefaultAuthenticator(oeFrom.getText(), opPassword.getText()));
                                  email.setSSLOnConnect(true);
                                  try
                                  {
                                          email.addTo(oeTo.getText(), oeToName.getText());
                                          email.setFrom(oeFrom.getText(), oeFromName.getText());
                                          email.setSubject(oeSubject.getText());
                                          email.setMsg(otv.getText());
                        
                                          // add the attachment
                                          email.attach(attachment);
                        
                                          // send the email
                                         
                                          email.send();
                                  }catch(EmailException e)
                                  {
                                          System.out.println("邮件发送失败 "+e.getMessage());
                                  }
                        }
                });
                
                obAttach.addClickedListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                OOFileChooser ofApple =new OOFileChooser();
                                ofApple.setMultipleSelect();
                                ofApple.createFilter();
                                ofApple.nameFilter("任意文件");
                                ofApple.editFilter("*.txt");
                                ofApple.editFilter("*.java");
                                ofApple.editFilter("*.rar");
                                ofApple.editFilter("*.zip");
                                ofApple.editFilter("*.doc");
                                //把filter的操作放入打开对话框中
                                ofApple.finishFilter();
                                //String[] filenames = ofApple.processOpen();
                                filenames = ofApple.processOpen();
                                
                                olAttachName.setText("你选择的文件是"+filenames[0].toString());
                                olAttachName.show();
                                olAttachSetName.show();
                                olAttachDescription.show();
                                oeAttachSetName.show();
                                oeAttachDescription.show();
                                //oeAttachSetName.setText(filenames);
                                int index = filenames[0].lastIndexOf(File.separator);
                                oeAttachSetName.setText(filenames[0].substring(index+1));
                                oeAttachDescription.setText("这个文件是关于***");
                        }
                });
                
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                GTK.gtk_init();
                MyEmailSend mes = new MyEmailSend();
                
                GTK.gtk_main();
        }

}

```
小软件运行的效果:
![EmialText的测试](/images/java/email.png)
![EmailAttachment附件的测试](/images/java/chuangjiang.png)
![文件选择框添加附件](/images/java/file.png)

