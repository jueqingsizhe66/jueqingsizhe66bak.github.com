---
layout: post
title: "3月21日Java周末班试题收集"
date: 2015-05-11 14:58:41 +0800
comments: true
categories: JavaBasic
---
<!--more-->

阶段性考试（考试6个小时，讲评+重写6个小时）
题目1：简述LinkedList和ArrayList的区别是什么？
题目2：简述Java 中Set有哪几种？区别是什么？
题目3：有如下一个类：
```java
class Person
{
private int age;
private String name;
public void setAge(int age)
{
    this.age = age;
}
public void setName(String name)
{
    this.name = name;
}
public void sayHello()
{
    System.out.println("你好，我是"+name+"，我"+age+"岁了");
}
}
```
采用反射的方法创建Person类的一个对象，并且通过反射的方法调用setAge、setName进行赋值，并且用反射的方法调用sayHello方法。
记录完成所需要的时长。
题目4：要求用户输入一个email地址，使用正则表达式检查用户输入的是否是合法的email地址，如果是合法的email地址，则把用户名和域名分别输出，比如用户输入yzk@rupeng.com，则输出“用户名为yzk，域名为rupeng.com”；
记录完成所需要的时长。
题目5：创建一个数据库表T_Students，包含Id（主键）、Name（姓名）、Num（学号）三个列。编写一个Socket服务器端，服务器接受如下的Socket指令：
客户端发送“1|学号”（比如"1|a001"），代表客户端要查询指定学号的学生的姓名，服务器端通过JDBC连接数据库进行查询，如果找到了则返回"ok|姓名"（比如"ok|张三"），如果没找到则返回"error|notfound"，如果服务器端查询过程中出现异常等则返回"error|servererror"。
客户端发送"2"，则代表客户端要查询学生总人数，服务器端通过JDBC连接数据库进行查询学生总人数，并且进行返回，比如"ok|30"，如果服务器端查询过程中出现异常等则返回"error|servererror"。
编写Socket客户端对于服务的两个指令进行测试。客户端和服务器端都使用控制台窗口就可以，不用图形界面。
记录完成所需要的时长。

第一题：
ArrayList 本质数组，优点： 易于获取缺点：不方便插入和删除。
LinkedList本质链表，优点：易于添加和删除。缺点：不方便获取。

第二题：
Set:HashSet  TreeSet     LinkedHashSet
HashSet的本质是链表数组，实现基于HashMap,速度快
       优点：添加和删除效率高
       缺点：获取慢（遍历都一样）,只能遍历一遍，Set集合无法直接获取某个元素
TreeSet：二叉树，注意查看root节点（这是二叉树结构的标志）  TreeSet关键是排序！（不是插入 和获取元素）
优点：增加和删除 
缺点：获取 
但是必须加入计数器也就是实现comparable接口，或者new Comparator
具体参看：http://www.rupeng.com/forum/thread-44838-1-1.html

LinkedHashSet:是基于HashSet只不过是使得插入获得存在顺序
未做深入研究
第三题：
Person类的设计：


```java
/**
* 解释：
*/
package com.reflect.test;

/**
* @author    叶昭良
* @time      2015年3月21日下午1:42:37
* @version   com.reflect.testPerson1 V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class Person1
{

        /**
         * @param args 
         * 原因：
         * 解决：
         * 功能：
         *       思考：        
         *       步骤：
         */
        private int age;

        private String name;

        public void setAge(int age)

        {

            this.age = age;

        }

        public void setName(String name)

        {

            this.name = name;

        }

        public void sayHello()

        {

            System.out.println("你好，我是"+name+"，我"+age+"岁了");

        }

}
```
测试Person类：



```java
/**
* 解释：
* 采用反射的方法创建Person类的一个对象，并且通过反射的方法调用setAge、setName进行赋值，并且用反射的方法调用sayHello方法。

记录完成所需要的时长

*/
package com.reflect.test;
import java.lang.reflect.*;
/**
* @author    叶昭良
* @time      2015年3月21日下午1:43:08
* @version   com.reflect.testTestPerson1Reflect V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class TestPerson1Reflect
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
                Person1 p1 = new Person1();
                Class testClass = p1.getClass();
                try
                {
                        Field nameField  = testClass.getDeclaredField("name");
                        Field ageField = testClass.getDeclaredField("age");
                        nameField.setAccessible(true);
                        ageField.setAccessible(true);
                        nameField.set(p1, "张三");
                        ageField.set(p1, 32);
                        System.out.println("通过对象调用sayHello方法");
                        p1.sayHello();
                        
                        Method helloSay = testClass.getDeclaredMethod("sayHello");
                        System.out.println("第二种方法打印sayHello");
                        helloSay.invoke(p1);
                        
                } catch (NoSuchFieldException | SecurityException | IllegalArgumentException | IllegalAccessException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                } catch (NoSuchMethodException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                } catch (InvocationTargetException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
        }

}
```
第四题：


```java
/**
* 解释：
* 题目4：要求用户输入一个email地址，使用正则表达式检查用户输入的
* 是否是合法的email地址，如果是合法的email地址，则把用户名和域名
* 分别输出，比如用户输入yzk@rupeng.com，则输出“用户名为yzk，
* 域名为rupeng.com”；
*/
package TestRegex;
import java.util.*;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
/**
* @author    叶昭良
* @time      2015年3月21日下午1:54:30
* @version   TestRegexTestRegexExam V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class TestRegexExam
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
                Scanner sc = new Scanner(System.in);
                String temp = null;
                while(true)
                {
                        System.out.println("请输入一个邮箱地址,比如zhaoturkkey@163.com,退出敲exit|quit");
                        temp= sc.nextLine();
                        if(temp.equalsIgnoreCase("exit")||temp.equalsIgnoreCase("quit"))
                        {
                                break;
                        }
                        
                        if(temp.matches("@"))
                        {
                                System.out.println("你输入的不是正确的邮箱地址！");
                                continue;
                        }
                        
                        int countAddr = 0;
                        Pattern p1 = Pattern.compile("@");
                        Matcher mp = p1.matcher(temp);
                        while(mp.find())
                        {
                                countAddr++;
                        }
                        if(countAddr == 1)
                        {
                                String[] piles = temp.split("@");
                                System.out.println("用户名为"+piles[0]+"  域名为"+piles[1]);
                        }else
                        {
                                System.out.println("你输入的邮箱有多个@");
                        }
                        //matches要求整体匹配！
                        System.out.println("sdfas|fsdfs".matches("fsdfs"));
                        String likeType = "23";
                          String pattern = "[a-zA-Z0-9]*[" + likeType + "]{1}[a-zA-Z0-9]*";
                          String sourceStr = "adfjaslfj23ldfalsf";
                             System.out.println(sourceStr.matches(".*"+likeType+".*"));
                }
        }

}
```
结果：
```
请输入一个邮箱地址,比如zhaoturkkey@163.com,退出敲exit|quit
zhaoturkkey@163.com
用户名为zhaoturkkey  域名为163.com
false
true
请输入一个邮箱地址,比如zhaoturkkey@163.com,退出敲exit|quit
```

第五题：


采用ORM的实现
具体查看http://www.rupeng.com/forum/thread-44526-1-1.html
满足ORM的设计条件：
*    约束条件：主键必须为Id，且是int类型，自动递增
*             字段名必须和属性名一样   也就是类的属性和表的字段保持一致
*             表名字和类的名字一样
Student表的设计：

```
create table student(
id int not null auto_increment primary key
name varchar(20) not null default '',
num varchar(20) not null default '');


```
Student类的设计：

```java
/**
* 
*/
package com.introspect.test;

/**
* @author    叶昭良
* @time      2015年3月21日下午2:10:39
* @version   com.introspect.testStudent V1.0
*/
//据说下面是符合javaBeans设计思想的普通类
public class Student
{
        //字段私有化  ，使用BeansInfo必须是用id  name  num而不是id name num
        /*private String id;
        private String name;
        private int num;*/
        
        private int id;
        private String name;
        private String num;
        
        //提供一个无参的构造函数
        public Student()
        {
                
        }
        @Override
        public String toString()
        {
                StringBuilder studentInfo = new StringBuilder();
                studentInfo.append("学生的id：").append(id).append(",姓名:").append(name).append(
                                ",学号是:").append(num);
                return studentInfo.toString();
        }        
        public int getid()
        {
                return id;
        }
        public void setid(int id)
        {
                this.id = id;
        }
        public String getname()
        {
                return name;
        }
        public void setname(String name)
        {
                this.name = name;
        }
        public String getnum()
        {
                return num;
        }
        public void setnum(String num)
        {
                
                this.num = num;
        }
}
	```
MyORM的修改
1： 修改select的id变为num即可  修改对应的代码段 
2： 重载select方法   变成只接受一个class的函数，具体如下所示

```java
public static Object select(Class clazz) throws SQLException
        {
                int count = 0;
                //用于数据库表
                String className = clazz.getSimpleName(); 
                StringBuilder sbSQL = new StringBuilder();
                
                //第一步 拼接  insert
                sbSQL.append("select count(*) tb from ").append(className);
                //获取到 ResultSet
                ResultSet rs = JDBCUtils.executeQuery(sbSQL.toString());
                while(rs.next())
                {
                        count = rs.getInt("tb");
                        System.out.println("总共有"+count+"个数据");
                }
                
                System.out.println("成功");
                return count;
                
                //先非泛型  再泛型的selectById
        }
```
最终的MyORM代码如下：

```java
/**
*  对象 Student
*  mysql表   Student
*  
*  类 表对应！  通过对象传递进数组，并利用内省机制，获得类中的所有属性！
*   并获得其中的值，然后组合成为一个sql语句，并执行JDBCUtils 来进行
*   增、删、修改、查等操作
*   
*   目的 ：对象 --mysql表 进行一一对应并且可以进行传递
*   简单的MyOrm.insert(
*              delete
*              select
*              update 等
*         
*    约束条件：主键必须为Id，且是int类型，自动递增
*             字段名必须和属性名一样   也就是类的属性和表的字段保持一致
*             表名字和类的名字一样
*/

package com.introspect.test;
import com.jdbc.test.*;

import java.beans.BeanInfo;
import java.beans.IntrospectionException;
import java.beans.Introspector;
import java.beans.PropertyDescriptor;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.*;  //需要存储字段的 list  同时需要存储值的list 
                     // 最后组装成一条mysql语句
/**
* @author    叶昭良
* @time      2015年3月4日下午8:57:46
* @version   com.introspect.testMyORM V1.0
*/
public class MyORM
{

        /**
         * sql = insert into Person(fieldName) values(?,?..)
         *         1                 2             3   4
         *  JDBCUTils.executeUpdate(sql,propValue)
         * @param args
         */
        public static void insert(Object obj) throws SQLException
        {
                Class clazz = obj.getClass();
                BeanInfo beanInfo = null;
                beanInfo = getBeanInfo(clazz);
                
                //用于数据库表
                String className = clazz.getSimpleName(); 
                
                PropertyDescriptor[] pro = beanInfo.getPropertyDescriptors();
                List<String> listFieldName = new ArrayList<String>();
                for(PropertyDescriptor temp:pro)
                {
                        String propName = temp.getName();
                        if(!propName.equals("id") && !propName.equals("class")) 
                        {
                                //扣除 id和class这两个内部字段
                                listFieldName.add(propName);
                        }
                }
                
                StringBuilder sbSQL = new StringBuilder();
                
                //第一步 拼接  insert
                sbSQL.append("insert into ").append(className);

                //第二步 拼接  字段
                String fieldNames = listFieldName.toString();
                sbSQL.append(fieldNames.replace('[', '(').replace(']', ')'));
        
                //第三步  拼接  值
                sbSQL.append(" values");
                
                //第四步  拼接  添加问号
                char[] paramMarkArray  = new char[listFieldName.size()];
                for(int i = 0 ; i < listFieldName.size() ; i++)
                {
                        paramMarkArray[i] = '?';
                }
                sbSQL.append(Arrays.toString(paramMarkArray).replace('[', '(').replace
                                (']', ')'));
                
                //最后一步    调用JDBCUtils的 executeUpdate
                List<Object> paramValues = new ArrayList<Object>();
                for(String propName : listFieldName)
                {
                        PropertyDescriptor propDesc = findPropertyDescriptor(propName, pro);
                        Object propValue =null;
                        //obj传入的类或者表名的对象
                        propValue = invoke(propDesc, obj);
                        paramValues.add(propValue);
                }
                //最终过程  toString  toArray的转换
                JDBCUtils.executeUpdate(sbSQL.toString(), paramValues.toArray());
                
        }
        /**
         * 
         * @param clazz
         * @return  返回一个beanInfo对象
         */
        private static BeanInfo getBeanInfo(Class clazz)
        {
                BeanInfo beanInfo = null;
                try
                {
                        beanInfo = Introspector.getBeanInfo(clazz);
                }catch(IntrospectionException e)
                {
                        throw new RuntimeException("内省出错！"+e.getMessage());
                }
                return beanInfo;
        }
        /**
         *  从propDesc中找名字为propName的PropertyDescriptor
         * @param propName
         * @param prop
         * @return
         */
        private static PropertyDescriptor findPropertyDescriptor(String propName,
                        PropertyDescriptor[] prop)
        {
                for(PropertyDescriptor temp : prop)
                {
                        if(temp.getName().equals(propName))
                        {
                                return temp;
                        }
                }
                return null;
        }
        /**
         *  执行某个字段的读方法  并返回 propValue
         * @param propDesc
         * @param obj
         * @return
         */
        private static Object invoke(PropertyDescriptor propDesc,Object obj)
        {
                Object propValue = null;

                try
                {
                        propValue = propDesc.getReadMethod().invoke(obj);
                } catch (IllegalAccessException | IllegalArgumentException
                                | InvocationTargetException e)
                {
                        // TODO Auto-generated catch block
                        throw new RuntimeException("获取"+propDesc.getName()+"错误");
                }
                return propValue; 
        }
        //------------------------insert 程序 结束----------------
        /**
         * delete from className where id = ?
         * 
         * 
         *   删除clazz对应表中的字段id为id的值   delete(Person.class,5);
         * @param clazz
         * @param id
         */
        public static void delete(Class clazz, int id) throws SQLException
        {
                BeanInfo beanInfo = null;
                beanInfo = getBeanInfo(clazz);
                
                //用于数据库表
                String className = clazz.getSimpleName(); 
                
                PropertyDescriptor[] pro = beanInfo.getPropertyDescriptors();
                List<String> listFieldName = new ArrayList<String>();
                
                
                StringBuilder sbSQL = new StringBuilder();
                
                //第一步 拼接  insert
                sbSQL.append("delete from ").append(className).append(" where id = ?");
                
                JDBCUtils.executeUpdate(sbSQL.toString(), id);
                
        }
        
        
        /**
         * select * from Person where id = 2;
         * 
         *   获取clazz对应表中id为字段为id的对应航的   并且填充到对象中
         *   Person p1 = (Person)select(Person.class,2)
         *   p1.getName(), p1.getAge();
         * @param clazz
         * @param id
         * @return
         */
        //----------------------delete 程序结束--------------------
        public static Object select(Class clazz,String num) throws SQLException
        {
                Object b1 = null;
                b1 = getInstance(clazz);
                
                BeanInfo beanInfo = null;
                beanInfo = getBeanInfo(clazz);
                
                //用于数据库表
                String className = clazz.getSimpleName(); 
                
                PropertyDescriptor[] pro = beanInfo.getPropertyDescriptors();
                List<String> listFieldName = new ArrayList<String>();
                for(PropertyDescriptor temp:pro)
                {
                        String propName = temp.getName();
                        if(!propName.equals("id") && !propName.equals("class")) 
                        {
                                //扣除 id和class这两个内部字段
                                listFieldName.add(propName);
                        }
                }
                
                StringBuilder sbSQL = new StringBuilder();
                
                //第一步 拼接  insert
                sbSQL.append("select * from ").append(className).append(" where num = ?");
                //获取到 ResultSet
                ResultSet rs = JDBCUtils.executeQuery(sbSQL.toString(), num);
                if(!rs.next())
                {
                        System.out.println("当前版本没有"+num+"的信息");
//                        return;
                }else
                {
                        System.out.println("当前版本有"+num+"的信息");
                        for(String propName : listFieldName)
                        {
                                PropertyDescriptor propDesc = findPropertyDescriptor(propName, pro);
                                //obj传入的类或者表名的对象
                                invoke(propDesc, b1,rs.getObject(propName));
                                //invoke(propDesc, b1,rs.getString(propName));
                        }
                }
                //设置num字段
                PropertyDescriptor propDesc1 = findPropertyDescriptor("num", pro);
                invoke(propDesc1, b1,num);
                System.out.println("成功");
                return b1;
                
                //先非泛型  再泛型的selectById
        }
        
        public static Object select(Class clazz) throws SQLException
        {
                int count = 0;
                //用于数据库表
                String className = clazz.getSimpleName(); 
                StringBuilder sbSQL = new StringBuilder();
                
                //第一步 拼接  insert
                sbSQL.append("select count(*) tb from ").append(className);
                //获取到 ResultSet
                ResultSet rs = JDBCUtils.executeQuery(sbSQL.toString());
                while(rs.next())
                {
                        count = rs.getInt("tb");
                        System.out.println("总共有"+count+"个数据");
                }
                
                System.out.println("成功");
                return count;
                
                //先非泛型  再泛型的selectById
        }
        private static Object getInstance(Class clazz)
        {
                Object b1 = null;
                try
                {
                        b1 = clazz.newInstance();
                } catch (InstantiationException | IllegalAccessException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                return b1;
        }
        //private static void invoke(PropertyDescriptor propDesc,Object obj,String value)
        
        private static void invoke(PropertyDescriptor propDesc,Object obj,Object value)
        {

                //有问题 因为
                /*try
                {
                        propDesc.getWriteMethod().invoke(obj,value);
                } catch (IllegalAccessException | IllegalArgumentException
                                | InvocationTargetException e)
                {
                        // TODO Auto-generated catch block
                        throw new RuntimeException("获取"+propDesc.getName()+"错误");
                } */
                try
                {
                        Method methodSet= propDesc.getWriteMethod();
                        if(methodSet !=null)
                        {
                                methodSet.invoke(obj,value);
                        }
                        
                } catch (IllegalAccessException | IllegalArgumentException
                                | InvocationTargetException e)
                {
                        // TODO Auto-generated catch block
                        throw new RuntimeException("获取"+propDesc.getName()+"错误");
                } 
        }
        
        //------------------select 结束------------------
        
        // update Person set age='' where id=4;
        
        /**
         *   设置id= id的对象 ， 他的年龄为。。。 也可以名字 ，具体修改update 语句
         * @param clazz
         * @param newValue    设置年龄的新值
         * @param id          设置某个id
         * @throws SQLException
         */
        public static void update(Class clazz,Object newValue,int id) throws SQLException
        {
                BeanInfo beanInfo = null;
                beanInfo = getBeanInfo(clazz);
                
                //用于数据库表
                String className = clazz.getSimpleName(); 
                
                PropertyDescriptor[] pro = beanInfo.getPropertyDescriptors();
                List<String> listFieldName = new ArrayList<String>();
                for(PropertyDescriptor temp:pro)
                {
                        String propName = temp.getName();
                        if(!propName.equals("id") && !propName.equals("class")) 
                        {
                                //扣除 id和class这两个内部字段
                                listFieldName.add(propName);
                        }
                }
                
                StringBuilder sbSQL = new StringBuilder();
                
                //第一步 拼接  insert
                //可以设置 update 那个字符串  比如 String temp = "age"
                sbSQL.append("update ").append(className).append(" set age = ").append(newValue).append(""
                                + " where id = ?");
                JDBCUtils.executeUpdate(sbSQL.toString(), id);
        }

}
```

关于JDBCUtils的片段：请参考http://www.rupeng.com/forum/thread-44526-1-1.html
测试新版本MyORM的有效性


```java
/**
* 解释：
*/
package com.introspect.test;

import java.sql.SQLException;

/**
* @author    叶昭良
* @time      2015年3月21日下午2:21:40
* @version   com.introspect.testFifthExam V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class FifthExam
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
                Student s1 = new Student();
                //插入数据
/*                s1.setname("zhaoliang");
                s1.setnum("a001");*/
/*                s1.setname("xinran");
                s1.setnum("a002");*/
/*                s1.setname("liming");
                s1.setnum("a003");*/
                s1.setname("wanglei");
                s1.setnum("a004");
                try
                {
                        //插入数据部分
                        //MyORM.insert(s1);
                        //测试查询第一部分
                        System.out.println(MyORM.select(Student.class));
                        
                        //测试查询第二部分
                        //s1= (Student)MyORM.select(Student.class,"a001");
                        //s1= (Student)MyORM.select(Student.class,"a002");
                        System.out.println(s1.toString());
                } catch (SQLException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
        }

}
```
结果很好，并插入数据到了表中

紧接着进行网络编程   Server端和Client段，参考了我的复习笔记的http://www.rupeng.com/forum/thread-44838-1-1.html网络编程部分 ，针对Server的主要修改部分是ServerRead, 从客户端读取的字符串，需要进行两个判断  1：  1|a001     如何是这种情况  则使用MyORM.select(Student.class,num)         2: 2  如果输入的是一个2,则使用 MyORM.select(Student.class)返回总共的学生数 ，Server端代码如下：


```java
/**
* 解释：
*/
package com.introspect.test;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Font;
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
import java.net.ServerSocket;
import java.net.Socket;

import javax.swing.JOptionPane;
import javax.swing.UIManager;


/**
* @author    叶昭良
* @time      2015年3月21日下午2:56:04
* @version   com.introspect.testTestExamServer V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class TestExamServer
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
         private Button  bn = null;

       
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
              //          System.out.println("对方说:"+str);
                        ta.append("客户端说："+str+"\n");
                        if(str.equalsIgnoreCase("再见"))
                        {
                            close();
                            System.exit(0);
                        }
                        if(str.matches(".*\\|.*"))
                        {
                                System.out.println("的确存在");
                                String[] piles = str.split("[|]");
                                Student s1 = (Student)MyORM.select(Student.class, piles[1]);
                                ta.append("str的姓名是:"+s1.getname());
                        }
                        if(str.matches("2"))
                        {
                                ta.append("服务器对客户端说：总共有"+MyORM.select(Student.class)+"个学生\n"); 
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
Client端代码如下：


```java
/**
* 解释：
*/
package com.introspect.test;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Font;
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
import javax.swing.UIManager;


/**
* @author    叶昭良
* @time      2015年3月21日下午2:56:17
* @version   com.introspect.testTestExamClient V1.0
* 功能： 
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class TestExamClient
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
         private Button  bn = null;

       
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
                   //     System.out.println("对方说:"+str);
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
                   //    e.printStackTrace();
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
![客户端读取服务器段数据库的学生信息](/images/java/kehuduan.png)


