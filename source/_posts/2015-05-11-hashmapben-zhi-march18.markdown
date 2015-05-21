---
layout: post
title: "hashmap本质March18"
date: 2015-05-11 14:58:42 +0800
comments: true
categories: JavaBasic
---
<!--more-->

HashMap:
Hashmap本质：
----> ----> ----> ---->----> ----> ----> ----> ---->---->----> ----> ----> ---->---->----> ----> ----> ---->---->
  |            |                |             |
  |            |                 |             |
  |            |                 |
  |            |
  |
  |
  |
优点：因为内部结构为哈希表,所以添加删除查询等操作都很快
缺点：HashMap不能直接遍历keySet() , values() , entrySet()三个方法作为遍历方法
由于Map 把key 和 value 封装到了Entry对象中,所以并不会对key和value直接操作,所以key和value可以为null
HashMap已经替换掉HashTable（只不过还有一个参与分子Properties还在大量使用）
|--set
  |---keySet
  |---entrySet
|--Collection

```java
/**
* 
*/
package com.collections.test;

/**
* @author    叶昭良
* @time      2015年2月25日上午1:38:30
* @version   com.collections.testTestHashMap V1.0
*/
import java.util.*;
public class TestHashMap
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                Map map = new HashMap();
                map.put("1", "zhangsan");
                map.put("2", "lisi");
                map.put("3", "cbd");
                
                System.out.println(map.get("1"));
                
                Set keySet = map.keySet();
                
                Iterator it = keySet.iterator();
                while(it.hasNext())
                {
                        Object o = it.next();
                        System.out.println(map.get(o)+":"+o);
                }
                
                System.out.println("The Second method to get Value---");
                Collection value = map.values();
                Iterator it1 =value.iterator();
                while(it1.hasNext())
                {
                        System.out.println(it1.next());
                }
                
                map.remove("1");
                System.out.println("The Third Method to get value----");
                Set keyset1 = map.entrySet();
                Iterator itp = keyset1.iterator();
                while(itp.hasNext())
                {
                        Map.Entry o  = (Map.Entry)itp.next();
                        System.out.println(o+"，其中 键位："+o.getKey()+",值为："+o.getValue());
                }
        }

}
```
HashMap源码追踪：


HashMap源码调研的基础知识:
1.位运算符

用于容量的翻倍，以及限定在某一范围内：
>>   
>>>   不考虑符号 ，空位都以0补齐 0补最高位 （>>  >>>正数无差别，负数有差别）
&   与运算
```java
package com.collections.test;

import java.util.HashMap;
import java.util.HashSet;



/**
* @author    叶昭良
* @time      2015年2月25日上午11:06:09
* @version   com.collections.testDeepHashMap V1.0
*/
public class DeepHashMap
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
/**
*  Long类型的equals的实现
*     public boolean equals(Object obj) {
        if (obj instanceof Long) {
            return value == ((Long)obj).longValue();
        }
        return false;
    }
*/
                System.out.println(new Long(1).equals(new Integer(1)));
                System.out.println(new Long(1).intValue()==new Integer(1).intValue());
                System.out.println(new Long(1).longValue()==new Integer(1).intValue());
                System.out.println(new Long(1).longValue());
                System.out.println(new Integer(1).intValue());
                System.out.println(new Long(100).hashCode());
                System.out.println(new Integer(100).hashCode());
//                System.out.println(new Integer(100).hashCode(100));
                HashMap<Integer,String> hm = new HashMap<Integer,String>();
                System.out.println(hm.hashCode());
//                HashSet<String> hs ;
                System.out.println( (1 << 30)/1024/1024/1024); //1G
                System.out.println(8>>>2); //+++
                System.out.printf("%o -- %x  --- %X\n ----\n",-14,-14,-14);
                System.out.println(-14>>>2);
                System.out.println(Integer.toBinaryString(-14));
                System.out.println(-14>>2);
                //把一个字符串变为 奇数
                //  >>>
                System.out.println(Integer.parseInt("00111111111111111111111111111100",2));
                //  >>
                //Integer.parseInt("-010101010",2) 可以带符号
                System.out.println(Integer.toBinaryString(14));
                //~取反运算 仅仅针对 整数
                System.out.println(~14);
                System.out.println(Integer.toBinaryString(~14));
                //如何解析带符号位的
                System.out.println("2的3次方 的错误答案"+(2^3)); // ^在java当中是异或操作
                System.out.println("2的3次方 的正确答案"+Math.pow(2, 3)); 
                //System.out.println(11111111111111111111111111110010);
                /*
                 * 封存在一个函数内部
                 * System.out.println("通过异或的方式进行验证");
                System.out.println(Long.parseLong("11111111111111111111111111111111",2)^(Long.parseLong("11111111111111111111111111110010",2)));
                // 1101  8+4+1 = 13
                System.out.println((Long.parseLong("11111111111111111111111111111111",2)^(Long.parseLong("11111111111111111111111111110010",2)))+1);
                System.out.println(-((Long.parseLong("11111111111111111111111111111111",2)^(Long.parseLong("11111111111111111111111111110010",2)))+1));
                */
                System.out.println("通过自定义的获取负数的方法:"+parseNegativeInteger("11111111111111111111111111110010"));
                //负数的补码的计算是取反加1（然后在最后的数上加上负数）   正数的补码是直接计算
                System.out.println(Integer.parseInt("100",2)); //把100这个二进制数按照2进制方式解析
                
                //String.
                System.out.println(100);
                System.out.println("开始从高到低进行打印");
                //格式化输出 只有 %d  %o  %x  二进制得用toBinaryString
                for(byte b1 :longToByte8(100)) 
                {
                        System.out.printf("%d:",b1);
                }
                //我现在是取反
                
                // returns the number of one-bits 
                System.out.println(Integer.toBinaryString(170));
            System.out.println("Number of one bits = " + Integer.bitCount(170));
            
            // 牛逼算法！ 让n最终为 某一个范围内的两端值
            System.out.println(15|15>>>1);
            int n = 170; //使得 n都是
            /**
             * 或的运算就是保持最大值（与的运算是为了保持原样（当和0xff))
             *  13 :00000000 00000000 00000000  00001101
             *   6  00000000 00000000 00000000  00000110  
             * 15:  00000000 00000000 00000000  00001111
             *   一直保持15   当时最大值 则保持一样了
             */
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16; // 2^16
        System.out.println("Pow(2,16):"+Math.pow(2, 16));
        System.out.println(n);
        System.out.println(170>>>8);
        
        int tb=0;
        //由此可见 If的赋值是会影响到else的！！！
        //并且是有先后顺序的，这是在看hashmap的时候学到的
        if((tb=4) ==3)
        {
                System.out.println("In the if"+tb);
        }else 
        {
                System.out.println("In the else"+tb);
        }
        
        }
        /**
         * 计算机内部负数是用补码表示的，正数的原码与补码相同 (也就是所有数都以补码存在)
         * 你不要用这种连写的方式，改成：
         *   5  0101    3: 0011
         *       
         *        1:  0101 ^ 0011 =  0110
         *        2:  0110 ^ 0011 =  0101  == 5
         *        3:  0110 ^ 0101 =  0011  == 3  
         *                                      真心妙的操作
                i = i^j;
                j = i^j;
                i = i^j; 
                这样就可进行两个数的交换
         * @param sum
         * @return
         */
        public static long parseNegativeInteger(String number)
        {
                return -((Long.parseLong("11111111111111111111111111111111",2)^(Long.parseLong(number,2)))+1);
        }
    public static byte[] longToByte8(long sum) {
        byte[] arr = new byte[8];
        //一个字节有8 位   long是8个字节的
        arr[0] = (byte) (sum >> 56); //先提取最高位
        arr[1] = (byte) (sum >> 48); //第二高位
        arr[2] = (byte) (sum >> 40); //第三高位
        arr[3] = (byte) (sum >> 32);
        arr[4] = (byte) (sum >> 24);
        arr[5] = (byte) (sum >> 16);
        arr[6] = (byte) (sum >> 8);
        arr[7] = (byte) (sum & 0xff); //不超过258 15*16+16 =256
        return arr;
    }


}
```

由此得知HashMap的翻倍原理：
newCap = oldCap << 1



2.字节的截断工具：
&的截断作用

//0xff 其实就是一个字节，也就是最低的一个直接
//通过&来截断最低的一个直接
(byte) (c & 0xff);  来获得c的最低的一个直接（一般用于结尾   ^

p = tab[index = (n - 1) & hash]
ByteUtils字节工具：（一谈到工具，一般是静态方法）
```java
/**
* http://www.cnblogs.com/fangfan/p/4086662.html
*/
package com.collections.test;

import java.util.Arrays;

/**
* @author    叶昭良
* @time      2015年2月25日下午11:56:33
* @version   com.collections.testByteUtils V1.0
*/
public class ByteUtils
{

        /**
         * @param args
         */
          /**
     * 
     * <pre>
     * 将4个byte数字组成的数组合并为一个float数.
     * &是位与，&一般用于取一个字节（八位）的位数
     * </pre>
     * 
     * @param arr
     * @return
     */
    public static float byte4ToFloat(byte[] arr) {
        if (arr == null || arr.length != 4) {
            throw new IllegalArgumentException("byte数组必须不为空,并且是4位!");
        }
        int i = byte4ToInt(arr);
        return Float.intBitsToFloat(i);
    }

    /**
     * 
     * <pre>
     * 将一个float数字转换为4个byte数字组成的数组.
     * </pre>
     * 
     * @param f
     * @return
     */
    public static byte[] floatToByte4(float f) {
        int i = Float.floatToIntBits(f);
        return intToByte4(i);
    }

    /**
     * 
     * <pre>
     * 将八个byte数字组成的数组转换为一个double数字.
     * </pre>
     * 
     * @param arr
     * @return
     */
    public static double byte8ToDouble(byte[] arr) {
        if (arr == null || arr.length != 8) {
            throw new IllegalArgumentException("byte数组必须不为空,并且是8位!");
        }
        long l = byte8ToLong(arr);
        return Double.longBitsToDouble(l);
    }

    /**
     * 
     * <pre>
     * 将一个double数字转换为8个byte数字组成的数组.
     * </pre>
     * 
     * @param i
     * @return
     */
    public static byte[] doubleToByte8(double i) {
        long j = Double.doubleToLongBits(i);
        return longToByte8(j);
    }

    /**
     * 
     * <pre>
     * 将一个char字符转换为两个byte数字转换为的数组.
     * </pre>
     * 
     * @param c
     * @return
     */
    public static byte[] charToByte2(char c) {
        byte[] arr = new byte[2];
        arr[0] = (byte) (c >> 8);
        //0xff 其实就是一个字节，也就是最低的一个直接
        //通过&来截断最低的一个直接
        arr[1] = (byte) (c & 0xff);
        return arr;
    }

    /**
     * 
     * <pre>
     * 将2个byte数字组成的数组转换为一个char字符.
     * </pre>
     * 
     * @param arr
     * @return
     */
    public static char byte2ToChar(byte[] arr) {
        if (arr == null || arr.length != 2) {
            throw new IllegalArgumentException("byte数组必须不为空,并且是2位!");
        }
        return (char) (((char) (arr[0] << 8)) | ((char) arr[1]));
    }

    /**
     * 
     * <pre>
     * 将一个16位的short转换为长度为2的8位byte数组.
     * </pre>
     * 
     * @param s
     * @return
     */
    public static byte[] shortToByte2(Short s) {
        byte[] arr = new byte[2];
        arr[0] = (byte) (s >> 8);
        arr[1] = (byte) (s & 0xff);
        return arr;
    }

    /**
     * 
     * <pre>
     * 长度为2的8位byte数组转换为一个16位short数字.
     * </pre>
     * 
     * @param arr
     * @return
     */
    public static short byte2ToShort(byte[] arr) {
        if (arr != null && arr.length != 2) {
            throw new IllegalArgumentException("byte数组必须不为空,并且是2位!");
        }
        return (short) (((short) arr[0] << 8) | ((short) arr[1] & 0xff));
    }

    /**
     * 
     * <pre>
     * 将short转换为长度为16的byte数组.
     * 实际上每个8位byte只存储了一个0或1的数字
     * 比较浪费.
     * </pre>
     * 
     * @param s
     * @return
     */
    public static byte[] shortToByte16(short s) {
        byte[] arr = new byte[16];
        for (int i = 15; i >= 0; i--) {
            arr[i] = (byte) (s & 1);
            s >>= 1;
        }
        return arr;
    }

    public static short byte16ToShort(byte[] arr) {
        if (arr == null || arr.length != 16) {
            throw new IllegalArgumentException("byte数组必须不为空,并且长度为16!");
        }
        short sum = 0;
        for (int i = 0; i < 16; ++i) {
            sum |= (arr[i] << (15 - i));
        }
        return sum;
    }

    /**
     * 
     * <pre>
     * 将32位int转换为由四个8位byte数字.
     * </pre>
     * 
     * @param sum
     * @return
     */
    public static byte[] intToByte4(int sum) {
        byte[] arr = new byte[4];
        arr[0] = (byte) (sum >> 24);
        arr[1] = (byte) (sum >> 16);
        arr[2] = (byte) (sum >> 8);
        arr[3] = (byte) (sum & 0xff);
        return arr;
    }

    /**
     * <pre>
     * 将长度为4的8位byte数组转换为32位int.
     * </pre>
     * 
     * @param arr
     * @return
     */
    public static int byte4ToInt(byte[] arr) {
        if (arr == null || arr.length != 4) {
            throw new IllegalArgumentException("byte数组必须不为空,并且是4位!");
        }
        return (int) (((arr[0] & 0xff) << 24) | ((arr[1] & 0xff) << 16) | ((arr[2] & 0xff) << 8) | ((arr[3] & 0xff)));
    }

    /**
     * 
     * <pre>
     * 将长度为8的8位byte数组转换为64位long.
     * </pre>
     * 
     * 0xff对应16进制,f代表1111,0xff刚好是8位 byte[]
     * arr,byte[i]&0xff刚好满足一位byte计算,不会导致数据丢失. 如果是int计算. int[] arr,arr[i]&0xffff
     * 
     * @param arr
     * @return
     */
    public static long byte8ToLong(byte[] arr) {
        if (arr == null || arr.length != 8) {
            throw new IllegalArgumentException("byte数组必须不为空,并且是8位!");
        }
        return (long) (((long) (arr[0] & 0xff) << 56) | ((long) (arr[1] & 0xff) << 48) | ((long) (arr[2] & 0xff) << 40)
                        | ((long) (arr[3] & 0xff) << 32) | ((long) (arr[4] & 0xff) << 24)
                        | ((long) (arr[5] & 0xff) << 16) | ((long) (arr[6] & 0xff) << 8) | ((long) (arr[7] & 0xff)));
    }

    /**
     * 将一个long数字转换为8个byte数组组成的数组.
     */
    public static byte[] longToByte8(long sum) {
        byte[] arr = new byte[8];
        arr[0] = (byte) (sum >> 56);
        arr[1] = (byte) (sum >> 48);
        arr[2] = (byte) (sum >> 40);
        arr[3] = (byte) (sum >> 32);
        arr[4] = (byte) (sum >> 24);
        arr[5] = (byte) (sum >> 16);
        arr[6] = (byte) (sum >> 8);
        arr[7] = (byte) (sum & 0xff);
        return arr;
    }

    /**
     * 
     * <pre>
     * 将int转换为32位byte.
     * 实际上每个8位byte只存储了一个0或1的数字
     * 比较浪费.
     * </pre>
     * 
     * @param num
     * @return
     */
    public static byte[] intToByte32(int num) {
        byte[] arr = new byte[32];
        for (int i = 31; i >= 0; i--) {
            // &1 也可以改为num&0x01,表示取最地位数字.
            arr[i] = (byte) (num & 1);
            // 右移一位.
            num >>= 1;
        }
        return arr;
    }

    /**
     * 
     * <pre>
     * 将长度为32的byte数组转换为一个int类型值.
     * 每一个8位byte都只存储了0或1的数字.
     * </pre>
     * 
     * @param arr
     * @return
     */
    public static int byte32ToInt(byte[] arr) {
        if (arr == null || arr.length != 32) {
            throw new IllegalArgumentException("byte数组必须不为空,并且长度是32!");
        }
        int sum = 0;
        for (int i = 0; i < 32; ++i) {
            sum |= (arr[i] << (31 - i));
        }
        return sum;
    }

    /**
     * 
     * <pre>
     * 将长度为64的byte数组转换为一个long类型值.
     * 每一个8位byte都只存储了0或1的数字.
     * </pre>
     * 
     * @param arr
     * @return
     */
    public static long byte64ToLong(byte[] arr) {
        if (arr == null || arr.length != 64) {
            throw new IllegalArgumentException("byte数组必须不为空,并且长度是64!");
        }
        long sum = 0L;
        for (int i = 0; i < 64; ++i) {
            sum |= ((long) arr[i] << (63 - i));
        }
        return sum;
    }

    /**
     * 
     * <pre>
     * 将一个long值转换为长度为64的8位byte数组.
     * 每一个8位byte都只存储了0或1的数字.
     * </pre>
     * 
     * @param sum
     * @return
     */
    public static byte[] longToByte64(long sum) {
        byte[] arr = new byte[64];
        for (int i = 63; i >= 0; i--) {
            arr[i] = (byte) (sum & 1);
            sum >>= 1;
        }
        return arr;
    }
    
   
    public static void showMaxValAndminVal() {
        // 127
        System.out.println("Byte.Max_Value:"+Byte.MAX_VALUE);
        // -128
        System.out.println("Byte.MIN_VALUE:"+Byte.MIN_VALUE);

        // 32767
        System.out.println("Short.MAX_VALUE:"+Short.MAX_VALUE);
        // -32768
        System.out.println("Short.MIN_VALUE:"+Short.MIN_VALUE);

        // 65535 2的16次方-1
        System.out.println("(int) Character.MAX_VALUE:"+(int) Character.MAX_VALUE);
        // 0
        System.out.println("(int) Character.MIN_VALUE:"+(int) Character.MIN_VALUE);

        // 2147483647
        System.out.println("Integer.MAX_VALUE:"+Integer.MAX_VALUE);
        // -2147483648
        System.out.println("Integer.MIN_VALUE:"+Integer.MIN_VALUE);

        // 科学计数法.
        // 3.4028235E38
        System.out.println("Float.MAX_VALUE:"+Float.MAX_VALUE);
        // 1.4E-45
        System.out.println("Float.MIN_VALUE:"+Float.MIN_VALUE);

        // 9223372036854775807
        System.out.println("Long.MAX_VALUE:"+Long.MAX_VALUE);
        // -9223372036854775808
        System.out.println("Long.MIN_VALUE:"+Long.MIN_VALUE);

        // 科学计数法.
        // 1.7976931348623157E308
        System.out.println("Double.MAX_VALUE:"+Double.MAX_VALUE);
        // 4.9E-324
        System.out.println("Double.MIN_VALUE:"+Double.MIN_VALUE);
    }
    
    public static void transByte() {
        char c = 'z';
        byte[] charToByte2Arr = ByteUtils.charToByte2(c);
       

        short s = Short.MAX_VALUE;
        // System.out.println("Short.MAX_VALUE:" + s);
        byte[] shortToByte2Arr = ByteUtils.shortToByte2(s);
       

        byte[] shortToByte16 = ByteUtils.shortToByte16(s);
        System.out.println(Arrays.toString(shortToByte16));
        System.out.println(ByteUtils.byte16ToShort(shortToByte16));

        int i = Integer.MAX_VALUE;
        // System.out.println("Integer.MAX_VALUE:" + i);
        byte[] intToByte4Arr = ByteUtils.intToByte4(i);
        

        byte[] intToByte32Arr = ByteUtils.intToByte32(i);
        System.out.println(Arrays.toString(intToByte32Arr));
        System.out.println(ByteUtils.byte32ToInt(intToByte32Arr));

        long j = Long.MAX_VALUE;
        // System.out.println("Long.MAX_VALUE:" + j);
        byte[] longToByte8Arr = ByteUtils.longToByte8(j);
      

        byte[] longToByte64Arr = ByteUtils.longToByte64(j);
        System.out.println(Arrays.toString(longToByte64Arr));
        System.out.println(ByteUtils.byte64ToLong(longToByte64Arr));

        double d = 2.34;
        byte[] doubleToByte8Arr = ByteUtils.doubleToByte8(d);
       

        float f = 1.2f;
        byte[] floatToByte4Arr = ByteUtils.floatToByte4(f);
       
    }

        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
//                showMaxValAndminVal();
                transByte();
        }

}
```
补充^:
按位异或。比如二进制     1001 ^ 1100 = 0101
0^0=0，1^1=0 ，1^0 = 1，0^1=1
复制代码

3.理解重新调整大小


第一部分：理解重新调整大
```java
    /**
     * Implements Map.putAll and Map constructor
     *
     * @param m the map
     * @param evict false when initially constructing this map, else
     * true (relayed to method afterNodeInsertion).
     */
    final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) {
        int s = m.size();
        if (s > 0) {
            if (table == null) { // pre-size
                float ft = ((float)s / loadFactor) + 1.0F;
                int t = ((ft < (float)MAXIMUM_CAPACITY) ?
                         (int)ft : MAXIMUM_CAPACITY);
                if (t > threshold)
                    threshold = tableSizeFor(t);
            }
            else if (s > threshold)
                resize();
            for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
                K key = e.getKey();
                V value = e.getValue();
                putVal(hash(key), key, value, false, evict);
            }
        }
    }

    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    

    /**
     * Returns a power of two size for the given target capacity.
用于理解2的阶乘大小的实现。别人一直强调的2的阶乘的扩容
     */
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1; //
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```
4.理解添加

```java
   /**
     * Implements Map.putAll and Map constructor
     *
     * @param m the map
     * @param evict false when initially constructing this map, else
     * true (relayed to method afterNodeInsertion).
     */
    final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) {
        int s = m.size();
        if (s > 0) {
            if (table == null) { // pre-size
                float ft = ((float)s / loadFactor) + 1.0F;
                int t = ((ft < (float)MAXIMUM_CAPACITY) ?
                         (int)ft : MAXIMUM_CAPACITY);
                if (t > threshold)
                    threshold = tableSizeFor(t);
            }
            else if (s > threshold)
                resize();
            for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
                K key = e.getKey();
                V value = e.getValue();
                putVal(hash(key), key, value, false, evict);
            }
        }
    }



final Node<K,V>[] resize() {
//在创建新表之前 ，先保存旧表
        Node<K,V>[] oldTab = table;
//保存旧表的长度
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
//保存 上限值
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
//加倍了！！！！！！
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
//新标的长度  newTab为newCap,newCap默认为最大的Capacity  16
            Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
//分为两种情况进行扩充
                    if (e.next == null)
//e.hash 是Node类的final属性
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
// e.hash & oldCap 的作用是 若<oldCap 则范围e.hash
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }

```
5.理解说明：

```java
/**
     * Initializes or doubles table size.  If null, allocates in
     * accord with initial capacity target held in field threshold.
     * Otherwise, because we are using power-of-two expansion, the
     * elements from each bin must either stay at same index, or move
     * with a power of two offset in the new table.
     *
初始化或者使表格大小加倍。如果为null，分配和初始容量目标（根据threshold字段值） 另外地，因为我们使用2的阶乘的拓展，每个块的元素应该是处于相同的指数，或者在新表中移动2的阶乘</font> 
复制代码
<font size="3">没错我依然以putMapEntries作为话题的引发点。
两个参数：
    static final int MAXIMUM_CAPACITY = 1 << 30;//1G的大小 2^30次方 

    /**
     * The load factor used when none specified in constructor.
     */
static final float DEFAULT_LOAD_FACTOR = 0.75f;

static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
* size：该变量保存了该 HashMap 中所包含的 
key-value 对的数量。      
* threshold：该变量包含了 HashMap 能容纳的 key-value 对的极限，它的值等于 HashMap 的容量乘以负载因子（load factor）

当 size++ >= threshold 时，HashMap 会自动调用 resize 方法扩充 HashMap 的容量。每扩充一次，HashMap 的容量就增大一倍

```
6.HashMap和HashSet的区别
   Hashset是继承list，具有iterator特性
   HashMap继承自Map，不具有iterator特性
   两者的共同特点是都是linked数组（数组的bucket号都是一个链表）

7.理解Hash算法
<font size="3">其实就是hash调用自定义类的hashcode方法而已）以及contains key的过程。在HashSet或者HashMap中都有get方法，get方法中都有hash的影子，当然在containsKey中也是包含着hash()的方法，通过这个方法的调用才可以找到bucket(俗语：存储桶，用于装一条链表，可用于存储的桶的个数，也就是数组的长度，也叫做capacity容量，一般对应着一个负载因子算法，当超过最大容量乘上负载因子，就需要进行rehash的过程)</font>
复制代码
TreeMap:
本质：
优点：
缺点：
LinkedHashMap:
本质：  
优点：
缺点：
Iterator:
替换掉Enumeration,先前的Enumeration类的方法名的长度较长，不方便记忆，淘汰之。
泛型
泛型的本质是参数化类型。当我们再进行泛型定义的时候可以用T  K  V  E，但是当我们在使用泛型接口的时候 必须给泛型附上一个值，就好像给方法附上实参
Java的泛型实际上从编译的角度来说仅仅是一个类型擦除
    public static void doTest(List<?extends Parent> list)
    //包含Parend以及所有继承自Parent的子类
    public static void doTest(List<?super Parent> list)
    //包含Parend以及所有Parent的父类
泛型的兼容：
    
       //泛型的向后兼容
  
       //l1 只是回去找List的方法   
  
       List l1 =   new ArrayList<String>();
  
       l1.add("abc");
  
       l1.add(3);
  
泛型的不协变：
       //泛型的不协变
//     List<String>l3 = new ArrayList<Integer>(); //报错

Arrays:Arrays中 asList  不能使用add ，因为asList根本就没有改变数组的本质！！只不过可以使用遍历的方法常用方法：
sort                     给数组的元素排序
binarySearch     二分查找法查找元素
copyOf               数组复制
equals                  判断两个数组的元素是否相等
fill                         填充数组
hashCode            计算数组的元素的哈希值
toString                返回每个数组元素组成的字符串
asList        把数组转化为结构不可改变,但可以查询修改的集合 

Collections:
```java
package com.collections.test;

import java.util.ArrayList;
import java.util.Collections;

/**
* 
* @author    叶昭良
* @time      2015年3月19日下午1:25:44
* @version   com.collections.testTestCollections1 V1.0
* 功能：  测试 骰子 的乱序  随机排序
                步骤：
* 注意：
* 掌握：
                思考：
* 回顾：
*/
public class TestCollections1
{
        public static void main(String[] args)
        {
                ArrayList<Integer> arl = new ArrayList<Integer>();
                arl.add(4);
                arl.add(9);
                arl.add(2);
                arl.add(10);
                arl.add(3);
                Collections.shuffle(arl);
                for(Integer temp:arl)
                {
                        System.out.println(temp);
                }
        }
}
```java


Sort: 只对list排序    treeset内部就有排序   hashmap根本就不需要
binarySearch  二分查找  必须是list 而且该集合必须排好序
fill   只能对list集合有效
shuffle  洗牌   打乱集合的顺序

只有排完序才能进行binarySearch

```java
Collections.sort(pileOfApple);
                for(String temp:pileOfApple)
                {
                        System.out.println(temp);
                }
                //已排好序 才可以这样做 ! 否则不可以进行二分查找  Arrays类似的用法
                System.out.println(Collections.binarySearch(pileOfApple, "gd"));</font>
```
Collections常用方法：
sort       对List集合进行排序
binarySearch 方法,使用二分查找法查找元素,要求必须是List集合,而且必须已经排好序
fill                  方法,使用指定元素填充List集合
replaceAll      方法,使用指定元素替换所有的另一个元素,只对List有效
max   方法,返回一个集合中最大的元素
min
shuffle  洗牌,打乱集合元素的顺序
synchronizedList     返回指定集合的线程安全的包装集合
unmodifiableList     返回指定集合的不可修改的包装集合

多重数据结构的实现：

```java

package com.collections.test;

import java.util.HashMap;

/**
* @author    叶昭良
* @time      2015年3月3日下午6:54:32
* @version   com.collections.testTestMapMap V1.0
*/
import java.util.*;
public class TestMapMap
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                //Map<String,Map<String,Map<Integer,Double>>> stuTotal = new HashMap<String,Map<String,Map<Integer,Double>>>();
                Map<String, Integer> innerMap = new HashMap<String, Integer>();
                innerMap.put("innerKey", 2014);
                Map<String, Map<String, Integer>> map = new HashMap<String, Map<String, Integer>>();
                map.put("outerKey", innerMap);
                 
                Map<String, Integer> targetMap = map.get("outerKey");
                if(targetMap == innerMap){
                    System.out.println("You got the inner Map, and it is saved in targetMap!!!");
                }
                
                //原来嵌套集合数据结构是可以实现的
                Map<String,Map<String,Map<Integer,Double>>> outterMap = new HashMap<String, Map<String,Map<Integer,Double>>>();
                
                //==
        }

}
```
ToStringString函数的append可采用链式的append本质是：

```java
<font size="3"> @Override
    public StringBuilder append(boolean b) {
        super.append(b);
        return this;
    }

    @Override
    public StringBuilder append(char c) {
        super.append(c);
        return this;
    }
。。。。。</font>
```java
<font size="3">String类型的toString方法，会去调用valueOf函数，关键的调研过程:
其实关键是得看println接受的对象，以前一直认为是对象们的toString方法，没错的确是他直接引起，但是其实是因为println的重载方法来限制了对象必须去重写toString方法.
因为再对应String的println重载方法中，包含着String.valueOf所以需要进一步调用String类的valueOf函数，而valueOf函数则是调用了toString方法，所以最基本的函数toString。
不同的参数类型会去加载不同的println的方法！</font>


