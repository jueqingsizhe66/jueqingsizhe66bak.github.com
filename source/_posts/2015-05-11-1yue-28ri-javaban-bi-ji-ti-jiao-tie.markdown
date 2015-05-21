---
layout: post
title: "1月28日Java班笔记提交帖"
date: 2015-05-11 14:58:40 +0800
comments: true
categories: JavaBasic
---
<!--more-->

第一部分：笔记
引用杨老师笔记，的确经典
1、GTK命名规则
     1）一般以gtk_开头
     2）gtk_widget_代表这个函数可以应用于GtkWidget及所有子类。gtk_container_代表这个函数可以应用于GtkContainer及所有子类。
     3）gtk_***_new，创建一个控件并且返回它的标识(编号)。
     4）gtk_AAA_get_BBB，从AAA类型的控件的标识获得控件的BBB属性的值；gtk_AAA_set_BBB，设置指定标识的类型为AAA的控件的BBB属性的值

  分析上述结论： 
     a.        的确原始的gtk程序都是以gtk_开头的  ，而现在com.rupeng.gtk4j.*，则是利用GTK这个类封装起来这些方法。
     b.        一般的规律都是gtk_widget_开头表示继承GtkWidget及所有子类，所以需要有一张GTK的总体继承图
       看 总体继承图.gif
      说明：也就是说在思考widget的时候，一般接着思考Container、Entry、Label,针对Container有需要思考Bin\Box\Grid\Fixed，针对Bin有需要接着思      考Window|NoteBook|,ScrolledWindow|Button,针对Window有需要接着思考Dialog

     c. 之所以是gtk_***_new_*** 是因为***既可以代表 gtk_window_new  也可以代表gtk_progress_bar_new                   gtk_check_box_new   gtk_button_new_with_label   gtk_toggle_button_new等。
         d.  Gtk_AAA_get|set_BBB   其实AAA一般代表控件   BBB一般代表属性，也可以代表地方  比如  gtk_screen_get_width()   gtk_entry_set_max_length   gtk_widget_set_visible()
     Gtk_tool_item_set_toolkit_item   AAA也可以有两个_连接。

2. GTK的使用框架：
                 //初始化
                GTK.gtk_init();
                //建立窗口  设置成static int window变量
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                //设置窗口名称
                GTK.gtk_window_set_title(window, "计算器v1.0");
                //添加窗口
                GTK.gtk_widget_show(window);
                
                //关闭对话框
                GTK.g_signal_connect(window, "destroy", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                }, null);
                
                //网格布局  盒子布局   固定布局。。。 当前选择网格布局
            int houseGrid = GTK.gtk_grid_new();
            //载入控件
            //省略 一万行代码。。。。
            
            //载入到windows当中
                GTK.gtk_container_add(window, houseGrid);
                //启动循环
                GTK.gtk_main();
3. GTK.gtk_container_add(window,box1) ;在一个main中只能使用一次gtk_container_add..

4. CheckBox
int gtk_check_button_new_with_label(String label)
CheckButton继承自ToggleButton 
所以可以使用 void gtk_toggle_button_set_active(int toggle_button, boolean is_active) 设置是否选中
boolean gtk_toggle_button_get_active(int toggle_button) 获得是否选中


5.gtk_entry_new
基本的文本框设置
使用void gtk_label_set_text(int label, String str)修改Label的文本内容
void gtk_entry_set_max_length(int entry, int max)设置输入框最大的宽度
void gtk_entry_set_text(int entry, String text)设置输入框的文本
String gtk_entry_get_text(int entry)获得输入框的文本值

密码文本框设置：
void gtk_entry_set_visibility(int entry,boolean visible)当把visible设置为false的时候输入的内容都会显示成 gtk_entry_set_invisible_char(int entry,char inv_char)设置的字符。用于实现密码框



第二部分： 作业

case1: 固定布局的测试

/**
* @author 叶昭良
* @version  v1.0
*/
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;
//import com.rupeng.gtk4j.Utils;
public class TestGtkWidgetFixedLayout
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                GTK.gtk_init();
            int window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL); // 原来还有个POPUP估计是右键菜单
            GTK.gtk_widget_show(window);
            
            GTK.gtk_window_set_title(window, "霜雪"); // widget下没有 只有在window下有。
           // GTK.gtk_window_set_position(window, GTK.GTK_WIN_POS_CENTER);
            
            GTK.gtk_window_set_position(window, GTK.GTK_WIN_POS_CENTER_ALWAYS);
            //GTK.gtk_window_set_resizable(window, true);  //默认可以拉伸
            // 默认的窗口可以最大化， GTK.gtk_widget_set_maximize(widget)
           
            //新建了一个文本框
            int entry1  = GTK.gtk_entry_new();
            GTK.gtk_widget_show(entry1);
            //新建了一个button 
            int button1 = GTK.gtk_button_new_with_label("click me");
            GTK.gtk_widget_show(button1);
            
            //新建了一个标签
            int label1 =  GTK.gtk_label_new("请输入：");
            GTK.gtk_widget_show(label1);
            
            // 安静的关闭对话框
            GTK.g_signal_connect(window, "destroy", new IGCallBack()
            {
                    @Override
                    public void execute(int instance, int eventData, Object object)
                    {
                            // TODO 自动生成的方法存根
                            GTK.gtk_main_quit();
                    }
            }, null);
            
            //int box1= GTK.gtk_box_new(GTK., spacing)
            //新加了一个house  用来整租给文本框 +button+label
            int house = GTK.gtk_fixed_new();
            GTK.gtk_fixed_put(house, entry1,0,0);
            GTK.gtk_fixed_put(house,button1,50,0);
            GTK.gtk_fixed_put(house,label1,0,100); //很难适应屏幕大小！！所以一般很少用
            GTK.gtk_widget_show(house);
            
            GTK.gtk_container_add(window,house);
            
            GTK.gtk_main();
        }

}
复制代码



case2 盒子布局的测试


/**
* @author 叶昭良
* @version  v1.0
*/
import java.awt.AWTException;
import java.awt.Robot;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;
//import com.rupeng.gtk4j.Utils;
public class TestGtkWidgetBoxLayout
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                GTK.gtk_init();
            int window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL); // 原来还有个POPUP估计是右键菜单
            GTK.gtk_widget_show(window);
            
            GTK.gtk_window_set_title(window, "霜雪"); // widget下没有 只有在window下有。
           // GTK.gtk_window_set_position(window, GTK.GTK_WIN_POS_CENTER);
            
            GTK.gtk_window_set_position(window, GTK.GTK_WIN_POS_CENTER_ALWAYS);
            //GTK.gtk_window_set_resizable(window, true);  //默认可以拉伸
            // 默认的窗口可以最大化， GTK.gtk_widget_set_maximize(widget)
           
            //新建了一个文本框
            int entry1  = GTK.gtk_entry_new();
            GTK.gtk_widget_show(entry1);
            //新建了一个button 
            int button1 = GTK.gtk_button_new_with_label("click me");
            GTK.gtk_widget_show(button1);
            
            //新建了一个标签
            int label1 =  GTK.gtk_label_new("请输入：");
            int label2 =  GTK.gtk_label_new("");
            int label3 =  GTK.gtk_label_new("");
            
            // 显示标签
            GTK.gtk_widget_show(label1);
            GTK.gtk_widget_show(label2);
            GTK.gtk_widget_show(label3);
            // 安静的关闭对话框
            GTK.g_signal_connect(window, "destroy", new IGCallBack()
            {
                    @Override
                    public void execute(int instance, int eventData, Object object)
                    {
                            // TODO 自动生成的方法存根
                            GTK.gtk_main_quit();
                    }
            }, null);
            
            //int box1= GTK.gtk_box_new(GTK., spacing)
            //新加了一个house  用来整租给文本框 +button+label
            // gtk_box_new 的第一个参数 一般是两种赋值方式GTK.GTK_ORIENTATION_HORIZONTAL和_VERTICAL
            // 第二个参数表示的间隔space,如果设置为5则为5pt的间隔
            int houseApple = GTK.gtk_box_new(GTK.GTK_ORIENTATION_HORIZONTAL, 0);
            int houseBanana = GTK.gtk_box_new(GTK.GTK_ORIENTATION_HORIZONTAL, 0);
            int houseTree = GTK.gtk_box_new(GTK.GTK_ORIENTATION_VERTICAL, 0);
            //水平house
            GTK.gtk_box_pack_start(houseApple, label1, true, true, 0);
            GTK.gtk_box_pack_start(houseApple,entry1 , true, true, 0);
            GTK.gtk_box_pack_start(houseBanana,label2 , true, true, 0);
            GTK.gtk_box_pack_start(houseBanana,button1 , true, true, 0);
            GTK.gtk_box_pack_start(houseBanana,label3 , true, true, 0);
            //垂直house
            GTK.gtk_box_pack_start(houseTree, houseApple, true, true, 0);
            GTK.gtk_box_pack_start(houseTree, houseBanana, true, true, 0);
            

            // 显示三个容器 的house
            GTK.gtk_widget_show(houseApple);
            GTK.gtk_widget_show(houseBanana);
            GTK.gtk_widget_show(houseTree);
            
            GTK.gtk_container_add(window,houseTree);
            
            GTK.gtk_main();
            
            
        }

}
复制代码



case3 网格布局的测试

/**
* @author 叶昭良
* @version v1.0
* @version v1.1 改进了密码框  
*/
import java.awt.AWTException;
import java.awt.Robot;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;
//import com.rupeng.gtk4j.Utils;
public class TestGtkWidgetGridLayout
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                GTK.gtk_init();
            int window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL); // 原来还有个POPUP估计是右键菜单
            GTK.gtk_widget_show(window);
            
            GTK.gtk_window_set_title(window, "霜雪"); // widget下没有 只有在window下有。
           // GTK.gtk_window_set_position(window, GTK.GTK_WIN_POS_CENTER);
            
            GTK.gtk_window_set_position(window, GTK.GTK_WIN_POS_CENTER_ALWAYS);
            //GTK.gtk_window_set_resizable(window, true);  //默认可以拉伸
            // 默认的窗口可以最大化， GTK.gtk_widget_set_maximize(widget)
           
         
            // 安静的关闭对话框
            GTK.g_signal_connect(window, "destroy", new IGCallBack()
            {
                    @Override
                    public void execute(int instance, int eventData, Object object)
                    {
                            // TODO 自动生成的方法存根
                            GTK.gtk_main_quit();
                    }
            }, null);
            
            //int box1= GTK.gtk_box_new(GTK., spacing)
            //新加了一个网格屋子
            int houseGrid = GTK.gtk_grid_new();




            // 显示 的houseGrid
            GTK.gtk_widget_show(houseGrid);
            // 0开头是程序员的惯例
            testGtk(houseGrid,0);
            landInternet(houseGrid,2);
            

            
            GTK.gtk_container_add(window,houseGrid);
            GTK.gtk_main();
            
            
        }
        //新建一个测试的界面
        /**
         * 
         * @param houseGrid  整租的大房子  表示gtk_grid_new的返回值
         * @param start      在houseGrid分配到的第几行开始编号
         */
        public static void testGtk(int houseGrid,int start)
        {
                   //新建了一个文本框
            int entry1  = GTK.gtk_entry_new();
            GTK.gtk_widget_show(entry1);
            //新建了一个button 
            int button1 = GTK.gtk_button_new_with_label("click me");
            GTK.gtk_widget_show(button1);
            
            //新建了一个标签
            int label1 =  GTK.gtk_label_new("请输入：");
            int label2 =  GTK.gtk_label_new("");
            int label3 =  GTK.gtk_label_new("");
            
            // 显示标签
            GTK.gtk_widget_show(label1);
            GTK.gtk_widget_show(label2);
            GTK.gtk_widget_show(label3);
            
            //整租到housegrid
            // gtk_grid_attach(int grid, int child, int left,int top, int width, int height);
            GTK.gtk_grid_attach(houseGrid, label1, 0, start, 1, 1);
            GTK.gtk_grid_attach(houseGrid, entry1, 1, start, 1, 1);
            
            GTK.gtk_grid_attach(houseGrid, button1, 1, start+1, 1, 1);
        }
        
    //新建一个登陆界面的GTK
        public static void landInternet(int houseGrid,int start) 
        {
                   //新建了一个文本框
            int entry1  = GTK.gtk_entry_new();

            int entry2  = GTK.gtk_entry_new();
            
            //增加了文本框的密码框的设置y2, false);
            //可以设置显示的
            GTK.gtk_entry_set_visibility(entry2,false);  //字符 默认的密码框为*
            GTK.gtk_entry_set_invisible_char(entry2, '1'); //只能是字符  不能是"1"

            //新建了一个button 
            int buttonEnter = GTK.gtk_button_new_with_label("登陆");

            int buttonClosed = GTK.gtk_button_new_with_label("取消");

            int checkbox1= GTK.gtk_check_button_new_with_label("是否保存密码");
            //新建了一个标签
            int label1 =  GTK.gtk_label_new("用户名：");
            int label2 =  GTK.gtk_label_new("密码  ：");
            
            //为什么要声明为final暂时不是特别清楚
            final int label3 =  GTK.gtk_label_new("");
            
            // 显示标签
            GTK.gtk_widget_show(entry1);
            GTK.gtk_widget_show(entry2);
            GTK.gtk_widget_show(label1);
            GTK.gtk_widget_show(label2);
            GTK.gtk_widget_show(label3);
            GTK.gtk_widget_show(checkbox1);
            GTK.gtk_widget_show(buttonEnter);
            GTK.gtk_widget_show(buttonClosed);
            
            //整租到houseGrid
            GTK.gtk_grid_attach(houseGrid, label1, 0, start, 1, 1);
            GTK.gtk_grid_attach(houseGrid, entry1, 1, start, 1, 1);
            GTK.gtk_grid_attach(houseGrid, label2, 0, start+1, 1, 1);
            GTK.gtk_grid_attach(houseGrid, entry2, 1, start+1, 1, 1);
            GTK.gtk_grid_attach(houseGrid, checkbox1, 1, start+2, 1, 1);
            GTK.gtk_grid_attach(houseGrid, buttonEnter, 1, start+3, 1, 1);
            GTK.gtk_grid_attach(houseGrid, buttonClosed, 2, start+3, 1, 1);
            GTK.gtk_grid_attach(houseGrid, label3, 1, start+5, 1, 1);
            
            
            // 进行按钮的事件连接
            GTK.g_signal_connect(buttonEnter,"clicked",new IGCallBack()
            {
                    @Override
                    public void execute(int instance, int eventData, Object object)
                    {
                            // TODO 自动生成的方法存根
                            System.out.println("你点击了登陆");
                            GTK.gtk_label_set_text(label3, "你点击了登陆");
                    }
            },null);
            
            GTK.g_signal_connect(buttonClosed,"clicked",new IGCallBack()
            {
                    @Override
                    public void execute(int instance, int eventData, Object object)
                    {
                            // TODO 自动生成的方法存根
                            System.out.println("你点击了关闭");
                            GTK.gtk_label_set_text(label3, "你点击了关闭");
                    }
            },null);
        }

}
复制代码



case4 checkBox模拟 安装界面
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;
/**
* 
*/

/**
* @author 叶昭良
* @version  v1.0
*/
public class TestGtkCheckBox
{

        /**
         * @param args
         */
        private static int window ;
        private static int box;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根

        //        gtkHead(window,"gtkCheckbox测试");
                GTK.gtk_init();
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                GTK.gtk_window_set_title(window, "hello");
                GTK.gtk_widget_show(window);
                GTK.g_signal_connect(window,"destroy",new IGCallBack() 
                {

                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                        
                },null);
                box = GTK.gtk_box_new(GTK.GTK_ORIENTATION_VERTICAL, 0);
                int box2= GTK.gtk_box_new(GTK.GTK_ORIENTATION_VERTICAL,0);
                int box1= GTK.gtk_box_new(GTK.GTK_ORIENTATION_VERTICAL,0);
                testGtkCheckBox(window,box2);
                testGtkCheckBoxInstall(window,box1);
                GTK.gtk_box_pack_start(box, box2, false, false, 0);
                GTK.gtk_box_pack_start(box, box1, false, false, 0);

                GTK.gtk_container_add(window, box);
                GTK.gtk_widget_show(box);
                GTK.gtk_main();
                
        }

        
        public static void testGtkCheckBox(int window,int box2)
        {
        
                int cbMan = GTK.gtk_check_button_new_with_label("男");
                int cbWoman = GTK.gtk_check_button_new_with_label("女");
                
                
                GTK.gtk_box_pack_start(box2, cbMan, false,false        , 0);
                GTK.gtk_box_pack_start(box2, cbWoman, false,false        , 0);
                
                
                GTK.gtk_widget_show(cbMan);
                GTK.gtk_widget_show(cbWoman);
                GTK.gtk_widget_show(box2);
        
        }
        public static void testGtkCheckBoxInstall(int window,int box1)
        {
                int entryLicense = GTK.gtk_entry_new();
                GTK.gtk_entry_set_text(entryLicense, "产品使用声明:\n 尊重ISIS国际标准");

                //定义控件
                int boxInner = GTK.gtk_box_new(GTK.GTK_ORIENTATION_HORIZONTAL, 0);
                //为什么要修改为终态
                final int cbAgree = GTK.gtk_check_button_new_with_label("我同意上面的协议");
                final int btnInstall = GTK.gtk_button_new_with_label("install");
                int btnClosed  = GTK.gtk_button_new_with_label("close");
                GTK.gtk_widget_set_sensitive(btnInstall, false);
                
                //添加控件
                GTK.gtk_box_pack_start(box1, entryLicense, false,false        , 0);
                GTK.gtk_box_pack_start(box1, cbAgree, false,false, 0);
                GTK.gtk_box_pack_start(boxInner, btnInstall, false,false,0);
                GTK.gtk_box_pack_start(boxInner, btnClosed, false,false,0);
                GTK.gtk_box_pack_start(box1, boxInner, false,false        , 0);
                
                //显示控件
                GTK.gtk_widget_show(entryLicense);
                GTK.gtk_widget_show(cbAgree);
                GTK.gtk_widget_show(btnInstall);
                GTK.gtk_widget_show(btnClosed);
                GTK.gtk_widget_show(boxInner);
                GTK.gtk_widget_show(box1);
                
                //添加事件
                GTK.g_signal_connect(cbAgree, "clicked", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                boolean isChecked =  GTK.gtk_toggle_button_get_active(cbAgree);
                                
                                //写了gtk_checkbox发现没有set命令 于是用了widget..
                                GTK.gtk_widget_set_sensitive(btnInstall, isChecked);
                        }
                }, null);
        }

}
复制代码



通过上面的练习： 学习了GTK框架的编程方式，掌握了 label,entry,checkbox(继承toggle_button),Box Layout, Grid Layout , Fixed Layout的基本使用方法，包括clicked,destroy这两个信号的事件处理。  另外拓展的学习了匿名类 new IGCallback(){..}的使用方法。

![checkbox的结果](/images/java/checkbox.gif)
![盒子布局的结果](/images/java/盒子.gif)
![固定布局的结果](/images/java/固定.gif)
![网格布局的结果](/images/java/网格布局.gif)
![GTK的总体框架图](/images/java/总体框架图.gif)

