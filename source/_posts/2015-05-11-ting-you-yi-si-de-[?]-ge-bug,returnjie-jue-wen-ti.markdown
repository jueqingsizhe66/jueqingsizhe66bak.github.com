---
layout: post
title: "挺有意思的一个bug，return解决问题"
date: 2015-05-11 14:58:46 +0800
comments: true
categories: JavaBasic
---
<!--more-->
相信大家都写过省市联动选择的case，可是我写了一个版本却是bug不断，调了一个多小时，暂时未解，经杨老师指点知道问题所在，另外我写了另外一个版本的实现。目的是：当市发生变化的时候，可以把信息贴到一个标签上。

下面是一个暂时无bug的代码（其中红色是杨老师增加的调试语句）//如果去掉下面的return语句bug就出现了,bug是直接退出。
```java
if(apple < 0)
{
System.out.println("nothing in the city");
return;  //不能删掉
}
```
分析一下问题所在：
界面很简单： 1个省的控件   另一个市的控件， 当切换省的时候，对应的市也改变过来，同时选择一个城市会显示出省市的信息到标签上。为此，设置了两个时间  cmbProvince有一个事件 用于增加市的内容
                                  cmbCity       这个事件用于显示省市信息的内容到标签上


然而就这个简单的程序当时却没有想到 GTK.gtk_combo_box_text_remove_all(cmbCity)的函数可能会促发GTK.g_signal_connect(cmbCity, "changed", new IGCallBack()的执行，但是此时cmbCity没有任何的内容，还读取，null就来了，所以加入了一个return语句，来避免这种碰瓷的现象。
return如果一去除 就又出现问题，因为cmbCity其实没有内容，但是还在促发获取内容的事件。 而return 会导致cmbCity处于待命状态，或者是hold状态（这边我解释得不科学，但是基本可以明白） 当全部删除了，直接返回，然后就可以了不让他去促发cmbCity的改变事件，同时回到cmbProvince的事件（这个会到的过程是我猜测，不知道是否有理？ 因为我想到可能类似于入栈出栈的过程和递归的过程），然后继续往下执行，添加控件。

学习：
1：第一次使用   窗口-->复位透视图 进行调试 
2：第一次使用   if(?== null) return  等语句进行debug
总结：
事件的调用过程一定得自习思考，可能发生的冲突过程。
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;
/**
* 
*/

/**
* @author 叶昭良
* @version 省市联轴器 V1.0
*
*/
public class ProvincedSelect
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        static int cmbProvince;
        static int cmbCity;
        static int labelShow;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                //初始化
                GTK.gtk_init();
                //建立窗口
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                //显示窗口
                GTK.gtk_widget_show(window);
                // 安静关闭
                GTK.g_signal_connect(window, "destroy", new IGCallBack()
                {

                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                        
                }, null);
                //布局设置
                gridHouse = GTK.gtk_grid_new();                        
                GTK.gtk_widget_show(gridHouse);        
                //包含整租房
                GTK.gtk_container_add(window, gridHouse);
                //创建控件
                //
                labelShow = GTK.gtk_label_new("");
                cmbProvince = GTK.gtk_combo_box_text_new();
                cmbCity     = GTK.gtk_combo_box_text_new();

                
                int start =  0;
                //添加控件
                GTK.gtk_grid_attach(gridHouse, cmbProvince, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, cmbCity, 0,start+1, 1, 1);
                GTK.gtk_grid_attach(gridHouse, labelShow, 1,start+2, 1, 1);

                //显示控件

                GTK.gtk_widget_show(cmbProvince);
                GTK.gtk_widget_show(cmbCity);
                GTK.gtk_widget_show(labelShow);
                createProvince(window);
                

                
                //启动循环
                GTK.gtk_main();

        }
        
        public static void createProvince(int window)
        {
                

                GTK.gtk_combo_box_text_append(cmbProvince, "fj", "福建");
                GTK.gtk_combo_box_text_append(cmbProvince, "bj", "北京");
                GTK.gtk_combo_box_text_append(cmbProvince, "sh", "上海");
                GTK.gtk_combo_box_text_append(cmbProvince, "hn", "河南");
                GTK.gtk_combo_box_text_append(cmbProvince, "hb", "河北");
                GTK.gtk_combo_box_text_append(cmbProvince, "sd","山东");
                
                GTK.gtk_combo_box_set_active_id(cmbProvince, "bj");

//添加事件
                GTK.g_signal_connect(cmbProvince, "changed", new IGCallBack() 
                {

                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                
                                String province  =  GTK.gtk_combo_box_get_active_id(cmbProvince);
<font color="#ff0000">                                if(province==null)return;</font>
                                System.out.println(province);
                                GTK.gtk_combo_box_text_remove_all(cmbCity); //删除所有项
                                //GTK.gtk_combo_box_text_remove_all(cmbCity); //删除所有项
                                if(province.equals("fj"))
                                {
                                        
                                        GTK.gtk_combo_box_text_append(cmbCity, "zhz", "漳州");
                                        GTK.gtk_combo_box_text_append(cmbCity, "xm", "厦门");
                                        GTK.gtk_combo_box_text_append(cmbCity, "fz", "福州");
                                        GTK.gtk_combo_box_text_append(cmbCity, "qz", "泉州");
                                        
                                }else if(province.equals("bj"))
                                {
                                        GTK.gtk_combo_box_text_append(cmbCity, "cp", "昌平区");
                                        GTK.gtk_combo_box_text_append(cmbCity, "hd", "海淀区");
                                        GTK.gtk_combo_box_text_append(cmbCity, "tz", "通州区");
                                        GTK.gtk_combo_box_text_append(cmbCity, "cy", "朝阳区");
                                }else if(province.equals("hn"))
                                {
                               GTK.gtk_combo_box_text_append(cmbCity, "zz", "郑州");
                               GTK.gtk_combo_box_text_append(cmbCity, "zmd", "驻马店");
                               GTK.gtk_combo_box_text_append(cmbCity, "ny", "南阳");
                                }else if(province.equals("hb"))
                                {
                               GTK.gtk_combo_box_text_append(cmbCity, "sjz", "石家庄");
                               GTK.gtk_combo_box_text_append(cmbCity, "ts", "唐山");
                               GTK.gtk_combo_box_text_append(cmbCity, "qhd", "秦皇岛");
                                }else if(province.equals("sd"))
                                {
                                   GTK.gtk_combo_box_text_append(cmbCity, "jn", "济南");
                               GTK.gtk_combo_box_text_append(cmbCity, "qd", "青岛");
                               GTK.gtk_combo_box_text_append(cmbCity, "yt", "烟台");
                                }
                        }        
                }, null);
                
                //原来remove之后 也是会促发cmbCity的 信号  而导致改变 ，根源在于这个
                GTK.g_signal_connect(cmbCity, "changed", new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                 String tempProvince = GTK.gtk_combo_box_text_get_active_text(cmbProvince);
                                
<font color="#ff0000">                                int  apple  = GTK.gtk_combo_box_get_active(cmbCity);
                                if(apple < 0)
                                {
                                        System.out.println("nothing in the city");
                                        return;  //不能删掉
                                }</font>
                                
                                String tempCity = GTK.gtk_combo_box_text_get_active_text(cmbCity);
<font color="#ff0000">                                if(tempProvince==null||tempCity==null)return;</font>
                                //GTK.gtk_label_set_text(labelShow, "你准备去"+tempProvince+tempCity);
                                GTK.gtk_label_set_text(labelShow,tempProvince+tempCity);
                                System.out.println(tempProvince);
                                System.out.println(tempCity);

                        }
                }, null);
                
                
        }

}
```
我也是第一次 使用  窗口---->复位透视图  

