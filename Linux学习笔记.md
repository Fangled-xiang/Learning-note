下载centOS：http://mirror.nsc.liu.se/centos-store/7.6.1810/isos/x86_64/
终端默认打开/home目录

快捷键：

ctr+C 强制停止命令
ctl+alt+enter可以进入虚拟机的全屏

# Linux命令：

ifconfig
clear
su - root 切换管理员用户，exit退出
ls [-a -l -h] 路径      	 	-a是all所有目录包括隐藏文件  -l竖向显示细节（或者直接ll命令）   -h易于阅读的方式-lh必须与l一起用
cd 路径 		目录切换 不含参数回到home  、cd ..上一级 cd ../.. 回退 ~代表home目录，cd ./ .代表当前目录  直接cd回到用户目录
pwd		显示当前的工作目录

mkdir [-p] 路径	在目录下创建文件夹，-p创建不存在的目录、尽量在home目录下创建否则会有权限问题
touch 路径	创建文件
cat/more 路径 		查看文件cat直接全部显示more支持空格翻页q退出
cp [-r] test1 test2		复制test1 -r文件夹
mv t.txt Desktop/  		移动文件/文件夹
mv test1 test2 		改名字
rm [-r -f] 	  	-r删除文件夹，-f强制删除
rm *	通配符 test* 、 *test、  *test*  *.txt   如果想用作乘法，需要写成\*

which 命令	查找命令的程序文件
find 起始路径 -name "文件名"	可以到管理员用户查找、同时支持通配符	[-size +10K -100M]查找大小范围
grep "关键字" text.txt 	返回关键字所在行
wc 文件名		统计数量[-c 字节数 -m字符数 -l行数 -w单词数]
管道符 |  将左边的结果作为右边的输入   ls  -l /usr/bin  |  grep "test"  仅查看bin下关于test的文件信息   ls  -l /usr/bin  |  wc -l  统计文件数
echo “输出内容”		和print功能差不多  
`` 	飘号 类似于反斜杠`pwd`结构而不是打印pwm字母   ech `pwd`
<、<<重定向符，<覆盖，<<追加 	例如：echo "hello" > text.txt, ls>>text.txt
 tail [-f -num] 路径 		-f持续跟踪、-num查看尾部多少行

## vim 

文件路径	vim编辑器的三种工作模式：命令、输入、底线命令模式、一进来就是命令模式，输入和底线模式不能直接转换要经过命令模式
		命令模式快捷键：/搜索，n向下继续搜索，i输入，$末尾光标
			dd删除行，3dd删除当前三行，u撤销，ctr+r反向撤销，gg到首行，G到末尾，
			dG当前行删完，dgg当前以及向上全部删除，yy复制当前行，3yy复制三行
		命令模式iao->输入，esc键回退，	
		输入：进入底线模式：回车结束，wq是保存退出，q仅退，q！强制退出，set nu显示行号，粘贴时：set paste

# 权限管理：

su - root 切换管理员用户，exit退出(长期使用root有风险)
sudo 其他命令 ：使得当前的用户临时使用管理员的权限visudo，在最后行添加fangxiang ALL=(ALL)   NOPASSWD:ALL，为用户添加sudo的认证
用户（可以在多个用户组）、用户组

## 用户组管理：

在root下：groupadd 用户组名， groupdel 用户名 添加删除
		getent group 查看有哪些组
用户管理：  useradd [-g -d]用户名 -g指定用户组，-r指定用户的home目录（默认在home/用户名下）
	userdel[-r]  用户名  -r同时会删除home目录
	id 用户名 查看组属
	usermod -aG 用户组 用户名  将用户加入用户组
	getent passwd 查看有哪些用户信息
ls -l 显示出来的是 权限信息（10个槽，第一个是文件类型rwx可读可写可执行，后面每三个是所属用户、用户组、其他用户的权限）+所属用户+所属用户组

## chmod命令

：修改权限信息，只有所属用户和root可以修改
	chmod [-R] 权限 文件夹/文件  -R对文件夹内全部文件修改	chmod    u=rwx，g=rx,o=x     hello.txt
	chmod 751文件  "751"  :0- 1x 2w 3wx 4r 5rx 6rw 7rwx
	chmod +x test.sh
chowm 修改文件所属用户、用户组（只能root用户使用）
	chown [-R] [用户]：[用户组]   文件   -R全部进行操作

# 实用操作：

## 快捷键

   ctr+C/Z  强制停止   、  ctr+D 退出账户的登录、python等  、ctr+R查找历史命令 、	crt+L清屏=clear
   Ctr+shift+C/V/F 复制粘贴查找
   history显示历史命令+grep过滤 
   ctr+A命令开头、E结尾、<-、->左右跳一个单词

## 安装软件：

​	CentOS:.rpm yum  Ubuntu:.deb,apt
   yum命令安装软件（.rpm软件安装包）yum [-y] [install  remove  search]  软件名   -y是自动确认、要在root用户下
   	需要先换源参考：https://blog.csdn.net/2301_76952009/article/details/140275341?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7ECtr-1-140275341-blog-86640417.235%5Ev43%5Epc_blog_bottom_relevance_base9&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7ECtr-1-140275341-blog-86640417.235%5Ev43%5Epc_blog_bottom_relevance_base9&utm_relevant_index=2
systemctl  start/stop/status/enable/disable       控制系统/第三方软件（一般称为：服务）启动关闭、查看状态、开机自启开关
​	如ssh服务（FinalShell远程连接使用）、firewald等
软连接：文件/文件夹链接到其他位置：（快捷方式）：ln -s 文件 目的地
data 查看时间，格式可以自定义  data -d "+3 month"通过-d完成时间计算  、初次使用要切换时区，通过阿里云服务器手动校准时间：ntpdate -u ntp.aliyun.com
​	root下：rm -f /etc/localtime, ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime,  安装ntp软件，自动校准时间，并设置ntpd自启
IP地址：ifconfig, 显示多个网卡（CentOS的主网卡：ens33）：特殊IP： 127.0.0.1代指本机   0.0.0.0端口绑定/本机/所有
hostname :查看主机名、 hostnamectl set-hostname 修改主机名
通过域名解析访问IP地址 如baidu.com，私人系统（/etc/hosts、配置可以便捷使用IP）有记录直接打开，没有则去公开DNS服务器联网查询
固定IP：方便远程连接，重启可能会发生更改IP

## 网络传输：

ping [-c 3] ip/主机名 检查网络服务器是否可联通  ping -c 3 baidu.com
	wget [-b] url   下载网络文件（像迅雷）、-b是后台下载
	curl [-O] url    （类似与打开网页）发送网络请求下载文件时加-O/  获取信息时不用加  如cip.cc
端口：设备与外界通讯交流的出入口：物理端口（USB/HDMI接口等）、虚拟端口（计算机内部指定到某一程序）：IP小区、端口：门牌号
	Linux支持65535个端口有三类用途，公认端口：1-1023：内置服务，注册端口可以随意使用1024-49151、动态端口：联网临时使用
	nmap命令查看IP的端口占用情况、netstat -anp | grep 6000  查看某一端口占用情况

固定IP地址：现在vm上配置，然后vim /etc/sysconfig/network-scripts/ifcfg-ens33

添加IPADDR="192.168.88.130"
NETMASK="255.255.255.0"
GATEWAY="192.168.88.2"
DNS1="192.168.88.2"，修改：BOOTPROTO="static"，，然后 restart network

## 进程管理：

每个程序运行就会注册一个进程并分配一个进程ID（任务管理器）
	ps [-e -f]    -e显示全部进程 -f全部信息  ps -ef
	kill [-9] 进程ID  关闭进程，-9强制关闭
主机状态的监控：top命令，详解信息以及命令选项可以看视频
	df命令查看磁盘使用， sar命令查看网络统计信息：sar -n DEV num1（刷新间隔） num2（查看次数）

## 环境变量：

辅助系统运行的关键信息、全局的效果 PATH
	env命令、 echo$PATH取环境变量
	可以创建自己的env来完，需要添加可执行权限755
	环境变量设置：export 变量名=变量值（临时生效），用echo$变量名打印，
	永久生效：在当前用户： vim ~/.bashrc 在下面添加上面语句，如果对所有用户生效：在root，vim /etc/profile 添加，记得source 文件使其生效
文件上传下载：直接在FinalShell里面拖拽或者右键下载本地，或者rz，sz命令，同时在上传下载root文件时时也要考虑权限问题，可以连接配置

## 压缩和解压

：tar（仅封装无内存减小）和gzip（可以减少体积）
	tar [-c创建 -v显示 -x -f文件名 -z/gzip模式 -C] 
	常用组合： tar -zcvf test.tar 1.txt 2.txt 3.txt  创建压缩，    tar -zxvf test.tar -C 目录    解压到目录
	zip -r files    zip格式压缩-r有文件夹才用，    unzip test.zip -d test/  解压



Shell基础：命令语言、程序设计语言、一种应用程序、Linux内置的脚本（批量处理）
Linux默认的是/bin/bash 的shell   、bash shell 还有其他的shell  
代码规范： touch/vim创建test.sh、执行shell脚本chmod，运行必须写成./test.sh，不能直接test.sh不然会去默认系统路径找
#!/bin/bash  指定解释器的路径
shell指令堆积；可加可不加

# shell进阶：

变量：      class_name="name"  #类似于python，但是没有顶格的要求，对空格使用有要求,=不允许有空格，不加类型、    使用：echo $class_name,加上$符号就行
	time=`date +"%F  %T"`  echo $time
	readonly class_name设置为只读
	read -p 提示信息 变量名、  unset 变量名 删除变量
条件判断语句：
	if condition1
	then	
	     cmd1
	     cmd2...
	elif condition2
	then
	     cmd3
	else
	     cmd4
	fi  #结束

## 运算符：

多了一个特殊的文件测试运算符
算术(bash不支持简单数学运算，调用指令)：
	`expr $a +-\*/ $b`，、\*乘号和通配符的区别，expr 2 + 2（注意必需有空格）
	a=$b
	[ $a != == $b ]注意4个空格
关系运算符（只支持数字/“数字”）：
	[ $a -eq $b ]、 -ne不相等，-gt大于，-lt小于、-ge大于大于，-le小于等于
逻辑运算：[ !false]、 [ $a -lt 20 -o $b -gt 0 ]   -o或，-a且
字符串运算：[ $a = $b ]一个等判读字符串是否相等，[ -z $a ]检测长度是否为0、 -n是否不为0 、[ $a ]判断是否为空

## 文件测试运算符（重点且常用）：

​	[ -d $file ]  检测是否是目录，-f是否是文件，-r 可读？ -w -x  -s是否为空 -e 是否存在
脚本附带选项： $1 $2分别代表第一、第二个参数
​	if [ $1 = '-add' ]
​	then
​	   useradd $2
​	elif[ '-r' ]
​	then
​	   userdel -r $2
​	else
​	   echo "err"
​	fi
​	   
调用时：./test.sh -add  hostname    为了方便可以取别名：vim ~/.bashrc  中添加：alias userr=‘/root/test.sh’，source激活一下就可以直接调用 userr -add name
​	



# Linux + C++：

C++编译：
调用： ./out  ！！记得有./

一、linux下：没法auto

for(list<student>::iterator it=l.begin();it!=l.end();it++)
    {
        cout<<(*it).name_<<"--"<<(*it).age_<<"--"<<(*it).height_<<endl;
    }

或者指定g++的标准：   加上  -std=c++11



\#include <unistd.h>

pause()；   Linux系统无法识别system（“pause”）

## gcc/g++编译器：

文件编译的过程：（不常用但是要熟悉）
1.预处理：生成.i文件
	g++ -E test.cpp -o test.i   
2.编译，生成.s文件
	g++ -S test.i -o test.s
3.汇编，生成.o文件
	g++ -c  test.s -o test.o
4.链接  可执行文件
	g++ test.o -o test

## 重要参数：

1. -g（可以略）  test.cpp    待编译的的文件  （加上-g 才能gdb调试）
2. -o  output   输出可执行文件（不加默认是生成a.out）		-o2  -o[n]  对源代码进行优化并输出可执行文件
3.-l 连接库  -L 链接库文件所在的目录名：     -L目录 -l库
4. -I(大小i)  include文件    ： -I/myinclude
5.-wall 打印警告信息    -w关闭警告信息
6.-std=c++11   指定编译标准
7.-D 定义宏

例子：
g++   main.cpp    src/swap.cpp    -Iinclude    -o    out

## 生成库文件：

静态库：（先cd src）  g++   swap.cpp  -c   -I../include    汇编生成.o文件
			ar rs libswap.a swap.o  生产静态库文件

	（cd ..） g++ main.cpp -Iinclude  -Lsrc -lswap -o out  -std=c++11   链接可执行文件
动态库： g++ swap.cpp  -Iinclude -fPIC -share -o libswap.so  链接方式同上
	调用可执行程序时需要加上搜索路径：LD_LIBRARY_PATH=src   ./out2  （或者放到默认的搜索路径里）



## GDB调试器

（linux下最常用）

主要功能：设置断点、单步执行、查看值的变化等
1.常用调试命令参数：
	gdb 【文件名】进入调试：
		help(h)、 run（r）、  start开始单步执行、next（n）下一步，函数会直接执行、step（s）下一步会进入函数、  print i  查看变量值    display 持续追踪 
		list（n）从第n行开始查看代码、set 设置变量值、info（i）查看变量值、finish结束当前函数回到函数调用点
		backtrace（bt）查看函数的调用的栈帧和层级关系、frame（f）切换栈帧
		continue（c）继续执行、print（p） 打印值以及地址
		quit退出调试、break+num  （简写b） 在num行设置断点、  info breakpoints查看所有断点、delete  breakpoints num（d）删除断点
		display 追踪具体变量值、undisplay取消、watch 观察的变量发生修改时打印、i watch 显示观测点   enable/disable breakpoint 启用断点、
		 run argv【1】 argv【2】 调试时命令行传参
	回车：重复上一命令、编译加上-g 才能gdb调试

## 	IDE：VSCode

1.软件安装：

python版本更新：https://blog.csdn.net/sasafa/article/details/125577770

难受！！！需要升级glib、升级gcc，升级python，升级make  （没必要）

2.安装老版本的VSCode！！！！！

https://blog.csdn.net/qq_42371913/article/details/115294404
