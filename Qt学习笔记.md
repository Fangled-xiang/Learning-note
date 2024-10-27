# Qt运行环境

Qt：跨平台的C++开发库，开发GUI（graphical user interface）
		支持系统：PC、手机系统、嵌入式系统、stm32都可以     

Centos下：打开creater：  /opt/Qt5.12.9/Tools/QtCreator/bin/qtcreator.sh &  （加个&表示在后台运行）
找不到lGl：ln -s /usr/lib64/libGL.so.1 /usr/lib/libGL.so创建一下连接即可
			https://blog.csdn.net/qq_21438461/article/details/132913923



打开已有过程：打开pro文件即可

ui设计好了要先运行构建运行，不然可能ui->..会找不到！！！

多个过程可以右键选中的工程运行

# Qtcreater

1.创建时的父类：

QMainWindow：具有一个菜单栏和一个状态栏
QWidget：空白窗口，仅有一个关闭按钮
QDialog：只有ok和cancel两个选择按钮



this->setWindowTitle("服务端");  修改窗口名字



2.QT的模块：
QT   += core gui， 		其他模块，可以在官网相应的版本的文档中的Moduls中查看
		在pro文件添加完，使用时需要在头文件中include，下面对应的文件与编译的一一对应
		增加 TARGET = out_name  即可修改可执行文件名字

3.启动流程
int main(int argc, char *argv[])  argc是命令行参数个数，argv是命令行参数
QApplication针对Qwidgt应用程序、QGuiApplication非Qwidgt程序、QCoreApplication（无界面的应用程序）
		都是管理QT的运行、设置Qt应用程序

## 快捷键（指南里有）

​		ctl+alt+enter可以进入虚拟机的全屏

​	ctrl+滚轮修改界面**、对齐代码：ctr+a  +  ctr+i、**   

​		ctr+函数（转到定义）

​		在h文件声明自定义成员函数，右键refector，在cpp文件中创建

​		F4回到头函数、Help-Index里查看帮助文档

​		F4切换很好用

​		ctr+点击可以选中多个控件（不好点，点右侧名字也行）

​		tools-option-keyboar修改copy快捷键，Ctrl+Alt+PgDown向下复制一行（原本的快捷键与linux冲突了）



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

## Object Tree（对象树this）

设置父对象：

​	窗口之间需要相互联系，比如A显示在B上面，A需要指定B为父对象

```
方法一：button = new QPushButton(this);    //this即表示将这个窗口作为父对象
方法二：button->setParent(this);
```

对象树机制（方便管理内存）：先析构子对象，才析构父对象



## 添加资源文件：

ctr+n 选择新建Qt文件resource文件（右键qrc文件、open in editor）

设置prifix前缀（也就是路径的地址）、保存文件（一定要ctl+s保存，不保存不显示）



添加现有文件：(比如一个类的.h.cpp.ui文件)

​	现需要拷贝到工作的文件夹中，在在工程中右键添加现有文件，ctr+点击选中多个文件，open打开



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



### 自定义项设计

为项添加图标（以QQ好友为例）

​	右键添加新文件，选择QT的Qt设计界面类（from class）（使用widget基类）（就会生产一个新的ui界面）

​	然后利用label等进行设计（可以修改控件的名字（也就是成员变量））

```
QQItem::QQItem(QString icon,QString name,QWidget *parent) :
    QWidget(parent),
    ui(new Ui::QQItem)
{
    ui->setupUi(this);
    QImage image(icon);
    ui->icon->setPixmap(QPixmap::fromImage(image.scaled(ui->icon->width(), ui->icon->height())));
    ui->name->setText(name);
}

//记得在Widget初始化前要加上 声明
#include<qqitem.h>
class QQItem；

QQItem *qq1 = new QQItem(":/icons/icon0.jpg","小火车");
QListWidgetItem *item0 = new QListWidgetItem;
ui->listWidget_3->addItem(item0);
ui->listWidget_3->setItemWidget(item0, qq1);
```

补充：需要构建成员函数时，先在.h文件写好声明，然后右键选折定义-在cpp文件中打开





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



补充：右边属性的icon里面可以设置图标



ui->pushButton->setEnabled(true);//设置可用状态
ui->pushButton_2->setEnabled(true);



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



### QSpacerItem

隔离弹簧（用来调整位置）：

orientation方向属性，sizeTy：大小类型（固定/可缩放）、sizeHint：缺省大小（默认大小）

​	初始化：this->setLayout(ui->verticalLayout_4);



使用时，可以左右一个弹簧，一起选中建立水平布局，将sizeType改成fixed，就可以改变长度



## 输入类

### QLineRdit

输入控件（例：登录界面设计）

point-size：输入的字体大小

placeholderText：默认文字

echoMode：密码输入模式



## 显示类

​	**label：**

补充：QString::number(m)  转换成字符串类型

显式在lbael上：ui->label->setText("dasdas");



**Tex Browser**

​		文本浏览框

ui->textBrowser->append("客户端连接");



# 文本读写

可以利用QFile才操控IO口、在使用qss样式表文件时，也必须加上文件读取操作

## 读写流程

1.找到文件：QFileDialog类  ::getOpenFileName   返回字符串“路径/名字”

2.打开文本open（）方法

3.读写方法：readAll(),  readLine(),  write()

4.关闭close()



Qt支持的文本是UTF-8，打开其他会乱码

主要代码如下：

```
#include<QFile>
#include<QFileDialog>
#include<QDebug>

private:
    QFile file;

void Widget::on_pushButton_clicked()
{
    QString filename = QFileDialog::getOpenFileName(this, "选择文本", "/home/fangxiang");
    qDebug()<< filename;
    file.setFileName(filename);
    if( !file.open(QIODevice::ReadWrite) )
    {
        qDebug()<<"fail!!!!"<<endl;
        return;
    }
    ui->textEdit->setPlainText(  file.readAll()  );
    file.close();
}

void Widget::on_pushButton_2_clicked()
{
    if( file.fileName().isEmpty() )
    {
        return;
    }
    if( !file.open(QIODevice::ReadWrite) )
    {
        qDebug()<<"fail!!!!"<<endl;
        return;
    }
    file.write(ui->textEdit->toPlainText().toUtf8());
    file.close();
    ui->textEdit->clear();
}
```

补充：QString::number(m)  转换成字符串类型

显式在lbael上：

## 动态文件

类似于备忘录

```
#include"filedialog.h"  //自己写的类
#include<QFile>
#include<QDateTime>

void Widget::on_pushButton_3_clicked()
{
    QFile file;
    file.setFileName(QDateTime::currentDateTime().toString("MMddhhmmss")+".txt");
    file.open(QIODevice::ReadWrite);


    FileDialog *fileDialog = new FileDialog(this);
    fileDialog->resize(this->size());
    fileDialog->show();
    fileDialog->setModal(true);
    fileDialog->exec();

    QString temp = fileDialog->getTextEditContent();
    //自己写的成员函数{ return ui->textEdit->toPlainText(); }
    file.write(temp.toUtf8());
    file.close();

    if(temp.length()==0)
    {
        file.remove();
    }
    delete fileDialog;
}

```

在main函数设置文件的保存路径

```
#include<QDir>

QDir::setCurrent(QApplication::applicationDirPath());  //生成在Debug里面
```



# 绘画、图表、动画

## Qpainter类



基本使用：矩形、三角形、直线、圆、文字、路径

```
添加widget的成员函数
#include<QPainter>
void paintEvent(QPaintEvent *evernt) override;//右键转到定义Refacter-add in .cpp

//不需要调用就自己调用了
void Widget::paintEvent(QPaintEvent *evernt)
{
    QPainter painter(this);
    painter.setRenderHint(QPainter::Antialiasing); //抗锯齿

    QPen pen;
    pen.setWidth(5);
    pen.setColor(QColor("#888888"));
    painter.setPen(pen);

    //QBrush brush(QColor("#888888"));
    //painter.setBrush(brush);  填充模式

    painter.drawRect(200,100,100,100);

    QPolygon polygon;//多边形
    polygon.setPoints(3,100,20,200,100,50,300);
    painter.drawPolygon(polygon);

    painter.drawLine(400,400,500,500);

    QRectF rectF(0,0,200,100);
    painter.drawText(rectF, Qt::AlignHCenter,"你好11111");  //水平居中
}
```

轮播文字实现：+（QTimer定时器模块）

```
#include<QPainter>
#include<QTimer>
#include<QFontMetrics>

//定义成员
private:
    Ui::Widget *ui;
    QFont font;
    int offset;
    QTimer *timer;
    int textWidth;
private slots:
    void onTimerTimeOut();

	//初始化部分：
    font.setPixelSize(50);
    offset = 0;
    timer = new QTimer(this);
    timer->start(100);//单位ms
    QFontMetrics fontMetrics(font);
    textWidth = fontMetrics.width("你好11111");

    connect(timer, SIGNAL(timeout()), this, SLOT(onTimerTimeOut()));
    //！！！槽连接，定时器时间一到，就会进去槽函数修改offset值


void Widget::paintEvent(QPaintEvent *evernt)
{
pen.setColor(QColor(Qt::red));
painter.setPen(pen);
painter.setFont(font);
QRectF rectF = this->rect();
rectF.setLeft(this->rect().width()-offset);   //主要方法就是修改offset
painter.drawText(rectF, Qt::AlignCenter,"你好11111");
}


void Widget::onTimerTimeOut()
{
    if(offset < this->width() + textWidth){
        offset += 20;
    }
    else{
        offset = 0;
    }
    this->update();//！！记得要更新
}
```

## Qchart类

需要在安装creater时勾选，不在控件里面，



pro文件里加上，QT       += core   gui   charts

#include<QChart>

#include<QChartView>

#include<QValueAxis> //坐标轴

#include<QSplineSeries>  //曲线
#include<QLineSeries>  //折线

QT_CHARTS_USE_NAMESPACE  //命名空间也要导入



//创建一个图表视图

继承于控件里面的QGraphics View，右键选择提升promote为，输入名字QChartView，点添加，提升



//创建一个图表（初始化里面）

```
Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

    QChart *chart = new QChart();

	//坐标轴设置
    QValueAxis *AxisX = new QValueAxis();
    QValueAxis *AxisY = new QValueAxis();

    AxisX->setRange(0,5000);
    AxisY->setRange(0,100);

    AxisX->setTitleText("时间/ms");
    AxisY->setTitleText("温度/C");
    
    AxisX->setTickCount(10);
    AxisY->setTickCount(10);//网格

    AxisX->setLabelFormat("%d");
    AxisY->setLabelFormat("%d");

    chart->createDefaultAxes();
    chart->addAxis(AxisX, Qt::AlignBottom);
    chart->addAxis(AxisY, Qt::AlignLeft);

    chart->setTitle("温度与时间曲线");
    chart->legend()->setVisible(true);

	//曲线设置
    QSplineSeries *splineSeries = new QSplineSeries();

    splineSeries->append(0,50);
    splineSeries->append(1000,60);
    splineSeries->append(2000,80);
    splineSeries->append(3000,50);
    splineSeries->append(4000,30);
    splineSeries->append(5000,80);

    QPen pen(QColor("#888888"));
    splineSeries->setPen(pen);

	//图表曲线坐标轴联系起来
    chart->addSeries(splineSeries);
    splineSeries->attachAxis(AxisX);
    splineSeries->attachAxis(AxisY);

    ui->graphicsView->setChart(chart);
}
```



## QpropertyAnimation

几何动画、颜色动画、不透明度动画（例如：呼吸灯效果）

主要实现：

```
void Widget::on_pushButton_clicked()
{
    animation1->start();
}


#include<QPropertyAnimation>
private:
    QPropertyAnimation *animation1;
    QPropertyAnimation *animation2;
    QPropertyAnimation *animation3;
    
 
 //几何变换动画
Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

	//target是ui里面添加的作用目标的Widget控件
    animation1 = new QPropertyAnimation(ui->target, "geometry");
    animation1->setStartValue(QRect(0,0,100,100));
    animation1->setKeyValueAt(0.5,QRect(600,0,100,100));//中间位置
    animation1->setEndValue(QRect(600,400,200,200));
    animation1->setDuration(2000);
    animation1->setLoopCount(3);
    animation1->setEasingCurve(QEasingCurve::OutCubic);//设在曲线运动的速度曲线
}


//Widget没有颜色属性，借助 #include<QGraphicsColorizeEffect>
	QGraphicsColorizeEffect *grapheffect = new QGraphicsColorizeEffect(this);
    ui->target->setGraphicsEffect(grapheffect);
    animation2 = new QPropertyAnimation(grapheffect, "color");
    animation2->setStartValue(QColor(Qt::blue));
    animation2->setKeyValueAt(0.5,QColor(Qt::darkGray));//中间位置
    animation2->setEndValue(QColor(Qt::black));
    animation2->setDuration(2000);
    
//透明度变换，借助 #include<QGraphicsOpacityEffect>
	QGraphicsOpacityEffect *grapheffect2 = new QGraphicsOpacityEffect(this);
    ui->target->setGraphicsEffect(grapheffect2);
    animation3 = new QPropertyAnimation(grapheffect2, "opacity");
    animation3->setStartValue(0.0);
    animation3->setEndValue(1.0);
    animation3->setDuration(5000);
```

## Q_PROPERTY宏（实用）

class Rich : public QWidget
{
    Q_OBJECT

    Q_PROPERTY(double money READ money WRITE setMoney);//其中Read是必须的（读取money的值）

double money() const;
void setMoney(double m);



...}



void Rich::setMoney(double m)
{
    richmoney = m;
    ui->Lab->setText(QString::number(m));
}



rich->setProperty("money", 50000);  //会自动调用money的Write方法



## 自定义属性动画

实现动态数字增加效果

```
rich = new Rich(this);
QPropertyAnimation *pro = new QPropertyAnimation(rich, "money",this);
pro->setStartValue(100);
pro->setEndValue(800);
pro->setDuration(10000);
pro->setEasingCurve(QEasingCurve::Linear);//设在曲线运动的速度曲线
pro->start();
```

# QThread多线程

死循环/system（“sleep 5”）  ：会卡死，甚至无法关闭窗口、或者一段时间后恢复



线程和进程的区别：

centos上：ps -ef查看进程

ps -ef | grep Qt 可以查看当前编译的进程对的pid号

ps -mp 22401 查看该进程的线程（Qt默认会有五个线程，开辟之后再查看就会增加）

​		ls  /proc/22401/task/  查看线程号



关系：

​	一个**应用程序**至少要有一个进程，  QProcess，开辟多个进程

​	一个**进程**至少有一个**线程**，QThread开辟多个线程



主要实现代码：

```
#include<QDebug>
#include <QThread>
class MyThread;

private:
    Ui::Widget *ui;
    MyThread *myThread;
...    
    
    
class MyThread : public QThread
{
    Q_OBJECT
public:
    MyThread(QWidget *parent = nullptr)
    {
        Q_UNUSED(parent);
    }
    ~MyThread()
    {}
public:
    //只有run()方法会在新的线程里面
    void run()
    {
        qDebug()<<"现存开启！！！"<<endl;
        sleep(2); //sleep只能在线程里使用
        qDebug()<<"end！！！"<<endl;
        deleteLater();  //!!进程结束后会执行析构销毁线程
    }
};


void Widget::on_pushButton_clicked()
{
    myThread->start();

	//动态开辟线程
    MyThread *thread = new MyThread(this);
    thread->start();
}

void Widget::on_pushButton_2_clicked()
{
    if(! myThread->isFinished()){
       myThread->terminate();
    }
}
```

# 网络通信

## TCP通信

ens33：   “192.168.88.130”本机的地址，可以实现在局域网内的通信//在局域网范围内可能会发生改变

lo    “127.0.0.1” 本地环回地址，在本机内部的范围内通信



​	TCP协议：传输控制协议，面向连接（必须先建立TCP连接、客户端、服务端）、可靠、基于字节流、的传输层通信协议

TCP客户端：QTcpSocket类、

TCP服务端：QTcpSever、QTcpSocker类



服务端设计：

Sever服务端对象（监听某个ip的某一个端口--newConnection（）信号）  ---  通过nextPendingConnection（）动态创建一个对应的Socket对象对接受的信息进行处理，断开连接后销毁对象、可以创建多个

​			依靠write和readAll来接收和发送信息

```
QT       += core gui  network  //添加网络模块

#include<QTcpServer>
#include<QTcpSocket>

private:
    QTcpServer *tcpServer;

private slots:
    void mNewConnection();  //建立连接
    void receiveMessage();  //接收消息处理
    void mstateChanged(QAbstractSocket::SocketState state);  //状态改变打印状态

    void on_pushButton_3_clicked();//开始监听
    void on_pushButton_5_clicked(); //发送消息
    
    //初始化
    tcpServer = new QTcpServer(this);
    connect(tcpServer, SIGNAL(newConnection()), this, SLOT(mNewConnection()));
    
void Widget::mNewConnection()
{
    QTcpSocket *tempSocket = tcpServer->nextPendingConnection();

    ui->textBrowser->append("客户端ip：" + tempSocket->peerAddress().toString());
    ui->textBrowser->append("客户端端口：" + QString::number(tempSocket->peerPort()));

    connect(tempSocket,SIGNAL(readyRead()), this,SLOT(receiveMessage()));//接收消息后处理

    connect(tempSocket,SIGNAL(stateChanged(QAbstractSocket::SocketState)),
            this,SLOT(mstateChanged(QAbstractSocket::SocketState)));//连接后打印状态信息
}


void Widget::receiveMessage()
{
    QTcpSocket *tempSocket = (QTcpSocket *)sender();
    ui->textBrowser->append(tempSocket->readAll());
}

void Widget::mstateChanged(QAbstractSocket::SocketState state)
{
    QTcpSocket *tempSocket = (QTcpSocket *)sender();//不是new创建、只是一个指向的指针
    switch(state){
    case QAbstractSocket::UnconnectedState:
        ui->textBrowser->append("客户端断开连接");
        tempSocket->deleteLater();  //！！不能立马删除，利用latter用完删除
        break;
    case QAbstractSocket::ConnectingState:
        ui->textBrowser->append("客户端连接");
        break;
    default:
        break;
    }
}
    
    
    //按键启动监听和发送消息
    void Widget::on_pushButton_3_clicked()
{
    tcpServer->listen(QHostAddress("192.168.88.130"),9999);  
    //监听的端口要与客户端连接的端口一致
}

void Widget::on_pushButton_5_clicked()
{
    QList<QTcpSocket *> socketList = tcpServer->findChildren<QTcpSocket *>();
    foreach(QTcpSocket *tempSocket, socketList){
        tempSocket->write(ui->lineEdit->text().toUtf8());//每一个都发了
    }
}

void Widget::on_pushButton_4_clicked()
{
    tcpServer->close();
}
//可以给按钮增加按钮组，互斥（先给停止√上checkable和chacked），先监听才能断开
```

客户端设计：

```
QT       += core gui  network  //添加网络模块
#include<QTcpSocket>
#include<QHostAddress>

private:
    Ui::Widget *ui;
    QTcpSocket *tcpSocket;

private slots:
    void reciveMessage();

    void mstateChanged(QAbstractSocket::SocketState state);
    void on_pushButton_3_clicked();
    void on_pushButton_clicked();
    void on_pushButton_2_clicked();
 
Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);
    this->setWindowTitle("客户端");
    ui->pushButton_2->setEnabled(false);  //加上锁，必须先连接才能断开
    tcpSocket = new QTcpSocket(this);
    connect(tcpSocket,SIGNAL(readyRead()),this,SLOT(reciveMessage()));

    connect(tcpSocket,SIGNAL(stateChanged(QAbstractSocket::SocketState)),
            this,SLOT(mstateChanged(QAbstractSocket::SocketState)));
}

void Widget::reciveMessage()
{
    ui->textBrowser->append("服务端："+ tcpSocket->readAll());
}

void Widget::mstateChanged(QAbstractSocket::SocketState state)
{
    switch(state){
    case QAbstractSocket::UnconnectedState:
        ui->textBrowser->append("与服务端断开连接");
        ui->pushButton->setEnabled(true);
        ui->pushButton_2->setEnabled(false);
        break;
    case QAbstractSocket::ConnectingState:
        ui->textBrowser->append("连接服务端");
        ui->pushButton->setEnabled(false);
         ui->pushButton_2->setEnabled(true);
        break;
    default:
        break;
    }
}

void Widget::on_pushButton_3_clicked()
{
    if(tcpSocket->state() == QAbstractSocket::ConnectedState){
        tcpSocket->write(ui->lineEdit->text().toUtf8());
    }
    else{
        ui->textBrowser->append("未连接服务端");
    }
}

void Widget::on_pushButton_clicked()
{
    tcpSocket->connectToHost(QHostAddress("192.168.88.130"), 9999);
}

void Widget::on_pushButton_2_clicked()
{
    tcpSocket->disconnectFromHost();
}

```



## UDP通信



轻量级、不可靠、面向数据报的无连接协议

应用：QQ消息、语音、视频、直播等，QQ的文件传输是TCP

三种模式：单播（需要知道对方IP和端口）、广播（一般使用255.255.255.255面向所有）、组播：多点广播

代码如下：

```
QT       += core gui network

#include<QUdpSocket>
private:
    Ui::Widget *ui;
    QUdpSocket *udpSocket;

private slots:
    void readPendingDatagrams();
    void on_pushButton_clicked();
    void on_pushButton_2_clicked();
    void on_pushButton_3_clicked();
    void on_pushButton_4_clicked();
...//    
    
    Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);
    udpSocket = new QUdpSocket(this);

    connect(udpSocket, SIGNAL(readyRead()),
                 this, SLOT(readPendingDatagrams()) );

    ui->pushButton_3->setEnabled(false);
    ui->pushButton->setEnabled(false);
    ui->pushButton_4->setEnabled(false);
}



void Widget::readPendingDatagrams()
{
    QByteArray tempByteArray;
    tempByteArray.resize(udpSocket->pendingDatagramSize());
    QHostAddress ipaddr;
    quint16 port;//发送者的地址

    while(udpSocket->hasPendingDatagrams()){
        udpSocket->readDatagram(tempByteArray.data(),tempByteArray.size(), &ipaddr,&port);
    }

    ui->textBrowser->append(ipaddr.toString() +"--"+QString::number(port)+  " :  "+tempByteArray);
}


void Widget::on_pushButton_clicked()
{
    udpSocket->writeDatagram(ui->lineEdit->text().toUtf8(), QHostAddress("192.168.88.130"), 8888);//发送目标端口
}



void Widget::on_pushButton_2_clicked()
{
    ui->pushButton_2->setEnabled(false);
    ui->pushButton_3->setEnabled(true);

    ui->pushButton->setEnabled(true);
    ui->pushButton_4->setEnabled(true);

    udpSocket->bind(QHostAddress("192.168.88.130"), 8888);//0-65535
    //udpSocket->bind(8888);//IP可能会变，也可以直接绑定接收的端口
}

void Widget::on_pushButton_3_clicked()
{
    ui->pushButton_2->setEnabled(true);
    ui->pushButton_3->setEnabled(false);

    ui->pushButton->setEnabled(false);
    ui->pushButton_4->setEnabled(false);

    udpSocket->abort();//解绑
}

void Widget::on_pushButton_4_clicked()//广播
{
    udpSocket->writeDatagram(ui->lineEdit->text().toUtf8(), QHostAddress::Broadcast, 8888);
}
```
