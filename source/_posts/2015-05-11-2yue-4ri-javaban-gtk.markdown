---
layout: post
title: "2月4日Java班GTK"
date: 2015-05-11 14:58:41 +0800
comments: true
categories: JavaBasic
---
<!--more-->


第一部分：笔记

1：封装两个不需要Id的类，OORGB,OOCairo
  OORGB用于设置颜色值，并封装几个常见的颜色OORGB.RED OORGB.BLUE OORGB.GREEN..
  OOCairo则是一个绘图类，主要包括移动点，画线，画圆，画矩形等。当然先前的GTK.Cairo类的方法中，都是包含环境句柄，所以OOCairo的封装需要考虑环境句柄，并在封装之后不需要再
调入环境句柄(已封装）

2：封装GtkWidget为OOWidget
最为关键的一步是设置Id的工作。 每一个句柄都具有一个对象标识，而Gtk+默认是通过一个
整数来表示对象标识，于是需要把这个值设置为私有的Id(估计想到这个得费点时间。。）于是
就有了getId和setId。这一步的作用影响到下节的进一步封装，
javadoc的注释： /** + enter键，会自动导入方法的参量和返回值以及类的创建者等
javadoc的生成： 项目--->生成javadoc--->填入javadoc路径
                        jd2chm+httphelper 转化为chm文档

3. 利用上述方法，让所有的控件都继承自OOWidget(现在想想有点不合理），封装了21个类
     包括：OOBox, OOButton,OOCairo,OOCalendar,OODialog,OOEntry,OOFileChooser,OOGrid
             OOLabel,OOMenu,OOProgressBar, OORGB,OOScrollBar,OOSpinBox,OOStatusIcon,
             OOSwitch,OOTextIter,OOTextView,OOToolBar,OOTreeView,OOWidget 
     并进行了一个简单测试 TestOOCairo.java

上面的内容构成了源码v1.0版

进一步发现源码V1.0存在很多问题，没有照顾到继承关系图。

后来继续学习了杨老师的视频，发现源码v1.0并未体现GTK+继承关系
如图GTK继承结构关系图.png
    由此可见，并不是所有的控件是直接继承OOWidget的，并考虑到以下bug情况
bug1:
     Protected是指兄弟和儿子可以访问！！ 也就是不同包的不可以访问。
为什么setId()最好改为protected呢？
因为我们经常是让一个包打成tar，然后别人引用这个jar包！ 也就是和这个jar包不一样
也就是他无法执行这样的操作
也就是 比如OOEntry oe = new OOEntry();
Oe.setId(GTK.gtk_label_new())的换心操作。 不要让entry变成了label而你却是不知道。

bug2： 
把OOWidget 标记为抽象类！ 这样防止用户
*                                      
  直接OOWidget ow = new OOWidget() ; ow.show()
*                                       这完全是无意义的工作
这就是抽象的好处！！ 让子类去做，父类不要去实例化了


bug3:

见图Container是只大老虎.png ,发现先前写的大部分控件的封装都是container的子类，于是新建了OOContainer类，并重新改写了OOBox，OOGrid,OOTextView,OOToolBar,OOTreeView ,（修改的只是把extends 改为了OOContainer，这个工作量不大，发现有效  其他构造函数不需要改，修改完毕）
并发现我并未封装combobox,和radiobutton之类的，checkbox
并最终还使得button在原先的两个构造函数的基础上增加了图片资源的加载，和加载图片的位置控制
增加了8个工作量。

http://blog.csdn.net/uunubt/article/details/6089402
参考此文，琢磨出来RadioButton的用法：
int cbMan = GTK.gtk_radio_button_new_with_label(0, "男");
                int cbWoman = GTK.gtk_radio_button_new_with_label_from_widget(cbMan, "女");


OK经过了努力终于完成此项工作的封装
（修改的只是把extends 改为了OOContainer，这个工作量不大，发现有效  其他构造函数不需要改，修改完毕）

后来发现这种该法其实有不恰当的地方，应该是可以省略掉构造函数。但是这是仅仅如果构造函数和OOContainer是一样的情况（还是涉及到构造函数可以重写的问题）

bug4:

后来发现bug3并不完善，观察继承图发现很多的控件都是来自Gtkbin，见GtkBin是只小老虎.png. 
由于GtkBin是单控件容器，于是采用杨老师针对OOContainer的 add方法的Override重写（GtkBin也是OOContainer的一部分）
Override方法：
```java
@Override
        public void add(OOWidget ow)
        {
                int[] children = GTK.gtk_container_get_children(this.getId());
                if(children.length <= 0 )
                {
                        //调用父类的add方法
                        super.add(ow);
                        //super.add  调用父类
                        //this.add   调用本类
                        //super() 调用父类构造方法(在构造函数中)
                        //this()  调用本类的构造方法(在构造函数中，记得我为什写上括号)
                }else
                {
                        throw new IllegalArgumentException("容器中已经有了一个对象");
                }
        }
```



bug5:
并进一步修改了GTKButton, GTKImageButton   GGTKComboBox   GTKToolItem  GTKWindow
学习了封装原则： 能省则省，尽量调用父类实现的代码，减少代码量，减少出错的语句
最终把代码封装到了31个，见图2.0的所有类文件。

下面是我代码的提交记录：
    1：重新封装了程序 根据GTK的继承关系  所有的控件都来自GTKwidget  GTKBin是指只能添加一个空间的类
    GTKContainer是包含着GTKBin  GTKGrid GTKBox，GTKToolBar,OOTreeView,OOTextView
    2. 封装了GTKImage
    3. 封装了GTKCheckButton和radioButton，并琢磨了GTKRadioButton的用法
    4. 添加了GTKBin和GTKWindow的封装，至此基本上所有的东西，都已经面向对象化了
    5. 测试了OOPassword之类的用法，使得OOPassword继承自OOEntry
    6. 添加了OOFileChooser的封装
    7  修改OOButton 使其继承自OOBin,也就是只能添加单一空间
    8. 使得OOWidget变得抽象画，这样就不会出现 定义一个OOWidget对象的额情况，使得程序更具可靠性
    9. 使得OOWidget的setId变成protected，这样只有子类和兄弟类（同包）可以使用，不同包的不可以使用。
    10：增加了OOCombo(从OOBin继承得到)类充当 OOCombobox的父类
    11: 把ComboBoxText的 addLister 添加到OOCombo类中！！
    12: 增加了OOToggleButton类
    13：修改了CheckBox使其充当OOToggleButtonde
    14： 修改了OOScrollBar的继承项 从OOWidget到OOContainer

上面的内容构成了GTKEncapsulateV2.0源码 


第二部分 作业


源码V1.0版本.rar
源码V2.0版本.rar
![gtk继承关系图](/images/java/gtk继承关系图.png)
![](/images/java/Container是只大老虎)
![Gtkbin是只小老虎](/images/java/Gtkbin是只小老虎.png)
![v2.0的所有类文件](V2.0继承图.png)



	Bug再修复1  menu的clicked事件改为activate(menu  item响应 activate事件），这个activate让我花费了至少两个小时，因为把所有的OOMenu的过程                  重新在书写了一遍，并拆成孤立的四个片段分别是： OOMenuBar.java（将包装添入包装条，菜单条）    OOSubMenu.java（把菜单包装）    OOSingleMenu.java （绘制菜单） OOMenuVegetable.java （绘制一盘菜）。具体的也在源代码第三个版本。


Bug修复2    文件过滤器的 抽象方法的返回值从void变为 String[]，这样可以返回字符串（比如打开文件夹的文件名）
      //public abstract void processResponse();
        public abstract String[] processResponse();

Bug修复3    FileChooser的run方法 从void 变到int,  这样可以接受返回值，判断对应的执行行为
Bug修复4    修改了OOTextView 使其从
```java
public void addScrollBar(int width, int height)
        {
                OOScrollBar osb = new OOScrollBar();
                osb.setWidgetSize(width, height);
                osb.addView(this.getId());
        }
```
变到
```java
public void addScrollBar(OOGrid og,int start,int width, int height)
        {
                OOScrollBar osb = new OOScrollBar();
                osb.setWidgetSize(width, height);
                osb.addView(this.getId());
                osb.show();
                og.add(osb.getId(), start);
                
        }
```
让textview自动可以添加滚动条。

bug5修复
   使OOToolBar里面的addTool方法从void方法改变到返回值为ToolButton
```java
public void addTool(String toolName,String toolPic,int position)
        {
                tbApple[i] = new ToolButton(toolName);
                tbApple[i].show();
                tbApple[i].setStock(toolPic);
                GTK.gtk_toolbar_insert(this.getId(), tbApple[i].getId(), position);
                i++;
                
        }
```
到
```java
public ToolButton addTool(String toolName,String toolPic,int position)
        {
                tbApple[i] = new ToolButton(toolName);
                tbApple[i].show();
                tbApple[i].setStock(toolPic);
                GTK.gtk_toolbar_insert(this.getId(), tbApple[i].getId(), position);
                i++;
                return tbApple[i-1];
                
        }
```
这样就可以添加事件监听。

bug6修复    OOWindow的ExitAfterDestroy 的返回值的设置
//也就是当收到 destroy信号的时候 不一定是关闭的（一般我们写的时候是关闭的）
```java
        public void setExitAfterDestroy(boolean value)
        {
                this.exitAfterDestroy  = true;
        }
```
到
```java
public void setExitAfterDestroy(boolean value)
        {
                this.exitAfterDestroy  = value;
        }
``



针对OOMenu针对两种封装方法进行了测试：
1：封装方法1(体现在TestMenuFileChooserToolbar.java)
     所有的方法 都浓缩到OOMenu中 ，包括第一步创建 menubar    第二步创建submenu     第三步创建 menu   第四步创建menuitem
2：封装方法2(体现在TestNewMenuFourStep)
     把原先的OOMenu 分为四个类   第一步 OOMenuBar类      第二步 OOSubMenu类     第三步   OOSingleMenu类    第四步  OOMenuVegetable类

从上面看到也许第二种方法更加体现着面向对象的方法，这也是被逼的，一直测试为什么menuitem的clicked方法不通过，原来是因为menuitem响应的是activate信号。

同时也可以看到面向对象方法的清晰性
封装方法1 的源代码 TestMenuFileChooseAndToolBar.java:

```java
package GTKEncapsulate;



import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

/**
* 
* @author    叶昭良
* @time      2015年2月6日下午8:13:45
* @version   GTKEncapsulateTestMenuFileChooserAndToolbar V1.0
*                                                         并进一步采用封装的OOWindow编写程序，由于继承了OOBin，所以具有add 一个控件的功能
*                                                         由于OOWindow在构造函数中添加了destroy的事件监听，所以不需要再重写，但是现在的
*                                                         OOWindow的对象还是可以添加destroy监听，要是可以不能再添加destroy就好了！！！
*                                 
*                                                         另外发现了一个不对的地方，所有的控件现在都是可以弹出消息对话框，question对话框。
* 
* 这个测试出现在事件监听！还有bug，等到以后再次修改，估计是继承关系出现了问题
*解决了这个问题，原来是因为menu的监听是通过activate而不是click
*涉及到的文件      OOMenu.java （这是主要的类包含另外一种方法的OOMenuBar OOSubMenu OOSingleMenu)
*              OOMenuVegetable.java （这是原先从OOMenu剥离出来的一个文件）
*/
public class TestMenuFileChooserAndToolbar
{
/*        public IGCallBack IGCQuit = new IGCallBack()
        {
                
                @Override
                public void execute(int instance, int eventData, Object object)
                {
                        // TODO Auto-generated method stub
                        GTK.gtk_main_quit();
                }
        };*/
        static OOTextView ootv =  null;
        public static void main(String[] args)
        {
                //初始化还是需要的
                GTK.gtk_init();
                //创建一个OOWindow对象，其中已经包含了  
                OOWindow window = new OOWindow();
                window.setTitle("测试面向对象");
                window.setExitAfterDestroy(true);
                window.show();
                
                
                //创建布局对象
                OOGrid grid = new OOGrid();
                grid.show();
                //添加到window对象
                window.add(grid);
                
                
                
                //创建一个textview 界面
                ootv = new OOTextView();
                ootv.setType();
                ootv.show();
                ootv.addScrollBar(grid, 2, 500, 300);
                //创建menubar对象
                OOMenu omMenu = new OOMenu();
                omMenu.show();
                
                //创建垂直menubar（最终需要文件的下拉菜单 添加到menubar(水平的menubar)
                omMenu.createVerticalMenu("File");
                //创建单一的一盘菜
                omMenu.createMenu();
                
                //往菜盘中添菜,改变了return选项 使得可以返回Vegetable对象，从而进行事件监听
                //由于已经在内部show了 所以不需要show
                OOMenuVegetable vgNew = new OOMenuVegetable("新建");
                OOMenuVegetable vgOpen = new OOMenuVegetable("打开");
                OOMenuVegetable vgSave = new OOMenuVegetable("保存");
                OOMenuVegetable vgQuit = new OOMenuVegetable("退出");
                vgOpen.addActivateListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                youOpenfile(ootv);
                                
                        }
                });
                vgSave.addActivateListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                youSavefile();
                                
                        }
                });
                vgQuit.addActivateListener( new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                GTK.gtk_main_quit();
                        }
                });
                omMenu.addMenuVegetable(vgNew);
                omMenu.addMenuVegetable(vgOpen);
                omMenu.addMenuVegetable(vgSave);
                omMenu.addMenuVegetable(vgQuit);
                
                //已经show了，唯一需要的是添加进网格
                omMenu.addVerticalMenuTOBar();
                
                grid.add(omMenu.getId(), 0);
                
                //上面的部分都是关于menu的现在做一个关于
                OOToolBar otbApple = new OOToolBar();
                //必须设置上 否则太小了
                otbApple.setWidgetSize(300, 20);
                OOToolBar.ToolButton tbApple = otbApple.addTool("新建", GTK.GTK_STOCK_NEW, 0);
                OOToolBar.ToolButton tbBanana =otbApple.addTool("打开", GTK.GTK_STOCK_OPEN, 1);
                OOToolBar.ToolButton tbOrange =otbApple.addTool("保存", GTK.GTK_STOCK_SAVE, 2);
                tbBanana.addClickedListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                youOpenfile(ootv);
                        }
                });
                tbOrange.addClickedListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                youSavefile();
                        }
                });
                otbApple.show();
                grid.add(otbApple.getId(),1);
                
        
                
                GTK.gtk_main();
                
        }
        
        public static void youOpenfile(OOTextView ootv)
        {
                OpenFile ofApple = new OpenFile();
                ofApple.setMultipleSelect();
                ofApple.createFilter();
                ofApple.nameFilter("文本文件");
                ofApple.editFilter("*.txt");
                ofApple.editFilter("*.java");
                //把filter的操作放入打开对话框中
                ofApple.finishFilter();
                String[] filenames = ofApple.processResponse();
                showAllFiles(filenames,  ootv);
        }
        public static void youSavefile()
        {
                SaveFile ofBanana = new SaveFile("保存文件",GTK.GTK_FILE_CHOOSER_ACTION_SAVE,"保存");
                //把filter的操作放入打开对话框中
                
                ofBanana.processResponse();
        }
        
        public static void showAllFiles(String[] filenames,OOTextView otv)
        {
                for(int i = 0 ; i < filenames.length; i++)
                {
                        showOneFile(filenames[i],otv);
                }
        }
        
        public static void showOneFile(String filename, OOTextView otv)
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
                        otv.insertTextAtEnd("******************\n当期文件为"+filename+"\n******************\n\n"+temp+" 文件内容如下：\n+---------------------------------------------------------------------------+\n");
                        String content = null;
                        while((content = br.readLine())!=null) // -1读取完毕
                        {
                                //InsertStringToTextViewFunction(textview,fileToFile.toString());
                                otv.insertTextAtEnd(new String(content));
                                
                        }
                        otv.insertTextAtEnd("\n+---------------------------------------------------------------------------+\n******************\n文件"+filename+"读取结束\n******************\n");
                }
                catch(IOException e)
                {
                        System.out.println("文件读入异常");
                }
                
        }
}


class OpenFile extends OOFileChooser
{

        @Override
        public  String[] processResponse()
        {
                String[] filenames = null;
                // TODO Auto-generated method stub
                int ret = this.run();
                if(ret == GTK.GTK_RESPONSE_OK) 
                {
                        filenames = GTK.gtk_file_chooser_get_filenames(this.getId());
                        for(int i = 0 ; i< filenames.length ; i++)
                        {
                                
                                System.out.println("选中文件名"+i+": "+filenames[i]);
                        }
                        GTK.gtk_widget_destroy(this.getId()); //必须需要！！否则报错
                }else
                {
                        GTK.gtk_widget_destroy(this.getId());
                }
                return filenames;
        }                
}

class SaveFile extends OOFileChooser
{
        public SaveFile(String title, int action, String buttonText)
        {
                setId(GTK.gtk_file_chooser_dialog_new(title, 0, action, buttonText));
        }
        @Override
        public String[] processResponse()
        {
                String[] filenames = null;
                GTK.gtk_file_chooser_set_do_overwrite_confirmation(this.getId(), true);
                // TODO Auto-generated method stub
                System.out.println("已进入save");
                //SaveOneFile(filename,textview);
                int ret = GTK.gtk_dialog_run(this.getId());
                if(ret == GTK.GTK_RESPONSE_CANCEL)
                {
                        GTK.gtk_widget_destroy(this.getId());
                }else 
                {
                        String filename = GTK.gtk_file_chooser_get_filename(this.getId());
                        filenames[0] = filename;
                        System.out.println(filename);
                        GTK.gtk_widget_destroy(this.getId());
                        return  filenames;
                }
                return filenames;
                
        }
}
```



封装方法2 的源代码TestNewMenuFourStep.java:
```java
package GTKEncapsulate;

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

/**
* @author    叶昭良
* @time      2015年2月6日下午10:09:33
* @version   GTKEncapsulateTestNewMenuFourStep V1.0
* 
* 涉及到四个类       OOMenuBar.java
*               OOSubMenu.java
*               OOSingleMenu.java
*               OOMenuVetetable.menu   依次创建四个类即可
*/
public class TestNewMenuFourStep
{

        /**
         * @param args
         */
        static OOTextView ootv =  null;
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                //初始化还是需要的
                                GTK.gtk_init();
                                //创建一个OOWindow对象，其中已经包含了  
                                OOWindow window = new OOWindow();
                                window.setTitle("测试面向对象");
                                window.setExitAfterDestroy(true);
                                window.show();
                                
                                
                                //创建布局对象
                                OOGrid grid = new OOGrid();
                                grid.show();
                                //添加到window对象
                                window.add(grid);
                                
                                //创建一个textview 界面
                                ootv = new OOTextView();
                                ootv.setType();
                                ootv.show();
                                ootv.addScrollBar(grid, 2, 500, 300);
                                //创建一个大的menubar
                                OOMenuBar ombApple = new OOMenuBar();
                                //创建第二大的（可以有很多个）
                                OOSubMenu osmApple = new OOSubMenu("File");
                                
                                //创建第三大的（可以有很多个）
                                OOSingleMenu menuApple = new OOSingleMenu();
                                
                                //往菜盘中添菜,改变了return选项 使得可以返回Vegetable对象，从而进行事件监听
                                //由于已经在内部show了 所以不需要show
                                //创建第四大的（可以有很多个）
                                OOMenuVegetable vgNew = new OOMenuVegetable("新建");
                                OOMenuVegetable vgOpen = new OOMenuVegetable("打开");
                                OOMenuVegetable vgSave = new OOMenuVegetable("保存");
                                OOMenuVegetable vgQuit = new OOMenuVegetable("退出");
                                vgOpen.addActivateListener(new IGCallBack()
                                {
                                        
                                        @Override
                                        public void execute(int instance, int eventData, Object object)
                                        {
                                                youOpenfile(ootv);
                                                
                                        }
                                });
                                vgSave.addActivateListener(new IGCallBack()
                                {
                                        
                                        @Override
                                        public void execute(int instance, int eventData, Object object)
                                        {
                                                youSavefile();
                                                
                                        }
                                });
                                vgQuit.addActivateListener(new IGCallBack()
                                {
                                        
                                        @Override
                                        public void execute(int instance, int eventData, Object object)
                                        {
                                                // TODO Auto-generated method stub
                                                GTK.gtk_main_quit();
                                        }
                                });
                                //添加第四大到第三大
                                menuApple.addVegetable(vgNew);
                                menuApple.addVegetable(vgOpen);
                                menuApple.addVegetable(vgSave);
                                menuApple.addVegetable(vgQuit);
                                
                                //添加第三大到第二大  采用的是填充
                                osmApple.fillSubMenu(menuApple);
                                //添加第二大到第一大
                                ombApple.addSubMenuToBar(osmApple);
                                ombApple.show();
                                
                                grid.add(ombApple.getId(), 0);
                                
                                
                                //上面的部分都是关于menu的现在做一个关于
                                OOToolBar otbApple = new OOToolBar();
                                //必须设置上 否则太小了
                                otbApple.setWidgetSize(300, 20);
                                OOToolBar.ToolButton tbApple = otbApple.addTool("新建", GTK.GTK_STOCK_NEW, 0);
                                OOToolBar.ToolButton tbBanana =otbApple.addTool("打开", GTK.GTK_STOCK_OPEN, 1);
                                OOToolBar.ToolButton tbOrange =otbApple.addTool("保存", GTK.GTK_STOCK_SAVE, 2);
                                tbBanana.addClickedListener(new IGCallBack()
                                {
                                        
                                        @Override
                                        public void execute(int instance, int eventData, Object object)
                                        {
                                                // TODO Auto-generated method stub
                                                youOpenfile(ootv);
                                        }
                                });
                                tbOrange.addClickedListener(new IGCallBack()
                                {
                                        
                                        @Override
                                        public void execute(int instance, int eventData, Object object)
                                        {
                                                youSavefile();
                                        }
                                });
                                otbApple.show();
                                grid.add(otbApple.getId(),1);
                                //启动循环
                                GTK.gtk_main();
        }
        
        public static void youOpenfile(OOTextView ootv)
        {
                OpenFileNew ofApple = new OpenFileNew();
                ofApple.setMultipleSelect();
                ofApple.createFilter();
                ofApple.nameFilter("文本文件");
                ofApple.editFilter("*.txt");
                ofApple.editFilter("*.java");
                //把filter的操作放入打开对话框中
                ofApple.finishFilter();
                String[] filenames = ofApple.processResponse();
                showAllFiles(filenames,  ootv);
        }
        public static void youSavefile()
        {
                SaveFileNew ofBanana = new SaveFileNew("保存文件",GTK.GTK_FILE_CHOOSER_ACTION_SAVE,"保存");
                //把filter的操作放入打开对话框中
                
                ofBanana.processResponse();
        }
        
        public static void showAllFiles(String[] filenames,OOTextView otv)
        {
                for(int i = 0 ; i < filenames.length; i++)
                {
                        showOneFile(filenames[i],otv);
                }
        }
        
        public static void showOneFile(String filename, OOTextView otv)
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
                        otv.insertTextAtEnd("******************\n当期文件为"+filename+"\n******************\n\n"+temp+" 文件内容如下：\n+---------------------------------------------------------------------------+\n");
                        String content = null;
                        while((content = br.readLine())!=null) // -1读取完毕
                        {
                                //InsertStringToTextViewFunction(textview,fileToFile.toString());
                                otv.insertTextAtEnd(new String(content));
                                
                        }
                        otv.insertTextAtEnd("\n+---------------------------------------------------------------------------+\n******************\n文件"+filename+"读取结束\n******************\n");
                }
                catch(IOException e)
                {
                        System.out.println("文件读入异常");
                }
                
        }

}
//取和OpenFile不一样的类名防止重复
class OpenFileNew extends OOFileChooser
{

        @Override
        public  String[] processResponse()
        {
                String[] filenames = null;
                // TODO Auto-generated method stub
                int ret = this.run();
                if(ret == GTK.GTK_RESPONSE_OK) 
                {
                        filenames = GTK.gtk_file_chooser_get_filenames(this.getId());
                        for(int i = 0 ; i< filenames.length ; i++)
                        {
                                
                                System.out.println("选中文件名"+i+": "+filenames[i]);
                        }
                        GTK.gtk_widget_destroy(this.getId()); //必须需要！！否则报错
                }else
                {
                        GTK.gtk_widget_destroy(this.getId());
                }
                return filenames;
        }                
}

class SaveFileNew extends OOFileChooser
{
        public SaveFileNew(String title, int action, String buttonText)
        {
                setId(GTK.gtk_file_chooser_dialog_new(title, 0, action, buttonText));
        }
        @Override
        public String[] processResponse()
        {
                String[] filenames = null;
                GTK.gtk_file_chooser_set_do_overwrite_confirmation(this.getId(), true);
                // TODO Auto-generated method stub
                System.out.println("已进入save");
                //SaveOneFile(filename,textview);
                int ret = GTK.gtk_dialog_run(this.getId());
                if(ret == GTK.GTK_RESPONSE_CANCEL)
                {
                        GTK.gtk_widget_destroy(this.getId());
                }else 
                {
                        String filename = GTK.gtk_file_chooser_get_filename(this.getId());
                        filenames[0] = filename;
                        System.out.println(filename);
                        GTK.gtk_widget_destroy(this.getId());
                        return  filenames;
                }
                return filenames;
                
        }
}
```


    基本上两种封装方法的思路一样，就书写来说，第二种方法，更加的清晰些。

运行界面一样：

![软件的运行界面](/images/java/filechooser1.png)
![利用工具栏打开一个文件](/images/java/filechooser2.png)
![利用菜单栏保存一个文档](/images/java/filechooser3.png)




2月7号的笔记：
再次介绍主要的控件的封装过程
OOWindow的封装思路
这个控件是OOBin的子类，也就是该控件只能容纳一个控件，为此在封装之前必须封装OOBin。
OOWindow类的封装主要包含 1：构造函数创建window并添加关闭的监听  2：设置window文本  3：设置window全屏 4：设置winow可以调整大小 5：设置window居中

有一点不明白的是：为毛非得有这个setExitAfterDestroy(boolean value)判断在window的关闭程序的事件监听，直接关掉不是大吉？！ 


OOFileChooser为什么把它封装成抽象类？
OOFileChooser的处理GTK.gtk_dialog_run的返回值的函数被我封装成一个抽象函数，如下

```java
/**
         *   一个抽象方法 ，要求继承者去实现它
         */
        public abstract String[] processResponse();
```
这样只要子类继承了OOFileChooser就让他去实现对应的processResponse方法，因为也许每次你打开文件的时候需要的处理都是有所不一样的并且为了能够让打开的时候保存多个文件名字，在保存的时候又不需要保存多个文件 ，新建了一个抽象类的方法针对savafile
        public abstract void processResponse1();  //用于保存
        public abstract String[] processResponse(); //用于打开

OOBin的封装思路
之前在介绍OOWindow中提及了OOBin的单容器功能，而OOBin是继承自OOContainer的，比如OOGrid,OOBox的都是继承自OOContainer的，但是他们都是可以容纳多个控件，为此必须重写OOBin的add方法。主要的作用是利用gtk_container_get_children判断是否当前的OOBin对象是否有多个控件，如果有了一个则 不能再添加，如果没有则可以添加
```java
@Override
        public void add(OOWidget ow)
        {
                int[] children = GTK.gtk_container_get_children(this.getId());
                if(children.length <= 0 )
                {
                        //调用父类的add方法
                        super.add(ow);
                        //super.add  调用父类
                        //this.add   调用本类
                        //super() 调用父类构造方法(在构造函数中)
                        //this()  调用本类的构造方法(在构造函数中，记得我为什写上括号)
                }else
                {
                        throw new IllegalArgumentException("容器中已经有了一个对象");
                }
        }
```
OOWidget的封装思路
OOWidget是祖宗级的任务，必须是大大滴。试想所有的控件都是可以显示、隐藏、摧毁，于是可以把这些函数的封装都容纳在OOWidget中，这是一个比较实际而又有效的封装。 然而是否所有的控件都有click事件的监听？ 这点我表示质疑，比如GtkMenuItem控件就没有，他只是响应activate事件，所以把click的事件监听放在OOWidget里面还是有点问题的。
另外地，把所有的基于OODialog(OODialog基于OOWindow)的消息对话框showInfo,问题对话框showYesNo,确认对话框showOkCancle放在OOWidget,从当前考虑也是可以的，但是封装的函数内部的实现还是面向过程，可不可以把MessageInfo类也可以提取出来，当然可以。
当然首先得把OODialog修改为继承自OOWindow！ 因为OODialog也是单容器控件。然后再把OOCalendar，OOFileChooser也修改为继承自OODialog
并新建OOInputDialog（暂时未能实现，com.rupeng.gtk4j没有）,OOMessageDialog  因为这些Dialog都有一个共同的特点需要GTK.gtk_dialog_run，可以把这个方法封装在OODialog中
```java
/**
         *    在一切设置完毕后  必须要让他run起来，类似于线程的做法,并且一定要摧毁它 this.destroy..
         */
        public  int run()
        {
                int response =  GTK.gtk_dialog_run(this.getId());
                return response;
        }
```
这样所有的OOdialog子类都可以run起来，并且都是单容器控件了。也算是一个bug修复。


OOBox容器的修改：
public OOBox(int orientation)的封装改为 this（orientation,0)的实现，这样如果以后需要修改GTK.gtk_box_new..只需要修改一个地方即可，不至于留下bug,此方法可以思考
```java
public OOBox(int orientation,int spacing)
        {
                setId(GTK.gtk_box_new(orientation, spacing));
        }
        /**
         * 
         * @param orientation   方向的说明
         */
        public OOBox(int orientation)
        {
                this(orientation,0);  //调用本类的构造函数
                //setId(GTK.gtk_box_new(orientation,0));
        }
```


至此  源码v4.0已经增加到37个类，并进行了4个简单的测试。



2月8号笔记：
  接口
接口其实就是一个头文件，提供方法的声明或者变量的声(）

接口文件仅仅是一个能力的声明，我能够做什么，具体实现还得自己去做。

一句话总结：接口定义做什么，实现接口能力的类定义怎么做
    使用：
```java
1：drawingCallback
public interface OODrawCallback
{
        public void drawCairo(OOCairo cairo);
//        public void speak();
}


```
使用接口：

```java
//使用
public void addDrawListener(final OODrawCallback ocbApple) //声明了我要调用一个接口，里头可以假设已经
  //实现了OODrawCallback的接口，并进行调用。 等到OODrawingArea这个对象生成，并添加了

  //draw监听器，传入接口对象时候，则需要具体去实现借口对象的方法（并且是所有方法）
        {
                GTK.g_signal_connect(this.getId(), "draw", new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                
//通过现象来解释的话，在一个类中方法，调用了接口的未实现的类，那么当你
//定义这个包含接口类方法的对象时候，需要实现这个接口的未实现类，因为
//类似于Throws的方法，我就让你调用这个类的对象去实现它，你们谁调用
//谁就给我去维护，我就提供一本秘籍，你自己去修炼！！！！当前的话eventData
                         
//小结：  在"draw"信号发出 执行了一个IGCallBack的接口，通过内部类实现，
//          在execute实现IGCallback接口的execute方法，主要是定义了一个Cairo对象
//          传进去了绘画环境，然后通过新建了一个OODrawCallback提供一个内容的接口
//          并调用了drawCairo，而由于drawCairo是未定义的方法，且是一个接口的未实现
//          方法，所以允许放在一个定义类中空着，
// 但是必须在存在未实现方法的类接口的对象去实现它（注意是对象中）
// 
// 猜想： 一个class的定义，不带有implemetns,那么其内部有一个变量  CanWithouImplement = true;
//       如果一个calss定义，带有implements,那么其内的  CanWithouImplement = false、
//      通过这个猜想来解释为什么我们下面在未实现drawCairo方法还是可以调用了drawCairo
// 但是有一句话是正确的，这边肯定是声明了具有drawCairo的能力。（具体什么能力不知道）
// 猜想： implements ocbApple是必须实现ocbApple的所有方法，而ocbApple.drawCairo仅仅需要实现一个方法。
//     错误：因为在一开头就定义了final  OODrawCallback ocbApple,形参定义了接口声明，调用的时候需要传入
//           接口对象（一般可以通过内部类实现)
//     测试方法： 通过在OODrawCallback增加一个未实现方法，发现还是需要实现两种未实现方法
//再次小结： 这边仅仅是一个形式上的定义！接口在形参变量中出现，则在调用具有该形参变量表的函数时候，必须
//         去实现他们，当然在该方法的定义中可以直接使用该方法。相当于是在该形式方法中事先隐式调用了
复制代码
接口调用的实现：
//创建绘图
                 //1 面向对象的方法
                OODrawingArea odaApple = new OODrawingArea();
                odaApple.show();
                //必须setSize否则看不到  这点一定要注意了
                odaApple.setWidgetSize(300, 300);
                gridApple.add(odaApple, 0);
                // 第一种情况重画： 画面被挡住了，揭开画面时候
                // 第二种情况重画： 打开画面饿时候
                odaApple.addDrawListener(new OODrawCallback()
                {
                        
                        @Override
                        public void drawCairo(OOCairo cairo)
                        {
                                // TODO Auto-generated method stub
                                cairo.setPenColor(OORGB.RED);
                                cairo.drawRectangle(20, 20, 100, 100);
                                cairo.fill();
                                cairo.drawCircle(200, 200, 50);
                                cairo.stroke();
                        }
                });
```

2：treeview当中的双击事件
```java
        public void addDoubleClickedListener(final IGCallBack callback)
        {
                GTK.g_signal_connect(this.getId(), "button-press-event", new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                if(GTK.gdk_event_get_type(eventData)==GTK.GDK_2BUTTON_PRESS)
                                {
                                        //调用时候 需要再次重写他。
                                        callback.execute(instance, eventData, object);
                                }
                        }
                }, null);
```
TestTreeView
定义了一个 TestOOTreeView otv = new TestOOTreeView(); //通过内部接口

```java
  otv.addDoubleClickedListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                int[] selection = otv.getMultipleSelectRows();
                                for(int i = 0; i < selection.length; i++)
                                {
                                        System.out.println("selection's length== "+ selection.length+"当前行"+selection[i]);
                                        //values[i]  = otv.getliststore().getValue(selection[i], 1);
                                        OOMessageDialog.showInfo("选中的信息为"+otv.getliststore().getValue(selection[i], 1), "用户名");
                                        System.out.println("selection["+i+"]"+selection[i]);
                                }
                        }
                });
```



接口的多态，比如Speakable sp = new People()

```java
/**
* 
*/
package InterfacePractice;

/**
* @author    叶昭良
* @time      2015年2月8日上午9:30:40
* @version   InterfacePracticeTestSpeackable V1.0
*/
public class TestSpeackable implements Speakable
{

        /**
         * @param args
         */
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                new TestSpeackable().speak();
                new TestSpeackable().look();
                //利用多态来实现        
                Speakable sp = new TestSpeackable();
                sp.speak();
                
                //利用匿名类的方式 是吸纳了Speakable接口，并立马返回
                Speakable sp1 = new Speakable()
                {
                        
                        @Override
                        public void speak()
                        {
                                // TODO Auto-generated method stub
                                System.out.println("这也可以");
                        }
                        
                        @Override
                        public void look()
                        {
                                // TODO Auto-generated method stub
                                System.out.println("这也可以");
                        }
                };
                
                sp1.speak();
                sp1.look();
                
        }

        @Override
        public  void speak()
        {
                // TODO Auto-generated method stub
                System.out.println("yamadie");
        }

        @Override
        public void look()
        {
                // TODO Auto-generated method stub
                System.out.println("带你去看星星");
        }

}
```


小技巧：
Ctr+H可以在workspace项目里面查找。。。
Ctrl+shift+T 显示workspace的类
Ctrl+T  O则显示当前文件夹下的类(ctrl+O列举当前类的所有成员)


枚举类
```java
package GTKEncapsulate;

import com.rupeng.gtk4j.GTK;

/**
* @author    叶昭良
* @time      2015年2月8日下午4:07:49
* @version   GTKEncapsulateOOResponseType V1.0  用途 简化对话框返回值的判断
*                                   V2.0 parseResponseType 替换掉parseInt这样更加符合常规
*                                        封装返回值的一个返回值，不至于说传递的值是非法（非法值有提醒）
*                                        增加程序的稳定性
*/
public enum OOResponseType
{
        //对话框几种可能的返回值类型
        //GTK_RESPONSE_HELP    GTK_RESPONSE_DELETE_EVENT   GTK_RESPONSE_NONE
        OK(GTK.GTK_RESPONSE_OK),  //最常用放在第一个
        YES(GTK.GTK_RESPONSE_YES),
        NO(GTK.GTK_RESPONSE_NO),
        CANCEL(GTK.GTK_RESPONSE_CANCEL),
        ACCEPT(GTK.GTK_RESPONSE_ACCEPT), //如果改为分号则报错
        APPLY(GTK.GTK_RESPONSE_APPLY),
        REJECT(GTK.GTK_RESPONSE_REJECT),
        NONE(GTK.GTK_RESPONSE_NONE),        
        HELP(GTK.GTK_RESPONSE_HELP),
        DELETE_EVENT(GTK.GTK_RESPONSE_DELETE_EVENT);
        
        
        private  int value = 0;
        /**
         * 
         * @param value  构造函数的参数
         */
        private OOResponseType(int value)
        {
                this.value = value;
        }
        /**
         * 
         * @return  返回OOResponseType对象的int值
         */
        public int getValue()
        {
                return this.value;
        }
        /**
         * 
         * @param value   带解析的数字 在OOResponse枚举类中代表的枚举值
         * @return        返回一个OOResponseType对象
         */
        public static OOResponseType parseResponseType(int value)
        {
                OOResponseType[] apple = OOResponseType.values();
                for(int i = 0 ; i < apple.length; i++)
                {
                        if(value == apple[i].getValue())
                        {
                                return  apple[i];
                        }
                }
                throw new IllegalArgumentException("ResponseValue = "+value+"是个不合法的参数");
        }
}
```


OODialog的封装修改
OODialog封装的思路：分成两个区域进行封装 actionarea     contentarea     Run函数（返回responseType枚举对象）  response（解析responsetype类型，就是你传给一个responseType类型的参数给你在对话框上面响应一个函数）。

涉及到OOContainer    定义了一个无参构造函数和一个有参构造函数；无参构造函数是为了防止子类报错，因为当父类定义了有参构造函数，
                  无参构造函数默认会被屏蔽，所以增加一个protected无参构造函数，可以防止类似于OOContainer的子类OOBin不会 报错。
                   默认OOBin会调用的构造函数是   public OOBin(){super();}  ;//而super默认即是指 OOContainer(){}如果没有
                   则会报错。
       OODialog
       OOResponseType --?涉及到枚举的知识-?枚举类的定义减少用户输错的可能性，增加程序的稳定性


还是得再次使用枚举类OOMESSAGEType 以及OOButtonsType来显示不同的对话框类型！  类似于OOResponseType（当时并不知道考虑去封装这一步 还一直傻呵呵利用GTK_MESSAGE_ERROR之类的）,封装了一个信息类

再次复习了OOResponseTyep的parseResponseType的方法实现。


TreeView的封装思路
1：MVC的V  建立一个View
2：MVC的M  建立一个OOListStore  和OOTreeIter(注意和OOTextIter的区别）
3：MVC的C  用于控制view和listmodel
详看练习TestOOTreeView

一种新的编程方式：玻璃出来业务逻辑从main函数中，减少界面的组合过程，main只是负责显示。

```java
/**
* 
*/
package GTKEncapsulate;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

/**
* @author    叶昭良
* @time      2015年2月9日下午1:33:31
* @version   GTKEncapsulateTestOOTreeView2Window V1.0
*/
public class TestOOTreeView2Window extends OOWindow
{

        /**
         * @param args
         */
        private static OOTreeView otv ;
        public TestOOTreeView2Window() 
        {
                
                //设置布局
                otv = new  OOTreeView();
                otv.addField("ID", 0);
                otv.addField("Name", 1);
                otv.addField("Age", 2);
                
                otv.createModel(3);
                otv.setFieldValue("001", "yezhao", "21");
                otv.setFieldValue("002", "huowa", "32");
                otv.setFieldValue("003", "yefeng", "31");
                OOScrollBar osb = new OOScrollBar();
                osb.show();
                osb.setWidgetSize(300, 300);
                osb.addView(otv);
                //otv.addScrollBar(grid, 0, 200, 200);
                
                //显示
                otv.fillModel();
                otv.show();
                otv.setRecordColumn(2);
                otv.setResizeColumn(2);
                otv.setColumnSort(2);
                otv.setMultipleSelect();
                System.out.println(otv.getliststore().getValue(1, 1));
                System.out.println(otv.getliststore().getValue(2, 1));
                System.out.println(otv.getliststore().getValue(2, 2));
                otv.addDoubleClickedListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                int[] selection = otv.getMultipleSelectRows();
                                for(int i = 0; i < selection.length; i++)
                                {
                                        //System.out.println("selection's length== "+ selection.length+"当前行"+selection[i]);
                                        //values[i]  = otv.getliststore().getValue(selection[i], 1);
                                        OOMessageDialog.showInfo("选中的信息为"+otv.getliststore().getValue(selection[i], 1), "用户名");
                                //        System.out.println("selection["+i+"]"+selection[i]);
                                }
                        }
                });
                this.add(osb);
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                GTK.gtk_init();
                //TestOOTreeView2Window 的封装的责任是新建字段 添加数据 加入监听  main不负责这些事情，main只是显示
                TestOOTreeView2Window ttv2w = new TestOOTreeView2Window();
                ttv2w.setExitAfterDestroy(true);
                ttv2w.show();
                GTK.gtk_main();
        }

}
```


2月8号练习：

1：学习了接口类，并利用接口类进一步学习了匿名类对接口类的实现，在多处体现，比如比如打开文件夹 的处理可以放在匿名接口类
      比如封装的treeView的双击时间的匿名接口类的实现,比如画图板的draw事件的匿名接口类的实现

2：封装了OODrawingArea  和 OODrawCallback（待实现drawCairo方法）接口，进一步理解了匿名接口类

3：学习了枚举类用于封装OOResponseTyep   OOMessageType  OOFileAction   OOButtonsType四个枚举类，
    这样可以减少用户输入参数的错误

4：封装了消息对话框  OOMessageDialog  ，包含showInfo  showError 等，兵删掉OOWidget中的showInfo等函数

5：继承OODialog实现了OOInputDialog，了解了ActionArea和contentArea

6: 修改了OOFileChooser的实现，利用OOFileAction枚举类在修改，并增加部分函数

7：修改了OOTreeView的实现，玻璃出OOListStore和OOlistIter这两个类，并进一步的在OOTreeView的内部中新建了
   两个内部类OOColumn  OOSelection，更加体现面向对象的过程。

8：修改了OOToolBar的实现，为了让外包可以访问工具栏的按钮，增加了一个共有的OOToolButton类

9：修改了OOContainer类的方法，利用对象直接添加。

10：增加了一个测试包TestGTKEncapsulate，用于测试。

源码V5.0版本 类增加到46个类，不包含内部类，并做了外包测试，修正了使用，已在TestGTKEncapsulate文件夹。

面向对象：让对象自己去做自己的内部逻辑，就好像我们自己去打扮我们自己的程序，main（杨老师）定义了一个对象，然后一个show而已。面向对象其实就是师傅领进门修行在个人。


2-9号笔记和练习
第一部分 笔记
1计时器的使用
```java
                GTK.g_timeout_add(1000, new IGSourceFunc()
                {
                        
                        @Override
                        public boolean execute(Object userdata)
                        {
                                // TODO Auto-generated method stub
                                getTime();
                                return true;
                        }
                }, null);
```

说明： 1000表示1000ms也就是1s
          IGS...是一个回调函数，当return为true时候继续监听，当return为false则停止计时器的即使，所以如果一直为true，则计时器一直再走。
2：打开音乐
GTK.mci_play(filename)  打开音乐
      GTK.mci_close(deviceID) 关闭音乐

3： OOToolButton 没有OOToolItem的 setToolTip的方法，但是在OOToolButton可以设置图片
并进行监听。OOToolButton暂时用的多。

4. OODialog response的作用
理解了OODialog的response的作用之后，重新修改了，OOInputDialog等继承自OODialog的空间。
  response是使得添加进dialog的控件能够发出某种信号！！ 当我们定义完之后，后期新建一个对象，并run之后，就会返回一个信号（可能来自原先的dialog的信号，也可能是按钮的信号，这边的按钮不再用clicked事件进行外部监听了，但是其实是封装在对应的dialog子类控件中，表面上是活的对话框的OOResponseTyep的信号，其实内部也是触发了clicked事件，只不过是在clicked事件中，调用response函数，发出对应的OOResponseTyep的信号）

5：LED时钟
a. 冒号和空图片的切换
```java
colon1.setResourceImage("ledclock//"+(secondGe%2==1?"colon":"empty")+".png");
```

b.LED的整点报时
```java
String[] table = {"零","壹","贰","叁","肆","伍","陆","柒","扒","玖"};
if( minute ==0 && second == 0)
                {
                        System.out.println("北京时间"+table[hourShi]+table[hourGe]+"整");
                        OOMessageDialog.showInfo("北京时间"+table[hourShi]+table[hourGe]+"整", "整点消息");
                }
```


6：三重button：OOCalendarButton的封装
  目的：简化开发的过程，提供一个按钮，并让用户选择日期，回馈到按钮的标签上
  设计的三个类：
     OOCalendar (继承自OOContainer)
     OOCalendarDialog(继承自OODialog) 
     OOCalendarButton(继承自OOButton)


OOCalendarDialog的封装和OOInputDialog的封装基本上思路一样。在actionaArea添加两个按钮，在contentArea添加一个OOCalendar，进行对应的事件监听。

一个小bug:(自己犯的一点小错误)
OOCalendarButton 的
```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年mm月dd日");

改为
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日");
```


一个重点：
    三种调用类私有变量date的方式
1 可以用  date = ocd.getDate();    直接写上date,那么编译器会从内往外一层一层走，直到
找到date,所以可能会造成外层需要被赋值的date并未被赋值，而里层不需要赋值的date反而
赋值了
2.  this.date  也可能会造成错误，比如在一个匿名类中，那么this.date的意思就是匿名类的date,如果匿名类没有定义date，则报错

3 OOCalendarButton.this.date = ocd.getDate();  //最好的一种方法 
//OOCalendarButton的this对象，直接找到该类中的类变量。

第二部分  练习

1： 计时器
case1  progereeBar 的安装条
```java
package GTKEncapsulate;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;
import com.rupeng.gtk4j.IGSourceFunc;

/**
* @author    叶昭良
* @time      2015年2月9日下午3:13:29
* @version   GTKEncapsulateTestOOProgressBar2Window V1.0
*/
public class TestOOProgressBar2Window extends OOWindow
{

        /**
         * @param args
         */
        private OOLabel  olApple = null;
        private OOProgressBar opbApple = null;
        private OOButton obApple = null;
        public  TestOOProgressBar2Window()
        {
                
                OOBox obtemp = new OOBox();
                obtemp.show();
                
                olApple = new OOLabel("0");
                olApple.show();
                obtemp.add(olApple);
                opbApple = new OOProgressBar();
                opbApple.setText("请稍等");
                opbApple.showText("请稍后");
                opbApple.show();
                obtemp.add(opbApple);
                
                obApple = new OOButton("安装");
                obApple.show();
                obtemp.add(obApple);
                obApple.addClickedListener(new IGCallBack()
                {
                        
@Override
public void execute(int instance, int eventData, Object object)
{
        // TODO Auto-generated method stub
        GTK.g_timeout_add(1000, new IGSourceFunc()
        {
                
                @Override
                public boolean execute(Object userdata)
                {
                        // TODO Auto-generated method stu
                        String txt = olApple.getText();        
                        
                        int apple = Integer.parseInt(txt);
                        
                        apple++;
                        olApple.setText(Integer.toString(apple));
                        double banana = apple*0.1;
                        if(banana== 1.0)
                        {
                                OOMessageDialog.showInfo("安装完成", "安装");
                        }
                        opbApple.setProgress(banana);
                        return banana<=1;
                }
        }, null);
}
                });
                
                this.add(obtemp);
                
                
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                GTK.gtk_init();
                TestOOProgressBar2Window top2w = new TestOOProgressBar2Window();
                top2w.show();
                top2w.setExitAfterDestroy(true);
                GTK.gtk_main();
        }

}
```


case2 LED灯+整点报时（未加上声音改用弹出对话框）
```java
package TestGTKEncapsulate;



import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGSourceFunc;

import GTKEncapsulate.*;

/**
* @author    叶昭良
* @time      2015年2月10日下午1:17:40
* @version   TestGTKEncapsulateLEDClock V1.0
*/
public class LEDClock extends OOWindow
{

        /**
         * @param args
         */
        private OOImage oiHourShi;
        
        private OOImage oiHourGe;
        private OOImage oiMinuteShi;
        private OOImage oiMinuteGe;
        private OOImage oiSecondShi;
        private OOImage oiSecondGe;
        private int hourShi;
        private int hourGe;
        private int minuteShi;
        private int minuteGe;
        private int secondShi;
        private int secondGe;
        private OOBox obApple;
        private OOImage colon1;
        private OOImage colon2;
        private OOLabel olDate;
        public LEDClock()
        {
                obApple = new OOBox();
                obApple.show();
                
                olDate = new OOLabel("");
                olDate.show();

                obApple.addWidget(olDate);

                oiHourShi = new OOImage();
                oiHourGe = new OOImage();
                colon1 = new OOImage();
                oiMinuteShi = new OOImage();
                oiMinuteGe = new OOImage();
                colon2 = new OOImage();
                oiSecondShi = new OOImage();
                oiSecondGe = new OOImage();
                obApple.addWidget(oiHourShi);
                obApple.addWidget(oiHourGe);
                obApple.addWidget(colon1);
                obApple.addWidget(oiMinuteShi);
                obApple.addWidget(oiMinuteGe);
                obApple.addWidget(colon2);
                obApple.addWidget(oiSecondShi);
                obApple.addWidget(oiSecondGe);
                getTime();
                
                GTK.g_timeout_add(1000, new IGSourceFunc()
                {
                        
                        @Override
                        public boolean execute(Object userdata)
                        {
                                // TODO Auto-generated method stub
                                getTime();
                                return true;
                        }
                }, null);
                this.setWidgetSize(200, 100);
                this.add(obApple);
        }
        
        public void getTime()
        {
                String[] table = {"零","壹","贰","叁","肆","伍","陆","柒","扒","玖"};
                
                Date date = new Date();
                SimpleDateFormat sdf = new SimpleDateFormat("yyyy年-MM月-dd日 ");
                String labelText = sdf.format(date);
                //System.out.println(labelText);
                olDate.setText(labelText);
                Calendar cl = Calendar.getInstance();
                cl.setTime(date);
                int hour = cl.get(Calendar.HOUR_OF_DAY);
                int minute = cl.get(Calendar.MINUTE);
                int second = cl.get(Calendar.SECOND);
                
                hourShi = hour/10;
                hourGe  = hour%10;
                minuteShi = minute/10;
                minuteGe = minute%10;
                secondShi = second/10;
                secondGe  = second%10;
                
                
                oiHourShi.setResourceImage("ledclock//"+hourShi+".png");
                oiHourGe.setResourceImage("ledclock//"+hourGe+".png");
                colon1.setResourceImage("ledclock//"+(secondGe%2==1?"colon":"empty")+".png");
                oiMinuteShi.setResourceImage("ledclock//"+minuteShi+".png");
                oiMinuteGe.setResourceImage("ledclock//"+minuteGe+".png");
                colon2.setResourceImage("ledclock//"+(secondGe%2==1?"colon":"empty")+".png");
                oiSecondShi.setResourceImage("ledclock//"+secondShi+".png");
                oiSecondGe.setResourceImage("ledclock//"+secondGe+".png");
                
                if( minute ==0 && second == 0)
                {
                        System.out.println("北京时间"+table[hourShi]+table[hourGe]+"整");
                        OOMessageDialog.showInfo("北京时间"+table[hourShi]+table[hourGe]+"整", "整点消息");
                }
        
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                GTK.gtk_init();
                LEDClock lc = new LEDClock();
                lc.show();
                lc.setExitAfterDestroy(true);
                GTK.gtk_main();
        }

}
```


3小兵聊天工具：
事先封装了键盘上面的ASCII码  为一个枚举类OOKeycode:
```java
package GTKEncapsulate;

/**
* @author    叶昭良
* @time      2015年2月9日下午9:28:07
* @version   GTKEncapsulateOOKeycode V1.0  封装大部分的GTK对于键盘识别的keycode
*/
public enum OOKeycode
{
        //字母
                A("键盘上的a",65),
                B("键盘上的b",66),
                C("键盘上的c",67),
                D("键盘上的d",68),
                E("键盘上的e",69),
                F("键盘上的f",70),
                G("键盘上的g",71),
                H("键盘上的h",72),
                I("键盘上的i",73),
                J("键盘上的j",74),
                K("键盘上的k",75),
                L("键盘上的l",76),
                M("键盘上的m",77),
                N("键盘上的n",78),
                O("键盘上的o",79),
                P("键盘上的p",80),
                Q("键盘上的q",81),
                R("键盘上的r",82),
                S("键盘上的s",83),
                T("键盘上的t",84),
                U("键盘上的u",85),
                V("键盘上的v",86),
                W("键盘上的w",87),
                X("键盘上的x",88),
                Y("键盘上的y",89),
                Z("键盘上的z",90),
        //数字
            NUM_0("键盘上的0和)",48),
            NUM_1("键盘上的1和!",49),
            NUM_2("键盘上的2和@",50),
            NUM_3("键盘上的3和#",51),
            NUM_4("键盘上的4和[        DISCUZ_CODE_6        ]quot;,52),
            NUM_5("键盘上的5和%",53),
            NUM_6("键盘上的6和^",54),
            NUM_7("键盘上的7和&",55),
            NUM_8("键盘上的8和*",56),
            NUM_9("键盘上的9和(",57),
        //比较重要的几个按键
            SHIFT("键盘上的SHIFT键",16),
            CTRL("键盘上的ctrl键",17),
            ALT("键盘上的ALT键",18),
            CAPS_LOCK("键盘上的大写键",20),
            ESC("键盘上的ESC键",27),
            TAB("键盘上的Tab键",9),
            RIGTH_SHIFT("键盘上右边的SHIFT键",161),
            RIGHT_CTRL("键盘上右边的ctrl键",163),
            RIGHT_ALT("键盘上右边的ALT键",165),
            WINDOWS("键盘上的WINDOWS键",91),
            FN("键盘上的FN键",17),
            BACKSPACE("键盘上的baskspace回退键",8),
            SPACE("键盘上的空格键",32),
        //方向键
            LEFT("键盘上的左方向键",37),
            UP("键盘上的上方向键",38),
            RIGHT("键盘上的右方向键",39),
            DOWN("键盘上的下方向键",40),

        //小键盘数字 当前情况下必须是在小键盘开启的情况下，否则会匹配错误
            SMALLBOARD_0("小键盘上的0和insert",96),
            SMALLBOARD_1("小键盘上的1和end",97),
            SMALLBOARD_2("小键盘上的2和向下方向键",98),
            SMALLBOARD_3("小键盘上的3和pgdn",99),
            SMALLBOARD_4("小键盘上的4和向左方向键",100),
            SMALLBOARD_5("小键盘上的5",101),
            SMALLBOARD_6("小键盘上的6和向右方向键",102),
            SMALLBOARD_7("小键盘上的7和home",103),
            SMALLBOARD_8("小键盘上的8和向上方向键",104),
            SMALLBOARD_9("小键盘上的9和pgdn",105),
        //小键盘上的+ - * /
            SMALLBOARD_PLUS("小键盘上的加号",107),
            SMALLBOARD_MINUS("小键盘上的减号",109),
            SMALLBOARD_MULTIPLY("小键盘上的乘号",106),
            SMALLBOARD_DIVIDE("小键盘上的除号",111),
        //小键盘上的enter和点号（delete),enter和主键盘一样
            SMALLBOARD_DELETE("小键盘上的点号",110),
            ENTER("小键盘上的ENTER以及主键盘的ENTER",13),

        //右手边的一些符号键
            SINGLEQUOTE("键盘上的双引号和单引号",222),
            SEMICOLON("键盘上的分号和冒号colon键",186),
            COMMA("键盘上的逗号键和<",188),
            POINT("键盘盘上的句号和>",190),
            SLASH("键盘上的/正斜杠表示除法和问号",191),
            BACKSLASH("键盘上的反斜杠\\和竖线",220),
            LEFTSQUARE("键盘上的左中括号和左大括号",219),
            RIGHTSQUARE("键盘上的右中括号和右大括号",221),
            
            DASH("键盘上的破折号和减号",189),
            EQUAL("键盘上的等号和加号",187),
            REVERSEQUOTE("键盘上的反引号和约等号",192),
            
          //一些不经常用的
//            PRINT("键盘上的PRINT",)
            PAUSE("键盘上的暂停键和break",19),
            DELETE("键盘上的delete和insert键",46),
            HOME("键盘上的HOME键",36),
            PAGEUP("键盘上的PAGEUP",33),
            PAGEDOWN("键盘上的PAGEDOWN",34),
            END("键盘上的END",35),
            NUMLOCK("键盘上的NUMLOCK",144),
            
            //F1--F12
            F1("键盘上的F1键",112),
            F2("键盘上的F2键",113),
            F3("键盘上的F3键",114),
            F4("键盘上的F4键",115),
            
            F5("键盘上的F5键",116),
            F6("键盘上的F6键",117),
            F7("键盘上的F7键",118),
            F8("键盘上的F8键",119),
            F9("键盘上的F9键",120),
            F10("键盘上的F10键",121),
            F11("键盘上的F11键",122),
            F12("键盘上的F12键",123),
            ;



        
        //ctrl shift tab caps_lock  , . ; / ' [ ]的
        private int keycode;
        private String keyName;
        private OOKeycode(String keyName,int keycode)
        {
                this.keyName = keyName;
                this.keycode = keycode;
        }
        
        public int getKeycode()
        {
                return this.keycode;
        }
        public String getKeyName()
        {
                return this.keyName;
        }
        
        public static OOKeycode parseOOKeycode(int keycode)
        {
                OOKeycode[] apples = OOKeycode.values();
                for(int i = 0 ; i < apples.length; i++)
                {
                        if(keycode == apples[i].getKeycode())
                        {
                                return apples[i];
                        }
                }
                throw new IllegalArgumentException(keycode+"是一个不合法的参数");
        }
}
```

修改了OOEntry, 添加一个键盘敲击响应事件
```java
public void addKeyPressListener(final IGCallBack callback)
        {
                GTK.g_signal_connect(this.getId(), "key-press-event", new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                callback.execute(instance, eventData, object);
                        }
                }, null);
        }
```
小兵聊天主程序

```java
package TestGTKEncapsulate;

import javax.xml.crypto.dsig.keyinfo.KeyValue;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

import GTKEncapsulate.OOContainer;
import GTKEncapsulate.OODialog;
import GTKEncapsulate.OOEntry;
import GTKEncapsulate.OOFileChooser;
import GTKEncapsulate.OOGrid;
import GTKEncapsulate.OOImage;
import GTKEncapsulate.OOKeycode;
import GTKEncapsulate.OOMessageDialog;
import GTKEncapsulate.OOMusic;
import GTKEncapsulate.OOMusicMode;
import GTKEncapsulate.OOScrollBar;
import GTKEncapsulate.OOTextView;
import GTKEncapsulate.OOWindow;
//import GTKEncapsulate.OpenFile;

/**
* @author    叶昭良
* @time      2015年2月9日下午8:20:47
* @version   TestGTKEncapsulateTestXiaoBingWindow V1.0
*                                                         V2.0  把所有的equalsIgnoreCase 改为contains 只需要字符串包含即可，
*/
public class TestXiaoBingWindow extends OOWindow
{

        /**
         * @param args
         */
        private  OOEntry oeApple ;
        private OOTextView otvApple;
        private OOImage oim;
        private OOMusic om;
        public TestXiaoBingWindow()
        {
                OOGrid grid = new OOGrid();
                oeApple = new OOEntry();
                oeApple.show();
                //oeApple.setText("请在这边输入文本：");
                otvApple = new OOTextView();
                otvApple.show();
                otvApple.setEditable(false);
                OOScrollBar osbApple = new OOScrollBar();
                osbApple.show();
                osbApple.setWidgetSize(300, 300);
                osbApple.add(otvApple);
                
                grid.add(osbApple, 0);
                grid.add(oeApple,1);
                grid.show();
                this.add(grid);
//                GTK.MCI_MODE_OPEN
                
                //设置监听
                oeApple.addKeyPressListener(new IGCallBack()
                {
                        
@Override
public void execute(int instance, int eventData, Object object)
{
        // TODO Auto-generated method stub
        int keycode = GTK.gdk_event_get_keycode(eventData);
        if(keycode == OOKeycode.ENTER.getKeycode())
        {
                
                String text = oeApple.getText();
                otvApple.insertTextAtEnd("你说:"+text+"\n");
                oeApple.setText("");
                //text.contains
                if(text.contains("你好啊"))
                {
                        otvApple.insertTextAtEnd("小兵说:"+"好你妹！\n");
                }else if(text.contains("脱衣舞"))
                {
                        otvApple.insertTextAtEnd("小兵说:"+"不跟你玩了\n");
                }else if(text.contains("今年几岁"))
                {
                        otvApple.insertTextAtEnd("小兵说:"+"女孩子的年龄不能随便告诉别人\n");
                }else if(text.contains("你是男的还是女的"))
                {
                        otvApple.insertTextAtEnd("Little Bing Said:"+"You guess\n");
                }else if(text.contains("让我们去外面玩吧"))
                {
                        otvApple.insertTextAtEnd("Little Bing Said:"+"去哪里玩？\n");
                }else if(text.contains("select"))
                {
                        OpenFile ofApple = new OpenFile();
                        ofApple.setMultipleSelect();
                        ofApple.createFilter();
                        ofApple.nameFilter("音乐文件");
                        ofApple.editFilter("*.MP3");
                        ofApple.editFilter("*.wav");
                        //把filter的操作放入打开对话框中
                        ofApple.finishFilter();
                        String[] filenames = ofApple.processResponse();
                        //在src文件夹下
                        om = new OOMusic(filenames[0]);
                        om.playOnce();
                }else if(text.contains("sing"))
                {
                        //在src文件夹下
                        om = new OOMusic("breathless.mp3",false);
                        om.playOnce();
                }else if(text.contains("pause"))
                {
                        //在src文件夹下
                        
                        om.pause();
                }else if(text.contains("resume"))
                {
                        //在src文件夹下
                        
                        om.pause();
                }else if(text.contains("close"))
                {
                        //在src文件夹下
                        
                        om.close();
                }else if(text.contains("show"))
                {
                        OODialog od = new OODialog();
                        OOContainer oct = od.createContentArea();
                        oim = new OOImage();
                        oim.setResourceImage("hunsha.jpg");
                        oim.setWidgetSize(200, 200);
                        oim.show();
                        oct.add(oim);
                        od.run();
                        od.destroy();
                }
                else if(text.contains("北京怎么样"))
                {
                        otvApple.insertTextAtEnd("Little Bing Said:"+"不怎么样，先这样，改天再聊\n");       
                }else
                {
                        otvApple.insertTextAtEnd("小兵说：没听见，再说一遍\n");
                }
        }
//                                System.out.println(OOKeycode.parseOOKeycode(keycode).getKeyName());
}
});
/*GTK.g_signal_connect(oeApple.getId(), "key-press-event", new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                int keycode = GTK.gdk_event_get_keycode(eventData);
                                
//                                System.out.println(keycode);
                                System.out.println(OOKeycode.parseOOKeycode(keycode).getKeyName());
                                //int value = GTK.gdk_event_get_keyval(eventData);
                                //String enterCharacterString= String.valueOf(keycode);
                                //System.out.println(enterCharacterString);
                                //char enterCharacter = enterCharacterString.charAt(0);
                                //String text = oeApple.getText();
                                //char lastchar = text.charAt(text.length()-1);
                                //System.out.println(text+"text length="+text.length());
//                                System.out.println("当前输入的字符"+enterCharacter+"的ascii码为:"+keycode+"\n并且的他的10进制数为"+value);
//                                OOMessageDialog.showInfo("当前输入的字符"+enterCharacter+"的ascii码为:"+keycode+"\n并且的他的10进制数为"+value, "学习ASCII码");
                                //System.out.println("当前输入的字符"+enterCharacter+"的ascii码为:"+keycode);
                                //OOMessageDialog.showInfo("当前输入的字符"+enterCharacter+"的ascii码为:"+keycode, "学习ASCII码");
                        }
                }, null);*/
}
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                GTK.gtk_init();
                TestXiaoBingWindow txbw = new TestXiaoBingWindow();
                txbw.setExitAfterDestroy(true);
                txbw.show();
                GTK.gtk_main();
        }
        class OpenFile extends OOFileChooser
        {

                @Override
                public  String[] processResponse()
                {
                        String[] filenames = null;
                        // TODO Auto-generated method stub
                        int ret = this.run().getValue();
                        if(ret == GTK.GTK_RESPONSE_OK) 
                        {
                                filenames = GTK.gtk_file_chooser_get_filenames(this.getId());
                                for(int i = 0 ; i< filenames.length ; i++)
                                {
                                        
                                        System.out.println("选中文件名"+i+": "+filenames[i]);
                                }
                                GTK.gtk_widget_destroy(this.getId()); //必须需要！！否则报错
                        }else
                        {
                                GTK.gtk_widget_destroy(this.getId());
                        }
                        return filenames;
                }

                @Override
                public void processResponse1()
                {
                        // TODO Auto-generated method stub
                        
                }                
        }
}
```



4 音乐的使用+定时器+图片空间+textview === 女神表达神器之爱的誓言

在敲完誓言之后，开始播放音乐
未修复的bug: 有时候读着读着文字就崩溃了， 另外加入背景音乐也不行。。。相当不稳定。
最大的bug:  memery  error.......
```java
package TestGTKEncapsulate;
import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGSourceFunc;

import GTKEncapsulate.*;

/**
* @author    叶昭良
* @time      2015年2月10日上午1:17:57
* @version   TestGTKEncapsulateTestGrilGift V1.0 女朋友表达神器
*/
public class TestGirlGift extends OOWindow
{

        /**
         * @param args
         */
        private OOImage oim ;
        private OOTextView otv;
        private OOMusic om ;
        public TestGirlGift() 
        {
                OOBox ob =new OOBox(false);
                ob.show();
                oim = new OOImage();
                oim.setResourceImage("yumufeng.jpg");
                oim.show();
                ob.addWidget(oim);
                otv = new OOTextView();
                otv.show();
                otv.setType();
                OOScrollBar osb = new OOScrollBar();
                osb.show();
                osb.setWidgetSize(200, 220);
                osb.add(otv);
                ob.addWidget(osb);
                this.add(ob);
                
                final StringBuilder love = new StringBuilder();
                // +号之后都会出现bug
                final String Lovewords = "欣然:\r\n谢谢你给我的所有关怀和理解，尤其是那些孤立无助的时刻你温暖的陪伴，它让我始终强大坚定!\r\n"
                    +"我要让你成为世界上最幸福的女人,不是因为这一生积累的名望,地位与财富,而仅仅因为我默默恒久的爱!\r\n"
                    +"今天,说出这些话语是那么艰难却又那么快乐,这都是我这么长时间以来埋在心底的话语!这一切只是因为下面我要唱给"
                    +"你听的这首歌的名字:我如此爱你!\r\n落款人：叶昭良 ";
                love.append(Lovewords);
        //这个过程是慢慢地，但是一直持续的在运行
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
                                        om = new OOMusic("我如此爱你.mp3",true);
                                        om.playRepeat();
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
                
                //System.out.println("134");
/*                if(otv.getText().length() == Lovewords.length()-1)
                {
                        System.out.println("12");
                        OOMusic om = new OOMusic("我如此爱你.mp3",true);
                        om.playRepeat();
                }*/
                //不能放在里面。。。
                //暂时有问题  添加报错   只好让他在写字的时候专心写字，不放背景音乐
/*                om = new OOMusic("ISurrender.mp3",true);
                om.playOnce();*/
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                GTK.gtk_init();
                TestGirlGift tgg = new TestGirlGift();
                tgg.setTitle("女神表达神器之爱的誓言");
                tgg.show();
                tgg.setExitAfterDestroy(true);
                GTK.gtk_main();
        }

}
```



5OOCalendarButton的创建

具体的OOCalendarDialog   OOCalendarButton的封装详见   GTKEncapsulate源码6.0

```java
package TestGTKEncapsulate;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

import GTKEncapsulate.*;

/**
* @author    叶昭良
* @time      2015年2月10日下午2:48:55
* @version   TestGTKEncapsulateTestCalendar V1.0
*/
public class TestCalendar extends OOWindow
{

        /**
         * @param args
         */
        private OOCalendar ocl ;
        private OOCalendarDialog ocld;
        private OOBox obApple;
        private OOButton btnclick;
        private OOCalendarButton ocbApple;
        public TestCalendar()
        {
                ocl = new OOCalendar();
                ocl.show();
                
                btnclick = new OOButton("点击");
                btnclick.show();
                btnclick.addClickedListener(new IGCallBack()
                {
                        
                        @Override
                        public void execute(int instance, int eventData, Object object)
                        {
                                // TODO Auto-generated method stub
                                ocld = new OOCalendarDialog();
                                ocld.show();
                        }
                });
                
                ocbApple = new OOCalendarButton("选择日期");
                ocbApple.show();
                
                
                obApple = new OOBox();
                obApple.addWidget(ocl);
                obApple.addWidget(btnclick);
                obApple.addWidget(ocbApple);
                obApple.show();
                
                System.out.println(ocl.getDay()+ocl.getMonth()+ocl.getYear());
                this.add(obApple);
        }
        public static void main(String[] args)
        {
                // TODO Auto-generated method stub
                GTK.gtk_init();
                TestCalendar tc = new TestCalendar();
                tc.show();
                tc.setExitAfterDestroy(true);
                GTK.gtk_main();
        }

}
```


当前主要的OO库加上枚举类总的有56个，V6.0主要增加了  OOImageButton , OOKeycode,OOMusic, OOMusicMode ,OOMusicStatus,OOStockImage, OOStockSize,OOCalendarDialog，OOCalendarButton

GTKEncapsulate源码V6.0.rar




测试了一个toolbar控件，修正了ToolButton的构造函数没有设置ID的重大的bug

修改的代码：
```java
public class OOToolButton extends OOContainer
{
        /**
         * V2.0  新的构造方法
         * @param icon_widget
         * @param label
         */
        public OOToolButton(OOWidget icon_widget,String label)
        {
                //如果为0 则工具栏控件没有图片
                int icon_widgetID = (icon_widget==null)?0:icon_widget.getId();
                //GTK.gtk_tool_button_new(icon_widgetID, label) 已添加setId否则报错
                setId(GTK.gtk_tool_button_new(icon_widgetID, label));
        }
```


测试代码就是之前2月6号的TestMenuFileChooseAndToolBar.java
因为采用了新的封装的ToolButton类，所以重新测试，发现了在ToolButton类构造函数缺失setId()的问题，特此修正


OOFileChooser为抽象类，使用起来不是特别舒服，于是改变成为非抽象的，并提供一个接口，供用户使用。
具体OOFileChooser的修改如下：
县创建一个OOFileChooserInterface接口：
```java
/**
* 
*/
package GTKEncapsulate;

/**
* @author    叶昭良
* @time      2015年2月13日下午2:13:25
* @version   GTKEncapsulateOOFileChooserInterface V1.0
*/
public interface OOFileChooserInterface
{
        public void doit();
}
```



然后在OOFileChooser类中开放接口并加入默认的两个处理打开和保存的函数：

```java
package GTKEncapsulate;

import java.io.BufferedWriter;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.io.OutputStreamWriter;

import com.rupeng.gtk4j.GTK;
import com.rupeng.gtk4j.IGCallBack;

/**
* @author    叶昭良
* @time      2015年2月5日下午4:38:57
* @version   GTKEncapsulateOOFileChooser V1.0 把类标记为abstract，
*                                 这样的类无法被直接实例化（new），这就叫抽象类。
*  V2.0   修改了run方法
*  V3.0   抽象方法的返回值从void 该到了String[]
*  v4.0   修改OOFileChooser继承类为来自OODailog
*         并把public int run()方法提到OODialog中
*  V5.0   利用OOFileAction枚举类 重新编写了OOFIleChooser
*  V6.0   创建了setDoOverWrittenConfirmatio函数，并在封装构造函数中
*         运用了一个判断技巧，增加了 getFileName()  setCurrentFilename
*         createFileFolder 
*  V7.0   从抽象类转变到非抽象类
*         增加了一个文件接口 OOFileChooserInterface 供用户使用
*         增加了打开文件和保存文件的默认处理方式
*/
public  class OOFileChooser extends OODialog
{
        private static Filter fi ;
        //private static int response;  //对话框的响应
        /**
         * 
         * @param title     有参构造函数的窗口标题
         * @param action    有参构造函数的 行为，打开、保存。。
         * @param buttonText  有参构造函数的 按钮的标签名字 
         *  创建一个文件对话框，对话框一定要run一下。。。。。 并且一定要摧毁它
         */
/*        public OOFileChooser(String title, int action, String buttonText)
        {
                setId(GTK.gtk_file_chooser_dialog_new(title, 0, action, buttonText));
        }*/
        public OOFileChooser(String title, OOFileAction action, String buttonText)
        {
                setId(GTK.gtk_file_chooser_dialog_new(title, 0, action.getValue(), buttonText));
                //一个封装技巧。
                if(action ==OOFileAction.SAVE)
                {
                        setDoOverWrittenConfirmation(true);
                }
                
        }
        
        /**
         *   无参构造函数  默认为打开文件对话框说
         */
        public OOFileChooser()
        {
                this("打开文件", OOFileAction.OPEN, "打开");
        }
        /**
         *   设置文件打开窗口的多选
         */
        public void setMultipleSelect() 
        {
                GTK.gtk_file_chooser_set_select_multiple(this.getId(), true);
        }
        /**
         * 
         * @param do_overwrite_confirmation  在save模式下，保存时需要判断是否覆盖
         */
        public void setDoOverWrittenConfirmation(boolean do_overwrite_confirmation)
        {
                GTK.gtk_file_chooser_set_do_overwrite_confirmation(this.getId(), do_overwrite_confirmation);
        }
        /**
         * 
         * @param text  设置当前选择的文件名为text
         */
        public void setCurrentFilename(String text)
        {
                GTK.gtk_file_chooser_set_current_name(this.getId(), text);
        }
        
        /**
         * 
         * @param create_folders  布尔值 用于判断是否创建。。。不清楚这边。
         */
        public void createFileFolder(boolean create_folders)
        {
                GTK.gtk_file_chooser_set_create_folders(this.getId(), create_folders);
        }
        /**
         *   创建一个过滤器
         */
        public void createFilter()
        {
                fi = new Filter();
        }
        /**
         * 
         * @param text     设置过滤器的名字
         */
        public void nameFilter(String text)
        {
                fi.setFilterName(text);
        }
        /**
         * 
         * @param pattern   增加过滤器的后缀
         */
        public void editFilter(String pattern)
        {
                fi.addFilterPattern(pattern);
        }
        
        public void finishFilter()
        {
                GTK.gtk_file_chooser_add_filter(this.getId(), fi.getId());
        }
        

        
        
        public String[]  getFileNames()
        {
                return GTK.gtk_file_chooser_get_filenames(this.getId());
        }
        
        public String  getFileName()
        {
                return GTK.gtk_file_chooser_get_filename(this.getId());
        }
        /**
         *   一个抽象方法 ，要求继承者去实现它
         *   多了一个
         *   
         *   V 可不可以用接口来实现，估计也是一样的下过
         */
        /**
         * 
         * @param ofci  开放了一个OOFileChooserInterface接口，
         *              用于处理打开文件和保存文件，需要进行的额外操作
         */
        public void processResponse(final OOFileChooserInterface ofci)
        {
                ofci.doit();
        }
        /**
         *   默认处理打开的函数的方式，得到文件！！ 显示到textview和treeview交给用户去做
         *   而processSave则交给OOFileChooser的processSave去做
         *   新版本改用OOResponseTyep的类型进行判断
         */
        public String[] processOpen()
        {
                String[] filenames = null;
                // TODO Auto-generated method stub
                OOResponseType ret = this.run();
                //int ret = this.run().getValue();
                if(ret == OOResponseType.OK) 
                {
                        filenames = GTK.gtk_file_chooser_get_filenames(this.getId());
                        for(int i = 0 ; i< filenames.length ; i++)
                        {
                                
                                System.out.println("选中文件名"+i+": "+filenames[i]);
                        }
                        //GTK.gtk_widget_destroy(this.getId()); //必须需要！！否则报错
                        this.destroy();
                }else
                {
                        //GTK.gtk_widget_destroy(this.getId());
                        this.destroy();
                }
                return filenames;
        }
        /**
         *   默认处理保存对话框的方式   基本上默认是大部分采用的方式，
         *   保存文件的打开文件流有OOFileChooser去做
         *   
         *   暂时直接接受textview的文本信息，类似的可以使用OOTreeview
         *   
         *   新版本改用OOResponseTyep的类型进行判断
         */
        public void processSave(OOTextView ootv)
        {
                //GTK.gtk_file_chooser_set_do_overwrite_confirmation(this.getId(), true);
                // TODO Auto-generated method stub
                System.out.println("已进入save");
                //SaveOneFile(filename,textview);
                OOResponseType ret = this.run();
                //int ret = GTK.gtk_dialog_run(this.getId());
                if(ret == OOResponseType.CANCEL)
                {
                        this.destroy();
                        //GTK.gtk_widget_destroy(this.getId());
                }else 
                {
                        String filename =this.getFileName() ;
                        //保存文件
                        SaveOneFile(filename,ootv);
                        System.out.println(filename);
                        //GTK.gtk_widget_destroy(this.getId());
                        this.destroy();
                        
                }
        }
        
        
        public static void SaveOneFile(String filename, OOTextView ootv)
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
                        
                        /*int textBuffer =  GTK.gtk_text_view_get_buffer(textview);
                        //这是一个空的iter，需要用textBuffer进行赋值
                        int textIter = GTK.gtk_text_iter_new();  
                        
                        GTK.gtk_text_buffer_get_end_iter(textBuffer, textIter);
                        // 或者textview的textBuffer的末尾！
                        String tempText = GTK.gtk_text_buffer_get_text(textBuffer);*/
                        
                        //while(tempText != null) // -1读取完毕
                        {
                                //InsertStringToTextViewFunction(textview,fileToFile.toString());
                                bw.write(ootv.getText());
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
        
        
/*        public abstract void processResponse1();  //用于保存
        public abstract String[] processResponse(); //用于打开
*/        /**
         * 
         * @author    叶昭良
         * @time      2015年2月5日下午5:07:17
         * @version   GTKEncapsulateFilter V1.0
         */
        class Filter extends OOWidget
        {
                public Filter()
                {
                        setId(GTK.gtk_file_filter_new());
                }
                /**
                 * 
                 * @param text   设置过滤器的名字
                 */
                public void setFilterName(String text)
                {
                        GTK.gtk_file_filter_set_name(this.getId(), text);
                }
                /**
                 * 
                 * @param pattern  设置过滤器的过滤类型
                 */
                public void addFilterPattern(String pattern)
                {
                        GTK.gtk_file_filter_add_pattern(this.getId(), pattern);
                }
                
                
        }
}
```


附上 最新版本的封装GTK的源代码： 感兴趣的鹏友，可以下载交流哈。
最新的提交记录：1.修正了OOToolButton 缺少setId的一个重大bug
2.原先的OOFileChooser为抽象类，看起来不爽，于是再给OOFileChooser配置了一个接口和两个默认的处理函数
   一个用于打开一个用于关闭
3.修改了项目文件相关的  TestXiaoBingWindow.java  TestNewMenuFourStep.java  
4.利用OOResponseTyep的判断方式修改代码
基本测试通过，同时顺便复习了内部类的接口类的实现和在一个类中添加接口的方式。
具体可以参看本贴第一次讲过的关于内部类实现接口的问题包含OODrawCallback和OOTreeView的双击事件的内部类的实现。
