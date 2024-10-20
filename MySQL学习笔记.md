# 基础

客户端<-->web服务器 <-->数据库服务器：动态项目必不可少、mysql，oeacle产品、关系型数据库
与php是黄金搭档，工作环境：LAMP（linux、apach、mysql、php）、LNMP（nginx）

登陆：mysql -u root -p

MySQL图形化界面：sqlyog、navicat、datagriap；

SQL语言基础：操作《关系型》数据库的编程语言StructureQueryLanguage、统一标准
基础：（够使用了）SQL语言、函数、约束、多表查询、事务（50%）
进阶：（涉及底层了）存储引擎、索引、SQL优化、锁、集群（80%）
运维：日志、主从复制、分库分表、读写分离...略

关系型数据库：多张相互连接的二维表组成：用表存储、用SQL语言操作，客户端连接DBMS管理系统，再用SQL操作DB

SQL语句：单行/多行分号结尾！！，--/#注释，/*  多行注释 */
分类：DDL（definition定义）、DML（manipulation操作）、DQL（query查询）、DCL（contrl控制、用户以及用户权限）

## 一、DDL语句

DDL：show Database；  select database（）；  create database [if not exists] 数据库名； drop database 数据库名；  ues 数据库名；
show tables；查看所有表、desc 表名，查看表结构；  show create table 表名；查看建表语句；
create tables 表名（
	字段  类型【comment “注释”】，
	字段  类型 [comment “注释”]		字符串  name varchar（50）；
） 【comment “注释”】
	数据类型：
	   数值类型： tinyint 1（-128-127 unsigned 0-255）、smallint 2、mediumint 3、 int 4、bigint 8、float 4、double 8byte；decimal精度标度
	   字符串类型，char（10） 定长（固定空间，但性能高）、varchar变长、tinyblob二进制数据（视频音频等）、tinytext、--、medium--、long--
	   时间日期类型：date 3byte ****-**-**格式，time 3 **：**：**， years ****、datetime、timestamp时间戳
             alter table 表名 add 字段名 类型 【comment】； drop 字段名；rename to 新表名；#添加完1列之后一个用update添加数据了  
	drop table 表名；truncate table 表名；删除表内容但是表结构还在。
	修改数据类型/都修改： alter table 表 modify 字段 类型；  change 旧字段 新字段 类型；

## 二、DML语句

DML：对数据增删改，insert、update、delete
insert into 表名（字段1，字段2...） values（值1，值2）	、直接values（v1，v2...）所有  #添加一行
	批量添加：insert into 表名 （字段1，字段2）values（值1.1，值1.2），（值2.1，值2.2）... 同上  字符串和日期要加“”
update 表名 set 字段1=v1，字段2=v2...【where 条件】：   ：where id=1；
delete from 表名 【where 条件】；
	修改一列：

## 三、DQL语句

select
	字段列表
from
	表名列表（单/多表）
where
	条件列表
group by
	分组字段列表
having
	分组后条件
order by
	排序条件
limit
	分页参数

执行顺序，from、where、group、order、select、limit

基础查询：select 字段1，字段2.. from 表名； select * from 表名；所有
	设置别名： select 字段1【as 别名1】，字段2【】.. from 表名；
	去除重复：select distinc 字段列表 from 表名；
条件查询where：>=、!=、between...and...、 in(...,...)、 is not /null、&& 、|| 、！
	 like 占位符（模糊匹配：name like '__',一个下划线代表一个符号，'%x',最后一个是x的，%代表任意个，_代表一个）
聚合函数：配合分组查询：count，统计数量，max、min、avg、sum：按列的
	语法：select 聚合函数（字段列表）from 表；  	null值的不参与计算
分组查询：select 字段列表 from 表名 where ...  group by 分组字段名 【having 分组后的过滤条件】
	select gender,count(*) from emp group by gender;
	 select workaddress, count(*) from emp where age <45 group by workaddress  having count(*) >=3;
排序查询：select 字段列表 from 表 order by 字段1 ASC，字段2，DESC；  默认ASC升序，DESC降序
分页查询：imit  10（自己算）， 5 	：起始索引（当前页码数-1）*每页条数 ，  查询记录数：查询第3页，每页显示5条
	limit 5；查询第一页的前五个
	不同数据库实现方式不同

## 四、DCL语句

四、DCL语句：用户与权限、SQL开发人员用的少、DBA数据库管理人员用得多
查询用户： use mysql； select * from user；
创建用户：create uesr  '用户名'@'主机名' identified by '密码';  主机名为有登录权限的主机、@’%‘则任意主机都可以
修改用户密码： alter user '用户名'@'主机名' identified with  mysql_native_password by '新密码';
删除用户：drop user '用户名'@'主机名'；
	set global validate_password_policy=LOW; # 密码安全级别低
	set global validate_password_length=4; # 密码长度最低4位即可
权限控制（常用）：all、select、insert，update、delete删除数据、alter修改表、drop/create删除数据库/表
	查询某个用户的权限：show grants for '用户名'@'主机名'
	授予权限：grant 权限列表 on 数据库.表名 to '用户名'@'主机名'
	撤销权限：revoke 权限列表 on database.table from '用户名'@'主机名'

## 函数：

1.字符串函数：concat（s1，s2，s3..）拼接、lower（）、upper大小写、lpad、rpad（str，n，pad）用字符串pad对str进行左右填充
	trim去头尾的空格、substing（str，star，len）从star（从1开始算）取len个
	调用：select 函数（）；update emp set id = lpad(id,2,'0');
2.数值函数：ceil，floor、round上下四舍五入取整，mod（x，y）取余，rand（）0-1随机数
3.日期函数：curdate()、curtime、now（）返回日期加时间、year（data）、month、day（date）、返回日期的年月日
	date_add（date，interval ***  /10 day/ 5 year）时间值加上一个时间间隔exper后的时间，datadiff（date1，date2）时间间隔天数
4.流程函数：if（value，t，f）根据value返回t/f，ifnull（value1，value2）1为空则返回2不为空返回1
	case when [val1] then .... when [val2] then ....else ...end、 	  case [exper]  when[val] then ... when[val2] then ...else ...end; 
	类似于if和case语句

## 约束：

保证数据库中数据的有效性（创建表的时候设置）
not null、 unique、primary key（主键约束，唯一标识、非空＋唯一，auto_increment自增）、default（默认约束，没给值存入默认值）
check（检查约束8.0.16后才有）、foreign key（外界约束，使两张表建立连接保证一致性）
例：create table user(
       id int primary key auto_increment,
       name varchar(10)  not null  unique ,
       age int check ( age>0 && age <= 120 ),
       status char(1) default  '1',
       gender char(1)
   );
外界约束：父表和子表：  alter table 表名 add constraint   外键名称（自己取）   foreign key  reference 主表（主表列名）；
	alter table    emp      add constraint       fk_emp_dept_id             foreign   key(detp_id)   !!!reference dept(id)  【on   update/delete  行为】；
	【行为】：no action/restrict 就是默认的不允许删除、cascade会将子表数据一起删除、set null 可以删除父表数据，子表外键设为null
	删除外键：alter table emp drop fk_emp_dept_id；
	黄色钥匙是主键，蓝色是外键（唯一且不空）、有关联数据就不能直接删了

## 多表查询：

   多表关系：一对多（部门和员工）、多对多（学生和课程、建立中间表）、一对一（加上唯一约束unique、同时也可以建立自连接）
	外键必需关联主键，primary key（一般是id）
   多表查询（子查询/连接查询）：select * from emp，dept； 会全部显示所有组合m*n个 （笛卡尔积）
	消除无效笛卡尔积：+ where emp.dpt_id = dept.id;
	连接查询：内连接（差交集）、外连接（左外连接：左表以及交集）、自连接（当前表与自身的连接，必须使用表别名）
  内连接：隐式：select 字段列表 from 表1，表2  where..；
	       取别名：select e.name， d.name from emp e！，dept d（没有as） where..；
	显式：select 字段列表 from 表1 inner join 表2 on 连接条件；
   外连接：左外连接：查询表1所有数据，以及和表二交集的数据：比如：所有员工的部分信息（包含没有加入部门的员工信息）
	select 字段列表 from 表1 left outer join 表2 on 连接条件；
   自连接：select 字段列表 from 表A 别名A  join 表A 别名B on....；  比如：查询所有员工以及其领导名字（在同一张表内）；
   联合查询：就是多次结果直接合并起来（可重复、要保证字段列表一致）  中间靠 union 【all】来连接； 没有all可以去重
   子查询(嵌套查询)：select * from t1 where  colum = （select colum from t2）；
	标量子查询：子查询返回是标量
	列子查询：子查询结果是一列，常用：in、not in 、any（满足一个） 、all（都需满足）
		salary > all/any/in(子查询)
	行子查询：常用=，!= 、in、not in,&&
	表子查询：常用in：与员工a，b两人职位和薪资相同的员工信息  （job，salary）in （子查询a、b的职位和薪资）、查薪资等级

## 事务：

一组操作的集合，要么全部成功，要么全部失败
	开启事务：查看/设置事务的提交方式 select @@autocommit；    set  @@autocommit =0；
	提交事务：commit； （事务完成之后后面加上手动提交）
	回滚事务：rollback；（在异常时会回退）
	（推荐）方式二：不修改提交方式：start transaction ; ....；commit； rollback；
事务四大特性：
	原子性：一起成功/失败，不可分割
	一致性：完成时，所有数据状态保持一致（转账总量不变）
	隔离性：事务a和b同时发生操作员工数据库，二者不相互影响
	持久性：一旦提交/回滚，对数据的改变是永久的（存在磁盘里）
并发事务：并发问题：（脏读：读到一个还没提交的数据，不可重复读：两次读取不同，幻读：读没有插入时又存在了）
事务隔壁级别：read uncommitted （均有出现），read committed（不会脏读），repeatable read（默认的，只会幻读），serializable（均不出现）
	权衡效率性能、安全性、并发性能；
 	查看：select @@transaction_isolation;  设置：set 【session/global（当前/全部会话窗口）】transaction isolation level {四种级别}；   



# 进阶

内容：存储引擎、索引（最核心、重要）、SQL优化、视图/存储过程/触发器，锁，innoDB引擎，Mysql管理

## 一、存储引擎

Mysql体系结构：连接层、服务层、引擎层（可插拔，index索引实现）、存储层（磁盘）
客户端连接器：PHP，Python、java（jdbc）、ruby
存储引擎：存储、索引、更新查询实现，基于表的（不图表可以选择不同的引擎）、默认engine = innodb；show engine；查看使用comment
	innoDB：支持事务、外键约束、行级锁，对应  xxx.ibd  文件；
	myisam：不支持外键、访问快 （文件对应3个：.MYD，.MYI，.sdi文件）
	memory：存在内存中，访问快，但断电消失、可作临时、模式hash哈希索引

## 二、索引

1.常见：B+树（默认）、hash索引、R-tree（空间索引）、full-text全文索引（倒排索引实现），其他：二叉树、红黑树...数据结构
2.前四种：innodb支持b+、full-text， mylsam不支持哈希索引、memory支持哈希和b+；  
3.B+树：主要（可以去了解一下）
4.哈希索引：哈希函数、数据结构已学（不支持范围匹配、和排序操作，查询快，但是功能有限，所以选b+树）
5.索引分类（关键字）：主键索引：primary、唯一索引：unique，常规索引、全文索引：fulltext
	第二种分类：二级索引（叶子节点存放id（主键、唯一））、根据id进入聚集索引（叶子节点存储所有数据），---》回表查询
6.索引的语法：
	创建：create 【关键字】 index index-name  on  table-name （字段/列表）
	查看：show index from table-name；
	删除：drop index index-name on table；
7.sql性能分析与优化：  show global status like ’com_____（七个下划线，）‘;查看增删改查的频率，来决定要不要优化；
	查看慢语句的log、show profiles等查看耗时、explain查看是否用到了索引  等工具，同时改变索引，执行查看效率是否有提升

## 三、mysql语句的优化

插入、主键、order by 、update、count、group by等语句的优化 
       可以看看小结.......

## 四、视图

虚拟的表（可以封装SQL逻辑）、！！！！可能有用
       存储过程、存储函数、触发器 
      看小结..pass

## 五、锁

（解决事务相关的问题）  ！！！  感觉有用
   全局锁：只读
   表级锁：锁住对于表的数据、易发生锁冲突
   行级锁：锁住对应行的数据

## 六、Mysql管理

常用工具：1.  mysql [option]  [database]:  -u 指定用户 -p密码  -h指定IP  -e 退出（-e可以不用连接mysql，批量处理脚本）
	例如：mysql -u ... -p ... [database]  -e  "sql语句"  :就可以在终端显示而不用登陆

 	2.  mysqladmin -u .. -p ...   （具体语法  -help 查看）  管理相关的指令、建立数据库、用户等（不用连接数据库）
	3.mysqlbinlog  日志管理（运维..pass）
	4.mysqlshow   客户端查找对象的工具（数据库、索引、表等）
	5. mysqldum 在不同数据库间进行数据迁移备份