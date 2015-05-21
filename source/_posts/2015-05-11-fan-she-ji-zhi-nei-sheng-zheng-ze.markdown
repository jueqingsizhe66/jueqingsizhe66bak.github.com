---
layout: post
title: "反射机制内省正则"
date: 2015-05-11 14:58:46 +0800
comments: true
categories: JavaBasic
---
<!--more-->
2：反射机制
不行，则逆着来.
类（Class）
三种方式

```java
//Test class
                //*************************************************************
                //第一种方式获得类
                System.out.println("//*************************************************************");
                Class clazz1 = Person.class;
                //第二种方式获得类
                System.out.println("第一种方式获得类"+clazz1.getName());
                Person personClass = new Person();
                Class clazz2 = personClass.getClass();
                System.out.println("第二种方式获得类"+clazz1.getName());
                //第三种方式获得类
                try
                {
                        Class clazz3 = Class.forName("com.reflect.test.Person");
                        System.out.println("第三种方式获得类"+clazz1.getName());
                        System.out.println("三种类方式都是同一个类！因为类只加载一次"+(clazz2==clazz1)+(clazz2==clazz3));
                        System.out.println("包的名字"+clazz3.getPackage().getName());
                        System.out.println("简单类名字（不带包）"+clazz3.getSimpleName());

                        System.out
                                        .println("------------------------------通过Class对象获得类的构造函数---------------------");
                        Constructor[] constructors = clazz3.getConstructors();
                        for (Constructor constructor : constructors) {
                                System.out.println(constructor);
                        }

                        System.out.println("------------------通过Class对象获得类的字段----------------");
                        Field[] fields = clazz3.getDeclaredFields();
                        for (Field field : fields) {
                                System.out.println(field);
                        }

                        System.out.println("--------通过Class对象获得类的方法----------");

                        Method[] methods = clazz3.getMethods();// 返回所有的public 方法,包括父类声明的public方法
                        for (Method method : methods) {
                                System.out.println(method);
                        }

                } catch (ClassNotFoundException e1)
                {
                        // TODO Auto-generated catch block
                        e1.printStackTrace();
                }
```java
最主要的是Class.forName();

字段

设置字段的属性，并使用创建的对象调用方法.
```java
//*************************************************************
                //TestField  测试字段开始
                //*************************************************************
                System.out.println("//*************************************************************");
                try
                {
                        /**
                         *  打印出所有字段
                         *  
                         *  字段类的一个特点设置
                         */
                        Field[] fields = clazz.getDeclaredFields();
                        for(Field temp : fields)
                        {
                                System.out.println(temp);
                        }
                        Person personLiMing = (Person) clazz.newInstance();
                        /**
                         * 测试添加一个名字的字段
                         */
                        //Field nameField = clazz.getField("Name"); //会报异常
                        //Field nameField = clazz.getDeclaredField("name"); //注意区分大小写
                        Field nameField = clazz.getDeclaredField("Name");
                        //一定得设置？
                        nameField.setAccessible(true);
                        nameField.set(personLiMing,"李明");
                        
                        Object o = nameField.get(personLiMing);
                        System.out.println(o);
                        personLiMing.sayHi();
                } catch (InstantiationException | IllegalAccessException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("创建新的实例异常");
                }catch( NoSuchFieldException | SecurityException e)
                {
                        System.out.println("没有那个字段");
                }
```java

构造函数

主要作用：产生对象

```java
/**
                 *  分为两个部分测试构造函数 
                 *  1：  有参的 ，注意有参的时候String.class int.class
                 *  2：  无参的
                 */
                //TestConstructor
                /**
                 * 打印所有的类的构造函数
                 */
                try
                {
                        Constructor con1 = clazzConstructor.getDeclaredConstructor();
                        
                        Object o = con1.newInstance();
                        
                        Person personWanghao = (Person)o;
                        personWanghao.sayHi();
                        
                        System.out.println("------------------------");
                        
                        Constructor<Person> con2 = clazzConstructor.getDeclaredConstructor(String.class,int.class);
                        Object o2 = con2.newInstance("汪峰",22);
                        //很浅显的道理  只有构造函数可以新建对象！！上面的代码正是
                        //体现如此！        
                        Person personWangfeng = (Person)o2;
                        personWangfeng.sayHi();
                        
                } catch (NoSuchMethodException | SecurityException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("获取构造函数异常");
                }catch (InstantiationException | IllegalAccessException
                                | IllegalArgumentException | InvocationTargetException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("通过构造函数创建实例异常");
                }
               
```java

方法

Invoke函数，调用对象的方法

```java
//*************************************************************
                //TestMethod
                /**
                 *  开始测试 所有Person类的普通方法
                 *  
                 *  方法类的一个特点，方法对应具体的参数！类似于构造函数
                 *  都需要考虑重载的问题
                 */
                System.out.println("//*************************************************************");
                System.out.println("开始测试 所有Person类的普通方法---------");
                Method[] methods = clazzMethod.getDeclaredMethods();
                
                for(Method temp: methods)
                {
                        System.out.println(temp);
                }
                
                //测试Person的sayHi方法
                try
                {
                        Method sayHi = clazzMethod.getDeclaredMethod("sayHi");
                        //Method singSong = clazz.getDeclaredMethod("singSong");
                        
                        Object o = clazzMethod.newInstance();
                        
                        Person personPig = (Person)o;
                        personPig.sayHi();
                        System.out.println("通过普通方法1 进行Invoke1！！！");

                        sayHi.invoke(o);
                        System.out.println("通过普通方法1 进行Invoke2！！！");
                        //反射的感觉有点像，逆着来的感觉
                        //通过方法调用对象！！并执行某个动作！
                        //而不是通过对象调用方法执行某个动作
                        sayHi.invoke(personPig);

                        Class returnType = sayHi.getReturnType();
                        System.out.println("Person类 sayHi的返回值类型"+returnType);
                        
                        int modifies = sayHi.getModifiers();
                        //private 的值为0   public的值为1
                        System.out.println("是private?:"+Modifier.isPrivate(modifies));
                        System.out.println(modifies);
                        
                        /**
                         *  必须隐藏 ！否则下面的无法执行！ 有对应的参数  需要设置上 参数才可以
                         *  否则找不到 singSong() 无参的函数
                         */
                /*        Method singSong = clazzMethod.getDeclaredMethod("singSong");
                        System.out.println(singSong);*/
                        
                        Method singSong2 = clazzMethod.getDeclaredMethod("singSong", String.class);
                        Person personDandan = new Person("淡淡",22);
                        singSong2.invoke(personDandan, "好汉歌");
                        System.out.println(singSong2.getReturnType());
                        
                        //System.out.println("Person类singSong的返回值类型"+clazz.getDeclaredMethod("singSong").getReturnType());
                } catch (NoSuchMethodException | SecurityException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("没有对应的方法"+e.getMessage());
                }catch (InstantiationException | IllegalAccessException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("获取实例异常"+e.getMessage());
                }catch (IllegalArgumentException | InvocationTargetException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("Invoke异常"+e.getMessage());
                }
```

一个餐厅的菜单实例：

   通过一个food接口，创建多样菜单，并且易于拓展性。


```java
Food.properties:
Apple=com.reflect.test.food.Apple
Banana=com.reflect.test.food.Banana
HongShaoRou=com.reflect.test.food.HongShaoRou
QingJiaoRouSi=com.reflect.test.food.QingJiaoRouSi
SuanTaiChaoRou=com.reflect.test.food.SuanTaiChaoRou
TangCuLiYu=com.reflect.test.food.TangCuLiYu
XiaoChaoRou=com.reflect.test.food.XiaoChaoRou
```
Food接口：

```java
package com.reflect.test.food;

public interface Food {

        String getFoodName();

}
```
依次创建各个食物：（类似方法）
红烧肉：



```java
package com.reflect.test.food;

public class HongShaoRou implements Food {

        public static final String foodName = "红烧肉";

        @Override
        public String getFoodName() {
        return foodName;
        }

}
```


青椒肉丝：

```java
package com.reflect.test.food;

public class QingJiaoRouSi implements Food {

        public static final String foodName = "青椒肉丝";

        @Override
        public String getFoodName() {
                return foodName;
        }

}
```

。。。。
餐厅类的实现：

```java
/**
* 情景 : 有一家餐厅,时常更换菜品 , 客人进店时向客人展示所有的菜品,
* 要求开发一个扩充性好的程序,就是更换菜品时只需要做一些简单的配置就可以
* 
* 反射基本上 自己自学 看的    视频没怎么听

*/
package com.reflect.test;

import java.util.ArrayList;


/**
*  通过下面的餐厅类  很方便的通过Food接口 
*  添加更多的新菜！ 只要你有新定义一个  食物.java 然后在
*  food.properties添加一个键值对即可
*/



/**
* @author    叶昭良
* @time      2015年3月3日下午4:44:15
* @version   com.reflect.testRestaurant V1.0
*/
import com.reflect.test.food.*;

import java.util.*;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;


public class Restaurant 
{

        /**
         * @param args
         */
        //没有想到用list表来接数据（一定要有集合的概念）
        private List<Food>  foods = new ArrayList<Food>();
        
        //定义一个init函数 用于在构造函数内部 调用，并初始化菜单！准备开张 开店铺
        public Restaurant()
        {
        }
        public void init()
        {
                try
                {
                        InputStream is = Restaurant.class.getResourceAsStream("food.properties");
                        Properties prop = new Properties();
                        /**
                         *  要知道 food.properties 的key-value的写法
                         *  Prop对象就是一个map对象
                         *  先前只写上一个值是错误的！ 必须写上
                         *  Apple=com.reflect.test.food.Apple
                         */
                        //必须加在src底下
                        //prop.load(new FileInputStream("food.properties"));
                        try
                        {
                                prop.load(is);
                        } catch (IOException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }
                        Collection  coll1 = prop.values();
                        for(Object foodClassName:coll1)
                        {
                                Class temp;
                        
                                temp = Class.forName((String)(foodClassName));
                                
                                Food food = (Food)temp.newInstance();
                                foods.add(food);
                        }
                }catch (ClassNotFoundException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("未找到对应的类");
                }catch (InstantiationException e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("产生实例的异常");
                } catch (IllegalAccessException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                
        }
        public void showFoods()
        {
                for(Food temp:foods)
                {
                        //Food接口创建的一个唯一接口方法
                        System.out.println(temp.getFoodName());
                }
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                //开了一家店
                Restaurant res = new Restaurant();
                System.out.println("欢饮各位宾客的到来-- 鞭炮响起");
                System.out.println("******************************");
                res.showFoods();
        }

}
```

3：内省

在满足javaBean规范的类中，在不清楚字段的前提下，通过getter &setter获得所有的字段，一般和反射联合使用。


Javabean 其实就是满足规范，具体规范是有

私有变量
Getter andsetter方法
至少有一个无参构造函数


然后可以通过 类 Introspector 来获取某个对象的 BeanInfo 信息，然后通过 BeanInfo 来获取属性的描述器（ PropertyDescriptor ），通过这个属性描述器就可以获取某个属性对应的 getter/setter 方法，然后我们就可以通过反射机制来调用这些方法。javabean的实例对象称之为值对象（ValueObject）,因为这些bean中通常只有一些信息字段和存储方法，没有功能性方法。
分为两种情况，只是设置或者得到某个具体的字段

两外一种是获得所有的字段。

```java
/**
* 
*/
package com.introspect.test;

import java.beans.BeanInfo;
import java.beans.IntrospectionException;
import java.beans.Introspector;
import java.beans.PropertyDescriptor;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
* @author    叶昭良
* @time      2015年3月3日下午6:34:08
* @version   com.introspect.testTestStudentIntrospect V1.0
*/
public class TestStudentIntrospect
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                Student wangxiuyan = new Student();//new Student("001","王秀艳",30);
                String fieldName="Age";
                //通过javabeans-->具体某个属性---> 通过反射获取方法！ -->再调用方法
                getProperty(wangxiuyan, fieldName);
                setProperty(wangxiuyan, fieldName);
                getProperties(wangxiuyan, fieldName);
        
        }
        private static void getProperties(Student stu,String fieldName)
        {
                try
                {
                        BeanInfo  beanInfo = Introspector.getBeanInfo(stu.getClass());
                        PropertyDescriptor[] pro = beanInfo.getPropertyDescriptors();
                        for(PropertyDescriptor temp : pro)
                        {
                                if(temp.getName().equals(fieldName))
                                {
                                        Method methodx = temp.getReadMethod();

                                        System.out.println(methodx.invoke(stu)); 
                                        
                                }
                        }
                } catch (IntrospectionException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }catch (IllegalAccessException | IllegalArgumentException
                                | InvocationTargetException e)
                {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                
        }
        
        private static void getProperty(Student stu,String fieldName)
        {
                try
                {
                        PropertyDescriptor pro = new PropertyDescriptor(fieldName, Student.class);
                        Method methodx = pro.getReadMethod(); //
                        Object ojx = methodx.invoke(stu);
                        System.out.println(ojx);
                } catch (IntrospectionException  e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("属性描述获取失败");
                }catch(IllegalAccessException | IllegalArgumentException | InvocationTargetException e)
                {
                        System.out.println("invoke调用失败！");
                }
                
        }
        private static void setProperty(Student stu,String fieldName)
        {
                try
                {
                        PropertyDescriptor pro = new PropertyDescriptor(fieldName, Student.class);
                        Method methodx = pro.getWriteMethod(); //
                        methodx.invoke(stu,8);
                        System.out.println(stu.getAge());
                } catch (IntrospectionException  e)
                {
                        // TODO Auto-generated catch block
                        System.out.println("属性描述获取失败");
                }catch(IllegalAccessException | IllegalArgumentException | InvocationTargetException e)
                {
                        System.out.println("invoke调用失败！");
                }        
        }

}
```


ORM的实现在：http://www.rupeng.com/forum/thread-44526-1-1.html


4：正则表达式

字符串内置函数：

String的split、replace、replaceAll函数。

在正则表达式中$表示在另一个字符串中获取正则表达式字符串匹配的小组号，$1表示第一个。。

              \1 表示在同一个匹配正则表达式获得第一个匹配项(\2则表示在同一个匹配正则表达式获得第二个引用的匹配项)

```java
//字符串测试
System.out.println("fdasfsd".indexOf('d'));

        System.out.println("fdasfsd".lastIndexOf('d'));

        String[] temp ="sdfddfdsgsdfddg".split("([a-z])\\1+");
        for(String p : temp)
        {
        System.out.println(p);
        }

        System.out.println("sdfddfdsgsdfddg".replaceAll("([a-z])\\1+","*"));

        System.out.println("sdfddfdsgsdfddg".replaceAll("([a-z])\\1+","$1"));
```

       
Pattern&Matcher组合

```java
//工作中的  正则获取方式
                System.out.println("2020".matches("\\d{3,10}"));
                
                String pat = "Welcome to china to have a pinny lunch ! Start the lesson";
                Pattern p = Pattern.compile("\\b[a-z]{5}\\b");
                Matcher m = p.matcher(pat);
                while(m.find())
                {
                        System.out.println(m.group());
                }
```


常见的存储结构：


|-----顺序存储结构 特点是借助于数据元素的相对存储位置来表示数据元素之间的逻辑结构；
|-----链式存储结构 特点是借助于指示数据元素地址的指针表示数据元素之间的逻辑结构。
|-----散列存储结构 顺序+算列。
|-----索引存储结构 顺序+索引。


常见的逻辑结构：
对数据及其关系的抽象逻辑描述，对立与计算机，与机器实现无关。
根据定义的关系不同，数据的逻辑结构分为四种：
|-----集合结构。数据元素之间未定义任何关的松散集合。
|-----线性结构。数据元素之间定义了次序关系的集合（全序集合），描述的是1对1关系。
|-----树形结构。数据元素之间定义了层次关系的集合（偏序集合），描述的是1对多关系。
|-----图状结构。数据元素之间定义了网状关系的集合，描述的是多对多关系。


数据结构，就是相互之间存在一种或多种特定关系的数据元素的集合。可以简单表示为：数据结构 = 数据 + 关系。同一数据元素集合，所定一的关系不同，构成不同的数据结构。 数据结构包括逻辑结构和存储结构两个方面

