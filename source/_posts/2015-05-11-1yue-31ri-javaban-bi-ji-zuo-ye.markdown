---
layout: post
title: "1月31日Java班笔记作业"
date: 2015-05-11 14:58:40 +0800
comments: true
categories: JavaBasic
---
<!--more-->


第一部分：笔记
First: SpinBox
背景：spinButton 控制了文本框的数字，所以不需要研究entry的数字限制形式

第一点：微调按钮（SpinButton）从GtkEntry继承，只能输入数字。
基本函数使用说明：
1.         int gtk_spin_button_new_with_range(double  min,double  max,double  step)  创建实例。min最小值、max最大值、单步变化值。double可以用来表示小数。
2.        double gtk_spin_button_get_value(int spin_button)  获得double类型的值。
int gtk_spin_button_get_value_as_int(int spin_button) 以整数类型获得值，小数部分忽略。
void gtk_spin_button_set_value(int spin_button,double value)  设置值

学会：解决问题的能力。1 .查文档  2.google+baidu  3.FAQ

Second: ComboBox
第一点：GtkComboBoxText从GtkComboBox继承，GtkComboBoxText子类定义ComboBox.
基本函数：(使用说明)
第一步： int gtk_combo_box_text_new()创建一个ComboBoxText；
第二部： void gtk_combo_box_text_append(int combo_box,String id, String text) 附加一个文本，并且给它设定一个自定义id（String类型）。
第三步：
String gtk_combo_box_get_active_id(int combo_box)得到选中行的自定义id
gtk_combo_box_set_active_id(int combo_box,String active_id)设定自定义id等于active_id的项被选中
void gtk_combo_box_text_remove_all(int combo_box)清除所有内容
第四步  当选中一行的时候会发出“changed”信号 GTK.g_signal_connect(….);

Third: GTKIMAGE
第一点：GtkImage 可以分为三种方式加载图片资源，包括：磁盘文件（file），项目资源（resource），内置图片（STOCK），对应三个
家在函数gtk_image_set_from_file，gtk_image_set_from_resource，gtk_image_set_from_stock。

基本函数使用说明：
1.        void gtk_image_set_from_file(int image, String filename)从文件全路径为filename的文件中加载图片。注意文件路径的转义问题，“\\”因为“\”有特殊含义，后面讲字符串会详细讲。

2.        void gtk_image_set_from_resource(int image, String resName)从项目资源中加载图片，格式"com/rupeng/1.jpg"（其中com/rupeng表示com.rupeng包），不要以/开头，把资源放在src里面！！！
png可以背景透明，jpg、jpege压缩图片, Gif  动态图片

3.        GTK.gtk_image_new_from_stock(GTK.GTK_STOCK_ABOUT, GTK.GTK_ICON_SIZE_LARGE_TOOLBAR); 从GTK的内置图片载入图片。
设置按钮图片一般是    GTK.GTK_STOCK*
设置大小的时候  利用GTK.GTK_ICON就可以弹出几个。。。

Fourth: GTKIMAGE的简单调研
        /**
         * 把资源resName保存到临时文件夹，返回值为临时文件的全路径
         * 
         * @param resName
         *            com/rupeng/a.mp3这样的格式，注意不以/开头
         * @return
         */
        public synchronized static String saveResourceToTemp(String resName)
        {
                // 如果resname之前释放到过临时文件，并且临时文件还存在，则不再重复释放，直接返回之前的文件，以提高性能
                if (resourceCache.containsKey(resName)) //资源缓存中判断是否存在图片
                {
                        String cachedFileName = resourceCache.get(resName);//从资源缓存中获取图片
                        if (new File(cachedFileName).exists())
                        {
                                return cachedFileName;
                        } else
                        {
                                resourceCache.remove(resName);
                        }
                }
                // 获取文件扩展名
                String[] strs = resName.split("\\."); //利用字符串的split函数
                String extesion = strs[strs.length - 1]; //获取最后一个

                InputStream inStream = GTK.class.getClassLoader().getResourceAsStream(resName);//ClassLoader.getSystemResourceAsStream(resName);
//通过反射机制 自动载入图片资源的ＩｎｐｕｔＳｔｒｅａｍ ！！！利用文件名字，这也是一个悬念        
        if (inStream == null)
                {
                        throw new RuntimeException("找不到资源" + resName);
                }
                try
                {                        
                        //java 1.6后，运行在Windows下，如果启用了Guest账户，则由于createTempFile调用了SecureRandom 会卡5秒钟
                        //因此第一次运行createTempFile会非常慢
                        //参考：http://stackoverflow.com/questions/2608763/why-does-first-call-to-java-io-file-createtempfilestring-string-file-take-5-se
                        //File tempFile = File.createTempFile("temp", "." + extesion);
                        //因此不用createTempFile，改用存到项目的restemp文件夹下
                        File resDir = new File(System.getProperty("user.dir"), "gtk/temp");// *.dll放的文件夹
                        if (!resDir.exists())
                        {
                                resDir.mkdirs();//创建临时文件夹
                        }
                        inStream.mark(Integer.MAX_VALUE);　／／不清楚作何用
                        String md5 = Utils.getMD5(inStream);//用md5值做文件名
                        inStream.reset();//指针复位到mark标记的位置，便于后面保存文件        
                        
                        // 如果不加扩展名，mci_send_command则无法识别文件类型
                        File tempFile = new File(resDir,md5+"."+extesion); //创建一个File
                        FileOutputStream outStream = new FileOutputStream(tempFile); //写出一个文件
                        Utils.copy(inStream,outStream); //把当前的图片资源靠背出去
                        inStream.close();
                        outStream.close();
                        
                        resourceCache.put(resName, tempFile.getAbsolutePath());//放到绝对路径下
                        return tempFile.getAbsolutePath(); //最终返回图片
                } catch (IOException e)
                {
                        throw new RuntimeException(e);
                }
        }

        /**
         * 从java资源中加载资源图片生成image。不是gtk提供的native方法
         * 
         * @param resName
         * @return
         */
        public static int gtk_image_new_from_resource(String resName)
        {
                return gtk_image_new_from_file(saveResourceToTemp(resName));
复制代码



Fifth: TextView
背景：先前entry是当行文本框
TextView基本函数使用说明：
第一步： 创建多行文本夹
            int gtk_text_view_new()创建多行文本
第二步： 进行接本的设置
          void gtk_text_view_set_wrap_mode(int text_view,int wrap_mode) 设置自动换行模式。wrap_mode可选值：GTK_WRAP_NONE(不自动换行)；GTK_WRAP_CHAR(在任意字符换行)；GTK_WRAP_WORD(保持单词完整性换行，)；GTK_WRAP_WORD_CHAR (尽量保持单词完整性，实在不行也可以在任意字符换行)

第三步：  添加文字和读取文字
        读取文字
        String text = GTK.gtk_text_buffer_get_text(textbuffer);
   添加文字
GTK.gtk_text_buffer_set_text(textbuffer, loveWords)
额外的标注：
      一般第三步采用的方式适合比较小的String，对于较大的情况可以再加一个TextIter类似可以在多行文本或者对话框
添加一个滚动条ScrolledBar
TextIter使用说明：
1：TextIter是对TextBuffer遍历的遍历器。
2： int gtk_text_iter_new()创建一个空的TextIter，使用完后用gtk_text_iter_free(int iter)释放。
3： void gtk_text_buffer_get_start_iter(int buffer,int iter)用buffer这个TextBuffer的开始位置去初始化iter这个TextIter（开头插入  或者用backward指针  向后读）
     void gtk_text_buffer_get_end_iter(int buffer, int iter)用buffer这个TextBuffer的结束位置去初始化iter这个TextIter（结尾插入  或者也可以用forward指针 向前读）
     (*)用gtk_text_buffer_get_iter_*系列方法移动迭代器的位置。
4.   void gtk_text_buffer_insert(int buffer, int iter,String text)在buffer这个TextBuffer的iter这个TextIter的当前位置插入文本text

5          gtk_text_iter_free(int iter)  释放textIter资源


Sixth: Cairo
1： 画布板   是一个框架，一般仅仅在第一步创建的时候使用 GTK.gtk_drawing_area();
2： 画布环境(Cairo_t)，缺不了，在java的Cairo中直接在GTK.g_signal_connect中的draw时间中使用，对应execute函数的
eventsData 即使Cairo_t ,基本上每一个Cairo函数都少不了ct
3:  com.rupeng.gtk4j.Cairo简化了绘画的步骤，一般是
3.1        设置画笔的大小  Cairo.cairo_set_font…
3.2        设置画笔的颜色
3.3        Cairo.cairo_move_to(cr…)
3.4        Cairo.cairo_line_to(cr,,,)画直线
3.5        Cairo.cairo_arc(cr,…)画圆弧   记住最好是分段绘制也就是每画一个加入 GTK.gtk_stroke() 或者GTK.gtk_fill();
3.6        Cairo.cairo_rec  画矩形
3.7        文字的绘制
3.7.1        //画文字
3.7.2        Cairo.cairo_move_to(eventData, 180, 260);
3.7.3        Cairo.cairo_set_source_rgb(eventData, 0.4, 0.3, 0.2);
3.7.4        Cairo.cairo_set_font_size(eventData, 16);
3.7.5        //选择文字类型
3.7.6        Cairo.cairo_select_font_face(eventData, "宋体", Cairo.CAIRO_FONT_SLANT_ITALIC, Cairo.CAIRO_FONT_WEIGHT_BOLD);
3.7.7        Cairo.cairo_show_text(eventData,"三毛的自画像");
3.7.8        //Cairo.cairo_stroke(eventData);  //不需要这句话也可以

第二部分：作业
First: SpinBox 的使用
case1: 使用控件
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

/**
* @author 叶昭良
* @version SpinBoxv1.0
*/
public class TestGTKSPinBox
{

        /**
         * @param args
         *  
         */
        static int window;
        static int gridHouse;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                GTK.gtk_init();
                window  = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                GTK.gtk_window_set_title(window, "测试SpinBox");
                GTK.g_signal_connect(window, "destroy", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                }, null);
                GTK.gtk_widget_show(window);
                
                
                //开始建立布局
                 gridHouse = GTK.gtk_grid_new();
                GTK.gtk_widget_show(gridHouse);
                //开始插入控件
                int start = 0 ;
                createSpinbox(window,gridHouse,start);
                
                GTK.gtk_container_add(window, gridHouse);
                GTK.gtk_main();
        }
        /**
         * 
         * @param window     窗口标识
         * @param gridHouse  整租房标识
         * @param start      整租房的起始位置
         */
        public static void createSpinbox(int window,int gridHouse,int start)
        {
                //定义控件
                int label1 = GTK.gtk_label_new("数字：0-9");
                int label2 = GTK.gtk_label_new("范围: -10~10");
                final int labelSpinbox = GTK.gtk_label_new("");
                final int labelSpinboxApple = GTK.gtk_label_new("");
                final int spinbox = GTK.gtk_spin_button_new_with_range(0, 9, 1);
                final int spinboxApple = GTK.gtk_spin_button_new_with_range(-10, 10, 0.1);
                
        
                //添加控件到整租房间
                GTK.gtk_grid_attach(gridHouse, label1, 0, start, 1, 1); 
                GTK.gtk_grid_attach(gridHouse, spinbox, 1, start, 1, 1); 
                GTK.gtk_grid_attach(gridHouse, label2, 0, start+1, 1, 1); 
                GTK.gtk_grid_attach(gridHouse, spinboxApple, 1, start+1, 1, 1);
                
                GTK.gtk_grid_attach(gridHouse, labelSpinbox, 0, start+3, 1, 1);
                GTK.gtk_grid_attach(gridHouse, labelSpinboxApple, 0, start+4, 1, 1);
                //显示控件
                GTK.gtk_widget_show(label1);
                GTK.gtk_widget_show(spinbox);
                GTK.gtk_widget_show(label2);
                GTK.gtk_widget_show(spinboxApple);
                GTK.gtk_widget_show(labelSpinbox);
                GTK.gtk_widget_show(labelSpinboxApple);
                GTK.gtk_widget_show(gridHouse);
                
                
                //添加事件
                GTK.g_signal_connect(spinbox, "changed", new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                double apple = GTK.gtk_spin_button_get_value(spinbox);
                                GTK.gtk_label_set_text(labelSpinbox, "您当前选择的数字式："+Double.toString(apple));
                                
                        }
                }, null);
                
                GTK.g_signal_connect(spinboxApple, "changed", new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                double apple = GTK.gtk_spin_button_get_value(spinboxApple);
                                GTK.gtk_label_set_text(labelSpinboxApple, "您当前选择的数字式："+Double.toString(apple));
                                
                        }
                }, null);
        }

}

```

case2 ：建议四则运算器，加上指数和取余
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

/**
* @author 叶昭良
* @version SpinBoxv1.0
*/
public class TestGTKSPinBox
{

        /**
         * @param args
         *  
         */
        static int window;
        static int gridHouse;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                GTK.gtk_init();
                window  = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                GTK.gtk_window_set_title(window, "测试SpinBox");
                GTK.g_signal_connect(window, "destroy", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                }, null);
                GTK.gtk_widget_show(window);
                
                
                //开始建立布局
                 gridHouse = GTK.gtk_grid_new();
                GTK.gtk_widget_show(gridHouse);
                //开始插入控件
                int start = 0 ;
                createSpinbox(window,gridHouse,start);
                
                GTK.gtk_container_add(window, gridHouse);
                GTK.gtk_main();
        }
        /**
         * 
         * @param window     窗口标识
         * @param gridHouse  整租房标识
         * @param start      整租房的起始位置
         */
        public static void createSpinbox(int window,int gridHouse,int start)
        {
                //定义控件
                int label1 = GTK.gtk_label_new("数字：0-9");
                int label2 = GTK.gtk_label_new("范围: -10~10");
                final int labelSpinbox = GTK.gtk_label_new("");
                final int labelSpinboxApple = GTK.gtk_label_new("");
                final int spinbox = GTK.gtk_spin_button_new_with_range(0, 9, 1);
                final int spinboxApple = GTK.gtk_spin_button_new_with_range(-10, 10, 0.1);
                
        
                //添加控件到整租房间
                GTK.gtk_grid_attach(gridHouse, label1, 0, start, 1, 1); 
                GTK.gtk_grid_attach(gridHouse, spinbox, 1, start, 1, 1); 
                GTK.gtk_grid_attach(gridHouse, label2, 0, start+1, 1, 1); 
                GTK.gtk_grid_attach(gridHouse, spinboxApple, 1, start+1, 1, 1);
                
                GTK.gtk_grid_attach(gridHouse, labelSpinbox, 0, start+3, 1, 1);
                GTK.gtk_grid_attach(gridHouse, labelSpinboxApple, 0, start+4, 1, 1);
                //显示控件
                GTK.gtk_widget_show(label1);
                GTK.gtk_widget_show(spinbox);
                GTK.gtk_widget_show(label2);
                GTK.gtk_widget_show(spinboxApple);
                GTK.gtk_widget_show(labelSpinbox);
                GTK.gtk_widget_show(labelSpinboxApple);
                GTK.gtk_widget_show(gridHouse);
                
                
                //添加事件
                GTK.g_signal_connect(spinbox, "changed", new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                double apple = GTK.gtk_spin_button_get_value(spinbox);
                                GTK.gtk_label_set_text(labelSpinbox, "您当前选择的数字式："+Double.toString(apple));
                                
                        }
                }, null);
                
                GTK.g_signal_connect(spinboxApple, "changed", new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                double apple = GTK.gtk_spin_button_get_value(spinboxApple);
                                GTK.gtk_label_set_text(labelSpinboxApple, "您当前选择的数字式："+Double.toString(apple));
                                
                        }
                }, null);
        }

}

    ```

Second: ComboBox

case1: 你想要购买的是什么水果？  combobox
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

/**
* @author 叶昭良
* @version ComboBoxText v1.0
*
*/
public class TestGtkComboBoxText
{

        /**
         * @param args
         */
        static int window;
        //static int box;
        static int gridHouse;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                GTK.gtk_init();
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL); 
                GTK.gtk_window_set_title(window, "SpinBoxV1.0");
                GTK.gtk_widget_show(window);
                //安全关闭GTK
                GTK.g_signal_connect(window, "destroy", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                }, null);
                String names[] = {"Andy","Jerry","laulence","Lucy","jack"};
                // 创建布局
                gridHouse =  GTK.gtk_grid_new();
                GTK.gtk_widget_show(gridHouse);
                createComboBoxText(window,gridHouse,0);  //yij
                loadComboBox(window, names, gridHouse, 2); //之所以从2开始 是因为 0 1已经被使用
                GTK.gtk_container_add(window,gridHouse);
                //启动循环
                GTK.gtk_main();
        }
        
        public static void createComboBoxText(int window,int gridHouse,int start)
        {
                final int comboBoxBig = GTK.gtk_combo_box_text_new();
                GTK.gtk_combo_box_text_append_text(comboBoxBig, "苹果");
                GTK.gtk_combo_box_text_append_text(comboBoxBig, "香蕉");
                GTK.gtk_combo_box_text_append_text(comboBoxBig, "葡萄");
                GTK.gtk_combo_box_text_append_text(comboBoxBig, "橘子");
                GTK.gtk_combo_box_text_append_text(comboBoxBig, "蜜柚");
                final int comboBoxApple = GTK.gtk_combo_box_text_new();
                GTK.gtk_combo_box_text_append_text(comboBoxApple, "红苹果");
                GTK.gtk_combo_box_text_append_text(comboBoxApple, "青苹果");
                GTK.gtk_combo_box_text_append_text(comboBoxApple, "小苹果");
                final int comboBoxBanana = GTK.gtk_combo_box_text_new();
                GTK.gtk_combo_box_text_append_text(comboBoxBanana, "天宝蕉");
                GTK.gtk_combo_box_text_append_text(comboBoxBanana, "竹蕉");
                GTK.gtk_combo_box_text_append_text(comboBoxBanana, "大香蕉");
                final int comboBoxGrape = GTK.gtk_combo_box_text_new();
                GTK.gtk_combo_box_text_append_text(comboBoxGrape, "青葡萄");
                GTK.gtk_combo_box_text_append_text(comboBoxGrape, "玫瑰香");
                GTK.gtk_combo_box_text_append_text(comboBoxGrape, "巨峰");
                GTK.gtk_combo_box_text_append_text(comboBoxGrape, "夏黑");
                final int comboBoxOrange = GTK.gtk_combo_box_text_new();
                GTK.gtk_combo_box_text_append_text(comboBoxOrange, "小橘子");
                GTK.gtk_combo_box_text_append_text(comboBoxOrange, "大橘子");
                GTK.gtk_combo_box_text_append_text(comboBoxOrange, "年橘");
                final int comboBoxYouzi = GTK.gtk_combo_box_text_new();
                GTK.gtk_combo_box_text_append_text(comboBoxYouzi, "白心蜜柚");
                GTK.gtk_combo_box_text_append_text(comboBoxYouzi, "红心蜜柚");
                GTK.gtk_combo_box_text_append_text(comboBoxYouzi, "臭蜜柚");
                
        /*        int label1 = GTK.gtk_label_new("您当前想要购买的是"+GTK.gtk_combo_box_text_get_active_text(comboBoxBig)
                                +"下的"+GTK.gtk_combo_box_text_get_active_text(comboBoxApple));*/
                final int label1 = GTK.gtk_label_new("您当前想要购买的是?");
                
                // 添加控件到整租房间
                
                GTK.gtk_grid_attach(gridHouse, comboBoxBig, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, comboBoxApple, 1, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, comboBoxBanana, 1, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, comboBoxGrape, 1, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, comboBoxOrange, 1, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, comboBoxYouzi, 1, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, label1, 0, start+1, 1, 1);
                
                //显示控件
                GTK.gtk_widget_show(comboBoxBig);
                GTK.gtk_widget_show(comboBoxApple);
                GTK.gtk_widget_show(label1);
                
                //设置combobox的激活状态
                GTK.gtk_combo_box_set_active(comboBoxBig, 0);
                GTK.gtk_combo_box_set_active(comboBoxApple, 0);
                
                //添加事件
                GTK.g_signal_connect(comboBoxBig, "changed", new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                String fruit = GTK.gtk_combo_box_text_get_active_text(comboBoxBig);
                                if(fruit.equalsIgnoreCase("苹果"))
                                {
                                        helpFunction(comboBoxApple,label1);
                                        GTK.gtk_widget_hide(comboBoxBanana);
                                        GTK.gtk_widget_hide(comboBoxGrape);
                                        GTK.gtk_widget_hide(comboBoxOrange);
                                        GTK.gtk_widget_hide(comboBoxYouzi);
                                }else if(fruit.equalsIgnoreCase("香蕉"))
                                {
                                        helpFunction(comboBoxBanana,label1);
                                        GTK.gtk_widget_hide(comboBoxApple);
                                        GTK.gtk_widget_hide(comboBoxGrape);
                                        GTK.gtk_widget_hide(comboBoxOrange);
                                        GTK.gtk_widget_hide(comboBoxYouzi);
                                }else if(fruit.equalsIgnoreCase("葡萄"))
                                {
                                        helpFunction(comboBoxGrape,label1);
                                        GTK.gtk_widget_hide(comboBoxApple);
                                        GTK.gtk_widget_hide(comboBoxBanana);
                                        GTK.gtk_widget_hide(comboBoxOrange);
                                        GTK.gtk_widget_hide(comboBoxYouzi);
                                }else if(fruit.equalsIgnoreCase("橘子"))
                                {
                                        helpFunction(comboBoxOrange,label1);
                                        GTK.gtk_widget_hide(comboBoxApple);
                                        GTK.gtk_widget_hide(comboBoxBanana);
                                        GTK.gtk_widget_hide(comboBoxGrape);
                                        GTK.gtk_widget_hide(comboBoxYouzi);        
                                }
                                else if(fruit.equalsIgnoreCase("蜜柚"))
                                {
                                        helpFunction(comboBoxYouzi,label1);
                                        GTK.gtk_widget_hide(comboBoxApple);
                                        GTK.gtk_widget_hide(comboBoxBanana);
                                        GTK.gtk_widget_hide(comboBoxGrape);
                                        GTK.gtk_widget_hide(comboBoxOrange);        
                                }
                        }
                }, null);
                
        }
        
        public static void helpFunction(final int comboBoxText,final int label1)
        {
                GTK.gtk_widget_show(comboBoxText);
                GTK.gtk_combo_box_set_active(comboBoxText, 0);
                GTK.g_signal_connect(comboBoxText, "changed", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object) 
                        {
                                String fruit = GTK.gtk_combo_box_text_get_active_text(comboBoxText);
                                GTK.gtk_label_set_text(label1, "您想要购买的"+fruit+"没有了！请到别家购买");
                        };
                }, null);
        }
        
        public static void loadComboBox(int window,String[] names, int gridHouse,int start)
        {
                int cmbApple = GTK.gtk_combo_box_text_new();
                for(int  i = 0 ; i < names.length; i++)
                {
                        GTK.gtk_combo_box_text_append_text(cmbApple, names[i]);
                }
                
                GTK.gtk_widget_show(cmbApple);
                GTK.gtk_grid_attach(gridHouse, cmbApple, 0, start, 1, 1);
        }
}

```

case2: 省市联动器 （上一次的水果选择器，未使用id方法。。）
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;
/**
* 
*/

/**
* @author 叶昭良
* @version 省市联动器 V1.0
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
                //GTK.gtk_combo_box_text_append(cmbProvince, "sh", "上海");
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
                                if(province==null)return;
                                System.out.println(province);
                                GTK.gtk_combo_box_text_remove_all(cmbCity); //删除所有项
                                //GTK.gtk_combo_box_text_remove_all(cmbCity); //删除所有项
                                if(province.equals("fj"))
                                {
                                        System.out.println("福建测试中");
                                        GTK.gtk_combo_box_text_append(cmbCity, "zhz", "漳州");
                                        GTK.gtk_combo_box_text_append(cmbCity, "xm", "厦门");
                                        GTK.gtk_combo_box_text_append(cmbCity, "fz", "福州");
                                        GTK.gtk_combo_box_text_append(cmbCity, "qz", "泉州");
                                        
                                }else if(province.equals("bj"))
                                {
                                        System.out.println("福建测试中");
                                        GTK.gtk_combo_box_text_append(cmbCity, "cp", "昌平区");
                                        GTK.gtk_combo_box_text_append(cmbCity, "hd", "海淀区");
                                        GTK.gtk_combo_box_text_append(cmbCity, "tz", "通州区");
                                        GTK.gtk_combo_box_text_append(cmbCity, "cy", "朝阳区");
                                }else if(province.equals("hn"))
                                {
                                        System.out.println("福建测试中");
                               GTK.gtk_combo_box_text_append(cmbCity, "zz", "郑州");
                               GTK.gtk_combo_box_text_append(cmbCity, "zmd", "驻马店");
                               GTK.gtk_combo_box_text_append(cmbCity, "ny", "南阳");
                                }else if(province.equals("hb"))
                                {
                                        System.out.println("福建测试中");
                               GTK.gtk_combo_box_text_append(cmbCity, "sjz", "石家庄");
                               GTK.gtk_combo_box_text_append(cmbCity, "ts", "唐山");
                               GTK.gtk_combo_box_text_append(cmbCity, "qhd", "秦皇岛");
                                }else if(province.equals("sd"))
                                {
                                        System.out.println("福建测试中");
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
                                
                                int  apple  = GTK.gtk_combo_box_get_active(cmbCity);
                                if(apple < 0)
                                {
                                        System.out.println("nothing in the city");
                                        return;
                                }
                                
                                String tempCity = GTK.gtk_combo_box_text_get_active_text(cmbCity);
                                if(tempProvince==null||tempCity==null)return;
                                //GTK.gtk_label_set_text(labelShow, "你准备去"+tempProvince+tempCity);
                                GTK.gtk_label_set_text(labelShow,tempProvince+tempCity);
                                System.out.println(tempProvince);
                                System.out.println(tempCity);

                        }
                }, null);
                
                
        }

}

```
Third: GTKIMAGE

case1  三种方式读入Image
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;


/**
* @author 叶昭良
* @version GtkImage v1.0
*
*/
public class TestGtkImage
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                // TODO 自动生成的方法存根
                GTK.gtk_init();
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL); 
                GTK.gtk_window_set_title(window, "GtkImage V1.0");
                GTK.gtk_widget_show(window);
                //安全关闭GTK
                GTK.g_signal_connect(window, "destroy", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                }, null);
                // 创建布局
                gridHouse =  GTK.gtk_grid_new();
                GTK.gtk_widget_show(gridHouse);
                
                int start  = 0;
                createGtkImage(window,gridHouse,start);
                GTK.gtk_container_add(window,gridHouse);
                //启动循环
                GTK.gtk_main();
        }
        
        public static void createGtkImage(int window,int gridHouse,int start)
        {
                int imgWall = GTK.gtk_image_new_from_file("E:\\娱乐频道\\图片频道\\xin\\greatWall.jpg");
                int imgHun = GTK.gtk_image_new_from_resource("yumufeng.jpg");
                int imgStock = GTK.gtk_image_new_from_stock(GTK.GTK_STOCK_ABOUT, GTK.GTK_ICON_SIZE_LARGE_TOOLBAR);
                
                
                int labelWall = GTK.gtk_label_new("通过磁盘文件读入长城照片");
                int labelHun = GTK.gtk_label_new("通过项目文件读入婚纱照片");
                int labelStock = GTK.gtk_label_new("通过GTK内置图片文件读入关于照片");
                
                //添加到整租房间
                GTK.gtk_grid_attach(gridHouse, imgWall, 0, start+1, 1, 1);
                GTK.gtk_grid_attach(gridHouse, imgHun, 1, start+1, 1, 1);
                GTK.gtk_grid_attach(gridHouse, imgStock, 2, start+1, 1, 1);
                GTK.gtk_grid_attach(gridHouse, labelWall, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, labelHun, 1, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, labelStock, 2, start, 1, 1);
                
                //显示控件
                GTK.gtk_widget_show(imgWall);
                GTK.gtk_widget_show(imgHun);
                GTK.gtk_widget_show(imgStock);
                GTK.gtk_widget_show(labelWall);
                GTK.gtk_widget_show(labelHun);
                GTK.gtk_widget_show(labelStock);
        }
}

```

case2  按钮图片
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;


/**
* @author 叶昭良
* @version ButtonImage v1.0
*
*/
public class TestGtkButtonImage
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                // TODO 自动生成的方法存根
                // TODO 自动生成的方法存根
                GTK.gtk_init();
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL); 
                GTK.gtk_window_set_title(window, "ButtonImage V1.0");
                GTK.gtk_widget_show(window);
                //安全关闭GTK
                GTK.g_signal_connect(window, "destroy", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                }, null);
                // 创建布局
                gridHouse =  GTK.gtk_grid_new();
                GTK.gtk_widget_show(gridHouse);
                
                int start  = 0;
                createButtonImage(window,gridHouse,start);
                GTK.gtk_container_add(window,gridHouse);
                //启动循环
                GTK.gtk_main();
        }
        
        public static void createButtonImage(int window,int gridHouse,int start)
        {
                int imgOpen = GTK.gtk_image_new_from_stock(GTK.GTK_STOCK_OPEN, GTK.GTK_ICON_SIZE_BUTTON);
                int imgClosed = GTK.gtk_image_new_from_stock(GTK.GTK_STOCK_CLOSE, GTK.GTK_ICON_SIZE_BUTTON);
                int imgFile = GTK.gtk_image_new_from_stock(GTK.GTK_STOCK_FILE, GTK.GTK_ICON_SIZE_BUTTON);
                int btnCommon = GTK.gtk_button_new_with_label("普通按钮");
                int btnOpen = GTK.gtk_button_new_with_label("打开");
                int btnClosed = GTK.gtk_button_new_with_label("关闭");
                int btnFile = GTK.gtk_button_new_with_label("文件");
                GTK.gtk_button_set_image(btnOpen, imgOpen);
                GTK.gtk_button_set_image(btnClosed, imgClosed);
                GTK.gtk_button_set_image(btnFile, imgFile);
                //添加控件 
                //GTK.gtk_grid_attach(gridHouse, imgOpen, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, btnCommon, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, btnOpen, 1, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, btnClosed,2, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, btnFile,3, start, 1, 1);
                //显示控件
                GTK.gtk_widget_show(btnOpen);
                GTK.gtk_widget_show(btnClosed);
                GTK.gtk_widget_show(btnCommon);
                GTK.gtk_widget_show(btnFile);
                
                GTK.gtk_button_set_image_position(btnClosed, GTK.GTK_POS_TOP); //GTK_POS
                GTK.gtk_button_set_image_position(btnFile, GTK.GTK_POS_RIGHT);
        }

}

```
Fourth: TextView

case1 女神表达神器：Love you !My Girl
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;



/**
* @author 叶昭良
* @version  TestView v1.0
*/
public class GTKTestTextView
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                GTK.gtk_init();
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL); 
                GTK.gtk_window_set_title(window, "女朋友告白神器 V1.0");
                GTK.gtk_widget_show(window);
                //安全关闭GTK
                GTK.g_signal_connect(window, "destroy", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                }, null);
                // 创建布局
                gridHouse =  GTK.gtk_grid_new();
                GTK.gtk_widget_show(gridHouse);
                
                int start  = 0;
                createTextView(window,gridHouse,start);
                GTK.gtk_container_add(window,gridHouse);
                //启动循环
                GTK.gtk_main();
        }
        
        public static void createTextView(int window, int gridHouse, int start)
        {
                int imgYumu = GTK.gtk_image_new_from_resource("yumufeng.jpg");
                int tvGirl = GTK.gtk_text_view_new();
                final int label = GTK.gtk_label_new("");
                int btnShow = GTK.gtk_button_new_with_label("显示");
                GTK.gtk_text_view_set_wrap_mode(tvGirl, GTK.GTK_WRAP_WORD);
                
                //添加控件
                GTK.gtk_grid_attach(gridHouse, imgYumu, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, tvGirl, 0, start+1, 1, 1);
                GTK.gtk_grid_attach(gridHouse, label, 0 ,start+2, 1,1);
                GTK.gtk_grid_attach(gridHouse, btnShow, 1 ,start+2, 1,1);
                //显示控件
                GTK.gtk_widget_show(imgYumu);
                GTK.gtk_widget_show(tvGirl);
                GTK.gtk_widget_show(label);
                GTK.gtk_widget_show(btnShow);
                
                //读取文本框里面的内容 ，只适用小量的文本，一般用迭代器
                //方法1   先从TextView获取int TextBuffer 
                    //    然后再从TextBuffer获取text
                final int textbuffer= GTK.gtk_text_view_get_buffer(tvGirl);
                String loveWords = "你就像那天上星星，点缀着我们两的星空，璀璨夺目；"
                                + "我愿与你携手共同奋进，原因和你共育我们的sons and grils,"
                                + "建立起一个幸福的家庭";
                
                //可以直接通过缓冲区编号  设置信息
                GTK.gtk_text_buffer_set_text(textbuffer, loveWords);
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
                
                
        }

}

```

case2 : TextIter  实现添加love you 字符串  并加入了滚动条，实现女神表达器的升级版
```java
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;



/**
* @author 叶昭良
* @version  TestView+TestIter v1.0
* @version  TestView+TestIter v2.0 加入了滚动条操作。
*/
public class GTKTestTextIter
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        static int scrolledBar;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                GTK.gtk_init();
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL); 
                GTK.gtk_window_set_title(window, "女朋友告白神器 V1.0");
                GTK.gtk_widget_show(window);
                //安全关闭GTK
                GTK.g_signal_connect(window, "destroy", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                }, null);
                // 创建布局
                gridHouse =  GTK.gtk_grid_new();
                GTK.gtk_widget_show(gridHouse);
                
                int start  = 0;
                createTextView(window,gridHouse,start);
                GTK.gtk_container_add(window,gridHouse);
                //启动循环
                GTK.gtk_main();
        }
        
        public static void createTextView(int window, int gridHouse, int start)
        {
                int imgYumu = GTK.gtk_image_new_from_resource("yumufeng.jpg");
                final int tvGirl = GTK.gtk_text_view_new();
                final int label = GTK.gtk_label_new("");
                final int labelIter = GTK.gtk_label_new("");
                int btnShow = GTK.gtk_button_new_with_label("显示");
                int btnShowIter = GTK.gtk_button_new_with_label("迭代器插入");
                GTK.gtk_text_view_set_wrap_mode(tvGirl, GTK.GTK_WRAP_WORD);
                int scrollBar = 0 ;
                //添加控件
                GTK.gtk_grid_attach(gridHouse, imgYumu, 0, start, 1, 1);
                createScrollBar(scrollBar,tvGirl,gridHouse,start);
                //GTK.gtk_grid_attach(gridHouse, tvGirl, 0, start+1, 1, 1);
                GTK.gtk_grid_attach(gridHouse, label, 0 ,start+2, 1,1);
                GTK.gtk_grid_attach(gridHouse, labelIter, 0 ,start+3, 1,1);
                GTK.gtk_grid_attach(gridHouse, btnShow, 1 ,start+2, 1,1);
                GTK.gtk_grid_attach(gridHouse, btnShowIter, 1 ,start+3, 1,1);
                //显示控件
                GTK.gtk_widget_show(imgYumu);
                GTK.gtk_widget_show(tvGirl);
                GTK.gtk_widget_show(label);
                GTK.gtk_widget_show(btnShow);
                GTK.gtk_widget_show(labelIter);
                GTK.gtk_widget_show(btnShowIter);
                
                //读取文本框里面的内容 ，只适用小量的文本，一般用迭代器
                //方法1   先从TextView获取int TextBuffer 
                    //    然后再从TextBuffer获取text
                final int textbuffer= GTK.gtk_text_view_get_buffer(tvGirl);
                String loveWords = "你就像那天上星星，点缀着我们两的星空，璀璨夺目；"
                                + "我愿与你携手共同奋进，原因和你共育我们的sons and grils,"
                                + "建立起一个幸福的家庭";
                
                //可以直接通过缓冲区编号  设置信息
                GTK.gtk_text_buffer_set_text(textbuffer, loveWords);
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
                
                
                GTK.g_signal_connect(btnShowIter, "clicked", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                helpFunction(tvGirl);
                        }
                }, null);
                
        }
        //如何遍历读取textview信息？？ 当前只能插入？
        public static void helpFunction(int textview)
        {
                //TextIter是一个TextView的迭代器。
                int textBuffer =  GTK.gtk_text_view_get_buffer(textview);
                int textIter = GTK.gtk_text_iter_new();  //这是一个空的iter，需要用textBuffer进行赋值
                GTK.gtk_text_buffer_get_end_iter(textBuffer, textIter);// 或者textview的textBuffer的末尾！
                GTK.gtk_text_buffer_insert(textBuffer, textIter, "I love you! Xinran");
                
                //GTK.gtk_text_buffer_g
                GTK.gtk_text_iter_free(textIter);
                
        }
        
        public static void createScrollBar(int scrolledBar,int textview,int gridHouse,int start)
        {
                scrolledBar = GTK.gtk_scrolled_window_new();
                GTK.gtk_widget_show(scrolledBar);
                GTK.gtk_grid_attach(gridHouse, scrolledBar, 0, start+1, 1, 1);
                GTK.gtk_widget_set_size_request(scrolledBar, 300, 50);
                GTK.gtk_container_add(scrolledBar,textview);
        }

}

```

case3 : 计算器version 3.0   加入了一些Sin等计算符   以及TextView   

```java
/**
* @author 叶昭良
* @version : 计算器v1.0
* @version : 计算器v 2.0  替换掉entry 采用了SpinBox和ComboBox
* @version : 计算器v 3.0  
*              1.使用button触发计算事件  加入了三角函数、对数函数等的计算，
*              2.使用TextView  and TextIter把结果输入到其中。。 
*              3.对于         
*
*/
import javax.swing.JOptionPane;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;
//import java.math.*;

public class TestCalc3
{

        /**
         * @param args
         */
        static int window;
        static boolean isEnter = false;

        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                //初始化
                GTK.gtk_init();
                //建立窗口  设置成static int window变量
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                //设置窗口名称
                GTK.gtk_window_set_title(window, "计算器v3.0");
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
                
                //网格布局
            int houseGrid = GTK.gtk_grid_new();
            //载入计算机控件
            
            Calc(window,houseGrid,0); //有两行
            specialCal(window,houseGrid,2); //有两行
            //载入到windows当中
                GTK.gtk_container_add(window, houseGrid);
                //启动循环
                GTK.gtk_main();
                
        }
        /**
         * 
         * @param window     窗口的标识
          * @param houseGrid  整租房间的标识
         * @param start      设置整租房间的起始行数
         */
        public static void Calc(int window,int houseGrid,int start)
        {
                final int sbOne = GTK.gtk_spin_button_new_with_range(-32768, 32767, 1);
                final int sbAnother = GTK.gtk_spin_button_new_with_range(-32768, 32767, 1);
                GTK.gtk_spin_button_set_value(sbOne, 12.0);
                GTK.gtk_spin_button_set_value(sbAnother, 4.0);
                final int cbbOperator = GTK.gtk_combo_box_text_new();
                GTK.gtk_combo_box_text_append_text(cbbOperator, "+");
                GTK.gtk_combo_box_text_append_text(cbbOperator, "-");
                GTK.gtk_combo_box_text_append_text(cbbOperator, "*");
                GTK.gtk_combo_box_text_append_text(cbbOperator, "/");
                GTK.gtk_combo_box_text_append_text(cbbOperator, "%");
                GTK.gtk_combo_box_text_append_text(cbbOperator, "^");
                
                GTK.gtk_combo_box_set_active(cbbOperator, 0);
                int btnEquals = GTK.gtk_button_new_with_label("=");
                final int label2 = GTK.gtk_label_new("我知道答案是什么");
                
                int labelCommon = GTK.gtk_label_new("普通的四则运算：");
                // Box用于存储  + - operator



                //加入整租
                GTK.gtk_grid_attach(houseGrid, labelCommon, 0, start, 1, 1);
                //在下一行再添加
                start= start + 1;
                GTK.gtk_grid_attach(houseGrid, sbOne, 0, start, 1, 1);
                GTK.gtk_grid_attach(houseGrid, cbbOperator, 1, start, 1, 1);
//                GTK.gtk_grid_attach(houseGrid, buttonPlus, 1, start, 1, 1);
//                GTK.gtk_grid_attach(houseGrid, buttonMinus, 1, start+1, 1, 1);
                GTK.gtk_grid_attach(houseGrid, sbAnother, 2, start, 1, 1);
                GTK.gtk_grid_attach(houseGrid, btnEquals, 3, start, 1, 1);
                GTK.gtk_grid_attach(houseGrid, label2, 4, start, 1, 1);
                
                //显示控件
                GTK.gtk_widget_show(labelCommon);
                GTK.gtk_widget_show(sbOne);
                GTK.gtk_widget_show(sbAnother);
                GTK.gtk_widget_show(cbbOperator);

                GTK.gtk_widget_show(btnEquals);
                GTK.gtk_widget_show(label2);
                //GTK.gtk_widget_show(box);
                GTK.gtk_widget_show(houseGrid);
                
                
                //加入事件控制机制 
                GTK.g_signal_connect(btnEquals, "clicked", new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                
                                // TODO 自动生成的方法存根
                                double plus1 = GTK.gtk_spin_button_get_value(sbOne);
                                double plus2 = GTK.gtk_spin_button_get_value(sbAnother);
                                
                                //增加一个 只读模式的entry
                                //GTK.gtk_editable_set_editable(entryOne, false); //只读不能写入。
                                
                                String cbbString = GTK.gtk_combo_box_text_get_active_text(cbbOperator);
                                //panduan(plus1,entryOne);
                                panduan(plus2,sbAnother);
                                double sum1 = 0;
                                if(cbbString.equalsIgnoreCase("+"))
                                {
                                         sum1 = plus1 + plus2;
                                        GTK.gtk_label_set_text(label2, Double.toString(sum1));
                                }else if(cbbString.equalsIgnoreCase("-"))
                                {
                                        sum1 = plus1 -plus2;
                                        GTK.gtk_label_set_text(label2, Double.toString(sum1));
                                }else if(cbbString.equalsIgnoreCase("*"))
                                {
                                        sum1 = plus1 * plus2;
                                        GTK.gtk_label_set_text(label2, Double.toString(sum1));
                                }else if(cbbString.equalsIgnoreCase("/"))
                                {
                                        panduan(plus2,sbAnother);
                                        sum1 = plus1 / plus2;
                                        GTK.gtk_label_set_text(label2, Double.toString(sum1));
                                }else if(cbbString.equalsIgnoreCase("%"))
                                {
                                        panduan(plus2,sbAnother);
                                        sum1 = plus1 % plus2;
                                        GTK.gtk_label_set_text(label2, Double.toString(sum1));
                                }else if(cbbString.equalsIgnoreCase("^"))
                                {
                                        //String temp = Double.toString(plus2);
                                        
                                        sum1 = Math.pow(plus1,plus2);
                                        GTK.gtk_label_set_text(label2, Double.toString(sum1));
                                }
                        }
                }, null);        
                
        }
        /**
         * 
         * @param plus1  entry1的数字字符串
         * @param entry1 entry1的标识
         */
        public static void panduan(double plus1,int sbAnother)
        {
                if(plus1 == 0)
                {
                        JOptionPane.showMessageDialog(null, "被除数不能为0,已经置为空 请重新输入");
                        GTK.gtk_spin_button_set_value(sbAnother, 1.0);;
                        return;
                }
        }
        /**
         * 
         * @param window     计算器窗口的标识
         * @param gridHouse  整租房的标识
         * @param start      整租房的编号
         */
        public static void specialCal(int window,int gridHouse,int start)
        {
                // 建立控件
                final int btnSin = GTK.gtk_button_new_with_label("sin");
                final int btnCos = GTK.gtk_button_new_with_label("cos");
                final int btnTan = GTK.gtk_button_new_with_label("tan");
                final int btnAsin = GTK.gtk_button_new_with_label("asin");
                final int btnAcos = GTK.gtk_button_new_with_label("acos");
                final int btnAtan = GTK.gtk_button_new_with_label("atan");
                final int btnLog = GTK.gtk_button_new_with_label("log");
                final int btnSqrt = GTK.gtk_button_new_with_label("sqrt");
                final int btnabs = GTK.gtk_button_new_with_label("abs");
                final int btnFloor = GTK.gtk_button_new_with_label("floor");
                final int btnCeil = GTK.gtk_button_new_with_label("ceil");
                final int btnToDegree = GTK.gtk_button_new_with_label("弧度变角度");
                final int btnToRadius = GTK.gtk_button_new_with_label("角度变弧度");
                final int spinbox= GTK.gtk_spin_button_new_with_range(-32767, 32767, 0.1);
                final int btnEquals = GTK.gtk_button_new_with_label("=");
                final int tv1 = GTK.gtk_text_view_new();
                final int labelSpecial = GTK.gtk_label_new("特殊的四则运算：(先点击运算符，再敲值，最后等号)");
                //设置GTK TextView的模式
                GTK.gtk_text_view_set_wrap_mode(tv1, GTK.GTK_WRAP_WORD);
                //设置滚动条
                final int scrolledBar = 0 ;


                GTK.gtk_spin_button_set_value(spinbox, 44.0);
                int innerGrid = GTK.gtk_grid_new();
                //添加控件
                GTK.gtk_grid_attach(innerGrid, btnSin, 0, 0, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnCos, 1, 0, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnTan, 2, 0, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnAsin, 0, 1, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnAcos, 1, 1, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnAtan, 2, 1, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnLog, 0, 2, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnSqrt, 1, 2, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnabs, 2, 2, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnFloor, 0, 3, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnCeil, 1, 3, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnToDegree, 0, 4, 1, 1);
                GTK.gtk_grid_attach(innerGrid, btnToRadius, 1, 4, 1, 1);
                
                GTK.gtk_grid_attach(gridHouse,labelSpecial,0,start,1,1);
                
                start = start + 1; //跳到下一行
                GTK.gtk_grid_attach(gridHouse, innerGrid, 0, start, 3, 5);
                GTK.gtk_grid_attach(gridHouse, spinbox, 3, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, btnEquals, 4, start, 1, 1);
                // GTK.gtk_grid_attach(gridHouse, tv1, 4, start+1, 1, 1);
                createScrollBar(scrolledBar,tv1,gridHouse,start);
                
                
                //显示控件
                GTK.gtk_widget_show(labelSpecial);
                GTK.gtk_widget_show(btnSin);
                GTK.gtk_widget_show(btnCos);
                GTK.gtk_widget_show(btnTan);
                GTK.gtk_widget_show(btnAsin);
                GTK.gtk_widget_show(btnAcos);
                GTK.gtk_widget_show(btnAtan);
                GTK.gtk_widget_show(btnSqrt);
                GTK.gtk_widget_show(btnLog);
                GTK.gtk_widget_show(btnabs);
                GTK.gtk_widget_show(btnCeil);
                GTK.gtk_widget_show(btnFloor);
                GTK.gtk_widget_show(btnToDegree);
                GTK.gtk_widget_show(btnToRadius);
                GTK.gtk_widget_show(tv1); //还是得先是  必须show 否则看不到
                GTK.gtk_widget_show(spinbox);
                GTK.gtk_widget_show(btnEquals);
                //GTK.gtk_widget_show(scrolledBar);  //不要在这边show只在scrollbar区域，不然报错
                GTK.gtk_widget_show(innerGrid);
                
                
                
                //添加按钮事件
                insertButtonEvent( btnSin,tv1);
                insertButtonEvent( btnCos,tv1);
                insertButtonEvent( btnTan,tv1);
                insertButtonEvent( btnAsin,tv1);
                insertButtonEvent( btnAcos,tv1);
                insertButtonEvent( btnAtan,tv1);
                insertButtonEvent( btnLog,tv1);
                insertButtonEvent( btnabs,tv1);
                insertButtonEvent( btnCeil,tv1);
                insertButtonEvent( btnFloor,tv1);
                insertButtonEvent( btnToDegree,tv1);
                insertButtonEvent( btnToRadius,tv1);
                
                
                GTK.g_signal_connect(spinbox,"changed",new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                isEnter = true;
                        }
                },null);
                
                GTK.g_signal_connect(btnEquals, "clicked", new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                double sum1  = 0 ;
                                double apple = GTK.gtk_spin_button_get_value(spinbox);
                                if(isEnter == false)
                                {
                                        JOptionPane.showMessageDialog(null, "您未选择特殊操作符，请重新选择，并输入数字，进行运算");
                                        GTK.gtk_spin_button_set_value(spinbox, 44.0);
                                        return;
                                }else
                                {
                                        String temp = GetStringFromTextViewFunction(tv1);
                                        if(temp.equals("sin"))
                                        {
                                                double orange = Math.toRadians(apple);
                                                sum1  = Math.sin(orange);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("cos"))
                                        {
                                                double orange = Math.toRadians(apple);
                                                sum1  = Math.cos(orange);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("tan"))
                                        {
                                                double orange = Math.toRadians(apple);
                                                sum1  = Math.tan(orange);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("asin"))
                                        {
                                                double orange = Math.toRadians(apple);
                                                sum1  = Math.asin(orange);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("acos"))
                                        {
                                                double orange = Math.toRadians(apple);
                                                sum1  = Math.acos(orange);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("atan"))
                                        {
                                                double orange = Math.toRadians(apple);
                                                sum1  = Math.atan(orange);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("log"))
                                        {
                                                sum1  = Math.log(apple);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("sqrt"))
                                        {
                                                sum1  = Math.sqrt(apple);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("abs"))
                                        {
                                                sum1  = Math.abs(apple);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("floor"))
                                        {
                                                sum1  = Math.floor(apple);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("ceil"))
                                        {
                                                sum1  = Math.ceil(apple);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("弧度变角度"))
                                        {
                                                sum1  = Math.toDegrees(apple);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }else if(temp.equals("角度变弧度"))
                                        {
                                                sum1  = Math.toRadians(apple);
                                                String finalResult = "("+apple+")"+" = "+sum1+"\n";
                                                InsertStringToTextViewFunction(tv1,finalResult);
                                        }
                                }
                        }
                }, null);
                
        }
        
        /**
         * 
         * @param scrolledBar  滚动条标识
         * @param textview     多行文本标识
         * @param gridHouse    整租房的标识
         * @param start        整租房的初始开始处
         */
        public static void createScrollBar(int scrolledBar,int textview,int gridHouse,int start)
        {
                scrolledBar = GTK.gtk_scrolled_window_new();
                GTK.gtk_widget_show(scrolledBar);
                //滚动条是更大的容器
                GTK.gtk_grid_attach(gridHouse, scrolledBar, 4, start+1, 1, 1); // 4改为0 报错
                GTK.gtk_widget_set_size_request(scrolledBar, 300, 50);
                GTK.gtk_container_add(scrolledBar,textview); // 添加textview到滚动条容器当中
        }
        
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
                                isEnter = false;
                        }
                }, null);
        }


}
```

Fifth: Cairo

case1 : 三毛头部自画像 V1.0

```java
/**
* @author 叶昭良
* @version GTK+Cairo v1.0
*/
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;
import com.rupeng.gtk4j.Cairo;
public class GTKTestCairo
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                //GTK初始化
                GTK.gtk_init();
                //窗口标识
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                //窗口标题
                GTK.gtk_window_set_title(window, "简易画图板v1.0");
                //显示窗口
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
                
                
                //设置布局方式
                gridHouse = GTK.gtk_grid_new();
                
                
                //创建控件
                int start = 0;
                createDraw(window, gridHouse, start);
                //circlePoint 原点设置为180
                createDrawLaughFace(window,gridHouse,start+1,180);
                
                
                GTK.gtk_widget_show(gridHouse);
                
                GTK.gtk_container_add(window,gridHouse);
                //启动循环
                GTK.gtk_main();
        }
        
        
        public static void createDraw(int window,int gridHouse,int start) 
        {
                //创建画图板 或者叫画布
                int  dan  = GTK.gtk_drawing_area_new();
                
                int label = GTK.gtk_label_new("三毛");
                //创建源
                //添加控件
                GTK.gtk_grid_attach(gridHouse, label, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, dan, 1, start, 1, 1);
                //显示画布
                GTK.gtk_widget_show(dan);
                GTK.gtk_widget_show(label);
                
                //设置画布的大小
                GTK.gtk_widget_set_size_request(dan, 300, 300);
                
                
                //开始绘制  利用事件draw 来不断的绘制
                
                GTK.g_signal_connect(dan, "draw",new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //eventDate才是画布源
                                //设置画笔的颜色
                                Cairo.cairo_set_source_rgb(eventData, 0.5, 0.6, 0.3);
                                //设置画笔的大小
                                Cairo.cairo_set_line_width(eventData, 3);
                                
                                
                                
                                //移动画笔点   画笔默认位置不是在（0,0）move_to不可少
                                //Cairo.cairo_move_to(eventData, 0, 0);
/*                                //画一条线七点
                                Cairo.cairo_line_to(eventData, 50, 50);
                                //画一条线重点
                                Cairo.cairo_line_to(eventData,100,100);
                                //接着划线
                                Cairo.cairo_line_to(eventData, 120, 120);*/
                                //画一个圆
                                Cairo.cairo_arc(eventData, 180, 180, 60, 0, 2*3.1415926);
                                //画一个圆弧
                                //Cairo.cairo_arc(eventData, 200, 200, 70,1.5*3.1415925, 2*3.1415926);
                                //显示画笔
                                Cairo.cairo_stroke(eventData); //Cairo.cairo_fill(eventData)不同的效果
                                
                                
                                //画一个圆弧  有下面实验知道是从x轴沿顺时针开始画图
/*                                Cairo.cairo_arc(eventData, 220, 220, 30,0, 0.5*3.1415926);

                                Cairo.cairo_fill(eventData); //Cairo.cairo_fill(eventData)不同的效果
                                
                                Cairo.cairo_arc(eventData, 220, 100, 30,0, 3.1415926);
                                Cairo.cairo_stroke(eventData);
                                Cairo.cairo_arc(eventData, 220, 30, 30,0, 1.5*3.1415926);
                                Cairo.cairo_fill(eventData);*/
                                //嘴巴的绘制
                                double Pi = 3.1415926;
                                Cairo.cairo_arc(eventData, 180, 180, 40, 0.25*Pi, 0.75*Pi);
                                Cairo.cairo_fill(eventData);
                                
                                //绘制两只眼睛
                                Cairo.cairo_arc(eventData, 160, 160, 10, 0, 2*Pi);
                                Cairo.cairo_arc(eventData, 200, 160, 10, 0, 2*Pi);
                                Cairo.cairo_fill(eventData);
                                
                                
                                //绘制胡须
                                Cairo.cairo_arc(eventData, 130, 160, 50, 0.166667*Pi, 0.6777777*Pi);
                                
                                Cairo.cairo_stroke(eventData);
                                
                                Cairo.cairo_arc(eventData, 230, 160, 50, 0.333333*Pi, 0.8333333*Pi);
                                Cairo.cairo_stroke(eventData);
                                
                                //绘制头发
                                Cairo.cairo_arc(eventData, 133.6, 136.72, 30,1.3333333*Pi ,1.833333*Pi );
                                
                                Cairo.cairo_stroke(eventData);
                                
                                Cairo.cairo_arc(eventData, 224.4, 136.72, 30, 1.166667*Pi, 1.6777777*Pi);
                                Cairo.cairo_stroke(eventData);
                                
                                
                                //移动画笔点   画笔默认位置不是在（0,0）move_to不可少
                                Cairo.cairo_move_to(eventData, 180, 120);
                            //画一条线七点
                                Cairo.cairo_line_to(eventData, 180, 90);
                                Cairo.cairo_stroke(eventData);
                        }
                }, null);
                
                
        }
        
        public static void createDrawLaughFace(int window,int gridHouse,int start,final int circlePoint) 
        {
                //创建画图板 或者叫画布
                int  dan  = GTK.gtk_drawing_area_new();
                int label = GTK.gtk_label_new("笑脸");
                //创建源
                //添加控件
                GTK.gtk_grid_attach(gridHouse, label, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, dan, 1, start, 1, 1);
                //显示画布
                GTK.gtk_widget_show(dan);
                GTK.gtk_widget_show(label);

                
                //设置画布的大小
                GTK.gtk_widget_set_size_request(dan, 300, 300);
                
                
                //开始绘制  利用事件draw 来不断的绘制
                
                GTK.g_signal_connect(dan, "draw",new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //eventDate才是画布源
                                //设置画笔的颜色
                                Cairo.cairo_set_source_rgb(eventData, 0.5, 0.6, 0.3);
                                //设置画笔的大小
                                Cairo.cairo_set_line_width(eventData, 3);                        
        
                                //画一个圆
                                Cairo.cairo_arc(eventData, circlePoint, circlePoint, 60, 0, 2*3.1415926);
                                //画一个圆弧
                                //Cairo.cairo_arc(eventData, 200, 200, 70,1.5*3.1415925, 2*3.1415926);
                                //显示画笔
                                Cairo.cairo_stroke(eventData); //Cairo.cairo_fill(eventData)不同的效果
                                
                                
                                //画一个圆弧  有下面实验知道是从x轴沿顺时针开始画图

                                //嘴巴的绘制
                                double Pi = 3.1415926;
                                Cairo.cairo_arc(eventData, circlePoint, circlePoint, 40, 0.25*Pi, 0.75*Pi);
                                Cairo.cairo_fill(eventData);
                                
                                //绘制两只眼睛
                                Cairo.cairo_arc(eventData, circlePoint-20, circlePoint-20, 10, 0, 2*Pi);
                                Cairo.cairo_arc(eventData, circlePoint+20, circlePoint-20, 10, 0, 2*Pi);
                                Cairo.cairo_fill(eventData);
                        }
                }, null);
                
                
        }

}

```
case2  三毛头部自画像  加入了文字的绘制
```java
import com.rupeng.gtk4j.Cairo;
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

/**
* 
*/

/**
* @author 叶昭良
* @version GTKCairoText V1.0
*
*/
public class GTKTestCairoText
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                //GTK初始化
                GTK.gtk_init();
                //窗口标识
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                //窗口标题
                GTK.gtk_window_set_title(window, "简易画图板v2.0");
                //显示窗口
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
                
                
                //设置布局方式
                gridHouse = GTK.gtk_grid_new();
                
                
                //创建控件
                int start = 0;
                createDraw(window, gridHouse, start);
                //circlePoint 原点设置为180

                
                
                GTK.gtk_widget_show(gridHouse);
                
                GTK.gtk_container_add(window,gridHouse);
                //启动循环
                GTK.gtk_main();
        }
        
        public static void createDraw(int window,int gridHouse,int start) 
        {
                //创建画图板 或者叫画布
                int  dan  = GTK.gtk_drawing_area_new();
                
                int label = GTK.gtk_label_new("三毛");
                //创建源
                //添加控件
                GTK.gtk_grid_attach(gridHouse, label, 0, start, 1, 1);
                GTK.gtk_grid_attach(gridHouse, dan, 1, start, 1, 1);
                //显示画布
                GTK.gtk_widget_show(dan);
                GTK.gtk_widget_show(label);
                
                //设置画布的大小
                GTK.gtk_widget_set_size_request(dan, 300, 300);
                
                
                //开始绘制  利用事件draw 来不断的绘制
                
                GTK.g_signal_connect(dan, "draw",new IGCallBack() 
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                //eventDate才是画布源
                                //设置画笔的颜色
                                Cairo.cairo_set_source_rgb(eventData, 0.5, 0.6, 0.3);
                                //设置画笔的大小
                                Cairo.cairo_set_line_width(eventData, 3);
                                
                                
                                
                                //移动画笔点   画笔默认位置不是在（0,0）move_to不可少
                                //Cairo.cairo_move_to(eventData, 0, 0);

                                //画一个圆
                                Cairo.cairo_arc(eventData, 180, 180, 60, 0, 2*3.1415926);
                                //画一个圆弧
                                //Cairo.cairo_arc(eventData, 200, 200, 70,1.5*3.1415925, 2*3.1415926);
                                //显示画笔
                                Cairo.cairo_stroke(eventData); //Cairo.cairo_fill(eventData)不同的效果
                                
                                
                                //画一个圆弧  有下面实验知道是从x轴沿顺时针开始画图

                                //嘴巴的绘制
                                double Pi = 3.1415926;
                                Cairo.cairo_arc(eventData, 180, 180, 40, 0.25*Pi, 0.75*Pi);
                                Cairo.cairo_fill(eventData);
                                
                                //绘制两只眼睛
                                Cairo.cairo_arc(eventData, 160, 160, 10, 0, 2*Pi);
                                Cairo.cairo_arc(eventData, 200, 160, 10, 0, 2*Pi);
                                Cairo.cairo_fill(eventData);
                                
                                
                                //绘制胡须
                                Cairo.cairo_arc(eventData, 130, 160, 50, 0.166667*Pi, 0.6777777*Pi);
                                
                                Cairo.cairo_stroke(eventData);
                                
                                Cairo.cairo_arc(eventData, 230, 160, 50, 0.333333*Pi, 0.8333333*Pi);
                                Cairo.cairo_stroke(eventData);
                                
                                //绘制头发
                                Cairo.cairo_arc(eventData, 133.6, 136.72, 30,1.3333333*Pi ,1.833333*Pi );
                                
                                Cairo.cairo_stroke(eventData);
                                
                                Cairo.cairo_arc(eventData, 224.4, 136.72, 30, 1.166667*Pi, 1.6777777*Pi);
                                Cairo.cairo_stroke(eventData);
                                
                                
                                //移动画笔点   画笔默认位置不是在（0,0）move_to不可少
                                Cairo.cairo_move_to(eventData, 180, 120);
                            //画一条线七点
                                Cairo.cairo_line_to(eventData, 180, 90);
                                Cairo.cairo_stroke(eventData);
                                
                                
                                //画文字
                                Cairo.cairo_move_to(eventData, 180, 260);
                                Cairo.cairo_set_source_rgb(eventData, 0.4, 0.3, 0.2);
                                Cairo.cairo_set_font_size(eventData, 16);
                                //选择文字类型
                                Cairo.cairo_select_font_face(eventData, "宋体", Cairo.CAIRO_FONT_SLANT_ITALIC, Cairo.CAIRO_FONT_WEIGHT_BOLD);
                                Cairo.cairo_show_text(eventData,"三毛的自画像");
                                //Cairo.cairo_stroke(eventData);  //不需要这句话也可以
                        }
                }, null);
                
                
        }

}
```
case3  折线图  复习了数组  + 对称折线

```java
/**
* @author    叶昭良
* @time      2015年2月1日上午11:30:12
* @version   TestLine V1.0
*/
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;
import com.rupeng.gtk4j.Cairo;

public class TestLine
{

        /**
         * @param args
         */
        static int window;
        static int gridHouse;
        static int scrolledBar;
        public static void main(String[] args)
        {
                // TODO 自动生成的方法存根
                //初始化
                GTK.gtk_init();
                window = GTK.gtk_window_new(GTK.GTK_WINDOW_TOPLEVEL);
                //添加窗口
                GTK.gtk_widget_show(window);
                //安静关闭
                GTK.g_signal_connect(window, "destroy", new IGCallBack()
                {
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                GTK.gtk_main_quit();
                        }
                }, null);
/*                //创建布局：
                gridHouse = GTK.gtk_grid_new();*/
                
                int drawArea = GTK.gtk_drawing_area_new();
                GTK.gtk_widget_set_size_request(drawArea, 500, 500);
                //GTK.gtk_container_add(window, gridHouse);
                GTK.gtk_container_add(window, drawArea);
                GTK.g_signal_connect(drawArea, "draw", new IGCallBack()
                {

                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                Cairo.cairo_move_to(eventData, 0, 500);
                                int[] arrApple = {50, 90, 80, 120, 10, 50};
                                
                                // 获取最大值
                                int max = arrApple[0];
                                for(int i = 1; i < arrApple.length; i++)
                                {
                                        if(max < arrApple[i])
                                        {
                                                max = arrApple[i];
                                        }
                                }
                                // 
                                for(int i = 0; i<  arrApple.length; i++)
                                {
                                        // 获取坐标(x,y)
                                        int  x =  (i+1)*50;
                                        int  y =  500 - 500*arrApple[i]/max;
                                        // 绘制坐标点
                                        Cairo.cairo_line_to(eventData, x, y);
                                }
                                // 创建绘画
                                Cairo.cairo_stroke(eventData);
                        }
                        
                }, null);
                 createDraw( window, drawArea, gridHouse,0);
                GTK.gtk_widget_show(drawArea);
                //添加循环
                GTK.gtk_main();
        }
        
        public static void createDraw(int window, int drawArea,int gridHouse,int start)
        {
                GTK.g_signal_connect(drawArea, "draw", new IGCallBack()
                {

                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO 自动生成的方法存根
                                Cairo.cairo_move_to(eventData, 0, 0);
                                int[] arrApple = {50, 90, 80, 120, 10, 50};
                                
                                // 获取最大值
                                int max = arrApple[0];
                                for(int i = 1; i < arrApple.length; i++)
                                {
                                        if(max < arrApple[i])
                                        {
                                                max = arrApple[i];
                                        }
                                }
                                // 
                                for(int i = 0; i<  arrApple.length; i++)
                                {
                                        // 获取坐标(x,y)
                                        int  x =  (i+1)*50;
                                        //int  y =  300 - 300*arrApple[i]/max;
                                        int  y =  500*arrApple[i]/max;
                                        // 绘制坐标点
                                        Cairo.cairo_line_to(eventData, x, y);
                                }
                                // 创建绘画
                                Cairo.cairo_stroke(eventData);
                        }
                        
                }, null);
                //createScrolledBar(window,drawArea,scrolledBar,start);
        }
/*         //滚动条无法用于drawArea
        public static void createScrolledBar(int window,int drawArea,int scrolledBar,int start) 
        {
                scrolledBar = GTK.gtk_scrolled_window_new();
                GTK.gtk_widget_set_size_request(scrolledBar, 300, 100);
                GTK.gtk_widget_show(scrolledBar);
                GTK.gtk_container_add(scrolledBar,drawArea);
                GTK.gtk_grid_attach(gridHouse, scrolledBar, 0, start, 1, 1);
                
        }*/

}
```
![三毛的自画像](/images/java/sanmao.gif)
![三毛+笑脸](/images/java/jianyi.gif)
![图片按钮](/images/java/ButtonImage.gif)
![计算器v2.0 使用spinbox](/images/java/calc2.gif)
![spinbox的数字范围](/images/java/spinbox1.gif)
![女神告白神器2 加入了迭代插入](/images/java/nvshen2.gif)
![女神告白神奇 TextView的初次使用](/images/java/nvshen.gif)
![水果购买选择器](/images/java/shuiguo.gif)
![省市联动器](/images/java/province.gif)
![笑脸的绘制测量](/images/java/huizhide.png)
![对称折线图](/images/java/symmetry.gif)
