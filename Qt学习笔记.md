# Qt运行环境

Qt：跨平台的C++开发库，开发GUI（graphical user interface）
		支持系统：PC、手机系统、嵌入式系统、stm32都可以     

Centos下：打开creater：  /opt/Qt5.12.9/Tools/QtCreator/bin/qtcreator.sh &  （加个&表示在后台运行）
找不到lGl：ln -s /usr/lib64/libGL.so.1 /usr/lib/libGL.so创建一下连接即可
			https://blog.csdn.net/qq_21438461/article/details/132913923



打开已有过程：打开pro文件即可

# Qtcreater

1.创建时的父类：

QMainWindow：具有一个菜单栏和一个状态栏
QWidget：空白窗口，仅有一个关闭按钮
QDialog：只有ok和cancel两个选择按钮

2.QT的模块：
QT   += core gui， 		其他模块，可以在官网相应的版本的文档中的Moduls中查看
		在pro文件添加完，使用时需要在头文件中include，下面对应的文件与编译的一一对应
		增加 TARGET = out_name  即可修改可执行文件名字

3.启动流程
int main(int argc, char *argv[])  argc是命令行参数个数，argv是命令行参数
QApplication针对Qwidgt应用程序、QGuiApplication非Qwidgt程序、QCoreApplication（无界面的应用程序）
		都是管理QT的运行、设置Qt应用程序

4.快捷键（指南里有）

​		ctrl+滚轮修改界面、对其代码：ctr+a  +  ctr+i、   ctr+函数（转到定义）

​		在h文件声明自定义成员函数，右键refector，在cpp文件中创建

​		F4回到头函数、Help-Index里查看帮助文档

​		F4切换很好用

​		ctr+点击可以选中多个控件（不好点，点右侧名字也行）

5.Qt编程规范：

​	文件命名都是小写、类名单词首字母大写、成员函数小写、一个类一个h/c文件

​		（ctr+N ，creater里面会同时创建，选择QObject）





## UI设计器

双击form下的.ui文件即可

​		要先选中控件：gemetry坐标位置，font字体大小

## Qt信号槽

（signal slots）

​	槽连接模型：发送者对象button-信号（函数click）、 接收者对象window（槽close：函数）

3种建立方式

右键go to slot可以进入成员函数

​		#include<QDebug>，  qDebug() << "buttom执行了" <<endl;终端打印

connect(  ui->pushButton, SIGNAL(click()), this , SLOT(close()) );  信号槽的代码实现

​		一个信号可以连接多个槽、多个信号也可以连接一个槽

自定义：（ctr+N ，creater里面会同时创建，选择QObject）

## 无UI文件编程

​		添加器件的头文件

//ui->setupUi(this);可以吧这行注释掉

​		以按键为例button = new QPushButton(this);即可显示在窗口上

    button->show();
    button->setText("按钮");
    button->setGeometry(50,150,100,50);
    this->resize(800,480);

## Object Tree（对象树）

设置父对象：

​	窗口之间需要相互联系，比如A显示在B上面，A需要指定B为父对象

```
方法一：button = new QPushButton(this);
方法二：button->setParent(this);
```

对象树机制（方便管理内存）：先析构子对象，才析构父对象



## 添加资源文件：

ctr+n 选择新建Qt文件resource文件（右键qrc文件、open in editor）

设置prifix前缀（也就是路径的地址）、保存文件（一定要ctl+s保存，不保存不显示）



## 样式表

自定义小部件的外观、ui界面、要右边选中！！否则可能选到主窗口右键sheetstyle

可以查看参考文档，stylesheet（注意是小写），找到需要的类(以QLabel为例)

QLabel{
     border-image: url(images/welcome.png);
 } 

无ui操作：

​	this->styleStyleSheet("QWidget{background-color:red; }  ")；



补充：

​		去除边框 {   border：none；  }

​		固定大小，最大最下尺寸调成一样



## qss文件

Qt应用程序相关的样式表文件、纯文本格式

先建好resource文件后，在里面右键添加新的resource文件并取名style.qss文件，生成的代码要删除

在main函数里：需要进行文件操作

```
#include<QFile>
QFile file(":/style.qss");
if(file.exists())
{
file.open(QFile::ReadOnly);
QString styleSheet = QLatin1String(file.readAll());  //以字符串方式保存读取结果
qApp->setStyleSheet(styleSheet);
file.close();
}
```

//ui->setupUi(this);可以吧这行注释掉进行无ui配置

注意，style.qss文件里面只留下    QLabel {background-color:red}  

# 控件

## 容器类

### QWidget

​		常用于顶层小部件，所有ui里的控件都继承于QWidget（再继承于QObject等类）

​		一般用来作背景容器：QWidget{background-color:  rgb(220,220,220)}，在上面放一些控件



### QDialog

对话框（类似于输入支付密码）

不在控件组，需要代码编辑（与Widget一样，三种窗口之一）

去除边框：setWindowFlag(Qt::FramelessWindowHint);

​						setAttribute(Qt::WA_TranslucentBackground);//透明化

```
主要代码：
#include<QDialog>
QDialog *dialog;

dialog = new QDialog(this);
dialog->setModal(true);//点击后其他地方不能点了

dialog->show(); //可以作为button的槽函数
```

没法修改圆角，可以加一个QFrame控件，

QFrame{background-color:white;
border-radius:25px}  就类似达到圆角设置



### QScroollArea

设置面板滚动视图效果

​	滚动条（足够大的时候不会出现）、会附带一个Qwidget（将其设置大一点就会出现效果了）

QScrollBar::handlel{  width: 30px;  background:white；}设置滚动条的效果（略...）



### QTabWidget

​	面板切换（类似于浏览器切换窗口）

​		勾选tabsclosable，同时编写槽函数选择tabCloseRequested{ui->tabWidget->removeTab(index);}， 即可实现可关闭

右键添加/删除页

右侧属性：修改currentTabText可以修改表签（name是变量名）

标签隐藏 QTabBar::tab{width:0px;}-----》或者使用stackWidget也行，不需要隐藏



**stackWidget：**操作和上面一致，除了没有标签



## item Widget类

### QListWidget

1. List Wedgit  表，继承于QListView！！很常用

   ​	双击进去添加项目

```
QListView{background-color:#dddddd;border:none}//常用配置
QListView::item{height:50px}
QListView::item:selected{background-color:white;  color:black}
```

配合TabWidget一起使用，先将标签隐藏 QTabBar::tab{width:0px;}

​		转到槽函数：currentRowChanged{  ui->tabWidget->setCurrentIndex(currentRow);  }

这周情况下，list要放在able外面！！（类似于QQ对话列表）

​	weiget类里的focusPolicy一般不需要，可以设置成无聚焦



动态添加项：

ui是在Qwidget里面修改大小

主要代码：

```
	ui->listWidget_2->addItem("王心果");
    ui->listWidget_2->addItem("李见坤");
    ui->listWidget_2->addItem("郭临序");

    ui->listWidget_2->takeItem(0);

    ui->listWidget_2->insertItem(0,"张三");//是在0号前面插入
```

信号：例：currentRowChanged；可以在里面打印获取内容：

qDebug()<< ui->listWidget_2->item(currentRow)->text() <<endl;



为项添加图标（以QQ好友为例）

​	右键添加新文件，选择QT的Qt设计界面类（使用widget基类）（就会生产一个新的ui界面）

​	然后利用label等进行设计（可以修改控件的名字（也就是成员变量））

```
#include<qqitem.h>
class QQItemtiem;

//注释掉etupUi(this);主界面的
QQitem *qqitem = new QQitem(this);

//item.cpp文件
设置项类，以及构造方式，传入名字和头像...可以参考例程！！

```



## 按键类

### QButton

QPushjButton

常用四个信号signal（click（），press（），released（）、toggled（））

​		其中toggle需要在初始化时加上ui->pushButton->setCheckable(true);//设置可选中ui里右下角也可以直接勾选

​			且是一个常闭/断开关、自锁功能  qDebug()<< "toggle!!"<< checked <<endl;可以查看开关状态

  样式表常用：QPushButton#pushButton_2 {border-image:url(:/icons/play.png)}

​		QPushButton#pushButton_2**:hover** {border-image:url(:/icons/play1.png)}	  //悬停时、checked选中/checked：hover选中且悬停时

​	也可以建立按钮组：然后成为互斥



QRadioButton：单选按钮（多个一起使用，只能选中其中一个，如果需要多个可以选中右键新建按钮组别，若需要多选，取消属性里的exclusive就行（需要先建成一个按钮组在选择按钮组）、不如用checkbox）

改变样式：

```
QRadioButton::indicator:unchecked{image:url(:/icons/pause.png)}
QRadioButton::indicator:checked{image:url(:/icons/play.png)}
QRadioButton{ font-size: 25px;color:black; border-radius:10px}  //radius圆角
```



QCheckBox：复选按钮（我觉得可以略）





## 布局

### Layout

margin&padding(外边距和内边距)

margin:一个控件边框到另一个控件边框的距离，外部距离

padding：内部另一个容器到边框的距离：内部距离

​	margin-top、bottom、left、right  / padding-top...

​	描边：border-top/right/bottom...（给边上色）

```
QpushButton{margin-top:50px; margin-top:50px;}
QpushButton{margin:50px;background_color:#CDCDB4; broder-width:50px; broer_style:solid;}
//也可以单独 broder-top-width
```

**QHBoxLayout(水平布局)**

spacing间隔距离，stretch拉伸因子，sizepolicy大小策略（1，1,2,2）四个控件1；1；2；2的水平比

一次性框选几个控件，然后再右侧修改（和ppt类似就）

​		初始化加上：this->setLayout(ui->horizontalLayout),将控件设置成和主窗口一样的长度

QHBoxLayout（垂直布局）--同上



​	在右侧的Layout中拖出来，然后将控件放进去，再加上代码



**QGridLayout（网格布局）**

​	从控件中拖出来，然后放控件、或者框选中点上面的栅格布局this->setLayout(ui->horizontalLayout)，就可以跟着窗口变化；



布局不满意可以选中布局，然后右键选择布局--打破布局



### QSplitter

PC端很常用！！，嵌入式用得少

分裂器，就是拖动改变窗口的大小、不在控件里面，在上面一栏（需要有两个qwidget）、创建完缩小要放大

orientation：设置方向

opaqueResize：false时拖动会有灰色的线，释放后才改变、true则实时更新

childrenCollapsible：true时，可以将子部件调整到0（拖到widget外时）

设置两个widget大小可以设置最小的大小、（不勾选childrenCollapsible：true）



**QSpacerItem：**

隔离弹簧（用来调整位置）：

orientation方向属性，sizeTy：大小类型（固定/可缩放）、sizeHint：缺省大小（默认大小）

​	初始化：this->setLayout(ui->verticalLayout_4);



## 输入类

### QLineRdit

输入控件（例：登录界面设计）

point-size：输入的字体大小

placeholderText：默认文字

echoMode：密码输入模式





