---
layout: post
title: "网络编程8-12节笔记代码March18th"
date: 2015-05-11 14:58:46 +0800
comments: true
categories: JavaBasic
---
<!--more-->

复习目录
1集合框架... 2
ArrayList: 2
栈的实现（先进后出）... 3
队列的实现（先进先出）... 5
LinkedList: 8
小结：... 10
HashSet: 10
TreeSet. 14
LinkedHashSet: 22
HashMap: 23
1.位运算符... 25
2.字节的截断工具：... 29
3.理解重新调整大小... 39
4.理解添加... 41
5.理解说明：... 44
6.HashMap和HashSet的区别... 44
7.理解Hash算法... 45
TreeMap: 45
LinkedHashMap: 45
Iterator: 45
泛型... 45
Arrays: 46
Collections: 46
多重数据结构的实现：... 48
ToString. 49
2：反射机制... 49
类（Class）... 50
字段... 51
构造函数... 52
方法... 53
3：内省... 58
4：正则表达式... 61
字符串内置函数：... 61
Pattern&Matcher组合：... 62

1集合框架
集合是存储对象的对象。
用于存储数据的数据结构，重要！进行源码分析。
要求：熟练掌握增删查改（针对每一种数据结构）！块增删查改等等。并掌握每一种数据结构的本质，优缺点

|--Collection
|---List
|-------ArrayList
|-------LinkedList
|-------HashLinkedList
|---Set
  |-------HashSet
  |-------TreeSet
|--Map
|-------HashMap
|-------TreeMap
|-------LinkedHashMap
两个工具类(默认是静态类static 方法)：
|---Arrays
|---Collections

重点：ArrayList , LinkedList, HashMap,进行HashMap的源码追踪。

ArrayList:
本质：数组，元素有序，可以重复
优点：查找(或者叫做获取get方法效率极高)和遍历
缺点：增加和删除
ArrayList和LinkedList替换掉Vector,Vector在长度不足时候是百分百增长，而前面两个是50%。至于线程的独立性可以让ArrayList和LinkedList接续Serializable

Vector的方法都是同步的(Synchronized),是线程安全的(thread-safe)，而ArrayList的方法不是，由于线程的同步必然要影响性能，因此,ArrayList的性能比Vector好。
当Vector或ArrayList中的元素超过它的初始大小时,Vector会将它的容量翻倍,而ArrayList只增加50%的大小，这样,ArrayList就有利于节约内存空间
栈的实现（先进后出）

```java
  			
/**
*  堆栈是先进后出
*/
package com.collections.test;

/**
* @author    叶昭良
* @time      2015年2月23日下午7:47:29
* @version   com.collections.testTestStack V1.0
*/
import java.util.*;
public class TestStack
{

         /**
          * @param args
          */
         private static List<String> list1 = new ArrayList<String>();
         /**
          *
          * @param apple  添加字符串到堆栈中
          */
         public void push(String apple)
         {
                   //list1.addLast(apple);
                   //list1.addOffer(apple);
                  
                   list1.add(apple);
         }
         /**
          *
          * @return  返回待删除的字符串
          */
         public String pop()
         {
                   int length = getListLength(); //笨蛋 直接用 list1.size()就可以了
                   //System.out.println("长度为："+length);
                   String apple = list1.get(length-1);
                   //String apple = list1.get(length-1);
                   //第一种方法
                   //return list1.removeLast() 更方便IE
                   //第二种方法
                   //return list1.pollLast();
                   list1.remove(length-1);
                   return apple;
         }
         /**
          *  得到堆栈的长度
          * @return
          */
         private static int getListLength()
         {
                   int sum =0;
                   Iterator<String> it = list1.iterator();
                   while(it.hasNext())
                   {
                            sum++;
                            it.next();
                   }
                   return sum;
         }
        
         public  void sayStatck()
         {
                   Iterator<String> it = list1.iterator();
                   while(it.hasNext())
                   {
                            System.out.println(it.next());
                   }
         }
         public static void main(String[] args)
         {
                   // TODO Auto-generated method stub
                   TestStack ts = new TestStack();
                   ts.push("apple");
                   ts.push("banana");
                   ts.push("orange");
                  
                  
                   ts.sayStatck();
                   String text = ts.pop();
                   ts.sayStatck();
         }

}


  
```java
队列的实现（先进先出）
  

```java
/**
*
*/
package com.collections.test;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/**
* @author    叶昭良
* @time      2015年2月23日下午8:07:29
* @version   com.collections.testTestQueue V1.0
*/
public class TestQueue
{

         /**
          * @param args
          */
         private static List<String> list1 = new ArrayList<String>();
         /**
          *
          * @param apple  添加字符串到堆栈中
          */
         public void push(String apple)
         {
                   list1.add(apple);
         }
         /**
          *
          * @return  返回待删除的字符串
          */
         public String pop()
         {
                   //System.out.println("长度为："+length);
                   String apple = list1.get(0);
                   //String apple = list1.get(length-1);
                   list1.remove(0);
                   return apple;
         }

        
         public  void sayStatck()
         {
                   Iterator<String> it = list1.iterator();
                   while(it.hasNext())
                   {
                            System.out.println(it.next());
                   }
         }
         /**
          * @param args
          */
         public static void main(String[] args)
         {
                   // TODO Auto-generated method stub
                   // TODO Auto-generated method stub
                   TestQueue ts = new TestQueue();
                   ts.push("apple");
                   ts.push("banana");
                   ts.push("orange");
                  
                  
                   ts.sayStatck();
                   String text = ts.pop();
                   System.out.println("Pop值："+text);
                   ts.sayStatck();
                  
                   String text1 = ts.pop();
                   System.out.println("Pop值："+text1);
                   ts.sayStatck();
         }

}

```
  

ArrayList源码追踪部分：
  
调查ArrayList发现了LastIndexOf的一个秒的地方，倒着过来找
  
   
  ```java
public int lastIndexOf(Object o) {
        if (o == null) {
            for (int i = size-1; i >= 0; i--)
                if (elementData==null)
                    return i;
        } else {
            for (int i = size-1; i >= 0; i--) //秒！
                if (o.equals(elementData))
                    return i;
        }
        return -1;
    }

```
   
      
下面的ArrayList.this.add真的是值得学习
  
   
 ```java      
public void add(E e) {
            checkForComodification();

            try {
                int i = cursor;
                ArrayList.this.add(i, e);
                cursor = i + 1;
                lastRet = -1;
                expectedModCount = modCount;
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }

```
   
  
这样的话 会直接定义到本类的add方法，而不会是别的
  
   ```java
    
public void add(int index, E element) {
        rangeCheckForAdd(index);

        ensureCapacityInternal(size + 1);  // Increments modCount!!
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
        elementData[index] = element;
        size++;
    }


  ``` 
   

LinkedList:
本质：链表，有序，可以重复
优点：增加和删除
缺点：查找（获取）

ListIterator的常用方法：
  
boolean    hasNext( )   //判断是否还要下一个元素
  
Object      next( )   //取出下一个元素,并把指针移动到下一个元素
  
int           nextIndex( )   //下一个元素的索引
  
boolean    hasPrevious()   //判断是否有前一个元素
  
Object      previous( )   //取出前一个元素,并把指针移动到前一个元素
  
int           previousIndex( )   //前一个元素的索引
  
void        add(Object   o)    //添加一个元素
  
void        remove( )   //删除指针指向的当前元素
  
void        set(Object   o)    //修改指针指向的当前元素
  
  

  

package com.collections.test;


```java
import java.util.*;

/**
* @author    叶昭良
* @time      2015年2月23日下午12:15:42
* @version   com.collections.testListTest V1.0
*/
public class ListTest
{

         /**
          * @param args
          */
         public static void main(String[] args)
         {
                   // TODO Auto-generated method stub
//               Collection c = new LinkedList();
                   /**
                    *   测试list内部有别于Collection的特有的方法
                    */
                   List<String> list1 = new ArrayList<String>();
                   list1.add("abc0");
                   list1.add("abc1");
                   list1.add("abc2");
                   System.out.println(list1);
                  
                   list1.add(1, "abchaha");
                   System.out.println(list1);
                  
                   for(int i = 0 ; i < list1.size(); i++)
                   {
                            System.out.println("list1["+i+"]="+list1.get(i));
                   }
                   for(Object o:list1)
                   {
                            System.out.println(o);
                   }
                  
                   list1.set(3, "fdsf");
                   System.out.println(list1);
                  
                   System.out.println("fdsf在list1的第"+list1.indexOf("fdsf")+"位");
                   //最后一次abc1出现的位置
                   System.out.println(list1.lastIndexOf("abc1"));
                   List list2 = list1.subList(1, 4);
                   System.out.println("list2:"+list2);
                  
                   ListIterator lit = list1.listIterator();
                   while(lit.hasNext())
                   {
                            Object  o = lit.next();
                            System.out.println("List:"+o);
                   }
         }
}


 ``` 

小结：
1：只想遍历  就会ArrayList
2：希望不断添加   最好就是LinkedList
3：可以使用Collections.synchronizedList()获得一个线程安全的ArrayList and LinkedList

HashSet:
本质：数组+链表，由entry链构成的数组，无序，不可重复。本质是HashMap
优点：添加和删除效率高
缺点：获取慢（遍历都一样）,只能遍历一遍，Set集合无法直接获取某个元素

添加：1 计算hashcode
     2. 根据hashcode定位到某一索引处（数组）
     3. 添加值到索引处的链表（判断是否重复）

所以hashset == 数组（存放hashcode）+链表（存值）（其实就是为了提高大链表的添加和遍历的速度）
HashSet的源码内部其实是HashMap

1:重写hashCode and equals

注意：hashCode  and equals的生成方式： 右键―>source--->hashCode and equals即可自动
     生成。
  
```java
package com.collections.test;

import java.util.HashSet;
import java.util.Iterator;

/**
* @author    叶昭良
* @time      2015年2月23日下午8:35:46
* @version   com.collections.testTestHashSet V1.0
*/
public class TestHashSet
{

         /**
          * @param args
          */
         public static void main(String[] args)
         {
                   // TODO Auto-generated method stub
                   HashSet<String> hs = new HashSet<String>();
                   hs.add("abc0");
                   hs.add("abc1");
                   hs.add("abc2");
                   hs.add("abc3");
                   hs.add("abc4");
                   hs.add("abc0");
                  
                   System.out.println(hs);
                  
                   //set集合没有索引 index  所以必须用iterator
                   Iterator it = hs.iterator();
                   while(it.hasNext())
                   {
                            System.out.println(it.next());
                   }
                  
                   /**
                    * Person类的hashCode被执行
                            Person类的hashCode被执行
                            Person类的hashCode被执行
                            Person类的hashCode被执行
                            Person类的hashCode被执行
                            Person类的equals方法被执行
                    */
                   Person p1 = new Person("zhao",32);
                   Person p2 = new Person("zhao1",34);
                   Person p5 = new Person("zhao1",34);
                   Person p3 = new Person("zhao2",35);
                   Person p4 = new Person("zhao3",36);
                  
                   HashSet<Person> hs1 = new HashSet<Person>();
                   hs1.add(p1);
                   hs1.add(p2);
                   hs1.add(p3);
                   hs1.add(p4);
                   hs1.add(p5);
                  
                   Iterator it1 = hs1.iterator();
                   while(it1.hasNext())
                   {
                            Person p11 = (Person)it1.next();
                            //System.out.println(p11.getName()+":"+p11.getAge());
                            System.out.println(p11);
                   }
         }

}

//自定类中加入set需要重写hashCode和equals方法   集合的不可重复性！！所以必须
//添加hashcode和equals这种比较低级的！如果是TreeSet还得考虑大小！必须是
///Comparable
class Person
{
        
         /*public int compareTo(String anotherString)
         {
                   return Name.compareTo(anotherString);
         }*/
         private int Age;
         private String Name;
        
         public Person( String name,int age)
         {
                   //super();
                   this.Age = age;
                   this.Name = name;
         }
         public int getAge()
         {
                   return this.Age;
         }
         public void setAge(int age)
         {
                   this.Age = age;
         }
         public String getName()
         {
                   return this.Name;
         }
         public void setName(String name)
         {
                   this.Name = name;
         }
         /// 右键source---》 generate equals and hashcode method
         @Override
         public int hashCode()
         {
                   System.out.println("Person类的hashCode被执行");
                   final int prime = 31;
                   int result = 1;
                   result = prime * result + Age;
                   result = prime * result + ((Name == null) ? 0 : Name.hashCode());
                   return result;
         }
         @Override
         public boolean equals(Object obj)
         {
                   System.out.println("Person类的equals方法被执行");
                   if (this == obj)
                            return true;
                   if (obj == null)
                            return false;
                   if (getClass() != obj.getClass())
                            return false;
                   Person other = (Person) obj;
                   if (Age != other.Age)
                            return false;
                   if (Name == null)
                   {
                            if (other.Name != null)
                                     return false;
                   } else if (!Name.equals(other.Name))
                            return false;
                   return true;
         }
         @Override
         public String toString()
         {
                   return "Person [Age=" + Age + ", Name=" + Name + "]";
         }                
}

```
TreeSet

本质：二叉树，注意查看root节点（这是二叉树结构的标志）  TreeSet关键是排序！（不是插入 和获取元素）
优点：增加和删除
缺点：获取

TreeSet右节点值大于父节点    左节点值小于父节点,根据这种推断，最小的是存在于左边的最下边！ 利用递归推断。

1:重写equals  and  hashcode
2:实现一个comparable接口或者一个Comparator类

第一种方法：实现comparable接口

只需要在Person类中增加implements comparable并重写compareTo(Object o)接口即可：

```java
class Person implements Comparable
{
   
    /*public int compareTo(String anotherString)
    {
       return Name.compareTo(anotherString);
    }*/
    private int Age;
    private String Name;
   
    public Person( String name,int age)
    {
       //super();
       this.Age = age;
       this.Name = name;
    }
    public int getAge()
    {
       return this.Age;
    }
    public void setAge(int age)
    {
       this.Age = age;
    }
    public String getName()
    {
       return this.Name;
    }
    public void setName(String name)
    {
       this.Name = name;
    }
    /// 右键source---》 generate equals and hashcode method
    @Override
    public int hashCode()
    {
       System.out.println("Person类的hashCode被执行");
       final int prime = 31;
       int result = 1;
       result = prime * result + Age;
       result = prime * result + ((Name == null) ? 0 : Name.hashCode());
       return result;
    }
    @Override
    public boolean equals(Object obj)
    {
       System.out.println("Person类的equals方法被执行");
       if (this == obj)
           return true;
       if (obj == null)
           return false;
       if (getClass() != obj.getClass())
           return false;
       Person other = (Person) obj;
       if (Age != other.Age)
           return false;
       if (Name == null)
       {
           if (other.Name != null)
              return false;
       } else if (!Name.equals(other.Name))
           return false;
       return true;
    }
    @Override
    public String toString()
    {
       return "Person [Age=" + Age + ", Name=" + Name + "]";
    }
    @Override
    public int compareTo(Object o)
    {
       // TODO Auto-generated method stub
       //按照年龄进行排序
       if(this == o)
       {
           return 0;
       }
       if(this == null)
       {
           return -1;
       }
       if(!(o instanceof Person))
       {
           return -1;
       }
      
       Person p1 = (Person)o;
       //先按照年龄排序
       int temp = p1.Age - this.Age;
      
       if(temp ==0)
       {
           if(this.Name == null)
           {
              if(p1.Name == null)
              {
                  return 0;
              }else
              {
                  return -1;
              }
           }else
           {
              return this.Name.compareTo(p1.Name);
           }
       }
       return 0;
    }
      
}


//第二种方法创建一个比较类，并返回一个比较器，在TreeSet的对象定义中使用

  
class Comparators
{
    public Comparator getComparator()
    {
       return new Comparator()
       {

           // 0 表示  o1  o2相等
           // 负数 表示  o1 < o2
           // 整数 表示  o1 > o2
           @Override
           public int compare(Object o1, Object o2)
           {
              if(o1 instanceof String)
              {
                  //字符串的比较
                  return compare((String) o1,(String) o2);
              }else if(o1 instanceof Integer)
              {
                  //整数的比较
                  return compare((Integer) o1,(Integer) o2);
              }else if(o1 instanceof Boolean)
              {
                  //布尔类型的比较
                  return compare((Boolean) o1,(Boolean) o2);
              }else if(o1 instanceof Person)
              {
                  //布尔类型的比较
                  return compare((Person) o1,(Person) o2);
              }else
              {
                  System.out.println("未找到合适的比较器  ");
                  return 1; //默认大于0
              }
           }
           //用于比较字符串
           public int compare(String o1, String o2)
           {
              //暂时备份一下 字符串
              String s1 = (String)o1;
              String s2 = (String)o2;
              //获取字符串的长度
              int len1 = s1.length();
              int len2 = s2.length();
              //获取最小值
              int n = Math.min(len1, len2);
              //转化为字符数组
              char[] v1 = s1.toCharArray();
              char[] v2 = s2.toCharArray();
             
              //设置位置参数
              int pos = 0;
             
              //从未到头  按照index下表从头到尾进行判断
              while(n-- != 0)
              {
                  char c1 = v1[pos];
                  char c2 = v2[pos];
                  if(c1 != c2)
                  {
                     return c1 - c2;
                  }
                  pos++;
              }
              return len1-len2; //相反则是反序 降序
           }
           //用于比较整数
           public int compare(Integer o1, Integer o2)
           {
              int val1 = o1.intValue();
              int val2 = o2.intValue();
              //return (val1 < val2 ? -1 :(val1 == val2 ? 0 : 1));
//            return (val1 > val2 ? 1 :(val1 == val2 ? 0 : -1));
              //升序
              //return (val2 > val1 ? -1 :(val1 == val2 ? 0 : 1));
              //降序
              return (val2 > val1 ? 1 :(val1 == val2 ? 0 : -1));
           }
           //用于比较布尔值
           public int compare(Boolean o1,Boolean o2)
           {
              return (o1.equals(o2)? 0: (o1.booleanValue()==true)?1:-1);
           }
          
           public int compare(final Person o1, final Person o2)
           {
              String Name1 = o1.getName();
              String Name2 = o2.getName();
              int Age1 = o1.getAge();
              int Age2 = o2.getAge();
              Boolean sex1 = o1.getSex();
              Boolean sex2 = o2.getSex();
              /*//第一次 线比较年龄
              return (compare(Age1,Age2)==0?
                     //第二次比较名字
                     (compare(Name1,Name2)==0?
                            //第三次比较性别
                            (compare(sex1,sex2)==0? 0 :compare(sex1,sex2)):
                                compare(Name1,Name2)):
                                   compare(Age1,Age2));*/
              //第一次  比较年龄
              return (compare(Name1,Name2)==0?
                     //第二次比较 岁数
                     (compare(Age1,Age2)==0?
                            //第三次比较岁数
                            (compare(sex1,sex2)==0?0:compare(sex1,sex2)):
                                compare(Age1,Age2)):
                                   compare(Name1,Name2));
                    
           }
       };
    }
}

```
  
并在TreeSet定义对象时候，采用如下方式：
  ```java
//TreeSet<Person> ts2 = new TreeSet<Person>(); //找到解决办法了
       TreeSet<Person> ts2 = new TreeSet<Person>(new Comparators().getComparator());

```
  
比较之前的
  ```java
TreeSet<Person1> ts1 = new TreeSet<Person1>();
```

  

另外此种方法也运用在Arrays.Sort当中：其中pdian 是一个person的数组
  ```java
Person[] pdian = new Person[]
              {
                  new Person("yezhao",34,Boolean.TRUE),
                  new Person("xinran",10,Boolean.FALSE),
                  new Person("zhaoliang",33,Boolean.TRUE),
                  new Person("zhaidc",30,Boolean.FALSE),
                  new Person("zhaidc",31,Boolean.FALSE),
              };
Arrays.sort(pdian, new Comparators().getComparator());

```
  

另外的可以通过匿名类来实现：
  ```java
Arrays.sort(pdian, new Comparator<Object>()
       {
           // 0 表示  o1  o2相等
           // 负数 表示  o1 < o2
           // 整数 表示  o1 > o2
           @Override
           public int compare(Object o1, Object o2)
           {
              if(o1 instanceof String)
              {
                  //字符串的比较
                  return compare((String) o1,(String) o2);
              }else if(o1 instanceof Integer)
              {
                  //整数的比较
                  return compare((Integer) o1,(Integer) o2);
              }else if(o1 instanceof Boolean)
              {
                  //布尔类型的比较
                  return compare((Boolean) o1,(Boolean) o2);
              }else if(o1 instanceof PersonMan)
              {
                  //布尔类型的比较
                  return compare((PersonMan) o1,(PersonMan) o2);
              }else
              {
                  System.out.println("未找到合适的比较器  ");
                  return 1; //默认大于0
              }
           }
           //用于比较字符串
           public int compare(String o1, String o2)
           {
              //暂时备份一下 字符串
              String s1 = (String)o1;
              String s2 = (String)o2;
              //获取字符串的长度
              int len1 = s1.length();
              int len2 = s2.length();
              //获取最小值
              int n = Math.min(len1, len2);
              //转化为字符数组
              char[] v1 = s1.toCharArray();
              char[] v2 = s2.toCharArray();
             
              //设置位置参数
              int pos = 0;
             
              //从未到头  按照index下表从头到尾进行判断
              while(n-- != 0)
              {
                  char c1 = v1[pos];
                  char c2 = v2[pos];
                  if(c1 != c2)
                  {
                     return c1 - c2;
                  }
                  pos++;
              }
              return len1-len2; //相反则是反序 降序
           }
           //用于比较整数
           public int compare(Integer o1, Integer o2)
           {
              int val1 = o1.intValue();
              int val2 = o2.intValue();
              //return (val1 < val2 ? -1 :(val1 == val2 ? 0 : 1));
//                       return (val1 > val2 ? 1 :(val1 == val2 ? 0 : -1));
              //升序
              //return (val2 > val1 ? -1 :(val1 == val2 ? 0 : 1));
              //降序
              return (val2 > val1 ? -1 :(val1 == val2 ? 0 : 1));
           }
           //用于比较布尔值
           public int compare(Boolean o1,Boolean o2)
           {
              return (o1.equals(o2)? 0: (o1.booleanValue()==true)?1:-1);
           }
          
           public int compare(final PersonMan o1, final PersonMan o2)
           {
              String Name1 = o1.getName();
              String Name2 = o2.getName();
              int Age1 = o1.getAge();
              int Age2 = o2.getAge();
              Boolean sex1 = o1.getSex();
              Boolean sex2 = o2.getSex();
              /*//第一次 线比较年龄
              return (compare(Age1,Age2)==0?
                     //第二次比较名字
                     (compare(Name1,Name2)==0?
                            //第三次比较性别
                            (compare(sex1,sex2)==0? 0 :compare(sex1,sex2)):
                                compare(Name1,Name2)):
                                   compare(Age1,Age2));*/
              //第一次  比较年龄
              return (compare(Name1,Name2)==0?
                     //第二次比较 岁数
                     (compare(Age1,Age2)==0?
                            //第三次比较岁数
                            (compare(sex1,sex2)==0?0:compare(sex1,sex2)):
                                compare(Age1,Age2)):
                                   compare(Name1,Name2));
                    
           }
       });


  ```


LinkedHashSet:
本质：链表+ 有序
优点：增加和删除
缺点：查找（获取）
Linkedlist也是有序但是可重复
LinkedHashSet 不可重复！
hashSet的高效率，但是无须！
LinkedHashSet可以当做HashSet来使用

Map集合基本操作：
  
l   添加
  
V         put(K key, V value)  //把一个键值对存入map集合中                                                  
  
void    putAll(Map<? extends K,? extends V> m) //把一组键值对存入到map集合中
  
l   删除
  
V         remove(Object key)  //删除key为指定对象的键值对
  
void    clear()    //情况map中的所有元素
  
l   判断
  
boolean     containsKey(Object key)  //判断是否包含key为指定对象的键值对
  
boolean     containsValue(Object value) //判断是否包含value为指定对象的元素
  
boolean     isEmpty() //判断map集合是否包含元素
  
l   获取
  
   V                                        get(Object key) //获得指定key对应的value
  
Set<K>                              keySet() //获得所有key组成的Set集合
  
Collection<V>                   values()  //获得所有value组成的Set集合
  
   Set<Map.Entry<K,V>>     entrySet() //获得所有键值对对象组成的Set集合
  
int                                     size()  //获得map集合的大小(存了多少元素)
  



