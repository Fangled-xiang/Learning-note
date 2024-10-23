# VScode

1.下载VScode

2.下载编译器MinGW（windows下用的）
3.下载插件、注意cmake还需要在官网下载
4.cmake使用（多文件编译）：建立CMakeList文件，ctrl+shift+p 配置、配置完生成build，cd进入build执行cmake生产makefile、使用编译器编译mingw-mian.exe执行
5.执行生成在build的output.exe文件即可，如果想VScode一键生成，要配置.VScode文件的task和launch文件
5.若需使用中文，需要点击右下角utf'-8，修改成simple-chinese  变成GBK（simple chinese）
6.cin读入中文问题：同上，修改成（记得选保存）

## Bug：

1.记得先关闭终端再一键运行启动，exe程序还在执行，没法生成新的
2.记得只能有一个main函数，多文件编译也只能有一个相同的函数（重载版本除外）
3.MakeFile文件里：add_executable（最后一部生成）一定要放在后面
4..a文件才是静态库，用动态库的时候会出问题，要用target_link（同时还要给出位置或者加入环境变量）
5.opencv没有为我们编译好的MINGW版本，他的编译器是MSVC版本，所以要自己编译source源码，或网上另外下载
6.再一键debug前，还是需要先通过cmake一下，就是建立新的文件夹的时候！！！！，（或者保存一下就可以）

7.include的时候会报错，一般要编译一下才引入或者保存makelist

8.有时候传入参数有问题，比如 read（string Path） ，，就修改成（const string Path）！！！

9.cin >> x >> y;输入的时候需要加空格分离

# c++进阶：

补充：

1.输入枚举类型需要强转       
        Gender gender;
        cout << "请输入性别：";
        cin >> (int&)gender;
2.指针前面加上 *就可以打印了，解指针操作 int*age；  cout<<*age<<endl;
3.清屏 system("cls")、暂停("pause")

4.强转：static_cast<int>

数据类型转换：atoi（）字符型转化成int

​						.c_str()  字符列表转换成字符串

5.using unit = unsigned int;，这样下面定义unit类型的时候，就知道是无符号整型了

## 一、内存模型

一、（ 编译后的）内存分区模型：提高灵活性
**代码区**（二进制代码）（共享、只读）、
**全局区**（全局变量、静态（static，放局部函数里面的也是）、常量（包括字符串常量和const修饰的全局变量）） （int）&a可以查看地址，相隔不远
**栈区**（编译器自动分配释放、函数参数值、局部变量（包括main函数内的非静态变量、以及局部的const常量）等）、返回局部变量的地址没有意义
**堆区**（程序员分配和释放delete）、主要是利用  new来开辟内存（~析构释放）  new 和 delete
			new返回的是类型的指针！！！！  int * p= new int（10）； 数组则是首地址， 数组释放delete[] arr;

## 二、引用

作用：给变量**取别名**  语法: 数据类型 &别名 = 原名；（必须初始化，且初始化之后不可改变，再指向别的原名）  int &b = a;
函数传参（值传递/地址传递（用指针接）/引用传递）、简化指针修改实参  add（int a）、（int * a）、（int &a）
	引用作函数返回值：可以作为左值 int &result = test（）   ； int & test（）  ；  
	int &test02(){static a = 10;return a}  ,     test02（） = 100；！！依然可以改变值
本质：一个指针常量（相当于： int * const ref = &a ）
常量引用：修饰形参，防止误操作  showvalue（**const int &v**）{}；  
将引用作为return 可以实现对一个一直操作

## 三、函数的重载

默认参数  ：func（int a， int b=20， int c=30），没传入参数则采用默认值、且从有默认值位置开始后面都要有默认值
	函数声明（例如头文件）和函数实现时只有一个能带默认参数；
函数占位参数：func（int a， int ）重载和函数指针有用到、可以有默认
函数重载：函数名可以相同、提高复用性（条件：再同一个作用域下，名称相同，参数不同（类型个数顺序）、返回值可以不同，但不能作为条件）
注意事项：使用引用传递（const int 与 int一起，func（a） ，func（10）来区分）
	默认参数时候，会出bug，尽量避免

## 四、类与对象

最核心

C++面向对象的三大特性：封装、继承、多态

### 一、封装

封装：class 类名{public：  private：protected： }；  具体的类就是对象  --> 类名 对象名;   属性可以放在public,再由用户设置参数 --->实例化（创建具体对象）
权限：公共、私有、保护protected（和私有一样，在继承的时候有区别：继承后儿子也能访问的内容）
对象的初始化和清理：构造函数和析构函数（必须有，不写默认为空函数）
	构造函数：类名{}将参数进行初始化：有参/无参  ，普通/拷贝构造

**拷贝构造** Person（const Person &p）、可以重载 Person（）、Person（int id）
		调用（推荐括号法）：Person p1； Person p2（10）；Person p3（p1）；  写成Person p1（）；会被当初函数声明
			匿名调用：Person(10) ;执行完会直接析构
		拷贝构造（和构造函数一样不写系统默认构造）：在将类作为参数传给一个函数时，会默认拷贝，，
			return 类时也会默认拷贝（这两种会自行调用）
			**深拷贝**（在堆区申请空间进行拷贝、使用到了 new时）/浅拷贝（简单赋值拷贝）：
			浅拷贝问题：堆区会重复析构释放，堆区数据共用的-->使用深拷贝解决：重新开辟内存自行编写拷贝函数
				例： 定义时使用 int * age；   -->age = new int （*p.age）    *p.age解指针
	析构函数：~类名{}  无需手动调用、销毁前自动析构调用，堆区的数据都需要编写析构，以及将野指针置nullptr
	初始化列表Person（int a ，int b, int c）  赋初值
		   ：m_A(20) ,m_B(30),m_C(40)  {}  写成一行行更直观
**类内声明，类外实现**：string Person：：getname（）{};！！另一种封装方式（）比如：.h文件和.cpp文件

类对象作为类成员：class A是 class B的 一个成员，会先构造A，在构造B，析构相反

**2.静态成员**：静态成员变量、静态成员函数
	static 所以对象共用一份数据、（编译时就分配内存了），不属于某个对象（可以直接通过类名访问而不用创建对象：Person：：m_A;）
	必须有初始值（同时，在类内声明{public:static int m_A;};，在类外!！！！s初始化  int Person：：m_A = 100;）私有的也无法访问
	
静态成员函数：所以对象共享，且只能访问静态成员变量 static void func（);,同样可以直接通过类名访问函数调用

3.
C++对象模型和**this指针：**
	成员变量和成员函数分开存储：只有非静态成员变量才会属于类的对象上
this指针：成员函数也只有一份实例，通过this指针来确定函数指向具体的对象（不用自己写）
	形参和成员变量同名，通过this区分，（this->age = age）当非静态成员函数返回对象本身可以使用 return *this（解指针操作）
	也是指针常量（不可以修改，在成员函数里面指向当前对象）
空指针访问成员函数： Person *p = NULL；  p.成员函数（）；-->如果里面含有this指针会报错

4.
const（放在后面） 修饰成员函数： void func（）  const
	不可以修改成员属性（成员属性声明时加上mytable就可以修改）；
常对象：const Person p；  常对象只能调用常函数 
5.
**友元：**friend  全局函数做友元、类作友元、成员函数作友元
   全局函数作友元：在class的最前面加入friend 全局函数声明（类最上边就行）
   类：同上，在类的最前面加上 friend class 类名；
   成员函数作友元：friend void 类名：：成员函数名（）；

6.运算符重载：（给运算符重新定义功能来适应不同的数据类型）  operator
	实现两个自定义的数据类型的运算、--》通过成员函数/全局函数重载
     "+ "重载：  成员函数实现：Person  operator+ （person &p）{ Person p3； ... ； return p3}
	        全局函数重载 Person operator+ （person &p1，person &p2）{ }；  调用时可以直接 Person p3 = p1 + p2；
	  运算符重载operator依然可以再发生函数重载：Person p2 = p1 +10；加int
     “<<”左移运算符重载：输出自定义类型 、通常不用成员函数实现（否则写出 p << cout）
	所以利用全局函数重载：ostream & operator<<(ostream &cout , Person &p) {  cout<<" " <<p.val  ....;  return cout};  ostream输出流类型的数据
      “++”递增运算符重载：具体实现需要再回头看（前置/后置可以靠占位符 int 来区分）
      “ = ”赋值运算符重载：拷贝对象（系统会默认生成，但是和拷贝构造一样是浅拷贝赋值过去，会有堆区共用析构的问题）
      “ >，==，！= ”关系运算符重载
     “ （） ”函数调用运算符重载：（也叫仿函数）：灵活、写法不固定（STL里面常用）

### 二.继承

（重中之重）

​        定义类时，下级成员有上级的共性，也有自己的特性
​        基础语法：class Java**：public BasePage**  {   }；  即可，减少重复的代码 
继承方式：**公共继承、保护继承、私有继承**     class 子类（派生类）：public/private/protected 父类（基类）{ ... }；
​	区别：父类的私类都是无法访问的，公有继承，公共还是公共，保护还是保护
​	          保护/私有继承，继承下来的都会变成保护/私有 ！！！

继承中的对象模型：
​	父类中所有非静态的成员属性都会继承（私有访问不到，也会继承下来）
​	构造/析构顺序：（和类中有类一样）：先构造父类、后析构父类
继承同名成员：子类定义的成员值，使用父类的要加作用域： s.Base：：m_A
​	   同名成员函数和成员调用一样：s.Base：：func（）；  重载版本也不行
​	   继承同名静态成员：和非静态一样，加上作用域
多继承：一个子类继承多个父类（实际开发中不建议多继承 ，父类之间同名需要加作用域）  class 子类：继承方式 父类1，继承方式 父类2
菱形继承：基类得到两个子类，孙子同时继承两个子类：也是靠作用域区分、会导致数据有两份，浪费资源（虚继承可以解决）  
​	虚继承（继承的是指针）： class sheep ：virtual public：Animal{}；    class Tuo：virtual public：Animal{}；   class Yangtuo ：public sheep，public tuo{}；



### 三、多态

静态多态：函数重载、运算符重载       

动态多态**：派生类和虚函数**实现运行时多态

区别：静态多态在编译阶段就确定函数地址、动态则是在运行阶段！！！

例子： 在**父类中**函数前 补上virtual void func（）；  这样就可以实现地址晚绑定，  因为test（Animal  &animal） 传入 子类cat时，会优先调用cat的同名函数
          **动态多态满足条件：1.有继承关系 2.子类需要重写父类的虚函数**
           使用：父类的指针或者引用，来执行子类的对象（如上）

优点：代码组织结构更清晰、可读性强、利于前期和后期的维护与拓展
开闭原则：对拓展进行开发、对修改进行关闭
	test（）{   Abscaculate *abc = new addcaculate ；}  new一下就可以指向子类对象、使用完记得释放 delete！！在堆区上的

纯虚函数和抽象类：纯虚函数： virtual 返回值类型 函数名 （参数列表） =0；  有一个纯虚函数就会成为抽象类，且不能依赖实例化对象。
	子类必需重写：   vitural 返回值类型 函数名 （参数列表）{}；
虚析构和纯虚析构：多态使用时，如果有类开辟到堆区new，父指针在释放时无法调用子类的析构代码--》虚析构/纯虚析构解决
	区别同上：无法实例化对象   virtual ~类名（）{} ；    virtual ~类名（） = 0；（纯虚析构需要有声明，在类外还需要有实现，父类有些开辟到堆区也需要析构）

## 五、文件操作：

#include<fstream>
文本文件、二进制文件、ofstream写操作、ifstream 读操作、fstream读写操作
写文件操作：
	ofstream ofs；  ofs.open（“路径”， 打开方式） ； ofs<<"数据" ； ofs.close（）；
	打开方式：ios：:in只读，ios::out写、ate 文件尾，app追加的方式、trunc存在先删除在创建，binary二进制
	    一起使用：ios::biary  |  ios::out  加上位或符号
读文件操作：
	ifstream ifs;     ifs.open(""，ios：：in)； //！！！一定记得是加绝对路径
	ifs.is_open() 判断打开
	读取方式： 第一种：buf[1024] = {}  ;   while（ifs >> buf）{ cout << buf<<endl;  };
		第二种：while（ifs.getline（buf， sizeof（buf））） {cout << buf<<endl;}；
		第三种：string buf；   while（getline（ifs， buf））{ cout << buf<<endl; }；
		第四种 ifs.get（）！=EOF  按字符读
二进制文件：写：ofs.write（（const char*）&p ， sizeof（p））；
	    读：ofs.read（（char*）&p ， sizeof（p））；   文件里时乱码，但是可以输出出来读得懂的

按行读取等可参考：https://blog.csdn.net/qq_41920323/article/details/141201645

字符串分割：https://zhuanlan.zhihu.com/p/669544380

可以考虑保存到csv文件中，可以有记事本打开，每个数据都是靠逗号隔开、也可以用excel打开



补充：

（还有bug没解决，放弃，不好用）c语言里FILE *fp的操作：https://blog.csdn.net/qq_73781875/article/details/136503842

    void readfile(const string filePath)  ！！！vscode里必需加const（自己摸索的）
    {
        FILE* pf = fopen(filePath.c_str(),"r");   //打开一个文件
        
        while (!feof(pf))  //读到末尾
        {
            char line[1024] = {0};
            fgets(line, 1024, pf);   //按行读取
            char *num = strtok(line,",");   //按，分隔
            while(num != nullptr)
            {
                ....
                num = strtok(nullptr,",");  //读完一个继续读，按，分隔
            }
        }
        fclose(pf);
        
    }



# C++提高部分：

泛型编程 + STL技术

## 一、模板：

提高复用性

1.函数模板： template<typename/class T>  
	     函数声明或定义；
         函数返回类型和形参可以先不确定类型；
	使用：自动类型推导(必需推导出一致的数据类型)，或者显示指定类型 swap<int>(a,b);（在无法自动推出时需要指定）
	自动类型推导无法发生隐式类型转换：比如int + char形的

2.与普通函数都可以实现：优先调用普通函数 ，非要使用可以加上空模板<>
	模板函数也可以重载
3.模板的局限性
	传入自定义类型无法进行比较、进行模板的重载实现具体化的模板
	方法一：运算符重载，方法二：利用具体化类的版本实现  在类的外边，  template<>  bool mycompare(Person &p1, Person &p2){   };
4.类模板
	template<typename NameType, typename AgeType>
	class Person{ };    调用时：Person<string, int> p1("孙悟空"， 999)；
	区别于函数模板：类模板无法自动类型推导必需指定，同时类模板参数列表可以有默认参数 template<typename NameType = string, typename AgeType = int>
	类模板的成员函数只有在调用的时候才回去创建
	类模板对象做函数参数：1.指定传入类型：void func（Person<string,int> &p）
			    2参数模板化： template<class T1 ,class T2> void func（Person<T1,T2> &p）
			     3.整个类模板化 template<class T> void func（T &p）
	类模板成员函数：
		类内实现：和之前一样，类外实现：**类内只写声明，类外**：template<class T1 ,class T2>
							Person<T1,T2>::Showperson(T1 name, T2 name){  };加上作用域和template
	**成员函数的类外实现**：template<class T1 ,class T2>   void Person<T1,T2>::showPeerson(){} ; 注意和上面的区别



**5.类模板的分文件编写**
	类模板成员函数的创建是在调用阶段，分文件编写可能会链接不到
		方式1：直接包含.cpp文件，方式2：将声明和实现写到同一个文件中，后缀名改成.hpp

6.类模板与继承：
	子类的父类是类模板，必需指定父类的T类型，如果想灵活指定T的类型，子类也需要变成类模板 
	class Son : public Base<string,int>{} ;    template<class T1 ,class T2,class T3> class Son2:punlic Base< T1 , T2>{  T3... };(子类如需要新的模板成员T3)

7.类模板与友元：
	全局函数类内实现：和之前一样，直接声明friend即可
	全局函数类外实现：类外需要加上temlate即可，类内也需要加上空模板<>,同时实现要写到类的上方，在实现的上面还需要声明类，类还要加上templete告知是模板类；（较复杂）

## 二、STL

（数据结构和算法的一套标准模板库）
1.STL：分为：容器、算法、迭代器（几乎采用模板/模板函数）、容器和算法之间通过迭代器进行无缝衔接（每个容器有自己的迭代器、类似于指针）

### STL的六大组件

​	   容器（存放各种数据结构：vector、list、deque、set、map）、算法（sort、find、copy）、迭代器、仿函数、适配器（配接器）、空间配置器
​          容器：序列式容器（强调排序和固定的位置）、关联式容器（二叉树结构、没有严格物理上的顺序关系）
​          算法：质变算法（拷贝、替换、删除等）、非质变算法（不更改元素内容：查找遍历等）  #include<algorithm>
​          迭代器：常用是双向迭代器、随机访问（中间某一个值）迭代器

### 常用的容器：

（详细看代码Upper部分：注意vector容器只是容器的一种）

linux下：没法auto

for(list<student>::iterator it=l.begin();it!=l.end();it++)
    {
        cout<<(*it).name_<<"--"<<(*it).age_<<"--"<<(*it).height_<<endl;
    }

或者指定g++的标准：   加上  -std=c++11

迭代器Iterator  it：begin（），可以偏移：  begin（）+i

1. **vector容器，**容器嵌套容器（类似于二维数组）

​	与数组的区别：单端、可动态拓展，insert也可以插入 ，参数传入迭代器： vec.begin（）+3  进行偏移，end（） -2 ；

2. **string容器**，本质上一个类，具体操作见code、析构和构造部分都是有写好的

3. **deque容器**：双端数组：区别于vector，头插入时不需要移动元素，但是vector访问更快                                       

​			工作原理：内部有一个中控器来维护缓冲区的内容，缓冲区存放真实数据，物理上不连续、逻辑上连续、有点像链表

4. stack容器：

   ​		特点：栈结构，先进后出（堆），只能在栈顶出栈、

   ​		栈无法遍历，可以empty、size

   构造：拷贝构造、pop、push、top（）返回栈顶元素 

   5.queue容器：

   ​			和stack操作基本类似、但是是先进先出，同时可以查看front和back的值

   6.List容器：构造同上，操作、swap、assign都一样

   ​			大小操作：resize、empty、size、clear都一样

   ​		多了一个remove（n）操作 、存取只能返回front和back，不支持随机访问、[] = 没有重载

   反转：reverse， L.sort（）排序、不能用全局的#include<algorithm>sort（begin，end）

​			对自定义类型的数据进行排序，需要写回调函数指定规则

 7. set（不允许重复）/multiset（可以重复）  容器

    ​	特点：在元素插入时会自动排序！！可以利用仿函数（本质上一个类，bool返回类型）改变排序规则

    ​	本质：关联式容器、底层靠二叉树BST树！！实现
    
    插入只有：insert方式（会返回是否插入成功   pair<set<int>>::iterator,   bool > ret
    
    ​			ret = s.insert（10）， ret.second  =true/false）
    
    ​		  pair对组的创建：返回两个数据 pair<typ1, typ2>  p(  ,   );   第一个fist，第二个second，访问方式
    
    拷贝构造等，size，swap，不支持resize操作，erase（pos/num） 即可以传入值/位置、 查找：find（）返回元素的迭代器，没找到返回s.end（），count（），统计个数
    
    ​	自定义类型数据的插入，以及设置排序规则（见code）
    
    
    
    仿函数：
    
    ​	class comparestu
    {
    public:
    ​    bool operator()(const student &s1,const student&s2)
    ​    {
    ​        if(s1.age_ == s2.age_)
    ​        {
    ​            return s1.height_  > s2.height_;
    ​        }
    
      return s1.age_  < s2.age_;}
    
    
    
    补充：
    
    ​	priority_queue()默认是大根堆、小根堆修改如下，自定义数据要重载>号
    
    PriorityQueue que( [](int a,int b){return a < b;});
    
    无序的unorder_map()、unorder_set（）
    
    
    
    **8.map/multimap容器：**
    
    特点：所有元素都是pair对组！，第一个值是键值、第二个是value实值，所有元素根据键值来自动排序
    
    ​		关联式容器，底层也是二叉树，multimap允许有重复的键值、索引可以高效查找
    
    构造：默认构造需要指定两个类型：  map<T1,T2>  mp;    m.insert（pair<int,int>(1,20)）
    
    支持：swap、size、empty
    
    插入删除：erase（pos）迭代器/区间（beg，end）  、erase（key）根据key值删
    
    查找、统计：find、count（key）
    
    人家是根据键值key来排序（仿函数应该是int参数）的、所以自定义类型数据排序根本没必要
    

### 函数对象、仿函数、谓词

重载函数的类（函数对象）   --》重载（）操作（仿函数） --》  返回时 bool类型（谓词）

​	函数对象：仿函数---**重载**函数调用操作符的类，其对象称为函数对象，当**重载（）**操作时也叫仿函数

本质：一个类，而不是函数、可以有自己状态 例如：count、可以作为参数传递

使用：  Myadd  myadd；  myadd（1,2）；  

​	**谓词：**一元谓词（返回bool类型的仿函数），opertor（）（一个参数一元谓词），

​				两个是二元谓词（常用来指定排序规则，sort函数需要、特别是自定义类型）



STL的内建函数对象：

​	算术仿函数、关系仿函数、逻辑仿函数：**#include<functional>**

算术仿函数：加减乘除取模取反plus、minus、multiplies、divides、modulus、negate（取反是一元运算）

关系仿函数：**greater、less**、equal_to、not_equal_to  less/greater_equal

逻辑仿函数：与或非：logica_and、or、not  很少用



## 三、算法

算法主要头文件：

《algorithm》、最主要，比较查找遍历修改复制等

《functional》、主要是类模板

《numeric》、只包括在序列上进行简单的数学运算的模板函数

1.遍历算法：for_each（beg，end，【函数/函数对象】）遍历容器、一般是print函数

​					transform搬运容器到另一个

2.查找算法：find返回迭代器、**find_if**（按条件查找beg，end、 条件的谓词返回bool类型）、

adjacent_find（查找相邻重复元素，传入区间，返回第一个迭代器位置）

（注意不能用传入&类型参数）						binary_search（二分查找，只能用在升序数列）、count、count_if

​			查找自定义类型、需要在自定义类内重载==号

3.排序算法：sort 支持随机访问的容器都可以用sort进行排序

​	sort（迭代器区间，【规则/谓词返回bool】）

​	random_shuffle（迭代器区间）  洗牌打乱顺序	

​	合并算法**merge**（beg1，end1，beg2，end2，beg3）目标容器的起始迭代器， 容器元素合并，vec3必须先分配内存，都是有序的，合并之后依然是有序的，否则乱序

​	reverse反转，首尾对调

4.拷贝/替换算法

​	copy（beg1，end1，beg2）、swap（容器1，容器2）而且可以自动扩容

​	replace（beg，end，oldval，newval）

​	replace_if（beg，end，谓词，newval）

5.算术生成算法  #include《numeric》

​		accumulate（区间， val）；

​		fill（区间，val）；

6.集合算法：求两个容器的交并差集（两个集合必须是有序的）

​	交集：set_intersection（区间1，区间2，目标起始的迭代器beg3）

​	并：set_union（）， 

​	 差：set_difference（）、第三个容器resize（max（size1， ..2））	

​			（区间1，区间2），求的是1和2的差集，即1有2没有的部分
















​	