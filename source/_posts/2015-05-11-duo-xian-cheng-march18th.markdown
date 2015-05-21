---
layout: post
title: "多线程March18th"
date: 2015-05-11 14:58:45 +0800
comments: true
categories: JavaBasic
---
<!--more-->
4:java多线程

新建状态(start)
可运行状态(runnable)
运行状态(run)
阻塞装填(sleep)
死亡状态（stop,dead）


------------------------------------------------------------------------------------

start--->Runnable----> Running--->Dead
   |               |
   |               |
Threads    implements
------------------------------------------------------------------------------------
Running--->Wait--->Blocked--->notify---->Runnable
------------------------------------------------------------------------------------
Running--->sleep--->Runnable
------------------------------------------------------------------------------------


线程和进程的区别？
一个进程占用实际的内存资源，线程依赖于进程，一个进程可以有多个线程
进程大，线程小
每一个程序运行都运行在一个进程内，而不是线程
一个进程可分为多个线程



售票案例
问题： 第70张票还没售完不能售第73张的票，但是实际却是销售了？

```java
/**
* 解释：
*/
package TestMultiThreads;

/**
* @author 叶昭良
* @time 2015年3月18日下午9:09:27
* @version TestMultiThreadsTestPesonThread V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestPesonThread
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub

A aa = new A();
Thread t1 = new Thread(aa);
Thread t2 = new Thread(aa);
Thread t3 = new Thread(aa);

/*        B bb = new B();
Thread t1 = new Thread(bb);
Thread t2 = new Thread(bb);
Thread t3 = new Thread(bb);*/

t1.start();
t2.start();
t3.start();
}

}

class A implements Runnable
{
private static int tickets = 0;
@Override
public void run()
{
// TODO Auto-generated method stub
while(true)
{
//synchronized("aaa")
{
if(tickets < 10)
{
System.out.println(Thread.currentThread().getName()+tickets);
tickets++;
}else
{
break;
}
}

}
}

}


class B implements Runnable
{
private static int tickets = 0;
@Override
public synchronized void run()
{
// TODO Auto-generated method stub
while(true)
{
if(tickets < 10)
{
System.out.println(Thread.currentThread().getName()+tickets);
tickets++;
}else
{
break;
}
}
}

}
```

解决方案1： 采用synchronized("aaa") 或者 synchronized(TestPesonThread.class) 都是可以的，  相当于在一扇门中
塞入了口香糖，得拿走了才能打开！一个坑的作用

```java
@Override
public void run()
{
// TODO Auto-generated method stub
while(true)
{
//synchronized("aaa")
synchronized(TestPesonThread.class)
{
if(tickets < 10)
{
System.out.println(Thread.currentThread().getName()+tickets);
tickets++;
}else
{
break;
}
}

}
}

```



结果：（注意加入Thread.sleep(1000）不然都会在thread0 或者thread1 thread2中执行，因为少）

```java
Thread-0正在售10
Thread-2正在售9
Thread-1正在售8
Thread-2正在售7
Thread-2正在售6
Thread-2正在售5
Thread-0正在售4
Thread-2正在售3
Thread-2正在售2
Thread-2正在售1
```




解决方案2 ：  采用synchronized的run方法也是可以的！ 相当于锁定函数代码块，霸占着 直到用完！ 但是这个方法仅仅针对一个线程，执行完！一个对象霸占一个方法

```java
/**
* 解释：
*/
package TestMultiThreads;

/**
* @author 叶昭良
* @time 2015年3月18日下午9:09:27
* @version TestMultiThreadsTestPesonThread V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestPesonThread
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub

A aa = new A();
Thread t1 = new Thread(aa);
Thread t2 = new Thread(aa);
Thread t3 = new Thread(aa);

/*        B bb = new B();
Thread t1 = new Thread(bb);
Thread t2 = new Thread(bb);
Thread t3 = new Thread(bb);*/

t1.start();
t2.start();
t3.start();
}

}

class A implements Runnable
{
private static int tickets = 0;
@Override
public synchronized void run()
{
// TODO Auto-generated method stub
while(true)
{
//synchronized("aaa")
//synchronized(TestPesonThread.class)
{
if(tickets < 10)
{
System.out.println(Thread.currentThread().getName()+"正在售"+tickets);
tickets++;
}else
{
break;
}
}

}
}

}


class B implements Runnable
{
private static int tickets = 0;
@Override
public synchronized void run()
{
// TODO Auto-generated method stub
while(true)
{
if(tickets < 10)
{
System.out.println(Thread.currentThread().getName()+tickets);
tickets++;
}else
{
break;
}
}
}

}
```



结果：

```java
Thread-0正在售10
Thread-0正在售9
Thread-0正在售8
Thread-0正在售7
Thread-0正在售6
Thread-0正在售5
Thread-0正在售4
Thread-0正在售3
Thread-0正在售2
Thread-0正在售1
```





由此我们可以得知：
方法的霸占解决掉乱序，但是其实仅仅只是一个线程再执行，不能达到目的；
而采用synchronized对象 比如字符串对象"aaa",不然类的class :TestPesonThread.class 放在循环快中都能够很好的解决多线程共用一个模块，必须执行一块完毕，才能执行下一块的问题！！perfect


案例2：生产和消费

目的：通过线程模拟一个在生产  一个在消费，并且当量小于三的时候，必须再生产，在量

wait方法的作用是使得当前调用wait方法所在部分（代码块）的线程（当前线程）停止执行，并释放当前获得的调用wait所在的代码块的锁，并在其他线程调用notify或者notifyAll方法时恢复到竞争锁状态（一旦获得锁就恢复执行）。
有点类似巫医的弹弹乐效果，你被弹了，就停止，当别人被弹了，你就可以运动了


注意：
wait被调用的时候必须在拥有锁（即synchronized修饰的）的代码块中。
恢复执行后，从wait的下一条语句开始执行，因而wait方法总是应当在while循环中调用，以免出现恢复执行后继续执行的条件不满足却继续执行的情况。
notify方法通知调用了wait方法，但是尚未激活的一个线程进入线程调度队列（即进入锁竞争），注意不是立即执行。并且具体是哪一个线程不能保证。另外一点就是被唤醒的这个线程一定是在等待wait所释放的锁。
notifyAll方法则唤醒所有调用了wait方法，尚未激活的进程进入竞争队列。




有一个wait框，同时有一个监工notify,负责叫醒被暂停的waiter,去继续做他的被中断的工作
synstack 类定义了synchronized的push和pop方法
producer 进行push调用（可以被中断，当达到生产线程的上限时 就中断）
customer 进行pop调用（可以被中断，当没有产品的时候  并在产品数为3的时候通知生产）

```java
/**
* 解释：
*/
package TestMultiThreads;

/**
* @author 叶昭良
* @time 2015年3月18日下午9:54:48
* @version TestMultiThreadsTestProducerCustomer V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestProducerCustomer
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub
SynStack ss = new SynStack();
Producer pro = new Producer(ss);
Customer custom = new Customer(ss);

//Thread tt = new Thread();
/*        pro.run();
custom.run();*/

Thread t1 = new Thread(pro);
t1.start();
Thread t2 = new Thread(custom);
t2.start();

}

}


class SynStack
{
private char[] ss = new char[6];
//private static int count = 0;
private int count = 0;
public synchronized void push(char c)
{
try
{
Thread.sleep(500);
} catch (InterruptedException e1)
{
// TODO Auto-generated catch block
e1.printStackTrace();
}
//if(count == ss.length)
while(count == ss.length)
{
try
{
System.out.println("生产已达上限，再生产容易造成供需不平衡");
this.wait();
} catch (InterruptedException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}
}
/*        if(count == 3)
{
this.notify(); //如果达到3的时候通知他们进行生产
}*/
if(count > 1)
{
System.out.println(count+"大家可以来消费了");
this.notify(); //一有则通知大家进行消费
}


System.out.printf("It generates %d product.It is %c\n",count,c);
ss[count] = c;
count++;
}
//synchronized 如果不加，则报错
public synchronized char pop()
{
try
{
Thread.sleep(100);
} catch (InterruptedException e1)
{
// TODO Auto-generated catch block
e1.printStackTrace();
}
//if(count == 0)
while(count == 0)
{
//this.notify();//通知push线程进行生产
try
{
System.out.println("产品已售空，请联系生产商");
this.wait();
} catch (InterruptedException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}
}
if(count == 3)
{
System.out.println("该督促生产了，并且可以继续销售");
this.notify(); //如果达到3的时候通知他们进行生产（push）

}
//this.notify();

count--;
//必须放在count--之后 才可以调用ss[count]
System.out.printf("It ate %d product.It is %c\n",count,ss[count]);
return ss[count];

}
}

class Producer implements Runnable
{
private SynStack ss = null;

public Producer(SynStack ss)
{
this.ss =ss;
}
@Override
public void run()        
{
// TODO Auto-generated method stub
//        ss.push('a');
/*        try
{
Thread.sleep(1000);
} catch (InterruptedException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}*/
char ch ;
for(int i =0 ; i < 20 ; i++)
{
ch = (char)('a'+i);
ss.push(ch);
}
}

}
class Customer implements Runnable
{
private SynStack ss = null;
public Customer(SynStack ss)
{
this.ss = ss;
}
@Override
public void run()
{

// TODO Auto-generated method stub
//        System.out.println(ss.pop());
/*        try
{
Thread.sleep(1000);
} catch (InterruptedException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}*/
for(int i = 0; i < 20 ; i++)
{
System.out.println(ss.pop());
}
}
}
```





结果：

```java
It generates 0 product.It is a
It generates 1 product.It is b
2大家可以来消费了
It generates 2 product.It is c
该督促生产了，并且可以继续销售
It ate 2 product.It is c
c
2大家可以来消费了
It generates 2 product.It is d
该督促生产了，并且可以继续销售
It ate 2 product.It is d
d
It ate 1 product.It is b
b
It generates 1 product.It is e
It ate 1 product.It is e
e
It generates 1 product.It is f
2大家可以来消费了
It generates 2 product.It is g
该督促生产了，并且可以继续销售
It ate 2 product.It is g
g
2大家可以来消费了
It generates 2 product.It is h
3大家可以来消费了
It generates 3 product.It is i
It ate 3 product.It is i
i
3大家可以来消费了
It generates 3 product.It is j
4大家可以来消费了
It generates 4 product.It is k
5大家可以来消费了
It generates 5 product.It is l
生产已达上限，再生产容易造成供需不平衡
It ate 5 product.It is l
l
It ate 4 product.It is k
k
It ate 3 product.It is j
j
该督促生产了，并且可以继续销售
It ate 2 product.It is h
2大家可以来消费了
It generates 2 product.It is m
h
3大家可以来消费了
It generates 3 product.It is n
It ate 3 product.It is n
n
3大家可以来消费了
It generates 3 product.It is o
It ate 3 product.It is o
o
3大家可以来消费了
It generates 3 product.It is p
4大家可以来消费了
It generates 4 product.It is q
It ate 4 product.It is q
q
4大家可以来消费了
It generates 4 product.It is r
It ate 4 product.It is r
r
4大家可以来消费了
It generates 4 product.It is s
5大家可以来消费了
It generates 5 product.It is t
It ate 5 product.It is t
t
It ate 4 product.It is s
s
It ate 3 product.It is p
p
该督促生产了，并且可以继续销售
It ate 2 product.It is m
m
It ate 1 product.It is f
f
It ate 0 product.It is a
a


```



注意：至于notify放置在代码的哪个地方，有待进一步的比较。


5.网络编程

一个网络程序需要考虑协议，IP，端口号。

协议： 
用于网络传输的规则、准则。在不同的计算机传输层中（7层架构） 有不同的数据包，
不同
层封包的不同方法

IP:  唯一标识互联网中的计算机
端口号：qq有自己的端口  mysql的3306端口   smtp的23端口等 



协议分为UDP,TCP
UDP的特点：无需等待对方确认，比如写信 有可能收不到。
TCP的特点：需要等待 对方连接，比如QQ视频聊天，打电话。三重连接


UDP编程：
知识点：
DatagramSocket 是一艘轮船（传递数据包）
DatagramPacket 是一个轮船上的货物（获取数据包）


ByteArrayInputStream and  DataInputStream 用于打包港口货物
ByteArrayOutputStream and  DataOutputStream 也用于打包港口货物


案例1：简单实现一个UDP的传输过程，服务器开启，客户端发送数据（暂时未实现服务器的发送  客户端的接受，实现简单，dos对应的dis  dis对应的dos)
Server:

```java
/**
* 解释：
*/
package TestNetwork;

import java.io.ByteArrayInputStream;
import java.io.DataInputStream;
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;

/**
* @author 叶昭良
* @time 2015年3月19日下午1:49:36
* @version TestNetworkTestDatagramSocket V1.0
* 功能： 测试UDP
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestDatagramSocket
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub
DatagramSocket ds = null;
DatagramPacket dp = null;

try
{
ds = new DatagramSocket(5566);
byte[] apple = new byte[1025];
/*for(int i =0 ; i < 20; i++)
{
apple = (byte) ((byte)'a'+i);
}*/
dp = new DatagramPacket(apple, apple.length);
while(true)
{
try
{
ds.receive(dp);
ByteArrayInputStream bais = new ByteArrayInputStream(dp.getData());
DataInputStream dis = new DataInputStream(bais);
System.out.println(dis.readLong());

} catch (IOException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}

}
} catch (SocketException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}

}

}
```


Client

```java
/**
* 解释：
*/
package TestNetwork;
import java.io.*;
import java.net.*;
/**
* @author 叶昭良
* @time 2015年3月21日上午1:39:22
* @version TestNetworkTestDatagramClient V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestDatagramClient
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub
DatagramSocket ds = null;
DatagramPacket dp = null;
try
{
ds = new DatagramSocket();
long m = 1000l;
ByteArrayOutputStream baos = new ByteArrayOutputStream();
DataOutputStream dos = new DataOutputStream(baos);

dos.writeLong(m);

byte[] apple = baos.toByteArray();
dp = new DatagramPacket(apple, apple.length,new
InetSocketAddress("127.0.0.1",5566));
ds.send(dp);
}catch(IOException e)
{

}
}

}
```



结果：
```java
先开启Server,再运行Client
1000
```





TCP编程：
知识点：
1.通过ServerSocket产生一个运行的所需的端口号
2.利用Socket进行编程，Socket源自于unix操作系统


案例1： 模拟UDP的类似程序，即服务器端等待数据 进行读取   客户端发送数据

Server:

```java
/**
* 解释：
*/
package TestNetwork;
import java.io.*;
import java.net.*;
/**
* @author 叶昭良
* @time 2015年3月19日下午2:30:08
* @version TestNetworkTestServer1 V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestServer1
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub
try
{
ServerSocket ss = new ServerSocket(5566);
while(true)
{
Socket s1 =ss.accept();
DataInputStream dis = new DataInputStream(s1.getInputStream());
System.out.println(dis.readUTF());
dis.close();
}
} catch (IOException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}

}

}
```



Client:

```java
/**
* 解释：
*/
package TestNetwork;
import java.io.*;
import java.net.*;
/**
* @author 叶昭良
* @time 2015年3月19日下午2:30:23
* @version TestNetworkTestClient1 V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestClient1
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub
Socket s1 =null;
OutputStream os = null;
DataOutputStream dos = null;
try
{
s1 = new Socket("127.0.0.1",5566);
os = s1.getOutputStream();
dos = new DataOutputStream(os);
dos.writeUTF("I am coming ,Sir");
dos.flush();


} catch (IOException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}finally
{
try
{
dos.close();
os.close();
s1.close();
} catch (IOException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}

}

}

}
```






案例2：改进案例1  是的服务器端也能够发送  客户端也能接受数据

Server:
```java

<font size="3">/**
* 解释：
*/
package TestNetwork;
import java.io.*;
import java.net.*;
/**
* @author 叶昭良
* @time 2015年3月19日下午2:38:37
* @version TestNetworkTestServer2 V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestServer2
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub
Socket s1 = null ;
DataOutputStream dos = null;
DataInputStream dis = null;
BufferedReader br = null;
try
{
ServerSocket ss = new ServerSocket(5566);
s1 = ss.accept();

dis = new DataInputStream(s1.getInputStream());
dos = new DataOutputStream(s1.getOutputStream());
br = new BufferedReader(new InputStreamReader(System.in));

while(true)
{
String apple = dis.readUTF();
System.out.println("客户端："+dis.readUTF());
if(apple.equals("bye"))
{
break;
}
apple = br.readLine();
dos.writeUTF(apple);
}
} catch (IOException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}finally
{
try
{
dos.close();
dis.close();
s1.close();
} catch (IOException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}
}

}

}
</font>

```



Client:
```java
/**
* 解释：
*/
package TestNetwork;
import java.io.*;
import java.net.*;
/**
* @author 叶昭良
* @time 2015年3月19日下午2:38:52
* @version TestNetworkTestClient2 V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestClient2
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub
Socket s1 = null;
OutputStream os = null;
InputStream is = null;
DataOutputStream dos = null;
DataInputStream dis = null;
BufferedReader br = null;
try
{
s1 = new Socket("127.0.0.1",5566);
os = s1.getOutputStream();
is = s1.getInputStream();
dos = new DataOutputStream(os);
dis = new DataInputStream(is);

dos.writeUTF("Come to the world of net server");
dos.flush();

br = new BufferedReader(new InputStreamReader(System.in));
while(true)
{
String str = br.readLine();
dos.writeUTF(str);
if(str.equalsIgnoreCase("byebye"))
break;
str = dis.readUTF();
System.out.println("服务器 said that" + str+"\n");
if(str.equalsIgnoreCase("byebye"))
break;

}
} catch (IOException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}
}

}
```





结果：可以在字符界面下进行对话
客户端：hi
hi
服务器 said thathi too,smida

出现的问题是，无法独立的切换客户端和服务器，client接受信息后，一直存在于while循环中,于是使用了线程，让他们自己对话吧！！


Server的实现：
一定要注意无论是Server还是Client的实现，try---catch之后不能有finally关闭，否则异常，socket中止

```java
/**
* 解释：
*/
package TestNetwork;

/**
* @author 叶昭良
* @time 2015年3月21日下午12:43:24
* @version TestNetworkTestServer3 V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
import java.io.*;
import java.net.*;
public class TestServer3 //throws Exception
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args) 
{
// TODO Auto-generated method stub
ServerSocket ss = null;
Socket s = null;
DataOutputStream dos = null;
DataInputStream dis = null;
try
{
ss = new ServerSocket(8888);
s = ss.accept();
dos = new DataOutputStream(s.getOutputStream());

dis = new DataInputStream(s.getInputStream());

new ServerRead(dis).start();
new ServerWrite(dos).start();
} catch (IOException e)
{
// TODO Auto-generated catch block
e.printStackTrace();
}

}

}


class ServerRead extends Thread
{
private DataInputStream dis = null;
public ServerRead(DataInputStream dis)
{
this.dis = dis;
}

public void run()
{
while(true)
{
try{
String str = dis.readUTF();
System.out.println("客户端说:"+str);
if(str.equalsIgnoreCase("byebye"))
break;
}catch(Exception e)
{
e.printStackTrace();
}
}
}
}
class ServerWrite extends Thread
{
private DataOutputStream dos = null;
public ServerWrite(DataOutputStream dos)
{
this.dos = dos;
}

public void run()
{
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
while(true)
{
try{
String str = br.readLine();
dos.writeUTF(str);
if(str.equalsIgnoreCase("byebye"))
break;
}catch(Exception e)
{
e.printStackTrace();
}//加入finally 关闭  dos.close()则异常。。。于是删掉Client & Server的四个finally即可！！！问题解决  原因未知！！！
}
}
}

```




Client的实现：

```java
/**
* 解释：
*/
package TestNetwork;
import java.io.*;
import java.net.*;
/**
* @author 叶昭良
* @time 2015年3月21日下午12:47:03
* @version TestNetworkTestClient3 V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestClient3
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args) throws Exception
{
// TODO Auto-generated method stub
Socket s = new Socket("127.0.0.1",8888);
DataOutputStream dos = new DataOutputStream(s.getOutputStream());

DataInputStream dis = new DataInputStream(s.getInputStream());

new ClientRead(dis).start();
new ClientWrite(dos).start();
}

}

class ClientRead extends Thread
{
private DataInputStream dis = null;
public ClientRead(DataInputStream dis)
{
this.dis = dis;
}

public void run()
{
while(true)
{
try{
String str = dis.readUTF();
System.out.println("服务器说:"+str);
if(str.equalsIgnoreCase("byebye"))
break;
}catch(Exception e)
{
e.printStackTrace();
}
}
}
}
class ClientWrite extends Thread
{
private DataOutputStream dos = null;
public ClientWrite(DataOutputStream dos)
{
this.dos = dos;
}

public void run()
{
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
while(true)
{
try{
String str = br.readLine();
dos.writeUTF(str);
if(str.equalsIgnoreCase("byebye"))
break;
}catch(Exception e)
{
e.printStackTrace();
}
}
}
}
```



结果：
```java
客户端的控制台：

--------------------------------------------------------
客户端说:fs
fsdf
客户端说:heihei
吃了吗
客户端说:我吃了啊，下午去哪里玩？
----------------------------------------------------------


服务器端的控制台：
----------------------------------------------------------

fs

服务器说:fsdf

heihei
服务器说:吃了吗
我吃了啊，下午去哪里玩？
服务器说:不然咱们去闯红灯吧

----------------------------------------------------------
```




问题：这里面的线程切换，没有生产和消费的明显，还需要进一步理解线程的切换，为什么就能够从Server的读线程，然后写入信息给client就可以切换到Client的读线程？
          仅仅加入了extends thread?


案例4： 在原先的基础上 加入了窗体，并进一步抽象到TCPClient  & TCPServer两个类下，通过TCPClient.launch  TCPServer.launch进行启动
美化效果操作：
```java
Font font = new Font("Dialog", Font.PLAIN, 12); //一下是改变默认的组建上显示的字体，这样更加美观一些
UIManager.put("MenuBar.font", font);
UIManager.put("MenuItem.font", font);
UIManager.put("Menu.font", font);
UIManager.put("PopupMenu.font", font);
UIManager.put("ToolBar.font", font);
UIManager.put("ToolTip.font", font);
UIManager.put("TabbedPane.font", font);
UIManager.put("Label.font", font);
UIManager.put("List.font", font);
UIManager.put("ComboBox.font", font);
UIManager.put("Button.font", font);
UIManager.put("Table.font", font);
UIManager.put("TableHeader.font", font);
UIManager.put("Tree.font", font);
UIManager.put("TextField.font", font);
UIManager.put("TextArea.font", font);
UIManager.put("TitledBorder.font", font);
UIManager.put("OptionPane.font", font);
UIManager.put("RadioButton.font", font);
UIManager.put("CheckBox.font", font);
UIManager.put("ToggleButton.font", font);
UIManager.put("Dialog.font", font);
UIManager.put("Panel.font", font);
```



Server的实现：

```java
/**
* 解释：
*/
package TestNetwork;
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;


//some error swing : controler
import javax.swing.*;

import java.awt.*;
import java.awt.event.*;
/**
* @author 叶昭良
* @time 2015年3月19日下午3:20:34
* @version TestNetworkTestServer4 V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestServer4
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub
Font font = new Font("Dialog", Font.PLAIN, 12); //一下是改变默认的组建上显示的字体，这样更加美观一些
UIManager.put("MenuBar.font", font);
UIManager.put("MenuItem.font", font);
UIManager.put("Menu.font", font);
UIManager.put("PopupMenu.font", font);
UIManager.put("ToolBar.font", font);
UIManager.put("ToolTip.font", font);
UIManager.put("TabbedPane.font", font);
UIManager.put("Label.font", font);
UIManager.put("List.font", font);
UIManager.put("ComboBox.font", font);
UIManager.put("Button.font", font);
UIManager.put("Table.font", font);
UIManager.put("TableHeader.font", font);
UIManager.put("Tree.font", font);
UIManager.put("TextField.font", font);
UIManager.put("TextArea.font", font);
UIManager.put("TitledBorder.font", font);
UIManager.put("OptionPane.font", font);
UIManager.put("RadioButton.font", font);
UIManager.put("CheckBox.font", font);
UIManager.put("ToggleButton.font", font);
UIManager.put("Dialog.font", font);
UIManager.put("Panel.font", font);
new TCPServer().launch();
}

}


class TCPServer
{
// class variables
// class variables
// connect
private ServerSocket ss = null;
private Socket s = null;
// data flow
private DataOutputStream dos = null;
private DataInputStream dis = null;
// UI
private Frame f = null;
private TextArea ta = null;
private TextField tf = null;
private Button bn = null;


public void launch()
{
createUI();
connect();
new ServerRead().start();
new ServerWrite().start();
}
// construct member
/*
public TCPServer()
{

}
*/
public void createUI()
{
Frame f = new Frame("服务器");
ta = new TextArea();
tf = new TextField();
Panel p = new Panel(new BorderLayout());
bn = new Button("发送");
p.add(tf,BorderLayout.CENTER);
p.add(bn,BorderLayout.EAST);

f.add(ta,BorderLayout.CENTER);
f.add(p,BorderLayout.SOUTH);

f.setSize(400,200);
f.setVisible(true);
//f.setVisable(true);
f.addWindowListener(new WindowAdapter()
{
public void windowClosing(WindowEvent e)
{
System.exit(0);
}
});
}
public void connect()
{
try{
ss = new ServerSocket(5599);
s = ss.accept();
dis = new DataInputStream(s.getInputStream());
dos = new DataOutputStream(s.getOutputStream());
}catch(Exception e)
{
System.exit(0);
}
}
public void close()
{
try{
dis.close();
dos.close();
ss.close();
s.close();
}catch(Exception e)
{
System.exit(0);
}
}

class ServerRead extends Thread
{
public void run()
{
while(true)
{
try{
String str = dis.readUTF();
// System.out.println("对方说:"+str);
ta.append("客户端说："+str+"\n");
if(str.equalsIgnoreCase("再见"))
{
close();
System.exit(0);
}
}catch(Exception e)
{
JOptionPane.showMessageDialog(f,"客户端已离开");
return;
// e.printStackTrace();
}
}
}
}
class ServerWrite extends Thread
{
public void run()
{
tf.addActionListener(new TCPServerListener());
bn.addActionListener(new TCPServerListener());
}
}
class TCPServerListener implements ActionListener
{
@Override
public void actionPerformed(ActionEvent e)
{
try{
String str = tf.getText();
tf.setText("");
ta.append("服务器说:"+str+"\n");
dos.writeUTF(str);
if(str.equalsIgnoreCase("再见"))
{
close();
System.exit(0);
}
}catch(Exception e1)
{
// e.printStackTrace();
System.exit(0);
} 
}
}
}
```





Client的实现：

```java
/**
* 解释：
*/
package TestNetwork;
import java.net.*;
import java.io.*;

// some error swing : controler
import javax.swing.*;

import java.awt.*;
import java.awt.event.*;
/**
* @author 叶昭良
* @time 2015年3月19日下午3:22:27
* @version TestNetworkTestClient4 V1.0
* 功能： 
步骤：
* 注意：
* 掌握：
思考：
* 回顾：
*/
public class TestClient4
{

/**
* @param args 
* 原因：
* 解决：
* 功能：
* 思考： 
* 步骤：
*/
public static void main(String[] args)
{
// TODO Auto-generated method stub
Font font = new Font("Dialog", Font.PLAIN, 12); //一下是改变默认的组建上显示的字体，这样更加美观一些
UIManager.put("MenuBar.font", font);
UIManager.put("MenuItem.font", font);
UIManager.put("Menu.font", font);
UIManager.put("PopupMenu.font", font);
UIManager.put("ToolBar.font", font);
UIManager.put("ToolTip.font", font);
UIManager.put("TabbedPane.font", font);
UIManager.put("Label.font", font);
UIManager.put("List.font", font);
UIManager.put("ComboBox.font", font);
UIManager.put("Button.font", font);
UIManager.put("Table.font", font);
UIManager.put("TableHeader.font", font);
UIManager.put("Tree.font", font);
UIManager.put("TextField.font", font);
UIManager.put("TextArea.font", font);
UIManager.put("TitledBorder.font", font);
UIManager.put("OptionPane.font", font);
UIManager.put("RadioButton.font", font);
UIManager.put("CheckBox.font", font);
UIManager.put("ToggleButton.font", font);
UIManager.put("Dialog.font", font);
UIManager.put("Panel.font", font);
new TCPClient().launch();
}

}
class TCPClient
{
// class variables
// class variables
// connect
private Socket s = null;
// data flow
private DataOutputStream dos = null;
private DataInputStream dis = null;
// UI
private Frame f = null;
private TextArea ta = null;
private TextField tf = null;
private Button bn = null;


public void launch()
{
createUI();
connect();
new ClientRead().start();
new ClientWrite().start();
}
// construct member
/*
public TCPClient()
{

}
*/
public void createUI()
{
Frame f = new Frame("客户端");
ta = new TextArea();
tf = new TextField();
Panel p = new Panel(new BorderLayout());
bn = new Button("发送");
p.add(tf,BorderLayout.CENTER);
p.add(bn,BorderLayout.EAST);

f.add(ta,BorderLayout.CENTER);
f.add(p,BorderLayout.SOUTH);

f.setSize(400,200);
f.setVisible(true);
//f.setVisable(true);
f.addWindowListener(new WindowAdapter()
{
public void windowClosing(WindowEvent e)
{
System.exit(0);
}
});
}
public void connect()
{
try{
s = new Socket("127.0.0.1",5599);
dis = new DataInputStream(s.getInputStream());
dos = new DataOutputStream(s.getOutputStream());
}catch(Exception e)
{
System.exit(0);
}
}
public void close()
{
try{
dis.close();
dos.close();
s.close();
}catch(Exception e)
{
System.exit(0);
}
}

class ClientRead extends Thread
{
public void run()
{
while(true)
{
try{
String str = dis.readUTF();
// System.out.println("对方说:"+str);
ta.append("服务器说:"+str+"\n");
if(str.equalsIgnoreCase("再见"))
{
close();
System.exit(0);
}
}catch(Exception e)
{
JOptionPane.showMessageDialog(f,"客户端已离开");
return;
// e.printStackTrace();
}
}
}
}
class ClientWrite extends Thread
{
public void run()
{
tf.addActionListener(new TCPClientListener());
bn.addActionListener(new TCPClientListener());
}
}
class TCPClientListener implements ActionListener
{
@Override
public void actionPerformed(ActionEvent e)
{
try{
String str = tf.getText();
tf.setText("");
ta.append("客户端说:"+str+"\n");
dos.writeUTF(str);
if(str.equalsIgnoreCase("再见"))
{
close();
System.exit(0);
}
}catch(Exception e1)
{
// e.printStackTrace();
System.exit(0);
} 
}
}
}
```


![服务器端和客户端的对话窗口](/images/java/客户端服务端.png)





##重写TCPServer1 & TCPClient1 并加入了  String ip = s.getInetAddress().getHostAddress();
并且调整了编写风格，把所有类使用的变量都放在类中，并把socket创建和连接  接受和发送都放在构造函数中。类似于OOGTK的编写风格http://www.rupeng.com/forum/forum.php?mod=viewthread&tid=44377。

```java
/**
* 解释：
*/
package TestNetwork;

/**
* @author    叶昭良
* @time      2015年3月22日下午3:49:11
* @version   TestNetworkRewriteTestServer1 V1.0
* 功能： 
                步骤：
                1: Server绑定端口
                2：接受
                3：循环接收
* 注意：  socket的主要作用就是   getOutputStream  和getInputStream来确定
*       表明socket交流的方向
* 掌握：
                思考：
* 回顾：
*/
import java.io.*;
import java.net.*;
public class RewriteTestServer1
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        ServerSocket ss = null;
        Socket s = null;
        DataInputStream dis = null;
        public RewriteTestServer1()
        {
                try
                {
                        ss = new ServerSocket(5566);
                        s = ss.accept();
                        String ip = s.getInetAddress().getHostAddress();
                        dis = new DataInputStream(s.getInputStream());
                        System.out.println("客户端"+ip+" said: "+dis.readUTF());
                } catch (IOException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }finally
                {
                        try
                        {
                                dis.close();
                                s.close();
                        }catch(IOException e)
                        {
                                
                        }                
                }
                
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                new RewriteTestServer1();
        }

}
```
TCPClient1:

```java
/**
* 解释：
*/
package TestNetwork;

/**
* @author    叶昭良
* @time      2015年3月22日下午3:54:39
* @version   TestNetworkRewriteTestClient1 V1.0
* 功能： 
                步骤：  
                1: Socket的建立
                2： 发送数据
                3：。。
* 注意：
* 掌握：
                思考：
* 回顾：
*/
import java.io.*;
import java.net.*;
public class RewriteTestClient1
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        Socket s = null;
        DataOutputStream dos = null;
        public RewriteTestClient1()
        {
                try
                {
                        s = new Socket("127.0.0.1",5566);
                        dos = new DataOutputStream(s.getOutputStream());
                        dos.writeUTF("Hello");
                        dos.flush();
                } catch (IOException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                new RewriteTestClient1();
        }

}
```
重写了TestServer2 
功能：   在第一版本服务器的基础上  修正了客户端的只发送 不接受
*         以及服务器端的只接受不发送

*         并用BufferedReader获取System.in的内容输送到客户端
当客户端 也就是ss.accept之后，服务器来了一声“欢迎，ohni sang 光临”
紧接着客户端做了动作，接收到了服务器来的欢迎词，然后开始写入 一些话提交给服务器
反复执行这个过程
```java
/**
* 解释：
*/
package TestNetwork;

/**
* @author    叶昭良
* @time      2015年3月22日下午4:08:24
* @version   TestNetworkRewriteTestServer2 V1.0
* 功能： 
*              在第一版本服务器的基础上  修正了客户端的只发送 不接受
*         以及服务器端的只接受不发送
*         并用BufferedReader获取System.in的内容输送到客户端
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾： 回顾了第一版本的ServerSocket   ss.accept()  以及DataInputStream(s.getInputStream());
*/
import java.io.*;
import java.net.*;
public class RewriteTestServer2
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        ServerSocket ss = null;
        Socket s = null;
        DataInputStream dis = null;
        DataOutputStream dos = null;
        BufferedReader br = null;
        String wordsFromClient  =null;
        String wordsFromServer  =null;
        public RewriteTestServer2()
        {
                createConnection();
                String ip = s.getInetAddress().getHostAddress();

                while(true)
                {
                        try
                        {
                                
                                wordsFromClient = dis.readUTF();
                                System.out.println("客户端IP:"+ip+" said:"+wordsFromClient);
                                if(wordsFromClient.equalsIgnoreCase("bye*"))
                                {
                                        break;
                                }
                                System.out.println("服务器，请您输入你想对客户端说什么：");
                                wordsFromServer = br.readLine();
                                dos.writeUTF(wordsFromServer);
                        } catch (IOException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }finally
                        {
                                try
                                {
                                        dos.close();
                                        dis.close();
                                        s.close();
                                } catch (IOException e)
                                {
                                        // TODO Auto-generated catch block
                                        e.printStackTrace();
                                }
                        }
                        
                }
        }
        /**
         * 功能： 创建一个读的连接，用于读取客户端的socket数据
         *       创建一个写的连接，通过BufferedReader进行缓冲读取System.in的数据
         *       并通过DataOutputStream发送出去
         *       思考：        
         *       步骤：
         */
        private void createConnection()
        {
                try
                {
                        ss = new ServerSocket(5566);
                        s =  ss.accept();
                        dis = new DataInputStream(s.getInputStream());
                        dos = new DataOutputStream(s.getOutputStream());
                        //增加了下面两句
                        dos.writeUTF("您好！我是服务器想要聊些什么:");
                        dos.flush();
                        br = new BufferedReader(new InputStreamReader(System.in));
                }catch(IOException e)
                {
                        System.out.println("创建连接问题");
                }
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                new RewriteTestServer2();
        }

}
```


RewriteTestClient2:
```java
/**
* 解释：
*/
package TestNetwork;
import java.io.*;
import java.net.*;
/**
* @author    叶昭良
* @time      2015年3月19日下午2:38:52
* @version   TestNetworkTestClient2 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class TestClient2
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                Socket s1 = null;
                OutputStream os = null;
                InputStream is = null;
                DataOutputStream dos = null;
                DataInputStream dis = null;
                BufferedReader br = null;
                try
                {
                        s1 = new Socket("127.0.0.1",5566);
                        os = s1.getOutputStream();
                        is = s1.getInputStream();
                        dos = new DataOutputStream(os);
                        dis = new DataInputStream(is);
                        
                        dos.writeUTF("Come to the world of net server");
                        dos.flush();
                        
                        br = new BufferedReader(new InputStreamReader(System.in));
                        while(true)
                        {
                                String str = br.readLine();
                                dos.writeUTF(str);
                                if(str.equalsIgnoreCase("byebye"))
                    break;
                str = dis.readUTF();
                System.out.println("服务器 said that" + str+"\n");
                                if(str.equalsIgnoreCase("byebye"))
                    break;

                        }
                } catch (IOException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
        }

}
```
我遇到了一个问题：

```java
java.net.SocketException: socket closed
        at java.net.SocketInputStream.socketRead0(Native Method)
        at java.net.SocketInputStream.read(SocketInputStream.java:150)
        at java.net.SocketInputStream.read(SocketInputStream.java:121)
        at java.net.SocketInputStream.read(SocketInputStream.java:203)
        at java.io.DataInputStream.readUnsignedShort(DataInputStream.java:337)
        at java.io.DataInputStream.readUTF(DataInputStream.java:589)
        at java.io.DataInputStream.readUTF(DataInputStream.java:564)
        at TestNetwork.RewriteTestClient2.<init>(RewriteTestClient2.java:52)
        at TestNetwork.RewriteTestClient2.main(RewriteTestClient2.java:100)
```
于是我删掉了两个finally解决了问题，但是我不明白为什么？



重写了RewriteTestServer3 & RewriteTestClient3 ：
出现的问题： 在客户端连接上后，客户端发送数据可以到服务器端，接着切换到服务器端，但是服务器的输入出现了问题，无论你输入一个字还是n个字都无法切换回客户端了
主要不同点： 
*         1: 把读和写的过程 从while中脱离出来 变成线程的形式(采用了implements形式出现而不是 extends threads)
*      即读线程和写线程
*  2：把BufferedReader放入到写出线程中 ，因为只有在那边读取控制台的输入字符
3:   把线程的开启部分放在了构造函数中  而不是  main函数中。
                步骤：
测试了： 把implements Runnable 该回到extends threads结果一样

              把线程的定义和开启放在main函数，并让dos dis变为static变量，也不能解决问题，依旧是卡在服务器中，无法切回到客户端




一个有趣的问题：（是不是跟synchronized有关系，没关系，加进去也没什么效果）
```java
class ServerReWrite21 extends Thread
{
        private DataOutputStream dos = null;
        private BufferedReader br = null;
        private String wordsFromServer = null;
        public ServerReWrite21(DataOutputStream dos)
        {
                this.dos = dos;
        }
        
        @Override
        public void run()
        {
                //System.out.println("dos="+dos);  如果在ServerReWrite21中加入了这句话，则运行客户端之后 直接跳到这边  为什么？？？？？？？
```
有问题的RewriteTestServer3

```java
/**
* 解释：
*/
package TestNetwork;

/**
* @author    叶昭良
* @time      2015年3月22日下午4:49:25
* @version   TestNetworkRewriteTestServer3 V1.0
* 功能：  
*         1: 把读和写的过程 从while中脱离出来 变成线程的形式
*      即读线程和写线程
*  2：把BufferedReader放入到写出线程中 ，因为只有在那边读取控制台的输入字符
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
import java.io.*;
import java.net.*;
public class RewriteTestServer31
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        ServerSocket ss = null;
        Socket s = null;
         DataInputStream dis = null;
         DataOutputStream dos = null;
         
        public RewriteTestServer31()
        {
                createConnection();
                ServerReRead21 sr= new ServerReRead21(dis);
                ServerReWrite21 sw = new ServerReWrite21(dos);
                sr.start();
                sw.start();
        }
        
        /**
         * 功能： 创建一个读的连接，用于读取客户端的socket数据
         *       创建一个写的连接，通过BufferedReader进行缓冲读取System.in的数据
         *       并通过DataOutputStream发送出去
         *       思考：        
         *       步骤：
         */
        private void createConnection()
        {
                try
                {
                        ss = new ServerSocket(5566);
                        s =  ss.accept();
                        dis = new DataInputStream(s.getInputStream());
                        dos = new DataOutputStream(s.getOutputStream());
                        //增加了下面两句
                        dos.writeUTF("您好！我是服务器想要聊些什么:");
                        dos.flush();
                }catch(IOException e)
                {
                        System.out.println("创建连接问题");
                }
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                new RewriteTestServer31();
        }

}

class ServerReRead21 extends Thread
{
        private DataInputStream dis = null;
        private String wordsFromClient = null;
        public ServerReRead21(DataInputStream dis)
        {
                this.dis = dis;
        }
        @Override
        public void run()
        {
                // TODO Auto-generated method stub
                try
                {
                        wordsFromClient = dis.readUTF();
                        System.out.println("客户端对你说："+wordsFromClient);  
                } catch (IOException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                } 
        }
        
}

class ServerReWrite21 extends Thread
{
        private DataOutputStream dos = null;
        private BufferedReader br = null;
        private String wordsFromServer = null;
        public ServerReWrite21(DataOutputStream dos)
        {
                this.dos = dos;
        }
        
        @Override
        public void run()
        {
                //System.out.println("dos="+dos);
                // TODO Auto-generated method stub
                br = new BufferedReader(new InputStreamReader(System.in));
                while(true)
                {
                        try
                        {
                                wordsFromServer = br.readLine();
                                dos.writeUTF(wordsFromServer);
                                //要不要break????? if(bye){break}
                        }catch(IOException e)
                        {
                                
                        }
                }
        }
        
}
```


有问题的RewriteTestClient31:


```java
/**
* 解释：
*/
package TestNetwork;

import java.io.BufferedReader;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

/**
* @author    叶昭良
* @time      2015年3月22日下午5:12:56
* @version   TestNetworkRewriteTestClient3 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestClient31
{

        Socket s = null;
        DataInputStream dis = null;
    DataOutputStream dos = null;
        
        public RewriteTestClient31()
        {
                createConnection();
                ClientReRead31 sr = new ClientReRead31(dis);
                ClientReWrite31 sw = new ClientReWrite31(dos);
                sr.start();
                sw.start();        
        }
        
        /**
         * 功能： 创建一个读的连接，用于读取客户端的socket数据
         *       创建一个写的连接，通过BufferedReader进行缓冲读取System.in的数据
         *       并通过DataOutputStream发送出去
         *       思考：        
         *       步骤：
         */
        private void createConnection()
        {
                try
                {
                        s= new Socket("127.0.0.1",5566);
                        dis = new DataInputStream(s.getInputStream());
                        dos = new DataOutputStream(s.getOutputStream());
                        //增加了下面两句
                        
                }catch(IOException e)
                {
                        System.out.println("创建连接问题");
                }
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                new RewriteTestClient31();

        }

}
//主要更换地方 wordsFromServer  和服务器的不同地方
class ClientReRead31 extends Thread
{
        private DataInputStream dis = null;
        private String wordsFromServer  = null;
        public ClientReRead31(DataInputStream dis)
        {
                this.dis = dis;
        }
        @Override
        public void run()
        {
                // TODO Auto-generated method stub
                try
                {
                        wordsFromServer = dis.readUTF();
                        System.out.println("服务器端对你说："+wordsFromServer);  
                } catch (IOException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                } 
        }
        
}
//主要更换地方 wordsFromClient  和服务器的不同地方
class ClientReWrite31 extends Thread
{
        private DataOutputStream dos = null;
        private BufferedReader br = null;
        private String  wordsFromClient= null;
        public ClientReWrite31(DataOutputStream dos)
        {
                this.dos = dos;
        }
        @Override
        public void run()
        {
                // TODO Auto-generated method stub
                br = new BufferedReader(new InputStreamReader(System.in));
                while(true)
                {
                        try
                        {
                                wordsFromClient = br.readLine();
                                dos.writeUTF(wordsFromClient);
                                //要不要break????? if(bye){break}
                        }catch(IOException e)
                        {
                                
                        }
                }
        }
        
}
```


如果我把所有的变量定义在main当中则是没有问题的（在implements Runnable)

没问题的版本
RewriteTestServer3extends

```java
/**
* 解释：
*/
package TestNetwork;

import java.io.BufferedReader;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

/**
* @author    叶昭良
* @time      2015年3月22日下午5:57:16
* @version   TestNetworkRewriteTestServer3extends V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestServer3extends
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                // TODO Auto-generated method stub
        ServerSocket ss = null;
        Socket s = null;
        DataOutputStream dos  = null;
        DataInputStream dis = null;
                try
                {
                        ss = new ServerSocket(8888);
                        s = ss.accept();
                        dos = new DataOutputStream(s.getOutputStream());

                        dis  = new DataInputStream(s.getInputStream());
                       

                new Thread(new ServerReadImp(dis)).start();
                new Thread(new ServerWriteImp(dos)).start();
                } catch (IOException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }

        }

}
class ServerReadImp implements Runnable
{
    private DataInputStream dis = null;
    public ServerReadImp(DataInputStream dis)
    {
        this.dis = dis;
    }

    public void run()
    {
        while(true)
        {
           try{
                String str = dis.readUTF();
                System.out.println("客户端说:"+str);
                if(str.equalsIgnoreCase("byebye"))
                    break;
           }catch(Exception e)
           {
               e.printStackTrace();
           }
        }
    }
}
class ServerWriteImp implements Runnable
{
    private DataOutputStream dos = null;
    public ServerWriteImp(DataOutputStream dos)
    {
        this.dos = dos;
    }

    public void run()
    {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        while(true)
        {
           try{
                String str = br.readLine();
                dos.writeUTF(str);
                if(str.equalsIgnoreCase("byebye"))
                    break;
           }catch(Exception e)
           {
               e.printStackTrace();
           }
        }
    }
}
```


问题较少的RewriteTestClient3extends

```java
/**
* 解释：
*/
package TestNetwork;

import java.io.BufferedReader;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.InputStreamReader;
import java.net.Socket;

/**
* @author    叶昭良
* @time      2015年3月22日下午5:57:44
* @version   TestNetworkRewriteTestClient3extends V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestClient3extends 
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */

        public static void main(String[] args) throws Exception
        {
                // TODO Auto-generated method stub
        Socket s = new Socket("127.0.0.1",8888);
        DataOutputStream dos = new DataOutputStream(s.getOutputStream());

        DataInputStream dis = new DataInputStream(s.getInputStream());

        new Thread(new ClientReadImp(dis)).start();
        new Thread(new ClientWriteImp(dos)).start();

        }

}
class ClientReadImp implements Runnable
{
    private DataInputStream dis = null;
    public ClientReadImp(DataInputStream dis)
    {
        this.dis = dis;
    }

    public void run()
    {
        while(true)
        {
           try{
                String str = dis.readUTF();
                System.out.println("服务器说:"+str);
                if(str.equalsIgnoreCase("byebye"))
                    break;
           }catch(Exception e)
           {
               e.printStackTrace();
           }
        }
    }
}
class ClientWriteImp implements Runnable
{
    private DataOutputStream dos = null;
    public ClientWriteImp(DataOutputStream dos)
    {
        this.dos = dos;
    }

    public void run()
    {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        while(true)
        {
           try{
                String str = br.readLine();
                dos.writeUTF(str);
                if(str.equalsIgnoreCase("byebye"))
                    break;
           }catch(Exception e)
           {
               e.printStackTrace();
           }
        }
    }
}
```


可以看到的确是和变量放在哪里有着很大的关系，但是暂时不知道怎么解释？
还有网络编程，当服务器发送过后，是如何切换控制权的？（服务器的run处于while循环中，如何切换，有notify?(在生产和消费当中有wait and notify进行通知）




另外前两个版本（RewriteTCPServer1 RewriteTCPServer2)我进行重新改写则是没有问题，第三个版本(RewriteTCPServer3)加入了线程则是有问题,
所以程序线程+程序变量肯定有一个存在问题，具体不清楚







通过学习第十五节的网络编程，了解到while(true){ss.accept() ;  new线程的作用} 之前TCPServer3和RewriteServer3并没有多线程的效果，得在
服务器当中添加如下代码：增加一个while(true) 让服务器一直在等待连接（不然再ctrl+F11 并没有新的客户端连接服务器）

```java
private void createConnection()
        {
                try
                {
                        ss = new ServerSocket(5566);
                        while(true)
                        {
                                s =  ss.accept();
                                ServerReRead2 sr = new ServerReRead2(dis);
                                ServerReWrite2 sw = new ServerReWrite2(dos);
                                Thread t1 = new Thread(sr);
                                Thread t2 = new Thread(sw);
                                t1.start();
                                t2.start();
                                dis = new DataInputStream(s.getInputStream());
                                dos = new DataOutputStream(s.getOutputStream());
                                //增加了下面两句
                                dos.writeUTF("您好！我是服务器想要聊些什么:");
                                dos.flush();
                        }
                                

                }catch(IOException e)
                {
                        System.out.println("创建连接问题");
                }
        }
```




有了这个意识之后，我发现之前的所谓的多线程的UI版本的TCPServer4 &  TCPClient4也是lier,根本没有实现。
具体是： 一个服务器端对应一个客户端，虽然你写着extends thread，也start了，但是在ss.accept
那边根本就没有循环的接收，暂时进行修改未通过，这是另一个问题。





有进步了。改进了TCPServer4
1：客户端可以新建多个（但是这个多个还是有问题)--->具体分析socket的while循环放的有问题（有待进一步测试）

问题1：服务器无法发送出去数据
问题2：客户端不切换 还好可以，一切换之后发送一条就卡死了

有问题的
RewriteServer4:


```java
/**
* 解释：
*/
package TestNetwork;

/**
* @author    叶昭良
* @time      2015年3月22日下午10:08:18
* @version   TestNetworkRewriteTestServer4 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Frame;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.*;
import java.net.*;

import javax.swing.JOptionPane;


public class RewriteTestServer4
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                new TCPServer1().launch();
        }

}


class TCPServer1
{
        // class variables
        // class variables
        // connect
         private ServerSocket ss = null;
         private Socket s = null;
         // data flow
         private DataOutputStream dos = null;
         private DataInputStream dis = null;
         // UI
         private Frame f = null;
         private TextArea ta = null;
         private TextField tf = null;
         private Button  bn = null;

       
         public void launch()
         {
             createUI();
             connect();

         }
        // construct member
        /*
        public TCPServer()
        {

        }
        */
        public void createUI()
        {
            Frame f = new Frame("服务器");
            ta = new TextArea();
            tf = new TextField();
            Panel p = new Panel(new BorderLayout());
            bn = new Button("发送");
            p.add(tf,BorderLayout.CENTER);
            p.add(bn,BorderLayout.EAST);

            f.add(ta,BorderLayout.CENTER);
            f.add(p,BorderLayout.SOUTH);

            f.setSize(400,200);
            f.setVisible(true);
            //f.setVisable(true);
/*            tf.addActionListener(new TCPServerListener());
            bn.addActionListener(new TCPServerListener());*/
            f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
        }
        public void connect()
        {
                
            try
            {
                    ss = new ServerSocket(5599);
                    while(true)
                    {
                            //阻塞这边 等待客户端的连接
                        s = ss.accept();
                        dis = new DataInputStream(s.getInputStream());
                        dos = new DataOutputStream(s.getOutputStream());
                        dos.writeUTF("欢迎你的到来"+s.getInetAddress().getHostAddress());
                        new UserReadThread().start();
                        new UserWriteThread().start();
                        System.out.println(Thread.currentThread().getName()+"已开启");
                    }
                
            }catch(Exception e)
            {
                    System.out.println("服务器端异常");
               // System.exit(0);
            }
        }
        public void close()
        {
            try{
                ss.close();
                s.close();
            }catch(Exception e)
            {
                System.exit(0);
            }
        }
        
        class UserReadThread extends Thread
        {
                @Override
                public void run()
                {
                        while(true)
                {
                   try{
                        String str = dis.readUTF();
              //          System.out.println("对方说:"+str);
                        ta.append("客户端说："+str+"\n");
                        if(str.equalsIgnoreCase("再见"))
                        {
                            close();
                            System.exit(0);
                        }
                   }catch(Exception e)
                   {
                       JOptionPane.showMessageDialog(f,"客户端已离开");
                       return;
                   //    e.printStackTrace();
                   }
                }
                }
        }
        class UserWriteThread extends Thread
        {
                @Override
                public void run()
                {
                tf.addActionListener(new TCPServerListener());
                bn.addActionListener(new TCPServerListener());
                }
        }
        class TCPServerListener implements ActionListener
        {
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                           String str = tf.getText();
                       if(!str.isEmpty())
                       {
                               tf.setText("");
                                ta.append("服务器说:"+str+"\n");
                                dos.writeUTF(str);
                                if(str.equalsIgnoreCase("再见"))
                                {
                                    close();
                                    System.exit(0);
                                }
                       }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
}
```
RewriteTestClient4:
/**
* 解释：
*/
package TestNetwork;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Frame;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.net.Socket;

import javax.swing.JOptionPane;

import TestNetwork.TCPClient.TCPClientListener;

/**
* @author    叶昭良
* @time      2015年3月22日下午10:08:37
* @version   TestNetworkRewriteTestClient4 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestClient4
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                 new TCPClient1().launch();
        }

}

class TCPClient1
{
        // class variables
        // class variables
        // connect
         private Socket s = null;
         // data flow
         private DataOutputStream dos = null;
         private DataInputStream dis = null;
         // UI
         private Frame f = null;
         private TextArea ta = null;
         private TextField tf = null;
         private Button  bn = null;

       
         public void launch()
         {
             createUI();
             connect();
           //  new ClientRead().start();
            // new ClientWrite().start();
         }
        // construct member
        /*
        public TCPClient()
        {

        }
        */
        public void createUI()
        {
            Frame f = new Frame("客户端");
            ta = new TextArea();
            tf = new TextField();
            Panel p = new Panel(new BorderLayout());
            bn = new Button("发送");
            p.add(tf,BorderLayout.CENTER);
            p.add(bn,BorderLayout.EAST);

            f.add(ta,BorderLayout.CENTER);
            f.add(p,BorderLayout.SOUTH);

            f.setSize(400,200);
            f.setVisible(true);
            //f.setVisable(true);
                tf.addActionListener(new TCPClientListener());
                bn.addActionListener(new TCPClientListener());
            f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
        }
        public void connect()
        {
            try{
                s = new Socket("127.0.0.2",5599);
                dis = new DataInputStream(s.getInputStream());
                dos = new DataOutputStream(s.getOutputStream());
                    try{
                            //少了这个while不行！则不能循环接收
                            while(true)
                            {
                                          String str = dis.readUTF();
                                       //System.out.println("对方说:"+str);
                                       ta.append("服务器说:"+str+"\n");
                                       if(str.equalsIgnoreCase("再见"))
                                       {
                                           close();
                                           System.exit(0);
                                       }
                            }
                               
                      }catch(Exception e)
                      {
                          JOptionPane.showMessageDialog(f,"客户端已离开");
                          return;
                      }
                   
            }catch(Exception e)
            {
                    System.out.println("客户端异常");
                //System.exit(0);
            }
        }
        public void close()
        {
            try{
                dis.close();
                dos.close();
                s.close();
            }catch(Exception e)
            {
                System.exit(0);
            }
        }


        class TCPClientListener implements ActionListener
        {
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                        String str = tf.getText();
                        tf.setText("");
                        ta.append("客户端说:"+str+"\n");
                        dos.writeUTF(str);
                        if(str.equalsIgnoreCase("再见"))
                        {
                            close();
                            System.exit(0);
                        }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
}
```
结果：
![有问题的Server和Client图](/images/java/结果.png)


1： 改用在main方法中定义循环的socket等待试试，即
     ServerSocket ss = new ServerSocket(5599);
        
            while(true)
            {
                    new TCPServer1(ss.accept()).launch();
            }
复制代码
去掉ServerRead那边的一种Connection函数中的循环的socket.accept:

```java
public void connect()
        {
                
            try
            {
                    //while(true)
                    //{
                            //阻塞这边 等待客户端的连接
                        dis = new DataInputStream(s.getInputStream());
                        dos = new DataOutputStream(s.getOutputStream());
                        dos.writeUTF("欢迎你的到来"+s.getInetAddress().getHostAddress());
                        new UserReadThread().start();
                        new UserWriteThread().start();
                        System.out.println(Thread.currentThread().getName()+"已开启");
                    //}
```
2: 使用静态代码段的方法，只创建一个UI界面，不再把createUI放在launch当中，否则 只有在连接上客户端会创建客户端的同时也创建了服务器
   服务器一直在增加，这不是我们所想的，只要一个服务器，于是采用static方法

至少在这边已经开始注意一个问题： 到底Client的read & write要不要加入线程？ 这边是没有，仅仅是服务器的read & write加入了线程？ 



具体的RewreiteTestServer4:

```java
/**
* 解释：
*/
package TestNetwork;

/**
* @author    叶昭良
* @time      2015年3月22日下午10:08:18
* @version   TestNetworkRewriteTestServer4 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Frame;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.*;
import java.net.*;

import javax.swing.JOptionPane;


public class RewriteTestServer4
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args) throws Exception
        {
                // TODO Auto-generated method stub
            ServerSocket ss = new ServerSocket(5599);
        
            while(true)
            {
                    new TCPServer1(ss.accept()).launch();
            }
            
        }

}


class TCPServer1
{
        // class variables
        // class variables
        // connect
         
         private Socket s = null;
         // data flow
         private DataOutputStream dos = null;
         private DataInputStream dis = null;
         // UI
         private static Frame f = null;
         private static TextArea ta = null;
         private static TextField tf = null;
         private static Button  bn = null;
         static
         {
                 createUI();
         }
         public  TCPServer1(Socket s)
         {
                 this.s = s;
         }
       
         public void launch()
         {

             connect();

         }
        // construct member
        /*
        public TCPServer()
        {

        }
        */
        public static void createUI()
        {
            Frame f = new Frame("服务器");
            ta = new TextArea();
            tf = new TextField();
            Panel p = new Panel(new BorderLayout());
            bn = new Button("发送");
            p.add(tf,BorderLayout.CENTER);
            p.add(bn,BorderLayout.EAST);

            f.add(ta,BorderLayout.CENTER);
            f.add(p,BorderLayout.SOUTH);

            f.setSize(400,200);
            f.setVisible(true);
            //f.setVisable(true);
/*            tf.addActionListener(new TCPServerListener());
            bn.addActionListener(new TCPServerListener());*/
            f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
        }
        public void connect()
        {
                
            try
            {
                    //while(true)
                    //{
                            //阻塞这边 等待客户端的连接
                        dis = new DataInputStream(s.getInputStream());
                        dos = new DataOutputStream(s.getOutputStream());
                        dos.writeUTF("欢迎你的到来"+s.getInetAddress().getHostAddress());
                        new UserReadThread().start();
                        new UserWriteThread().start();
                        System.out.println(Thread.currentThread().getName()+"已开启");
                    //}
                
            }catch(Exception e)
            {
                    System.out.println("服务器端异常");
               // System.exit(0);
            }
        }
        public void close()
        {
            try{
                s.close();
            }catch(Exception e)
            {
                System.exit(0);
            }
        }
        
        class UserReadThread extends Thread
        {
                @Override
                public void run()
                {
                        while(true)
                {
                   try{
                        String str = dis.readUTF();
              //          System.out.println("对方说:"+str);
                        ta.append("客户端说："+str+"\n");
                        if(str.equalsIgnoreCase("再见"))
                        {
                            close();
                            System.exit(0);
                        }
                   }catch(Exception e)
                   {
                       JOptionPane.showMessageDialog(f,"客户端已离开");
                       return;
                   //    e.printStackTrace();
                   }
                }
                }
        }
        class UserWriteThread extends Thread
        {
                @Override
                public void run()
                {
                tf.addActionListener(new TCPServerListener());
                bn.addActionListener(new TCPServerListener());
                }
        }
        class TCPServerListener implements ActionListener
        {
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                           String str = tf.getText();
                       if(!str.isEmpty())
                       {
                               tf.setText("");
                                ta.append("服务器说:"+str+"\n");
                                dos.writeUTF(str);
                                if(str.equalsIgnoreCase("再见"))
                                {
                                    close();
                                    System.exit(0);
                                }
                       }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
}
```
RewriteClient4:


```java
/**
* 解释：
*/
package TestNetwork;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Frame;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.net.Socket;

import javax.swing.JOptionPane;

import TestNetwork.TCPClient.TCPClientListener;

/**
* @author    叶昭良
* @time      2015年3月22日下午10:08:37
* @version   TestNetworkRewriteTestClient4 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestClient4
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                 new TCPClient1().launch();
        }

}

class TCPClient1
{
        // class variables
        // class variables
        // connect
         private Socket s = null;
         // data flow
         private DataOutputStream dos = null;
         private DataInputStream dis = null;
         // UI
         private Frame f = null;
         private TextArea ta = null;
         private TextField tf = null;
         private Button  bn = null;

       
         public void launch()
         {
             createUI();
             connect();
           //  new ClientRead().start();
            // new ClientWrite().start();
         }
        // construct member
        /*
        public TCPClient()
        {

        }
        */
        public void createUI()
        {
            Frame f = new Frame("客户端");
            ta = new TextArea();
            tf = new TextField();
            Panel p = new Panel(new BorderLayout());
            bn = new Button("发送");
            p.add(tf,BorderLayout.CENTER);
            p.add(bn,BorderLayout.EAST);

            f.add(ta,BorderLayout.CENTER);
            f.add(p,BorderLayout.SOUTH);

            f.setSize(400,200);
            f.setVisible(true);
            //f.setVisable(true);
                tf.addActionListener(new TCPClientListener());
                bn.addActionListener(new TCPClientListener());
            f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
        }
        public void connect()
        {
            try{
                s = new Socket("127.0.0.1",5599);
                dis = new DataInputStream(s.getInputStream());
                dos = new DataOutputStream(s.getOutputStream());
                    try{
                            //少了这个while不行！则不能循环接收
                            while(true)
                            {
                                          String str = dis.readUTF();
                                       //System.out.println("对方说:"+str);
                                       ta.append("服务器说:"+str+"\n");
                                       if(str.equalsIgnoreCase("再见"))
                                       {
                                           close();
                                           System.exit(0);
                                       }
                            }
                               
                      }catch(Exception e)
                      {
                          JOptionPane.showMessageDialog(f,"客户端已离开");
                          return;
                      }
                   
            }catch(Exception e)
            {
                    System.out.println("客户端异常");
                //System.exit(0);
            }
        }
        public void close()
        {
            try{
                dis.close();
                dos.close();
                s.close();
            }catch(Exception e)
            {
                System.exit(0);
            }
        }


        class TCPClientListener implements ActionListener
        {
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                        String str = tf.getText();
                        tf.setText("");
                        ta.append("客户端说:"+str+"\n");
                        dos.writeUTF(str);
                        if(str.equalsIgnoreCase("再见"))
                        {
                            close();
                            System.exit(0);
                        }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
}
```
结果：

通过分析发现，的确是需要把socket.accept放在main中，而不是connection中！ 为什么？ 也许是connection会跳出？ 具体的不太清楚，如果有人知道求证明之。。。。

另外还存在一个问题，为什么服务器发出的数据只是放在第一个连接的地方？ 这是不是跟Client没有线程有关系，如果我也建个clientRead & clientWrite线程？
![现在的切换和传递都不成问题](/images/java/切换不成问题.png)






1：增加了两个ClientRead & ClientWrite两个线程，尝试看一下是否可以群发，
2： 并把socket的创建 提到main当中

Client的重写：

```java
/**
* 解释：
*/
package TestNetwork;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Frame;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.BufferedReader;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.Socket;

import javax.swing.JOptionPane;



/**
* @author    叶昭良
* @time      2015年3月22日下午10:08:37
* @version   TestNetworkRewriteTestClient4 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestClient4
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                Socket s  = null;
                 try
                {
                        s = new Socket("127.0.0.1",5599);
                } catch (IOException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                 new TCPClient1(s).launch();
        }

}

class TCPClient1
{
        // class variables
        // class variables
        // connect
         private Socket s = null;
         // data flow
         private DataOutputStream dos = null;
         private DataInputStream dis = null;
         // UI
         private Frame f = null;
         private TextArea ta = null;
         private TextField tf = null;
         private Button  bn = null;
         public TCPClient1(Socket s)
         {
                 this.s = s;
         }
       
         public void launch()
         {
             createUI();
             connect();
             new ClientRead(dis).start();
             new ClientWrite(dos).start();
         }
        // construct member
        /*
        public TCPClient()
        {

        }
        */
        public void createUI()
        {
            Frame f = new Frame("客户端");
            ta = new TextArea();
            tf = new TextField();
            Panel p = new Panel(new BorderLayout());
            bn = new Button("发送");
            p.add(tf,BorderLayout.CENTER);
            p.add(bn,BorderLayout.EAST);

            f.add(ta,BorderLayout.CENTER);
            f.add(p,BorderLayout.SOUTH);

            f.setSize(400,200);
            f.setVisible(true);
            //f.setVisable(true);
                tf.addActionListener(new TCPClientListener());
                bn.addActionListener(new TCPClientListener());
            f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
        }
        public void connect()
        {
            try{
                
                dis = new DataInputStream(s.getInputStream());
                dos = new DataOutputStream(s.getOutputStream());
                   
            }catch(Exception e)
            {
                    System.out.println("客户端异常");
                //System.exit(0);
            }
        }
        public void close()
        {
            try{
                dis.close();
                dos.close();
                s.close();
            }catch(Exception e)
            {
                System.exit(0);
            }
        }
        class ClientRead extends Thread
        {
            private DataInputStream dis = null;
            public ClientRead(DataInputStream dis)
            {
                this.dis = dis;
            }

            public void run()
            {
                while(true)
                {
                   try{
                        String str = dis.readUTF();
                        ta.append("服务器说:"+str+"\n");
                        if(str.equalsIgnoreCase("再见"))
                        {
                            close();
                            System.exit(0);
                        }
                        System.out.println("服务器说:"+str);
                        if(str.equalsIgnoreCase("byebye"))
                            break;
                   }catch(Exception e)
                   {
                              JOptionPane.showMessageDialog(f,"客户端已离开");
                              return;
                   }
                }
            }
        }
        class ClientWrite extends Thread
        {
            private DataOutputStream dos = null;
            public ClientWrite(DataOutputStream dos)
            {
                this.dos = dos;
            }

            public void run()
            {
                BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
                while(true)
                {
                   try{
                        String str = br.readLine();
                        dos.writeUTF(str);
                        if(str.equalsIgnoreCase("byebye"))
                            break;
                   }catch(Exception e)
                   {
                       e.printStackTrace();
                   }
                }
            }
        }

        class TCPClientListener implements ActionListener
        {
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                        String str = tf.getText();
                        tf.setText("");
                        ta.append("客户端说:"+str+"\n");
                        dos.writeUTF(str);
                        if(str.equalsIgnoreCase("再见"))
                        {
                            close();
                            System.exit(0);
                        }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
}
```
结果：
结果分析发现，并没有丝毫的改进。what a pity.

![结果还是跟上例的client没有线程一样](/images/java/yiranmeiyou.png)


参考此文进行修改：

1：加入了三个主要集合   
  用户   user_list
  线程集
  消息集
2：服务器端的主要修正：
    1.在线程类的构造函数都添加 了start() 开启线程
    2.删掉了ServerWrite &ServerRead线程  都整合到ThreadServer类中
    3.增加了一个PrintOutThread用于向客户端发送消息
3：客户端的主要修正：
   新建了一个readLineThread() 读取服务器数据，并传递客户端数据
   删除了ClientReader  & ClientWriter  & launch等函数


基本能够实现转发的功能，但是
存在的问题：开启多个则计算机变得特别卡


RewriteServer5的实现：


```java
/**
* 解释：
*/
package TestNetwork;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Frame;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.*;





/**
* @author    叶昭良
* @time      2015年3月23日上午12:41:10
* @version   TestNetworkRewriteTestServer5 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestServer5
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args) throws Exception
        {
                // TODO Auto-generated method stub
            ServerSocket ss = new ServerSocket(5599);
        while(true)
        {
                new TCPServer2(ss.accept());
        }
            
        }

}
class TCPServer2
{
        // class variables
        // class variables
        // connect
         
         private Socket s = null;
         // data flow
         private DataOutputStream dos = null;
         private DataInputStream dis = null;
         // UI
         private static Frame f = null;
         private static TextArea ta = null;
         private static TextField tf = null;
         private static Button  bn = null;
         
         private static boolean isPrint = false;//是否输出消息标志
         private static List user_list = new ArrayList();//登陆用户集合
         private static List<ServerThread> thread_list = new ArrayList<ServerThread>();//服务器已启用的线程集合
         private static LinkedList<String> message_list = new LinkedList<String>();//存放消息队列
      
         static
         {
                 createUI();
         }
         public  TCPServer2(Socket s) throws Exception
         {
                 this.s = s;
                 new PrintOutThread();//创建向客户端发送消息线程
             new ServerThread(this.s);  
             //本以为可以让其线程安全的，没想到没什么效果 反而更卡
/*             Collections.synchronizedList(user_list);
             Collections.synchronizedList(thread_list);
             Collections.synchronizedList(message_list);*/
            // launch();
                 
         }
       
        // construct member
        /*
        public TCPServer()
        {

        }
        */
        public static void createUI()
        {
            Frame f = new Frame("服务器");
            ta = new TextArea();
            tf = new TextField();
            Panel p = new Panel(new BorderLayout());
            bn = new Button("发送");
            p.add(tf,BorderLayout.CENTER);
            p.add(bn,BorderLayout.EAST);

            f.add(ta,BorderLayout.CENTER);
            f.add(p,BorderLayout.SOUTH);

            f.setSize(400,200);
            f.setVisible(true);
            //f.setVisable(true);
/*            tf.addActionListener(new TCPServerListener());
            bn.addActionListener(new TCPServerListener());*/
            f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
        }
        

        public void close()
        {
            try{
                s.close();
            }catch(Exception e)
            {
                System.exit(0);
            }
        }
     
        class TCPServerListener implements ActionListener
        {
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                           String str = tf.getText();
                       if(!str.isEmpty())
                       {
                               tf.setText("");
                                ta.append("服务器说:"+str+"\n");
                                dos.writeUTF(str);
                                if(str.equalsIgnoreCase("再见"))
                                {
                                    close();
                                    System.exit(0);
                                }
                       }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
        
        /**
         * 服务器线程类
         */
        class ServerThread extends Thread{
                private Socket client;
                private String name = null;
                public ServerThread(Socket s) throws Exception
                {
                        this.client = s;
                        dis = new DataInputStream(client.getInputStream());
                dos = new DataOutputStream(client.getOutputStream());
                dos.writeUTF("成功连上服务器"+client.getInetAddress().getHostAddress());
                dos.writeUTF(Thread.currentThread().getName()+"已开启"+"请输入你的姓名");
                this.start();
                tf.addActionListener(new TCPServerListener());
                bn.addActionListener(new TCPServerListener());
                }
                @Override
                public void run()
                {
                        try
                        {
                                int flag = 0 ;
                                String line = dis.readUTF();
                                while(!"bye".equals(line))
                                {
                                        //查看在线用户列表
                        if ("showuser".equals(line)) {
                            dos.writeUTF(this.listOnlineUsers());
                            line = dis.readUTF();
                        }
                        //第一次进入，保存名字

                        if(flag++ ==0){
                            name = line;
                            user_list.add(name);
                            thread_list.add(this);
                            System.out.println("this="+this);
                            dos.writeUTF(name +"你好,可以开始聊天了...");
                            this.pushMessage("Client<" + name +">进入聊天室...+\n");
                            ta.append("Client<" + name +">进入聊天室...\n");
                        }else{
                                ta.append("Client<" + name +"> say : " + line+"\n");
                                this.pushMessage("Client<" + name +"> say : " + line+"\n");
                        }
                        line = dis.readUTF();
                                }
                        }catch(IOException e)
                        {
                                
                        }
                }
                
                 //放入消息队列末尾，准备发送给客户端
            private void pushMessage(String msg){
                message_list.addLast(msg);
                isPrint =true;
            }
              
            //向客户端发送一条消息(在PrintOutThread 有使用 于是变成了public修饰)
            public void sendMessage(String msg) throws Exception{
                dos.writeUTF(msg);
                dos.flush();
            }
              
            //统计在线用户列表  需要在showuser中使用  改为public
            private String listOnlineUsers() {
                String s ="--- 在线用户列表 ---\015\012";
                for (int i =0; i < user_list.size(); i++) {
                    s +="[" + user_list.get(i) +"]\015\012";
                }
                s +="--------------------";
                return s;
            }
        }
        /**
         * 监听是否有输出消息请求线程类,向客户端发送消息
         */
        class PrintOutThread extends Thread
        {
            public PrintOutThread()
            {
                start();
            }
              
            @Override
            public void run() 
            {
                while(true){
                    if(isPrint){//将缓存在队列中的消息按顺序发送到各客户端，并从队列中清除。
                        String message = message_list.getFirst();
                        for (ServerThread thread : thread_list) 
                        {
                            try
                                                        {
                                                                thread.sendMessage(message);
                                                        } catch (Exception e)
                                                        {
                                                                // TODO Auto-generated catch block
                                                                e.printStackTrace();
                                                        }
                        }
                        message_list.removeFirst();
                        isPrint = message_list.size() >0 ?true :false;
                    }
                }
            }
        }
}
```



RewriteClient5的实现：


```java
/**
* 解释：
*/
package TestNetwork;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Frame;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.BufferedReader;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.Socket;

import javax.swing.JOptionPane;

import TestNetwork.TCPClient1.ClientRead;
import TestNetwork.TCPClient1.ClientWrite;
import TestNetwork.TCPClient1.TCPClientListener;

/**
* @author    叶昭良
* @time      2015年3月23日上午1:57:54
* @version   TestNetworkRewriteTestClient5 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestClient5
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args) throws Exception
        {
                // TODO Auto-generated method stub
                Socket s  = null;
                s = new Socket("127.0.0.1",5599);

                 new TCPClient5(s);
        }

}
class TCPClient5
{
        // class variables
        // class variables
        // connect
         private Socket s = null;
         // data flow
         private DataOutputStream dos = null;
         private DataInputStream dis = null;
         // UI
         private Frame f = null;
         private TextArea ta = null;
         private TextField tf = null;
         private Button  bn = null;
         
         /**
          * 与服务器连接，并输入发送消息
          */
         public TCPClient5(Socket s) throws Exception
         {
                 this.s = s;
                 dis = new DataInputStream(s.getInputStream());
             dos = new DataOutputStream(s.getOutputStream());
             createUI();
             //new ClientRead(dis);
             //new ClientWrite(dos);
             new readLineThread();

         }
         /**
          * 用于监听服务器端向客户端发送消息线程类
          */
  
        public void createUI()
        {
            Frame f = new Frame("客户端");
            ta = new TextArea();
            tf = new TextField();
            Panel p = new Panel(new BorderLayout());
            bn = new Button("发送");
            p.add(tf,BorderLayout.CENTER);
            p.add(bn,BorderLayout.EAST);

            f.add(ta,BorderLayout.CENTER);
            f.add(p,BorderLayout.SOUTH);

            f.setSize(400,200);
            f.setVisible(true);
            //f.setVisable(true);
                tf.addActionListener(new TCPClientListener());
                bn.addActionListener(new TCPClientListener());
            f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
        }

        public void close()
        {
            try{
                dis.close();
                dos.close();
                s.close();
            }catch(Exception e)
            {
                System.exit(0);
            }
        }
        /**
         * 用于监听服务器端向客户端发送消息线程类
         */
        class readLineThread extends Thread
        {
              
            private BufferedReader buff = null;;
            public readLineThread(){
                    start();
                
            }
              
            @Override
            public void run() {
                try {
                    while(true){
                        String result = dis.readUTF();
                        if("byeClient".equals(result)){//客户端申请退出，服务端返回确认退出
                            break;
                        }else{//输出服务端发送消息
                                //ta.append(result);
                            System.out.println(result);
                           // dos.writeUTF(result);
                            ta.append("服务器传递说:"+result+"\n");
                            if(result.equalsIgnoreCase("再见"))
                            {
                                close();
                                System.exit(0);
                            }
                        }
                    }
                }catch (Exception e) {
                }
            }
        }
        
        class TCPClientListener implements ActionListener
        {
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                        String str = tf.getText();
                        tf.setText("");
                        ta.append("客户端说:"+str+"\n");
                        dos.writeUTF(str);
                        if(str.equalsIgnoreCase("再见"))
                        {
                            close();
                            System.exit(0);
                        }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
}

```



结果： 还有一问题待解决，运行多个界面，程序占用内存过大！！！！！！ 如何解决？？

![服务器转发客户端实现局部聊天](/images/java/jiben.png)

还有一个问题：服务器说的话只能发给一个人。

解决办法：
修改TCPServerListener的实现：
```java
private ServerThread st = null;
                public TCPServerListener(ServerThread s)
                {
                        st =s ;
                }
```
并在actionPerformed中增加 st.pushMessage();

```java
                   try{
                           String str = tf.getText();
                       if(!str.isEmpty())
                       {
                                       tf.setText("");
                                ta.append("服务器说:"+str+"\n");
                                dos.writeUTF(str);
                                if(str.equalsIgnoreCase("再见"))
                                {
                                    close();
                                    System.exit(0);
                                }
                                //ta.append("Client<" + name +"> say : " + line+"\n");
                                st.pushMessage("服务器说:"+str+"\n");
                       }
```
当然在ServerThread服务器线程类中，增加

```java
private ServerThread st = null; // 受到启发的作用！！ Client=s

this.st = this;
的确这样修改完有效果，能够群发了，但是就是有些哥们的客户端发的是两条重复信息，有些却是一条。


重要修改，能够保证服务器发的信息不至于发太多给某一个客户端：
                           String str = tf.getText();
                       if(!str.isEmpty())
                       {
                                       tf.setText("");
                                ta.append("服务器说:"+str+"\n");
                                //dos.writeUTF(str);  //如果不注释掉 其中有一个用户会得到两条重复信息，都把他放在message_list中进行转发，而不是特例的一个利用dos.writeUTF(str)
                                if(str.equalsIgnoreCase("再见"))
                                {
                                    close();
                                    System.exit(0);
                                }
                               
                                st.pushMessage("服务器说:"+str+"\n");
                       }



```


基于此我们的Server端如下：



```java
/**
* 解释：
*/
package TestNetwork;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Frame;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.*;





/**
* @author    叶昭良
* @time      2015年3月23日上午12:41:10
* @version   TestNetworkRewriteTestServer5 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestServer5
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args) throws Exception
        {
                // TODO Auto-generated method stub
            ServerSocket ss = new ServerSocket(5599);
        while(true)
        {
                new TCPServer2(ss.accept());
        }
            
        }

}
class TCPServer2
{
        // class variables
        // class variables
        // connect
         
         private Socket s = null;
         // data flow
         private DataOutputStream dos = null;
         private DataInputStream dis = null;
         // UI
         private static Frame f = null;
         private static TextArea ta = null;
         private static TextField tf = null;
         private static Button  bn = null;
         
         private static boolean isPrint = false;//是否输出消息标志
         private static List user_list = new ArrayList();//登陆用户集合
         private static List<ServerThread> thread_list = new ArrayList<ServerThread>();//服务器已启用的线程集合
         private static LinkedList<String> message_list = new LinkedList<String>();//存放消息队列
      
         static
         {
                 createUI();
         }
         public  TCPServer2(Socket s) throws Exception
         {
                 this.s = s;
                 new PrintOutThread();//创建向客户端发送消息线程
             new ServerThread(this.s);  
             //本以为可以让其线程安全的，没想到没什么效果 反而更卡
/*             Collections.synchronizedList(user_list);
             Collections.synchronizedList(thread_list);
             Collections.synchronizedList(message_list);*/
            // launch();
                 
         }
       
        // construct member
        /*
        public TCPServer()
        {

        }
        */
        public static void createUI()
        {
            Frame f = new Frame("服务器");
            ta = new TextArea();
            tf = new TextField();
            Panel p = new Panel(new BorderLayout());
            bn = new Button("发送");
            p.add(tf,BorderLayout.CENTER);
            p.add(bn,BorderLayout.EAST);

            f.add(ta,BorderLayout.CENTER);
            f.add(p,BorderLayout.SOUTH);

            f.setSize(400,200);
            f.setVisible(true);
            //f.setVisable(true);
/*            tf.addActionListener(new TCPServerListener());
            bn.addActionListener(new TCPServerListener());*/
            f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
        }
        

        public void close()
        {
            try{
                s.close();
            }catch(Exception e)
            {
                System.exit(0);
            }
        }
     
        class TCPServerListener implements ActionListener
        {
                private ServerThread st = null;
                public TCPServerListener(ServerThread s)
                {
                        st =s ;
                }
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                           String str = tf.getText();
                       if(!str.isEmpty())
                       {
                                       tf.setText("");
                                ta.append("服务器说:"+str+"\n");
                                //dos.writeUTF(str);
                                if(str.equalsIgnoreCase("再见"))
                                {
                                    close();
                                    System.exit(0);
                                }
                               
                                st.pushMessage("服务器说:"+str+"\n");
                       }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
        
        /**
         * 服务器线程类
         */
        class ServerThread extends Thread{
                private Socket client;
                private String name = null;
                private ServerThread st = null; // 受到启发的作用！！ Client=s
                public ServerThread(Socket s) throws Exception
                {
                        this.client = s;
                        this.st = this;
                        dis = new DataInputStream(client.getInputStream());
                dos = new DataOutputStream(client.getOutputStream());
                dos.writeUTF("成功连上服务器"+client.getInetAddress().getHostAddress());
                dos.writeUTF(Thread.currentThread().getName()+"已开启"+"请输入你的姓名");
                this.start();
                //tf的监听器
                tf.addActionListener(new TCPServerListener(this.st));
                bn.addActionListener(new TCPServerListener(this.st));
                }
                @Override
                public void run()
                {
                        try
                        {
                                int flag = 0 ;
                                String line = dis.readUTF();
                                while(!"bye".equals(line))
                                {
                                        //查看在线用户列表
                        if ("showuser".equals(line)) {
                            dos.writeUTF(this.listOnlineUsers());
                            line = dis.readUTF();
                        }
                        //第一次进入，保存名字

                        if(flag++ ==0){
                            name = line;
                            user_list.add(name);
                            thread_list.add(this);
                            System.out.println("this="+this);
                            dos.writeUTF(name +"你好,可以开始聊天了...");
                            //
                            this.pushMessage("Client<" + name +">进入聊天室...+\n");
                            //现在在服务器的大厅中
                            ta.append("Client<" + name +">进入聊天室...\n");
                        }else{
                                 //现在在服务器的大厅中
                                ta.append("Client<" + name +"> say : " + line+"\n");
                                this.pushMessage("Client<" + name +"> say : " + line+"\n");
                        }
                        line = dis.readUTF();
                                }
                        }catch(IOException e)
                        {
                                
                        }
                }
                 //放入消息队列末尾，准备发送给客户端
            public void pushMessage(String msg){
                message_list.addLast(msg);
                isPrint =true;
            }
              
            //向客户端发送一条消息(在PrintOutThread 有使用 于是变成了public修饰)
            public void sendMessage(String msg) throws Exception{
                dos.writeUTF(msg);
                dos.flush();
            }
              
            //统计在线用户列表  需要在showuser中使用  改为public
            private String listOnlineUsers() {
                String s ="--- 在线用户列表 ---";
                for (int i =0; i < user_list.size(); i++) {
                    s +="[" + user_list.get(i) +"]";
                }
                s +="--------------------";
                return s;
            }

        }
              
        /**
         * 监听是否有输出消息请求线程类,向客户端发送消息
         */
        class PrintOutThread extends Thread
        {
            public PrintOutThread()
            {
                start();
            }
              
            @Override
            public void run() 
            {
                while(true){
                    if(isPrint){//将缓存在队列中的消息按顺序发送到各客户端，并从队列中清除。
                        String message = message_list.getFirst();
                        for (ServerThread thread : thread_list) 
                        {
                            try
                                                        {
                                    //this.
                                    thread.sendMessage(message);
                                                        } catch (Exception e)
                                                        {
                                                                // TODO Auto-generated catch block
                                                                e.printStackTrace();
                                                        }
                        }
                        message_list.removeFirst();
                        isPrint = message_list.size() >0 ?true :false;
                    }
                }
            }
        }
}

```
Client端如下：

```java
/**
* 解释：
*/
package TestNetwork;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Frame;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.BufferedReader;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.Socket;

import javax.swing.JOptionPane;

import TestNetwork.TCPClient1.ClientRead;
import TestNetwork.TCPClient1.ClientWrite;
import TestNetwork.TCPClient1.TCPClientListener;

/**
* @author    叶昭良
* @time      2015年3月23日上午1:57:54
* @version   TestNetworkRewriteTestClient5 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestClient5
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args) throws Exception
        {
                // TODO Auto-generated method stub
                Socket s  = null;
                s = new Socket("127.0.0.1",5599);

                 new TCPClient5(s);
        }

}
class TCPClient5
{
        // class variables
        // class variables
        // connect
         private Socket s = null;
         // data flow
         private DataOutputStream dos = null;
         private DataInputStream dis = null;
         // UI
         private Frame f = null;
         private TextArea ta = null;
         private TextField tf = null;
         private Button  bn = null;
         
         /**
          * 与服务器连接，并输入发送消息
          */
         public TCPClient5(Socket s) throws Exception
         {
                 this.s = s;
                 dis = new DataInputStream(s.getInputStream());
             dos = new DataOutputStream(s.getOutputStream());
             createUI();
             //new ClientRead(dis);
             //new ClientWrite(dos);
             new readLineThread();

         }
         /**
          * 用于监听服务器端向客户端发送消息线程类
          */
  
        public void createUI()
        {
            Frame f = new Frame("客户端");
            ta = new TextArea();
            tf = new TextField();
            Panel p = new Panel(new BorderLayout());
            bn = new Button("发送");
            p.add(tf,BorderLayout.CENTER);
            p.add(bn,BorderLayout.EAST);

            f.add(ta,BorderLayout.CENTER);
            f.add(p,BorderLayout.SOUTH);

            f.setSize(400,200);
            f.setVisible(true);
            //f.setVisable(true);
                tf.addActionListener(new TCPClientListener());
                bn.addActionListener(new TCPClientListener());
            f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
        }

        public void close()
        {
            try{
                dis.close();
                dos.close();
                s.close();
            }catch(Exception e)
            {
                System.exit(0);
            }
        }
        /**
         * 用于监听服务器端向客户端发送消息线程类
         */
        class readLineThread extends Thread
        {
              
            private BufferedReader buff = null;;
            public readLineThread(){
                    start();
                
            }
              
            @Override
            public void run() {
                try {
                    while(true){
                        String result = dis.readUTF();
                        if("byeClient".equals(result)){//客户端申请退出，服务端返回确认退出
                            break;
                        }else{//输出服务端发送消息
                                //ta.append(result);
                            System.out.println(result);
                           // dos.writeUTF(result);
                            ta.append("服务器传递说:"+result+"\n");
                            if(result.equalsIgnoreCase("再见"))
                            {
                                close();
                                System.exit(0);
                            }
                        }
                    }
                }catch (Exception e) {
                }
            }
        }
        
        class TCPClientListener implements ActionListener
        {
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                        String str = tf.getText();
                        tf.setText("");
                        ta.append("客户端说:"+str+"\n");
                        dos.writeUTF(str);
                        if(str.equalsIgnoreCase("再见"))
                        {
                            close();
                            System.exit(0);
                        }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
}
```
结果：

1：之前的线程卡的问题，还没有解决，运行起来仍然感觉占用大量内存！！
2：为什么 第一个进入聊天系统的杨中科，会打印出三次呢？ 当然后面的消息正常，当然三次的意思是聊天系统进入了三个人，都教授和Rocket. 所以肯定是那部分有问题待解决  


![在这边有一个小问题，出现了三行打印](/images/java/服务器2.png)

![群发效果OK,服务器也没问题了](/images/java/基本满足题意.png)



http://www.cnblogs.com/linjiqin/archive/2013/05/30/3108188.html  欣赏了这篇佳作

基于上次的问题，打开的时候经常会出现多个打印的情况，
尝试如下修改：
“登录用户集合”和“服务器已启用线程集合”的list，如果变化不频繁，读多写少，使用CopyOnWriteArrayList。
“存放消息队列”用Queue接口比List接口更方便（每次直接POP，没消息时候还可以让广播线程自己阻塞在上面），具体的用ArrayBlockingQueue或者LinkedBlockingQueue都可以。http://my.oschina.net/leejun2005/blog/104955
   对应的message_list.addLast(msg)  变成了message_list.put(msg)              message_list.take()来取代 message_list.getFirst() & message_list.removeFirst()
效果就是居然没问题。居然也不卡了

Server端的修正版本：（Client端不需要修正）

```java

/**
* 解释：
*/
package TestNetwork;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Frame;
import java.awt.Panel;
import java.awt.TextArea;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.*;
import java.util.concurrent.CopyOnWriteArrayList;
import java.util.concurrent.LinkedBlockingQueue;





/**
* @author    叶昭良
* @time      2015年3月23日上午12:41:10
* @version   TestNetworkRewriteTestServer5 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class RewriteTestServer5
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        public static void main(String[] args) throws Exception
        {
                // TODO Auto-generated method stub
            ServerSocket ss = new ServerSocket(5599);
        while(true)
        {
                new TCPServer2(ss.accept());
        }
            
        }

}
class TCPServer2
{
        // class variables
        // class variables
        // connect
         
         private Socket s = null;
         // data flow
         private DataOutputStream dos = null;
         private DataInputStream dis = null;
         // UI
         private static Frame f = null;
         private static TextArea ta = null;
         private static TextField tf = null;
         private static Button  bn = null;
         
         private static boolean isPrint = false;//是否输出消息标志
         private static List user_list = new CopyOnWriteArrayList();//登陆用户集合
         private static List<ServerThread> thread_list = new CopyOnWriteArrayList<ServerThread>();//服务器已启用的线程集合
         private static LinkedBlockingQueue<String> message_list = new LinkedBlockingQueue<String>();//存放消息队列
        // private static List<String> message_list = new LinkedList<String>();//存放消息队列
      
         static
         {
                 createUI();
         }
         public  TCPServer2(Socket s) throws Exception
         {
                 this.s = s;
                 new PrintOutThread();//创建向客户端发送消息线程
             new ServerThread(this.s);  
             //本以为可以让其线程安全的，没想到没什么效果 反而更卡
/*             Collections.synchronizedList(user_list);
             Collections.synchronizedList(thread_list);
             Collections.synchronizedList(message_list);*/
            // launch();
                 
         }
       
        // construct member
        /*
        public TCPServer()
        {

        }
        */
        public static void createUI()
        {
            Frame f = new Frame("服务器");
            ta = new TextArea();
            tf = new TextField();
            Panel p = new Panel(new BorderLayout());
            bn = new Button("发送");
            p.add(tf,BorderLayout.CENTER);
            p.add(bn,BorderLayout.EAST);

            f.add(ta,BorderLayout.CENTER);
            f.add(p,BorderLayout.SOUTH);

            f.setSize(400,200);
            f.setVisible(true);
            //f.setVisable(true);
/*            tf.addActionListener(new TCPServerListener());
            bn.addActionListener(new TCPServerListener());*/
            f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
        }
        

        public void close()
        {
            try{
                s.close();
            }catch(Exception e)
            {
                System.exit(0);
            }
        }
     
        class TCPServerListener implements ActionListener
        {
                private ServerThread st = null;
                public TCPServerListener(ServerThread s)
                {
                        st =s ;
                }
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                           String str = tf.getText();
                       if(!str.isEmpty())
                       {
                                       tf.setText("");
                                ta.append("服务器说:"+str+"\n");
                                //dos.writeUTF(str);
                                if(str.equalsIgnoreCase("再见"))
                                {
                                    close();
                                    System.exit(0);
                                }
                               
                                st.pushMessage("服务器说:"+str+"\n");
                       }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
        
        /**
         * 服务器线程类
         */
        class ServerThread extends Thread{
                private Socket client;
                private String name = null;
                private ServerThread st = null; // 受到启发的作用！！ Client=s
                public ServerThread(Socket s) throws Exception
                {
                        this.client = s;
                        this.st = this;
                        dis = new DataInputStream(client.getInputStream());
                dos = new DataOutputStream(client.getOutputStream());
                dos.writeUTF("成功连上服务器"+client.getInetAddress().getHostAddress());
                dos.writeUTF(Thread.currentThread().getName()+"已开启"+"请输入你的姓名");
                this.start();
                //tf的监听器
                tf.addActionListener(new TCPServerListener(this.st));
                bn.addActionListener(new TCPServerListener(this.st));
                }
                @Override
                public void run()
                {
                        try
                        {
                                int flag = 0 ;
                                String line = dis.readUTF();
                                while(!"bye".equals(line))
                                {
                                        //查看在线用户列表
                        if ("showuser".equals(line)) {
                            dos.writeUTF(this.listOnlineUsers());
                            line = dis.readUTF();
                        }
                        //第一次进入，保存名字

                        if(flag++ ==0){
                            name = line;
                            user_list.add(name);
                            thread_list.add(this);
                            System.out.println("this="+this);
                            this.pushMessage("Client<" + name +">进入聊天室...+\n");
                            dos.writeUTF(name +"你好,可以开始聊天了...");
                            //
                            
                            //现在在服务器的大厅中
                            ta.append("Client<" + name +">进入聊天室...\n");
                        }else{
                                 //现在在服务器的大厅中
                                ta.append("Client<" + name +"> say : " + line+"\n");
                                this.pushMessage("Client<" + name +"> say : " + line+"\n");
                        }
                        line = dis.readUTF();
                                }
                        }catch(IOException e)
                        {
                                
                        }
                }
                 //放入消息队列末尾，准备发送给客户端
            public void pushMessage(String msg){
                    //msssage_list.
                //message_list.addLast(msg); //针对于LinkedList
                    try
                                {
                                        message_list.put(msg);
                                } catch (InterruptedException e)
                                {
                                        // TODO Auto-generated catch block
                                        e.printStackTrace();
                                }
                isPrint =true;
            }
              
            //向客户端发送一条消息(在PrintOutThread 有使用 于是变成了public修饰)
            public void sendMessage(String msg) throws Exception{
                dos.writeUTF(msg);
                dos.flush();
            }
              
            //统计在线用户列表  需要在showuser中使用  改为public
            private String listOnlineUsers() {
                String s ="--- 在线用户列表 ---";
                for (int i =0; i < user_list.size(); i++) {
                    s +="[" + user_list.get(i) +"]";
                }
                s +="--------------------";
                return s;
            }

        }
              
        /**
         * 监听是否有输出消息请求线程类,向客户端发送消息
         */
        class PrintOutThread extends Thread
        {
            public PrintOutThread()
            {
                start();
            }
              
            @Override
            public void run() 
            {
                while(true){
                    if(isPrint){//将缓存在队列中的消息按顺序发送到各客户端，并从队列中清除。
                        String message = null;
                                                try
                                                {
                                                        message = message_list.take();
                                                } catch (InterruptedException e1)
                                                {
                                                        // TODO Auto-generated catch block
                                                        e1.printStackTrace();
                                                }
                        //message = message_list.getFirst();
                        for (ServerThread thread : thread_list) 
                        {
                            try
                                                        {
                                    //this.
                                    thread.sendMessage(message);
                                                        } catch (Exception e)
                                                        {
                                                                // TODO Auto-generated catch block
                                                                e.printStackTrace();
                                                        }
                        }
                       // message_list.removeFirst();
                        //isPrint = false;
                        //这句话很重要！！！不能随便修改
                        isPrint = message_list.size()>0 ?true :false;
                    }
                }
            }
        }
}
```

![有问题的版本（但是基本满足题意）](/images/java/服务器3.png)

![不卡版本，且解决了登陆时候的多条打印问题](/images/java/居然不卡了.png)


1 动机是为了熟悉网络编程，于是重写了之前写过的代码。从TCPServer 1 2 3三个进行重新的改写
2 发现TCPServer3有问题？  
  在客户端连接上后，客户端发送数据可以到服务器端，接着切换到服务器端，但是服务器的输入出现了问题，
  无论你输入一个字还是n个字都无法切换回客户端了 

3 于是基于这个问题开始进行修改
   主要包含一个主要
                    版本1     Server有一对读写线程类   Client有一对读写线程类
                   版本2     Server有一对读写线程类   Client没有一对读写线程类
                   版本3    Server有一个ServerThread线程类    Client有一个readLineThread线程类 

真的是从原先的四个类，到最后抛弃了原先的四个类，重写了另外的几个类


RewriteServer1.java
RewriteServer2.java
RewriteServer3.java
RewriteServer4.java
RewriteServer5.java
最后的一个版本是RewriteServer5

4 版本3  引入了新的数据结构 CopyWriteArrayList和LinkedBlockingQueue 线程阻塞类

5: 注意了
   5.1 线程的使用(从thread到Runnable)，线程开启放置的位置，最后发现使用了多线程服务器客户端
   5.2 静态代码段static的使用
   5.3 ServerThread的静态变量的使用，配合上构造函数
   5.4 数据结构的使用




猛然发现一个特点：

```java
  GTK.g_timeout_add(100, new IGSourceFunc()
                {
                        
                        @Override
                        public boolean execute(Object userdata)
                        {
                                // TODO Auto-generated method stub
                                int len = otv.getText().length();
                                char ch = love.charAt(len);
                                otv.insertTextAtEnd(Character.toString(ch));
                                if(otv.getText().length() == love.length()-1)
                                {
                                        //om.close();
                                        try
                                        (
                                                        OOMusic om = new OOMusic("我如此爱你.mp3",true);
                                        )
                                        {
                                                        om.playRepeat();
                                        }
                                
                                        return true;
                                }else if(otv.getText().length() == love.length())
                                {
                                        return false;
                                }
                                else
                                { 
                                        //System.out.println("2");
                                        return true;
                                }

                        }
                }, null);
```
2：


```java
vgOpen.addActivateListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                youOpenfile(ootv);
                                
                        }
                });
```
3：


```java
//添加按钮事件
                GTK.g_signal_connect(btnShow, "clicked", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                String text = GTK.gtk_text_buffer_get_text(textbuffer);
                                GTK.gtk_label_set_text(label, text);
                        }
                }, null);
```
4：
前三个都是针对gtk的
awt的事件关闭监听，
```java
f = new JFrame("客户端");
f.addWindowListener(new WindowAdapter()
                    {
                        public void windowClosing(WindowEvent e)
                        {
                            System.exit(0);
                        }
                    });
```
当然在JFrame可以采用：
f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
复制代码
他们共同的特点是都采用匿名类，当做时间的响应函数或者回调函数，于是当前的网络编程的button和  textfield也是采用匿名类来实现：
```java
class TCPClientListener implements ActionListener
        {
            @Override
            public void actionPerformed(ActionEvent e)
            {
                   try{
                        String str = tf.getText();
                        tf.setText("");
                        ta.append("客户端说:"+str+"\n");
                        dos.writeUTF(str);
                        if(str.equalsIgnoreCase("再见"))
                        {
                            close();
                            System.exit(0);
                        }
                   }catch(Exception e1)
                   {
                      // e.printStackTrace();
                      System.exit(0);
                   } 
            }
        }
```

添加监听：

```java

         tf.addActionListener(new TCPClientListener());
                bn.addActionListener(new TCPClientListener());
```

注意匿名类的监听函数的匿名类对象实现回调

学习了匿名类的事件回调函数。

