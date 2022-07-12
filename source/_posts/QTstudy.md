---

title: Qt学习第一节|Qt基础
date: 2022-06-26 21:36:11
tags: [Qt6.2.3，C++]
index_img: https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628200812272.png
banner_img: https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628200812272.png
categories: Qt学习
---



## Qt

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628200812272.png)

基于C++的跨平台的图形引擎

发行于1991年 ~~就记了个大概，不知道的咱可以百度不是~~

### 优点

1.跨平台

2.接口简单

3.一定程度简化了内存回收

### 案例

1.WPS

2.linux-KDE

3.vlc多媒体播放

## 创建第一个Qt

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628111650699.png)



系统环境：windows11
软件环境：Qt6.2.3(MSVC 2019 64bit)

#### 新建工程

一开始可以直接打开官方的Qt Creator，暂且不需要使用MSVS上进行调试开发，

```
file-> new project
```



#### 选择模板（tempates)

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628110514448.png)

默认**Qt widget application**（最基本的，也是最常用的窗口应用）

新建文件名和选择路径需要注意：（名字 路径，都不要有中文）

#### 选择构建系统（build system）

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628110646484.png)

因为做的是第一个程序，要快速上手选择qmake（Qt自带，不过个人建议cmake）。

#### 细节（details）

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628111032398.png)

名字都可以默认，也可以更改随喜好，注意事项（中文不行，空格禁止）

**重点在于Base class（基类）**

窗口类型介绍：QMainWindow、QWidget、QDialog三个类都可以用来创建窗口，可以直接使用，也可以继承后使用。

　　QMainWindow窗口包含菜单栏、工具栏、状态栏、标题栏等，是最常见的窗口形式，也可以说是GUI程序的主窗口。

　　QDialog是对话框窗口的基类。对话框主要用来执行短期任务，或者与用户进行互动，它可以是模态的，也可以是非模态的。他没有菜单栏、工具栏、状态栏等。

　　如果是主窗口，就使用QMainWindow类；

　　如果是对话框，就使用QDialog类；

　　如果不确定，有可能作为顶层窗口，也有可能嵌入到其他窗口，就使用QWidget类。

​		第一个程序我们可以默认直接选QWidget类

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628202512898.png)

这个是生成一个UI界面，一开始做第一个程序可以不选，因为需要学习一下UI底层的实现代码。

#### Kits 选择

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628202115437.png)

这里我因为安装时选择需要MSVC调试开发，所以会出现MSVC，默认有MInGW,一般可全选，创建第一个项目的时候不容易出错

#### summary

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628202912401.png)选择版本控制系统，一开始第一个程序用不上，实际是做大项目开发用的源代码管理工具选择，比如说我们常见的git ，svn等。具体可看这篇CSDN博文：[源代码管理工具 ](https://blog.csdn.net/weixin_45627194/article/details/110050361)

点击完成至此，创建一个Qt项目完成

#### 常用快捷键：

```
//ctrl / 注释
//ctrl r 运行
//ctrl b 编译
//ctrl i 自动对齐
//F4 快捷切换同名文件
```

### 生成的代码文件

### 需要掌握的：

main.cpp:

```c++
#include "widget.h"

#include <QApplication>
//argc为命令行变量的数量
//*argv变量的数组
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);//a即为应用程序对象，有且仅有一个
    Widget w;
    //窗口对象 （子类widger ：public QWidget）
    //默认不显示；
    //需要以下函数调用，调用的是顶层窗口函数
    w.show();

    return a.exec();
     //让应用程序对象进入消息循环机制
    //类似while（true）
    //触发条件退出循环结束程序
    //堵塞你的代码运行
    //与system("pause")作用类似

    //之后的代码依旧会顺序执行。
}

```



widget.cpp

```
#include "widget.h"
#include<QDebug>
#include<QPushButton>


Widget::Widget(QWidget *parent): QWidget(parent)//这不就是初始化列表么
{
    //创建一个按钮
    QPushButton *btn = new QPushButton;
    //btn->show();//单独弹出一个顶层的窗口来弹出窗口控件
    //所以我们需要将Btn对象依赖在Widget窗口中
    btn->setParent(this);
    //函数名直译，设置父母。

    //给按钮显示值
    btn->setText("第一个按钮");

//    创建第二个按钮
    QPushButton *btn2=new QPushButton("第二个按钮",this);//有参构造
    //但是有问题，只会按照空间的大小创建窗口
    //我们可以自己设定默认窗口大小
    //可修改
    resize(600,400);
   //我想要窗口固定大小，就需要
    setFixedSize(600,400);
    //同理，按钮需要定义大小也可以
    btn->resize(50,20);

    //但是运行还是只有一个按钮没因为默认位置将第一个按钮覆盖显示了，我们需要移动他
    btn2->move (100,100);
    //这样我们就可以看到两个按钮了，这个时候我也可以更改应用标题
    setWindowTitle("第一个Qt");


    //问题：
    //我们的按钮都是开辟于堆区，不用去考虑释放内存的问题吗
    //引入对象树的概念
    //setparent(this)
    //关键函数：
	//将这个类与类下的对象放入对象树中
	//析构的时候   --接以下注释--
}

Widget::~Widget()
{
    qDebug("父类的析构");
    
    //先执行自行添加的代码，然后
    //底层在释放这个类之前判断是否有子类，如果有就找到子类析构，再执行自添加的代码，再判断，
    //直到找不到子类，确认是最后一个子类后释放这个子类下的对象
    //所以就会出现，先输出父类析构代码再输出子类的析构代码的情况
}
```



#### 信号和槽

Qt的学习重点，Qt的引以为豪的部分。

怎了么理解信号与槽？

我们现看个例子：

我打开一盏灯，灯亮了

信号的发送者，发送具体信号，信号的接收者，槽函数：信号的处理

connect （信号的发送者，发送具体信号；信号的接收者，信号的处理）

信号槽的优点，松散耦合：可以理解为信号发送与接收者没啥关联，通过connect的链接两端耦合在一起

```c++
    //连接函数
    connect(mybtn  ,&QPushButton::clicked,this,&Widget::close);
    //参数1：信号发送者，参数2：信号这里填入地址；
    //参数3：信号接收者，参数3；同样是地址槽（槽函数，执行行为）
```

QT库中有现成的信号与槽函数，当然我们也可以自己写一个，具体实现：

更多内容在下一篇文章再说，~~还没写呢。先玩游戏去辣~~

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220628211036994.png)

