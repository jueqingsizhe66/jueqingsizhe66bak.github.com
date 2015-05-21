---
layout: post
title: "2月1日Java班笔记作业"
date: 2015-05-11 14:58:41 +0800
comments: true
categories: JavaBasic
---
<!--more-->
第一部分 ：笔记
FileChooserDialog

1、        创建：int gtk_file_chooser_dialog_new(String title,int parentWindow, int action, String first_button_text)：action可选值GTK_FILE_CHOOSER_ACTION_*：OPEN(打开文件)、SAVE(保存文件)、SELECT_FOLDER(选择文件夹)、CREATE_FOLDER(创建文件夹)
2、gtk_file_chooser_set_select_multiple是否允许多选（一般是设置上）
gtk_file_chooser_set_current_name设置默认文件名
gtk_file_chooser_get_filename得到选择的文件名(单选)
gtk_file_chooser_get_filenames得到选择的文件名（数组，多选）
gtk_file_chooser_set_do_overwrite_confirmation选择已经存在的文件是否提示覆盖，一般是用于保存文件（在作业中有体现）
3、文件过滤器
gtk_file_filter_new：创建过滤器
gtk_file_filter_set_name设置显示的名字
gtk_file_filter_add_pattern添加过滤通配符：第二个参数格式“\*.txt”，若是多个则多次执行这个函数
gtk_file_chooser_add_filter(int chooser,int filter)将过滤器添加到chooser中


TreeView
Treemodal的作用是什么？
有点感觉了

TreeView是基于MVC模式设计的（关键点1）； 也就是说需要有一个M，同时又一个V
把M和V联系起来的函数就叫做C，所以也可以改名为MVF。
C的作用可能是修改M的值，并实时显示在V中； 又或者通过修改V的数据，也能够实时反馈到M中；
所以猜测应该是MVC.png：

所以我们应该需要拥有数据：M（也就是通过liststore创建）
  如何设置M是V的Model很重要,这样就实现了交互的过程，你变我显（你是M,我是V）,你变我改（你是V,我是M）
      当我们获得了数据就可以通过天猫的淘宝来卖了（这就是treeview） 
      我们也可以卸货和添货，这就是C了。
1.         GTK.gtk  columns 数据库的字段。
2.         原来iter就是数据库的行    Columns的列
3.          又明白了一点  在所有的GTK.gtk_*_new的int对象都不是简单的整数，而是一个对象的标识，比如int column是列对象标识
      Int  model是模型对象标识    int textview  是textview对象标识    int gridHouse是网格布局对象标识，  归根结底都得第一反应
      过来这是一个对象标识，认清这些对象就可以琢磨这些方法之间的逻辑关系了。也就是说如果一个方法有某个对象标识那么肯定
       就是说得事先创建这个对象或者通过反射机制获得。当然得看清是不是对象标识。通过对象标识认清逻辑关系（关键点2）

莫名其妙的多了 tree_model的创建  ， 应该加上一步 无论是gtk_tree_model_get_iter_first  还是gtk_tree_model_iter_next
又或者是gtk_tree_model_get_value 还是gtk_tree_model_set_value都涉及到 tree_model的对象标识，必须有tree_model对象标识，
联系到第一步然后把GtkListStore设置为GtkTreeView的Model  ，也就是list_store其实就是tree_model ，因为 ListStore实现了TreeModel接口，所以有的操作以gtk_tree_model开头。一切就算说通了  关键点3

关键性一步  设置数据仓库的显示。
void gtk_tree_view_set_model(int tree_view, int model)把listmodel对象设置显示到tree_view上（关键点4 显示出来）。
认清五个对象标识：
1 list_store（tree_model）对象标识  2.tree_iter对象标识   3. Tree_view对象标识  4. treeViewColumn对象标识 5.cellRender对象标识

一个思路：
思路：首先创建GtkListStore，把要显示数据放到GtkListStore中，接着创建GtkTreeView，添加列配置列属性，然后把GtkListStore设置为GtkTreeView的Model，数据就可以显示出来。对数据添加、遍历等操作的时候都要使用迭代器(类似TextView中的Iter)，int gtk_tree_iter_new()创建迭代器。
一般步骤：
1：  利用treeview创建界面，并创建所需的字段，达到View的目的
2：  根据字段数目，创建liststore的数据仓库，达到Model的目的
3：  利用iter对象标识，插入、遍历、删除、增加。
具体看 作业

另外treeview的行选择问题：
行选择要通过GtkSelection对象进行，int gtk_tree_view_get_selection(int tree_view)获得行选择对象
2、TreeView支持单选、多选等模式，默认是单选。void gtk_tree_selection_set_mode(int tree_selection,int type) 设置选择模式，第一个参数为GtkSelection；type可选值：GTK_SELECTION_SINGLE单选，GTK_SELECTION_MULTIPLE多选，其他不用管
3、int[] gtk_tree_view_get_selection_indices(int tree_view)获得选中行的序号，因为支持多选，所以返回数组
4、获得行选中改变信号，监听GtkSelection的"changed"信号。
5、获得双击事件，监听"button-press-event"信号，判断if(GTK.gdk_event_get_type(eventData)==GTK.GDK_2BUTTON_PRESS)得知是否是双击


ToolBar
1、int gtk_toolbar_new()创建工具栏容器
2、void gtk_toolbar_insert(int toolbar, int item,int pos)将工具栏项添加到工具栏，item：后面讲的工具栏项，pos插入的位置
3、工具栏项有按钮、下拉菜单按钮、分隔栏、开关等复杂内容，这里不介绍，只介绍简单常用的GtkToolButton。
int gtk_tool_button_new(int icon_widget,String label)创建GtkToolButton。icon_widget为显示的控件id，可以在按钮上显示其他控件，一般传0；label为标题。
void gtk_tool_button_set_stock_id(int button,String stock_id); 设置按钮上显示的图片。
响应工具栏按钮点击只要连接"clicked"信号即可

Calendar
1、日历Calendar
int gtk_calendar_new()
int gtk_calendar_get_year(int calendar)、int gtk_calendar_get_month(int calendar)、int gtk_calendar_get_day(int calendar)获得选择的年月日
信号："day-selected"：选择日期发生变化；"day-selected-double-click"双击一个日期。
gtk_progress_bar_\* 进度条
作业
gtk_switch_\*  开关
作业
menu
                //分三步
                //1创建单一菜单
                //2创建一个菜单头
                //3创建总的菜单条，把许多菜单头都挂到菜单条上
具体看toolbar的作业。

gtk_status_icon_\* 右下角的系统图盘图标
作业


第二部分 ：作业

文件选择器+工具栏

```java
import java.io.BufferedInputStream;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.OutputStreamWriter;

/**
* @author    叶昭良
* @time      2015年2月1日下午5:56:29
* @version   TestFileChooser V1.0  简易记事本  打开文件 保存文件 显示照片
* @version   TestFileChooser V2.0  增加了打开对话框  并添加了对话框内部控件的事件
* @version   TestFileChooser V3.0  增加了工具栏
*/
import com.rupeng.gtk4j.*;
public class TestFileChooser2
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        static int scrolledBar;
        static int textview;
        static int  btnApple;
        static String[] selectFiles = null;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                GTK.gtk_init();
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                GTK.gtk_widget_show(window);
                GTK.g_signal_connect(window, "destroy", new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                }, null);
                int btnApple = GTK.gtk_button_new_with_label("打开文件");
                int btnBanana = GTK.gtk_button_new_with_label("保存文件");
                int btnOrange = GTK.gtk_button_new_with_label("显示图片文件");
                int btnPeal   = GTK.gtk_button_new_with_label("打开对话框");
                //创建布局
                gridHouse= GTK.gtk_grid_new();
                GTK.gtk_widget_show(gridHouse);
                GTK.gtk_container_add(window, gridHouse);
                
                //GTK.gtk_widget_set_size_request(window, 500, 500);
                //添加控件到合租房
                int start = 0;
                createToolbar(gridHouse,start);
                GTK.gtk_grid_attach(gridHouse, btnApple, 0, 2, 1, 1);
                GTK.gtk_grid_attach(gridHouse, btnBanana, 1, 2, 1, 1);
                GTK.gtk_grid_attach(gridHouse, btnOrange, 2, 2, 1, 1);
                GTK.gtk_grid_attach(gridHouse, btnPeal, 3, 2, 1, 1);
                createTextView(window,gridHouse,3);
                
                //显示控件
                GTK.gtk_widget_show(btnApple);
                GTK.gtk_widget_show(btnBanana);
                GTK.gtk_widget_show(btnOrange);
                GTK.gtk_widget_show(btnPeal);
                
                //添加按钮事件
                 insertButtonEvent(btnApple);
                 insertButtonSaveEvent(btnBanana);
                 insertButtonPictureEvent(btnOrange);
                 insertButtonDialogEvent(btnPeal);
                //启动循环
                GTK.gtk_main();
        }
        public static void saveFile(int textview) 
        {
                int dlg1 = GTK.gtk_file_chooser_dialog_new("保存文件", 0, GTK.GTK_FILE_CHOOSER_ACTION_SAVE,"保存");
                GTK.gtk_file_chooser_set_do_overwrite_confirmation(dlg1, true);
                //String filename = GTK.gtk_file_chooser_get_filename(dlg1);
                System.out.println("已进入save");
                //SaveOneFile(filename,textview);
                int ret = GTK.gtk_dialog_run(dlg1);
                if(ret == GTK.GTK_RESPONSE_CANCEL)
                {
                        GTK.gtk_widget_destroy(dlg1);
                }else 
                {
                        String filename = GTK.gtk_file_chooser_get_filename(dlg1);
                        System.out.println(filename);
                        SaveOneFile(filename,textview);
                        GTK.gtk_widget_destroy(dlg1);
                }
                
        }
        public static String[] selectFile() 
        {
                int dlg = GTK.gtk_file_chooser_dialog_new("打开文件", 0, GTK.GTK_FILE_CHOOSER_ACTION_OPEN,"打开");
                GTK.gtk_file_chooser_set_select_multiple(dlg, true);
                
                //创建过滤器
                int filter = GTK.gtk_file_filter_new();
                GTK.gtk_file_filter_add_pattern(filter, "*.txt");
                GTK.gtk_file_filter_add_pattern(filter, "*.java");
                GTK.gtk_file_filter_set_name(filter, "文本文件");
                GTK.gtk_file_chooser_add_filter(dlg, filter);
                int ret = GTK.gtk_dialog_run(dlg);
                String[] filenames = null;
                if(ret == GTK.GTK_RESPONSE_OK) 
                {
                        filenames = GTK.gtk_file_chooser_get_filenames(dlg);
                        for(int i = 0 ; i< filenames.length ; i++)
                        {
                                
                                System.out.println("选中文件名"+i+": "+filenames[i]);
                        }
                        GTK.gtk_widget_destroy(dlg);
                }else
                {
                        GTK.gtk_widget_destroy(dlg);
                }
                return filenames;
        }
        public static void selectFile(int gridHouse,int start) 
        {
                int dlg = GTK.gtk_file_chooser_dialog_new("打开文件", 0, GTK.GTK_FILE_CHOOSER_ACTION_OPEN,"打开");
                GTK.gtk_file_chooser_set_select_multiple(dlg, true);
                
                //创建过滤器
                int filter = GTK.gtk_file_filter_new();
                GTK.gtk_file_filter_add_pattern(filter, "*.jpg");
                GTK.gtk_file_filter_add_pattern(filter, "*.png");
                GTK.gtk_file_filter_add_pattern(filter, "*.gif");
                GTK.gtk_file_filter_set_name(filter, "图片文件");
                GTK.gtk_file_chooser_add_filter(dlg, filter);
                int ret = GTK.gtk_dialog_run(dlg);
                String[] filenames = null;
                if(ret == GTK.GTK_RESPONSE_OK) 
                {
                        filenames = GTK.gtk_file_chooser_get_filenames(dlg);
                        for(int i = 0 ; i< filenames.length ; i++)
                        {

                                System.out.println("选中文件名"+i+": "+filenames[i]);
                                int temp = GTK.gtk_image_new_from_file(filenames[i]);
                                GTK.gtk_grid_attach(gridHouse, temp, i, start, 3, 1);
                                GTK.gtk_widget_show(temp);
                        }
                        GTK.gtk_widget_destroy(dlg);
                }else
                {
                        GTK.gtk_widget_destroy(dlg);
                }

        }
        
        public static void showAllFiles(String[] filenames,int textview)
        {
                for(int i = 0 ; i < filenames.length; i++)
                {
                        showOneFile(filenames[i],textview);
                }
        }
/*        public static void showOneFile(String filename, int textview)
        {
                try
                (
                        InputStream fis = new FileInputStream(filename);
                        
                        BufferedInputStream bis = new BufferedInputStream(fis);
                )
                {
                        int len = 0;
                        byte[] fileToFile = new byte[512*1024];
                        while((len = bis.read(fileToFile))!= -1) // -1读取完毕
                        {
                                //InsertStringToTextViewFunction(textview,fileToFile.toString());
                                InsertStringToTextViewFunction(textview,new String(fileToFile,"gb2312"));
                        }
                }
                catch(IOException e)
                {
                        System.out.println("文件读入异常");
                }
                
        }*/
        public static void showOneFile(String filename, int textview)
        {
                try
                (
                        InputStream is = new FileInputStream(filename);
                        InputStreamReader osr = new InputStreamReader(is);
                        BufferedReader br = new BufferedReader(osr);
                )
                {
                        
                        String  temp= filename.substring(filename.lastIndexOf('\\')+1);
                        System.out.println(temp);
                        InsertStringToTextViewFunction(textview,"******************\n当期文件为"+filename+"\n******************\n\n"+temp+" 文件内容如下：\n+---------------------------------------------------------------------------+\n");
                        String content = null;
                        while((content = br.readLine())!=null) // -1读取完毕
                        {
                                //InsertStringToTextViewFunction(textview,fileToFile.toString());
                                InsertStringToTextViewFunction(textview,new String(content));
                        }
                        InsertStringToTextViewFunction(textview,"\n+---------------------------------------------------------------------------+\n******************\n文件"+filename+"读取结束\n******************\n");
                }
                catch(IOException e)
                {
                        System.out.println("文件读入异常");
                }
                
        }
        
        public static void SaveOneFile(String filename, int textview)
        {
                try
                (
                        OutputStream os = new FileOutputStream(filename);
                        OutputStreamWriter osw = new OutputStreamWriter(os);
                        BufferedWriter bw = new BufferedWriter(osw);
                )
                {
                        
                        String  temp= filename.substring(filename.lastIndexOf('\\')+1);
                        System.out.println(temp);
                        
                        int textBuffer =  GTK.gtk_text_view_get_buffer(textview);
                        int textIter = GTK.gtk_text_iter_new();  //这是一个空的iter，需要用textBuffer进行赋值
                        GTK.gtk_text_buffer_get_end_iter(textBuffer, textIter);// 或者textview的textBuffer的末尾！
                        String tempText = GTK.gtk_text_buffer_get_text(textBuffer);
                        
                        //while(tempText != null) // -1读取完毕
                        {
                                //InsertStringToTextViewFunction(textview,fileToFile.toString());
                                bw.write(tempText);
                                bw.newLine();
                        //        GTK.gtk_text_iter_backward_char(textIter);
                        //        tempText = GTK.gtk_text_buffer_get_text(textBuffer);
                        }
                }
                catch(IOException e)
                {
                        System.out.println("文件读入异常");
                }
                
        }
        public static void createTextView(int window, int gridHouse,int start)
        {
                textview =  GTK.gtk_text_view_new();
                createScrolledBar(window,textview,gridHouse,start);
                //GTK.gtk_text_view_set_wrap_mode(textview,GTK.GTK_WRAP_WORD);
                GTK.gtk_text_view_set_wrap_mode(textview,GTK.GTK_WRAP_WORD_CHAR);
                GTK.gtk_widget_show(textview);
        }
        public static void createScrolledBar(int window, int textview, int gridHouse,int start)
        {
                scrolledBar = GTK.gtk_scrolled_window_new();
                GTK.gtk_widget_show(scrolledBar);
                GTK.gtk_widget_set_size_request(scrolledBar, 200, 200);
                GTK.gtk_grid_attach(gridHouse, scrolledBar, 0, start, 1, 1);
                GTK.gtk_container_add(scrolledBar, textview);
                
        }
        
        //封装了  TextView的迭代器操作。。。
        /**
         * 
         * @param textview  多行文本TextView的标识
         * @param temp      插入TextView 的字符串。
         */
        public static void InsertStringToTextViewFunction(int textview,String temp)
        {
                //TextIter是一个TextView的迭代器。
                int textBuffer =  GTK.gtk_text_view_get_buffer(textview);
                int textIter = GTK.gtk_text_iter_new();  //这是一个空的iter，需要用textBuffer进行赋值
                //GTK.gtk_text_iter_forward_to_end(textIter);
                GTK.gtk_text_buffer_get_end_iter(textBuffer, textIter);// 或者textview的textBuffer的末尾！
                GTK.gtk_text_buffer_insert(textBuffer, textIter, temp);
                
                //GTK.gtk_text_buffer_g
                GTK.gtk_text_iter_free(textIter);
                
        }
        /**
         * 
         * @param textview   TextView的多行文本的标识
         * @return           返回的字符串
         */
        public static String GetStringFromTextViewFunction(int textview)
        {
                //TextIter是一个TextView的迭代器。
                int textBuffer =  GTK.gtk_text_view_get_buffer(textview);
                int textIter = GTK.gtk_text_iter_new();  //这是一个空的iter，需要用textBuffer进行赋值
                GTK.gtk_text_buffer_get_end_iter(textBuffer, textIter);// 或者textview的textBuffer的末尾！
                String temp = GTK.gtk_text_buffer_get_text(textBuffer);
                
                String[] splitArray = temp.split("\\n");
                temp = splitArray[splitArray.length-1];
                System.out.println(temp);
                return temp;
                //GTK.gtk_text_buffer_g
                //GTK.gtk_text_iter_free(textIter);
                
        }
        public static boolean showInfo(  String  message,String title)
        {
                int msgDlg = GTK.gtk_message_dialog_new(0, GTK.GTK_DIALOG_DESTROY_WITH_PARENT|
                                GTK.GTK_DIALOG_MODAL,GTK.GTK_MESSAGE_INFO, GTK.GTK_BUTTONS_OK,message);
                GTK.gtk_window_set_title(msgDlg, title);
                int ret = GTK.gtk_dialog_run(msgDlg);
                GTK.gtk_widget_destroy(msgDlg);
                return ret == GTK.GTK_RESPONSE_OK;
        }
        public static void createDialog()
        {
                int dialogApple = GTK.gtk_dialog_new();
                int btnApple = GTK.gtk_button_new_with_label("幽灵点击");
                
                //action_are获得一个 类似box,grid的作用，可以添加容器
                int areaDialog = GTK.gtk_dialog_get_action_area(dialogApple);
                GTK.gtk_widget_show(btnApple);
                GTK.gtk_container_add(areaDialog, btnApple);
                
                GTK.g_signal_connect(btnApple, "clicked", new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                showInfo("欣然是不是很美","那可不");
                        }
                }, null);
                
                int ret = GTK.gtk_dialog_run(dialogApple);
                if(ret == GTK.GTK_RESPONSE_OK)
                {
                        System.out.println("再见");
                        GTK.gtk_widget_destroy(dialogApple);
                }
                GTK.gtk_widget_destroy(dialogApple);
        }

        //     ********************事件整体处理 区域***************************
        /**
         * 
         * @param btnSin   按钮的标识
         * @param tv1      textview的标识
         */
        public static void insertButtonEvent(final int btnSin,final int tv1)
        {
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                String temp = GTK.gtk_button_get_label(btnSin);
                                InsertStringToTextViewFunction(tv1,temp);
                                //isEnter = false;
                        }
                }, null);
        }
        public static void insertButtonEvent(final int btnSin)
        {
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                GTK.gtk_widget_show(scrolledBar);
                                GTK.gtk_widget_set_size_request(scrolledBar, 400, 400);
                                String temp = GTK.gtk_button_get_label(btnSin);
                                selectFiles =  selectFile() ;
                                //isEnter = false;
                                showAllFiles(selectFiles, textview);
                        }
                }, null);
        }
        public static void insertButtonSaveEvent(final int btnSin)
        {
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                saveFile(textview);
                        }
                }, null);
        }
        public static void insertButtonPictureEvent(final int btnSin)
        {
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                GTK.gtk_widget_hide(scrolledBar);
                                selectFile(gridHouse, 2) ;
                        }
                }, null);
        }
        
        public static void insertButtonDialogEvent(final int btnSin)
        {
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                createDialog();
                        }
                }, null);
        }

        public static void createToolbar(int gridHouse,int start)
        {
                //创建工具栏容器
                int tbApple = GTK.gtk_toolbar_new();
                GTK.gtk_widget_show(tbApple);
                GTK.gtk_widget_set_size_request(tbApple, 300, 20);
                GTK.gtk_grid_attach(gridHouse, tbApple, 0, start, 1, 1);
                
          int btnNew = GTK.gtk_tool_button_new(0, "新建");
          GTK.gtk_tool_button_set_stock_id(btnNew, GTK.GTK_STOCK_NEW);
          GTK.gtk_widget_show(btnNew);
          GTK.gtk_toolbar_insert(tbApple, btnNew, 0);
                   
          final int btnOpen = GTK.gtk_tool_button_new(0, "打开");
          GTK.gtk_tool_button_set_stock_id(btnOpen, GTK.GTK_STOCK_OPEN);
          GTK.gtk_widget_show(btnOpen);
          GTK.gtk_toolbar_insert(tbApple, btnOpen, 1);

          GTK.g_signal_connect(btnOpen, "clicked",new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                System.out.println("hello");
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                GTK.gtk_widget_show(scrolledBar);
                                GTK.gtk_widget_set_size_request(scrolledBar, 400, 400);
                                //String temp = GTK.gtk_button_get_label(btnOpen);
                                selectFiles =  selectFile() ;
                                //isEnter = false;
                                showAllFiles(selectFiles, textview);
                        }
                }, null);

          int btnSave = GTK.gtk_tool_button_new(0, "保存");
          GTK.gtk_tool_button_set_stock_id(btnSave, GTK.GTK_STOCK_SAVE);
          GTK.gtk_widget_show(btnSave);
          GTK.gtk_toolbar_insert(tbApple, btnSave, 2);
          GTK.g_signal_connect(btnSave, "clicked",new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                saveFile(textview);
                        }
                }, null);
          
          
        }
}

```

treeview 简单的增加用户，调整treeview的样式，未实现删除
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

/**
* @author    叶昭良
* @time      2015年2月2日下午6:02:11
* @version   TestGTKTreeView V1.0  一个基本的treeview的使用流程  + 字段创建+记录添加
* @version   TestGTKTreeView V2.0  增加了遍历处理 以及显示某行某列的信息
* @version   TestGTKTreeView V3.0  增加了”列控制“ ！！treeview 的某列可显示，可拉伸 ，可排序，可调整
*                                  列控制只是改变treeview的表现，而不会改变liststore的model的内部值
* @version   TestGTKTreeView V4.0  增加了添加记录的功能         
* @version   TestGTKTreeView V5.0  增加了getSelection 进行行操作， 并未取出数据，也并未删除数据                 
*/ 
public class TestGTKTreeView
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        static int liststore;
        static int listiter;
        static boolean columnVisible = false; //用static 变量  不要用final变量。
        static boolean columnResize = true; //用static 变量  不要用final变量。
        static boolean columnRecordable = true; // 设置可拉动
        static int entryID;
        static int entryName;
        static int entryAge;
        static int treeViewApple;
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                GTK.gtk_init();
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                GTK.gtk_widget_show(window);
                GTK.gtk_window_set_title(window, "TreeView TestVersion");
                GTK.g_signal_connect(window, "destroy", new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                GTK.gtk_main_quit();
                        }
                }, null);
                int start = 0;
                //创建并添加布局
                gridHouse = GTK.gtk_grid_new();
                GTK.gtk_container_add(window, gridHouse);
                GTK.gtk_widget_show(gridHouse);
                //3  treeview   是一个界面，相当于是一个淘宝前台， V  V的作用
                treeViewApple = GTK.gtk_tree_view_new();
                //4 建立字段  比较费劲
                //4.1创建字段
                int columnID = GTK.gtk_tree_view_column_new_with_attributes("ID", GTK.gtk_cell_renderer_text_new(), 0);
                //4.2添加字段
                GTK.gtk_tree_view_append_column(treeViewApple, columnID);
                //4.3创建字段
                int columnName = GTK.gtk_tree_view_column_new_with_attributes("Names",GTK.gtk_cell_renderer_text_new(),1);
                //4.4添加字段
                GTK.gtk_tree_view_append_column(treeViewApple,columnName);
                //4.5创建字段
                int  columnAge = GTK.gtk_tree_view_column_new_with_attributes("Ages",GTK.gtk_cell_renderer_text_new(),2);
                GTK.gtk_tree_view_append_column(treeViewApple,columnAge);
                
                //创建控件
                //1  list_store  是一个数据结构库，相当于一个仓库  M model的作用
                liststore = GTK.gtk_list_store_new(3); //3代表三个字段：ID  Names   Age
        //        GTK.gtk_list_store_append(list_store, iter);
                //2  list_iter,list_iter是一个内部的数据迭代的控制过程，不需要显示 C的作用，控制
                listiter = GTK.gtk_tree_iter_new();

                //5  第五步 把 iter和listStore联系起来
                          //GTK.gtk_text_buffer_get_end_iter 类似于treeview的buffer和iter联系起来
                           // 以后就可以通过GTK.gtk_text_buffer_insert默认是一个一个iter的执行。
                GTK.gtk_list_store_append(liststore, listiter); //指针开始指到第一个位置
                // 6 逐个iter添加数据到liststore当中:每个iter的意思，就是逐行或者逐个记录。
                GTK.gtk_list_store_set_value(liststore, listiter, 0, "001");
                GTK.gtk_list_store_set_value(liststore, listiter, 1, "YinMuHuaDao");
                GTK.gtk_list_store_set_value(liststore, listiter, 2, "35");
                GTK.gtk_list_store_append(liststore, listiter); //如果不加入，指针不下移，只会覆盖掉前面的数据
                GTK.gtk_list_store_set_value(liststore, listiter, 0, "002");
                GTK.gtk_list_store_set_value(liststore, listiter, 1, "Taiyanghua");
                GTK.gtk_list_store_set_value(liststore, listiter, 2, "29");
                GTK.gtk_list_store_append(liststore, listiter);
                GTK.gtk_list_store_set_value(liststore, listiter, 0 ,"003");
                GTK.gtk_list_store_set_value(liststore, listiter, 1 ,"Xiaojun");
                GTK.gtk_list_store_set_value(liststore, listiter, 2 ,"10");
                // 之所以添加9次是因为  3个字段*3条记录 == 9
                
                //7  使用完iter之后一定要记得 关掉它
                GTK.gtk_tree_iter_free(listiter);
                
                //8 关键的一步，把数字显示出来
                GTK.gtk_tree_view_set_model(treeViewApple, liststore);
                
                //并同时把它显示出来，前台必须显示
                GTK.gtk_widget_show(treeViewApple);
                //最后添加到合租房中
                //GTK.gtk_grid_attach(gridHouse, treeViewApple, 0, 1, 3, 3);
                
                
                start = 1;
                createScrolledBar(gridHouse,start,treeViewApple);
                start = 4;
                createPrintButton(gridHouse,start);
                createLabel(gridHouse, start, "ID");
                //createEntry(entryID, gridHouse, start);
                entryID = GTK.gtk_entry_new();
                GTK.gtk_entry_set_max_length(entryID, 30);
                GTK.gtk_widget_show(entryID);
                GTK.gtk_grid_attach(gridHouse, entryID, 5, start, 1, 1);
                start = 5;
                createShowButton(gridHouse,start,1,1);
                createLabel(gridHouse, start, "名字");
                //createEntry(entryName, gridHouse, start);
                entryName = GTK.gtk_entry_new();
                GTK.gtk_entry_set_max_length(entryName, 30);
                GTK.gtk_widget_show(entryName);
                GTK.gtk_grid_attach(gridHouse, entryName, 5, start, 1, 1);
                start = 6 ; 
                createHideColumnButton(gridHouse,treeViewApple,start,1);
                createLabel(gridHouse, start, "年龄");
                //createEntry(entryAge, gridHouse, start);
                entryAge = GTK.gtk_entry_new();
                GTK.gtk_entry_set_max_length(entryAge, 30);
                GTK.gtk_widget_show(entryAge);
                GTK.gtk_grid_attach(gridHouse, entryAge, 5, start, 1, 1);
                start = 7 ; 
                createResizeColumnButton(gridHouse,treeViewApple,start,1);
                createInsertRecordButton(gridHouse,start);
                start = 8;
                createRecoredColumnButton(gridHouse,treeViewApple,start,1);
                start = 9;
                createSortColumnButton(gridHouse,treeViewApple,start,1);
                
                //加入行选择事件
                rowSelection();
                //3
                
                //添加控件
                
                //显示控件
                //GTK.gtk_widget_show(liststore);
                
                //启动循环
                GTK.gtk_main();
        }
        
        /**
         * 
         * @param gridHouse     网格对象布局的标识
         * @param start         网格布局的所处行数
         * @param treeview      treeview的对象标识
         */
        public static void createScrolledBar(int gridHouse,int start,int treeview)
        {
                int scrollbar = GTK.gtk_scrolled_window_new();
                GTK.gtk_container_add(scrollbar, treeview);
                GTK.gtk_widget_set_size_request(scrollbar, 200, 200);
                GTK.gtk_widget_show(scrollbar);
                GTK.gtk_grid_attach(gridHouse, scrollbar, 0, start, 3, 3);
        }
        /**
         * 
         * @param treeView       treeView对象标识
         * @param nth_columns    添加字段到第几列
         * @param column_name    字段的名字
         * @note  一般是一次创建记录
         */
        public static void createColumn(int treeView, int nth_columns, String column_name)
        {
                int temp_column = GTK.gtk_tree_view_column_new_with_attributes(column_name, GTK.gtk_cell_renderer_text_new(), nth_columns);
                GTK.gtk_tree_view_append_column(treeView, temp_column);
        }
        /**
         * 
         * @param ID      记录的ID号
         * @param Names   记录的名字
         * @param Age     记录的年龄
         * @note      需要多次创建记录
         */
        public static void createRecord(String ID, String Names, String Age,int listiter)
        {
                
                GTK.gtk_list_store_append(liststore, listiter); //指针下一继续添加的作用
                //append两个作用 1：添加 2：iter指针下移
                GTK.gtk_list_store_set_value(liststore, listiter, 0, ID);
                GTK.gtk_list_store_set_value(liststore, listiter, 1, Names);
                GTK.gtk_list_store_set_value(liststore, listiter, 2, Age);
                
        }
        
        public static void printRecord(int liststore)
        {
                //必须新建，不能利用原先的listiter,哪边需要用控制，就需要建一个控件。！！！否则报错！！不能用全局的 listiter
                int iter = GTK.gtk_tree_iter_new();
/*                if(GTK.gtk_tree_model_get_iter_first(liststore, listiter))
                {
                        do
                        {
                                String ID = GTK.gtk_tree_model_get_value(liststore, listiter, 0);
                                String Names = GTK.gtk_tree_model_get_value(liststore, listiter, 1);
                                String Ages = GTK.gtk_tree_model_get_value(liststore, listiter, 2);
                                //showInfo("这个员工的ID是"+ID+",他的名字是"+Names+",并且它的年龄是"+Ages,"显示员工信息");
                                System.out.println("这个员工的ID是"+ID+",他的名字是"+Names+",并且它的年龄是"+Ages);
                                
                        }while(GTK.gtk_tree_model_iter_next(liststore,listiter));
                }else
                {
                        System.out.println("表中无数据");
                }*/
                // 减少代码的深度。
                if(!GTK.gtk_tree_model_get_iter_first(liststore, iter))
                {
                        System.out.println("treeview没有数据  请添加记录");
                        return;
                }
                do
                {
                        String ID = GTK.gtk_tree_model_get_value(liststore, iter, 0);
                        String Names = GTK.gtk_tree_model_get_value(liststore, iter, 1);
                        String Ages = GTK.gtk_tree_model_get_value(liststore, iter, 2);
                        //showInfo("这个员工的ID是"+ID+",他的名字是"+Names+",并且它的年龄是"+Ages,"显示员工信息");
                        System.out.println("这个员工的ID是"+ID+",他的名字是"+Names+",并且它的年龄是"+Ages);
                        
                }while(GTK.gtk_tree_model_iter_next(liststore,iter));
                
                GTK.gtk_tree_iter_free(iter);//释放了 还是有错误。。。
        }
        
        public static void showInfo(  String  message,String title)
        {
                int msgDlg = GTK.gtk_message_dialog_new(0, GTK.GTK_DIALOG_DESTROY_WITH_PARENT|
                                GTK.GTK_DIALOG_MODAL,GTK.GTK_MESSAGE_INFO, GTK.GTK_BUTTONS_OK,message);
                GTK.gtk_window_set_title(msgDlg, title);
                int ret = GTK.gtk_dialog_run(msgDlg);
                 
                if( ret == GTK.GTK_RESPONSE_OK)
                {
                        GTK.gtk_widget_destroy(msgDlg);
                }else
                {
                        GTK.gtk_widget_destroy(msgDlg);
                }

        }
        public static boolean showWarning(  String  message,String title)
        {
                int msgDlg = GTK.gtk_message_dialog_new(0, GTK.GTK_DIALOG_DESTROY_WITH_PARENT|
                                GTK.GTK_DIALOG_MODAL,GTK.GTK_MESSAGE_WARNING, GTK.GTK_BUTTONS_OK,message);
                GTK.gtk_window_set_title(msgDlg, title);
                int ret = GTK.gtk_dialog_run(msgDlg);
                GTK.gtk_widget_destroy(msgDlg);
                return ret == GTK.GTK_RESPONSE_OK;
        }
        public static void createPrintButton(int gridHouse,int start)
        {
                int btnApple = GTK.gtk_button_new_with_label("遍历");
                GTK.gtk_widget_show(btnApple);
                GTK.gtk_grid_attach(gridHouse, btnApple, 0, start, 1, 1);
                insertButtonEvent(btnApple);
        }
        public static void createShowButton(int gridHouse,int start,int row, int column)
        {
                int btnApple = GTK.gtk_button_new_with_label("获取表第"+row+"行"+"第"+column+"列");
                GTK.gtk_widget_show(btnApple);
                GTK.gtk_grid_attach(gridHouse, btnApple, 0, start, 1, 1);
                insertShowEvent(btnApple,row,column);
        }
        public static void createHideColumnButton(int gridHouse,int treeViewApple,int start,int nthcolumn)
        {
                int btnApple = GTK.gtk_button_new_with_label("隐藏|显示第"+nthcolumn+"列");
                GTK.gtk_widget_show(btnApple);
                GTK.gtk_grid_attach(gridHouse, btnApple, 0, start, 1, 1);
                insertHideEvent(treeViewApple, btnApple, nthcolumn);
        }
        
        public static void createResizeColumnButton(int gridHouse,int treeViewApple,int start,int nthcolumn)
        {
                int btnApple = GTK.gtk_button_new_with_label("赋予第"+nthcolumn+"列可调整大小的权限");
                GTK.gtk_widget_show(btnApple);
                GTK.gtk_grid_attach(gridHouse, btnApple, 0, start, 1, 1);
                insertResizeEvent(treeViewApple, btnApple, nthcolumn);
        }
        public static void createRecoredColumnButton(int gridHouse,int treeViewApple,int start,int nthcolumn)
        {
                int btnApple = GTK.gtk_button_new_with_label("赋予第"+nthcolumn+"列可移动的权限");
                GTK.gtk_widget_show(btnApple);
                GTK.gtk_grid_attach(gridHouse, btnApple, 0, start, 1, 1);
                insertRecordableEvent(treeViewApple, btnApple, nthcolumn);
        }
        public static void createSortColumnButton(int gridHouse,int treeViewApple,int start,int nthcolumn)
        {
                int btnApple = GTK.gtk_button_new_with_label("赋予第"+nthcolumn+"列可排序的权限");
                GTK.gtk_widget_show(btnApple);
                GTK.gtk_grid_attach(gridHouse, btnApple, 0, start, 1, 1);
                insertSortEvent(treeViewApple, btnApple, nthcolumn);
        }
        public static void createInsertRecordButton(int gridHouse,int start)
        {
                int btnApple = GTK.gtk_button_new_with_label("插入数据");
                GTK.gtk_widget_show(btnApple);
                GTK.gtk_grid_attach(gridHouse, btnApple, 5, start, 1, 1);
                insertRecordEvent(btnApple);
        }
        public static void createLabel(int gridHouse, int start,String labelName)
        {
                int labelApple = GTK.gtk_label_new(labelName);
                GTK.gtk_widget_show(labelApple);
                GTK.gtk_grid_attach(gridHouse, labelApple, 4, start, 1, 1);
                
        }
        public static void createEntry(int entryApple,int gridHouse, int start)
        {
                entryApple = GTK.gtk_entry_new();
                GTK.gtk_entry_set_max_length(entryApple, 30);
                GTK.gtk_widget_show(entryApple);
                GTK.gtk_grid_attach(gridHouse, entryApple, 5, start, 1, 1);
        }
        public static void insertButtonEvent(final int btnSin)
        {
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                printRecord(liststore);
                        }
                }, null);
        }
        public static void insertShowEvent(final int btnSin,final int row, final int column)
        {
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                showColumnRow(row,column);
                        }
                }, null);
        }
        public static void insertHideEvent(final int treeViewApple,final int btnSin, final int nthcolumn)
        {
                //final boolean columnVisible = true;
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                tree_view_column_set_hide(treeViewApple,nthcolumn,columnVisible);
                                columnVisible = !columnVisible;
                        }
                }, null);
        }
        public static void insertResizeEvent(final int treeViewApple,final int btnSin, final int nthcolumn)
        {
                //final boolean columnVisible = true;
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                tree_view_column_resize(treeViewApple,nthcolumn,columnResize);
                                columnResize = !columnResize;
                        }
                }, null);
        }
        
        public static void insertRecordableEvent(final int treeViewApple,final int btnSin, final int nthcolumn)
        {
                //final boolean columnVisible = true;
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                tree_view_column_recordabel(treeViewApple,nthcolumn,columnRecordable);
                                columnRecordable = !columnRecordable;
                        }
                }, null);
        }
        public static void insertSortEvent(final int treeViewApple,final int btnSin, final int nthcolumn)
        {
                //final boolean columnVisible = true;
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //double sbValue = GTK.gtk_spin_button_get_value(spinbox);
                                tree_view_column_sort(treeViewApple,nthcolumn,1);
                                //columnRecordable = !columnRecordable;
                        }
                }, null);
        }
        public static void insertRecordEvent(final int btnSin)
        {
                //final boolean columnVisible = true;
                GTK.g_signal_connect(btnSin, "clicked",new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                String ID = GTK.gtk_entry_get_text(entryID);
                                System.out.println(ID);
                                if(ID.equalsIgnoreCase(""))
                                {
                                        showWarning("ID为空，请重新输入","警告");
                                        return;
                                }
                                String Name = GTK.gtk_entry_get_text(entryName);
                                if(Name.equalsIgnoreCase(""))
                                {
                                        showWarning("Name为空，请重新输入","警告");
                                        return;
                                }
                                String Age = GTK.gtk_entry_get_text(entryAge);
                                if(Age.equalsIgnoreCase(""))
                                {
                                        showWarning("Age为空，请重新输入","警告");
                                        return;
                                }
                                insertRecord(ID,Name, Age, treeViewApple);
                                 
                                
                        }
                }, null);
        }
        public static void insertRecord(String ID,String Name,String Age,int treeViewApple)
        {
                int iter = GTK.gtk_text_iter_new();
                if(!GTK.gtk_tree_model_get_iter_first(liststore, iter))
                {
                        createRecord( ID,  Name,  Age,iter);
                }
                else
                {
                        do
                        {
                                //System.out.println("hello");
                        }while(GTK.gtk_tree_model_iter_next(liststore, iter));
                        //while(GTK.gtk_tree_model_iter_next(liststore, iter)); 
                        // 记一次调bug经理，，经常把iter_next写成了 iter_first导致不断的循环
                        //出现无限循环，最后嵌入 system打印出来即可。
                        createRecord( ID,  Name,  Age,iter);
                }
                GTK.gtk_text_iter_free(iter);
                GTK.gtk_tree_view_set_model(treeViewApple, liststore);
        }
        public static void showColumnRow(int row, int column)
        {
                int i = 0;
                if(0 == getNumberOfListstore(liststore))
                {
                        System.out.println("100块都不给 还想查！");
                        return ;
                }
                if(row > getNumberOfListstore(liststore))
                {
                        System.out.println("记录数没有！  请重新查询");
                        return;
                }
                int iter  = GTK.gtk_tree_iter_new();
                GTK.gtk_tree_model_get_iter_first(liststore, iter);
                do
                {
                        if(i == row)
                        {
                                String temp = GTK.gtk_tree_model_get_value(liststore, iter, column);
                                System.out.println(temp);
                                showInfo(temp,"显示信息");
                                return;
                        }
                        i++;
                }while(GTK.gtk_tree_model_iter_next(liststore, iter));
                
        }
        public static int getNumberOfListstore(int liststore)
        {
                int sum = 0;
                int iter = GTK.gtk_tree_iter_new();
                if(!GTK.gtk_tree_model_get_iter_first(liststore, iter))
                {
                        return sum;
                }
                do
                {
                        sum++;
                }while(GTK.gtk_tree_model_iter_next(liststore, iter));
                return sum;
        }
        
        public static void tree_view_column_set_hide(int treeViewApple,int nthColumn,boolean visible)
        {
                int column =GTK.gtk_tree_view_get_column(treeViewApple, nthColumn);
                GTK.gtk_tree_view_column_set_visible(column, visible);
        }
        public static void tree_view_column_resize(int treeViewApple,int nthColumn,boolean visible)
        {
                int column =GTK.gtk_tree_view_get_column(treeViewApple, nthColumn);
                GTK.gtk_tree_view_column_set_resizable(column, visible);
        }
        public static void tree_view_column_recordabel(int treeViewApple,int nthColumn,boolean visible)
        {
                int column =GTK.gtk_tree_view_get_column(treeViewApple, nthColumn);
                GTK.gtk_tree_view_column_set_reorderable(column, visible);
        }
        public static void tree_view_column_sort(int treeViewApple,int nthColumn,int sortcolumnid)
        {
                int column =GTK.gtk_tree_view_get_column(treeViewApple, nthColumn);
                GTK.gtk_tree_view_column_set_sort_column_id(column, sortcolumnid);
        }
        
        public static void rowSelection()
        {
                int selection = GTK.gtk_tree_view_get_selection(treeViewApple);
                GTK.gtk_tree_selection_set_mode(selection, GTK.GTK_SELECTION_MULTIPLE);
                GTK.g_signal_connect(treeViewApple, "button-press-event", new IGCallBack()
                { 
                 @Override
                 public void execute(int instance, int eventData, Object object)
                 {
                  if(GTK.gdk_event_get_type(eventData)==GTK.GDK_2BUTTON_PRESS)
                  {
                   int[] indices = GTK.gtk_tree_view_get_selection_indices(treeViewApple);
                   System.out.println(indices[0]);
                  }
                 }
                }, null);
        }
        
/*        public static void deleteOneRecore()
        {
                int selection = GTK.gtk_tree_view_get_selection(treeViewApple);
                
        }*/
}

```

case3 工具栏+进度条+menu+statusIcon+开关按钮+calendar
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

/**
* @author    叶昭良
* @time      2015年2月3日上午1:24:01
* @version   TestToolBar V1.0
* @version   TestToolBar V2.0  增加了一个日历控件,添加了天数变化时间和双击时间
* @version   TestToolBar V2.0  增加了进度条的创建，和按钮事件 控制进度条
* @version   TestToolBar V4.0  建议的开关按钮，未实现信号的接受。。。
* @version   TestToolBar V5.0  增加了一个菜单控件
* @version   TestToolBar V6.0  增加了一个StatusIcon的简单控件        
*/
public class TestToolBar
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        static double progress = 0.0;
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                GTK.gtk_init();
                window= GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                GTK.gtk_widget_show(window);
                GTK.g_signal_connect(window, "destroy", new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                GTK.gtk_main_quit();
                        }
                }, null);
                
                //设置网格布局
                gridHouse = GTK.gtk_grid_new();
                GTK.gtk_widget_show(gridHouse);
                GTK.gtk_container_add(window, gridHouse);
                
                //定义一些常用的控件
                int start = 0;
                createToolbar(gridHouse,start);
                start =1;
                createCalendar(gridHouse,start);
                start = 2;
                createProgressBar(gridHouse, start);
                start = 3;
                createSwitchOn(gridHouse, start);
                start = 4;
                createMenu(gridHouse,start);
                start = 5;
                createStatusIcon(gridHouse,start);
                //启动循环
                GTK.gtk_main();
        }
        
        public static void createToolbar(int gridHouse,int start)
        {
                //创建工具栏容器
                int tbApple = GTK.gtk_toolbar_new();
                GTK.gtk_widget_show(tbApple);
                GTK.gtk_widget_set_size_request(tbApple, 300, 20);
                GTK.gtk_grid_attach(gridHouse, tbApple, 0, start, 1, 1);
                
          int btnNew = GTK.gtk_tool_button_new(0, "新建");
          GTK.gtk_tool_button_set_stock_id(btnNew, GTK.GTK_STOCK_NEW);
          GTK.gtk_widget_show(btnNew);
          GTK.gtk_toolbar_insert(tbApple, btnNew, 0);
                   
          int btnOpen = GTK.gtk_tool_button_new(0, "打开");
          GTK.gtk_tool_button_set_stock_id(btnOpen, GTK.GTK_STOCK_OPEN);
          GTK.gtk_widget_show(btnOpen);
          GTK.gtk_toolbar_insert(tbApple, btnOpen, 1);
          
          int btnSave = GTK.gtk_tool_button_new(0, "保存");
          GTK.gtk_tool_button_set_stock_id(btnSave, GTK.GTK_STOCK_SAVE);
          GTK.gtk_widget_show(btnSave);
          GTK.gtk_toolbar_insert(tbApple, btnSave, 2);
        }
        
        public static void createCalendar(int gridHouse, int start) 
        {
                final int clApple = GTK.gtk_calendar_new();
                int labelApple = GTK.gtk_label_new("");
                GTK.gtk_widget_show(clApple);
                GTK.gtk_widget_show(labelApple);
                GTK.gtk_grid_attach(gridHouse, clApple, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, clApple, 1, start, 1, 1);
                GTK.gtk_widget_set_size_request(clApple, 200, 200);
                
                GTK.g_signal_connect(clApple, "day-selected", new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                int day = GTK.gtk_calendar_get_day(clApple);
                                int month = GTK.gtk_calendar_get_month(clApple);
                                int year = GTK.gtk_calendar_get_year(clApple);
                                System.out.println("用户选择了"+year+"年"+month+"月"+day+"日");
                                //showInfo("用户选择了"+year+"年"+month+"月"+day+"日","选择的日期");
                        }
                }, null);
                GTK.g_signal_connect(clApple, "day-selected-double-click", new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                int day = GTK.gtk_calendar_get_day(clApple);
                                int month = GTK.gtk_calendar_get_month(clApple);
                                int year = GTK.gtk_calendar_get_year(clApple);
                                showInfo("用户选择了"+year+"年"+month+"月"+day+"日","选择的日期");
                        }
                }, null);
        }
        public static boolean showInfo(  String  message,String title)
        {
                int msgDlg = GTK.gtk_message_dialog_new(0, GTK.GTK_DIALOG_DESTROY_WITH_PARENT|
                                GTK.GTK_DIALOG_MODAL,GTK.GTK_MESSAGE_INFO, GTK.GTK_BUTTONS_OK,message);
                GTK.gtk_window_set_title(msgDlg, title);
                int ret = GTK.gtk_dialog_run(msgDlg);
                GTK.gtk_widget_destroy(msgDlg);
                return ret == GTK.GTK_RESPONSE_OK;
        }
        
        public static void createProgressBar(int gridHouse,int start)
        {
                final int pbApple =GTK.gtk_progress_bar_new();
                int btnApple =GTK.gtk_button_new_with_label("加快进度");
                GTK.gtk_widget_show(btnApple);
                GTK.gtk_widget_show(pbApple);
                GTK.gtk_grid_attach(gridHouse, pbApple, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, btnApple, 1, start, 1, 1);
                
                GTK.gtk_progress_bar_set_fraction(pbApple, progress);
                        //GTK.gtk_progress_bar_set_pulse_step()
/*                        try
                        {
                                Thread.sleep(1000);
                        } catch (InterruptedException e)
                        {
                                // TODO Auto-generated catch block
                                e.printStackTrace();
                        }*/
                GTK.g_signal_connect(btnApple, "clicked", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                if(progress <= 1.0)
                                {
                                        progress = progress + 0.2;
                                        GTK.gtk_progress_bar_set_fraction(pbApple, progress);
                                }else
                                {
                                        System.out.println("stop clicked");
                                }
                                
                        }
                }, null);
                
                GTK.gtk_progress_bar_set_text(pbApple, "请稍后");
        }
        
        public static void createSwitchOn(int gridHouse, int start)
        {
                final int switchOff = GTK.gtk_switch_new();
                final int label = GTK.gtk_label_new("");
                GTK.gtk_widget_show(switchOff);
                GTK.gtk_widget_show(label);
                GTK.gtk_grid_attach(gridHouse, switchOff, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, label, 1, start, 1, 1);
                GTK.gtk_switch_set_active(switchOff, true);
                //GTK.g_signal_connect(switchOff, "state-set", new IGCallBack()
                
                GTK.g_signal_connect(switchOff, "activate", new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                System.out.println("hello");
                                        if(GTK.gtk_switch_get_active(switchOff))
                                        {
                                                //GTK.gtk_label_set_text(label, "你打开了");
                                                System.out.println("hello");
                                        }
                                        else
                                        {
                                                //GTK.gtk_label_set_text(label, "你关闭了");
                                                System.out.println("fuck");
                                        }
                        }
                }, null);

        }
        public static void createMenu(int gridHouse, int start)
        {
                //分三步
                //1创建单一菜单
                //2创建一个菜单头
                //3创建总的菜单条，把许多菜单头都挂到菜单条上
                //创建一个菜单
                int menu = GTK.gtk_menu_new();
                
                int file = GTK.gtk_menu_item_new_with_label("_F文件");
                int new1  = GTK.gtk_menu_item_new_with_label("新建");
                int open1 = GTK.gtk_menu_item_new_with_label("打开");
                int closed = GTK.gtk_menu_item_new_with_label("关闭");
                int import1 = GTK.gtk_menu_item_new_with_mnemonic("_Import导入");
                //int image = GTK.gtk_menu_tool_button_new_from_stock(GTK.GTK_STOCK_NEW);
                //fengexian 
                GTK.gtk_widget_show(menu);
                GTK.gtk_widget_show(new1);
                GTK.gtk_widget_show(open1);
                GTK.gtk_widget_show(closed);
                GTK.gtk_widget_show(file);
                GTK.gtk_widget_show(import1);
                
                GTK.gtk_menu_shell_append(menu, file);
                GTK.gtk_menu_shell_append(menu, new1);
                GTK.gtk_menu_shell_append(menu, open1);
                GTK.gtk_menu_shell_append(menu, closed);
                GTK.gtk_menu_shell_append(menu, import1);
                // 菜单条
                int menubar = GTK.gtk_menu_bar_new();
                GTK.gtk_widget_show(menubar);
                GTK.gtk_grid_attach(gridHouse, menubar, 0, start, 1, 1);
                
                //2 添加一个menu 到menu小头
                int file_item =  GTK.gtk_menu_item_new_with_label("File");
                GTK.gtk_widget_show(file_item);
                GTK.gtk_menu_item_set_submenu(file_item, menu);
                
                //3 添加一个 menu小头到menubar
                GTK.gtk_menu_shell_append(menubar, file_item);
//                GTK.gtk_menu_attach_to_widget(menubar, attach_widget);
                GTK.g_signal_connect(closed, "activate",new IGCallBack(){
                                @Override
                                public void execute(int instance, int eventData, Object object)
                                {
                                        // TODO Auto-generated method stub
                                        GTK.gtk_main_quit();
                                }
                        }, null);
                }
        
        public static void createStatusIcon(int gridHouse,int start)
        {
                int status = GTK.gtk_status_icon_new();
                //GTK.gtk_widget_show(status);
                //GTK.gtk_grid_attach(gridHouse, status, 0, status, 1, 1);
                GTK.gtk_status_icon_set_from_stock(status, GTK.GTK_STOCK_CONNECT);
                GTK.gtk_status_icon_set_tooltip_text(status, "hello"); //的确是会显示在下方
                GTK.gtk_status_icon_set_visible(status, true);
                
                GTK.g_signal_connect(status, "popup-menu", new IGCallBack() {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
/*                                int menu = GTK.gtk_menu_new();
                                
                                int file = GTK.gtk_menu_item_new_with_label("_F文件");
                                int new1  = GTK.gtk_menu_item_new_with_label("新建");
                                int open1 = GTK.gtk_menu_item_new_with_label("打开");
                                int closed = GTK.gtk_menu_item_new_with_label("关闭");
                                int import1 = GTK.gtk_menu_item_new_with_mnemonic("_Import导入");
                                //int image = GTK.gtk_menu_tool_button_new_from_stock(GTK.GTK_STOCK_NEW);
                                //fengexian 
                                GTK.gtk_widget_show(menu);
                                GTK.gtk_widget_show(new1);
                                GTK.gtk_widget_show(open1);
                                GTK.gtk_widget_show(closed);
                                GTK.gtk_widget_show(file);
                                GTK.gtk_widget_show(import1);
                                
                                GTK.gtk_menu_shell_append(menu, file);
                                GTK.gtk_menu_shell_append(menu, new1);
                                GTK.gtk_menu_shell_append(menu, open1);
                                GTK.gtk_menu_shell_append(menu, closed);
                                GTK.gtk_menu_shell_append(menu, import1);
                                GTK.gtk_menu_popup(menu);*/
                                showInfo("hello","wenho");
                        }
                }, null);
/*                GTK.g_signal_connect(status, "statusIconActivate", new IGCallBack() {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                showInfo("hello","wenho");
                          
                        }},null);*/
        
        }
}
```
![MVC交互图](/images/java/MVC.gif)
![简易记事本+工具栏](/images/java/jianyijishi.gif)
![工具栏+menu+statusICON+prograssBar+switchOFF+calendar](/images/java/yang.gif)

