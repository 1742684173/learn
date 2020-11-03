mysql安装步骤http://jingyan.baidu.com/article/642c9d34aa809a644a46f717.html

数据库

http://pan.baidu.com/s/1o6Ecw0U?qq-pf-to=pcqq.c2c

tj53





# **997.Linux普通用户配置mysql**

## **997.1.创建my.cnf**

[client]  

port=3336  

socket=/home/zxl/mysql/mysql.sock  

[mysqld]

port=3336

basedir=/home/zxl/mysql

datadir=/home/zxl/mysql/data

pid-file=/home/zxl/mysql/mysql.pid

socket=/home/zxl/mysql/mysql.sock

log_error=/home/zxl/mys



## **997.2.安装mysql**

./bin/mysqld --defaults-file=/home/zxl/mysql/my.cnf --initialize --user=zxl --basedir=/home/zxl/mysql --datadir=/home/zxl/mysql/data 

./bin/mysqld  --initialize --user=zxl --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data 

w=3#h:Fz3MX<



## **997.3.启动**

./bin/mysqld_safe --defaults-file=/home/zxl/mysql/my.cnf --user=zxl

./bin/mysqld_safe --user=zxl



## **997.4.登录**

./bin/mysql -u root -p -S /home/zxl/mysql/mysql.sock



**988.错误及解决方法**

**错误1：**This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)

解决方法：set global log_bin_trust_function_creators=TRUE;



**989.数据的导入导出**

**1.表移动**

导出：mysqldump -uroot -p --databases 库名 > /mymediadb.sql

进入：需进入mysql，然后用 source 执行文件，如：source wjdc.sql

语法

备份 SELECT * INTO {OUTFILE | DUMPFILE} 'file_name' FROM tbl_name

恢复 LOAD DATA [LOW_PRIORITY] [LOCAL] INFILE 'file_name.txt' [REPLACE |

IGNORE]  INTO TABLE tbl_name  

//LOW_PRIORITY不用对数据库进行上锁，数据的导入将被推迟到没有客户读表为止

//如果指定local则从本地进行读数据，没有指定则从服务器上进行读

//REPLACE导入数据时如果有重复(根据主键来判断)就替换，用ignore是跳过此行

方法一

导出数据(有时候不能导入c盘,因为权限的问题)

步骤1 对表进行上锁 ，防止在导出时对表进行更新LOCK TABLES 表名 READ

步骤2 导出数据 

select * from test into outfile 'd:/data.txt' fields terminated by',' lines terminated by'\r\n';

步骤3 解锁 unlock tables;

导入数据

步骤1 LOCK TABLES 表名 WRITE

步骤2 load data infile 

'd:/data.txt' into table 表名 fields

 terminated by ' '   --列间隔

  enclosed by ''''  --空值

lines terminated by '\r\n'; --换行

步骤三：解锁 unlock tables;

**990.乱码处理**

乱码处理

1、找到my.ini

2、进行修改或添加 

![my.ini](D:/software/YoudaoNote/%E6%9C%89%E9%81%93%E7%AC%94%E8%AE%B0/qq90F107DAA1E3639B3668DE4D65FCB5C8/374373bdcb254f908fa570884c97710b/attachment.png)

3、重启(我的电脑->右击->管理->服务)

4、创建表的时候要创建编码

"create table  friendsList (qq varchar(7) primary key,remark varchar(20),groupName varchar(20)) charset utf8 collate utf8_general_ci;"

**991.存储过程和函数**

https://blog.csdn.net/ycl295644/article/details/45887039

存储过程

***********************************************************************************************

一、create procedure([IN|OUT|INOUT] 参数名 参数类型)

  \1. IN输入参数：表示该参数的值必须在调用存储过程时指定，在存储过程中修改参数的值不能被返回，为默认值

  \2. OUT 输出参数：该值可在存储过程内部被改变，并可返回

  \3. INOUT输入输出参数：调用时指定，并且可被改变和返回

  例1 in函数

  **delimiter** //  --为了不让分号来中止

​    create procedure 名称(IN age int)

​    begin

​      语法;

​    end; --注意分号

  // --结束当前的存储过程

  **delimiter ; --注**意这里分号要和delimiter用空格分开 ，后面的语句就开始用分号进行结尾了

二、变量定义 DECLARE variable_name [,variable_name...] datatype [DEFAULT value]  

1. DECLARE l_int int unsigned default 4000000; 
2. DECLARE l_numeric number(8,2) DEFAULT 9.95; 
3. DECLARE l_date date DEFAULT '1999-12-31'; 
4. DECLARE l_datetime datetime DEFAULT '1999-12-31 23:59:59'; 
5. DECLARE l_varchar varchar(255) DEFAULT 'This will not be padded'; 

三、变量赋值 SET 变量名 = 表达式值 

  如：set @age = 18;

四、查询存储过程

select name from mysql.proc where db=’数据库名’;//查看一个数据库里面有哪些存储过程

show create procedure 数据库.存储过程名; //查看存储过程的详细

五、语句

  \1. if-then -else语句

​    例 1、create procedure pro1(in sex char(2))

​    begin

​      declare my_sex char(2);

​       if sex = '男' then set my_sex='男';

​       else set my_sex='女';

​       end if;

​       select * from stu where Ssex = my_sex;

​    end;

   例2.分页查询

​    create procedure pro(in page int)

​    begin

​    declare allRecord int;

​     declare pageSize int;

​     declare maxPage int;

​     declare beginPage int;

​     declare endPage int；

​     select count(*) into allRecord from stu;--查询总共的记录

​     set pageSize = 3;--设置第页显示的页数

​     set maxPage = ceiling(allRecord/pageSize);--总共有多少页

​     --固定页数在正常范围内

​     if page > maxPage then set page = maxPage; end if;

​     if page < 1 then set page = 1; end if;

​     set beginPage = (page-1)*3;--起始页

​     select * from stu limit beginPage,pageSize;

​    end;

2.case语句

create procedure pro(IN a int,IN b int,IN code char(1))

begin

 declare s int;

 case code

 when '+' then set s = a+b;

 when '-' then set s = a-b;

 end case;

 select concat(a,code,b,'=',+s);

end;

3.循环语句

例1.create procedure pro(in num int)

begin

 declare sum int default 0;

 declare i int;

 set i = 0;

 while i < num do

  set sum=sum+i;  

  set i=i+1;

 end while;

 select sum;

end;

4.游标的使用

  声明游标：declare 游标名 cursor for select_statement

  open游标：open 游标名

  fetch游标： fetch 游标名 into var_name,......

  close游标：close 游标名

 例1.

create procedure pro()

 begin

 declare no varchar(7);

 declare name varchar(20);

 declare allName varchar(200);

 declare sex varchar(2);

 declare first_cur cursor for

  select sno,sname,ssex from stu;

 declare exit handler for not found close first_cur;

 open first_cur;

 repeat

  fetch first_cur into no,name,sex;

  select concat(no,' ',name,' ',sex);

 until 0 end repeat; --0是结束判断条件

 close first_cur;

end;

****************************************************************************

异常捕获

DECLARE handler_type HANDLER FOR condition_value[,....] sp_statement

handler_type取值范围：CONTINEU | EXIT|UNDO

condition_value取值范围：SQLSTATE[VALUE] sqlstate_value | condition_name |SQLWARNING |NOT FOUND |SQLEXCEPTION|mysql_erro_code

SQLWARNING是对所有以01开关的SQLSTATE代码的速记

NOT FOUND 是对所有以02开头的SQLSTATE代码的速记

SQLEXCEPTION是对所有没有被SQLWARNIN或NOT FOUND捕获的SQLSTATE代码的速记

例1.EXIT

DROP PROCEDURE IF EXISTS pro;

CREATE PROCEDURE pro(IN no VARCHAR(7),IN name VARCHAR(20))

BEGIN

 DECLARE exit HANDLER FOR SQLEXCEPTION,SQLWARNING,NOT FOUND

 ROLLBACK;

 START TRANSACTION;

 insert into stu(Sno,Sname) values('00000',name);

 insert into stu(Sno,Sname) values(no,name);

 COMMIT;

END;

例2.CONTINUE自定义错误码

DROP PROCEDURE IF EXISTS pro;

CREATE PROCEDURE pro(IN no VARCHAR(7),IN name VARCHAR(20))

BEGIN

 DECLARE t_error INT DEFAULT 0;

 DECLARE CONTINUE HANDLER FOR SQLEXCEPTION SET t_error=1;

 START TRANSACTION;

 insert into stu(Sno,Sname) values('00000',name);

 insert into stu(Sno,Sname) values(no,name);

 IF t_error = 1 THEN 

  ROLLBACK;

  SELECT 'ERROR';

 ELSE

   COMMIT;

  SELECT 'SUCCESS';

 END IF;

END;

****************************************************************************

函数：跟存储过程很相近，区别在于存储过程没有返回值，而函数有返回值

create function pro() returns int

begin

set @x=1,@y=2;

repeat 

set @x=@x+1;

until @x>5 end repeat;

return @x;

end;

**************************************************************************************

**992.权限与安全**

1、MySQL权限系统通过下面两个阶段进行认证

1.1、对连接的用户进行身份认证，合法的通过认证，不合法的用户拒绝连接

注：是通过IP地址和用户名联合进行确认的(默认创建有用户root@localhost)

1.2、对通过认证的合法用户赋予本应的权限，用户可以 在这些权限范围内对数据库做相应的操作

注：当用户进行连接的时候，权限的存取过程有以下两个阶段

先从user表中的host、usertkg password这3个字段中判断连接的IP、用户名和密码是否存在于表中，如果存在，则通过验证，否则拒绝验证。如果通过验证，则按照以下权限表的顺序得到数据库权限：user->db->tables_priv->columns_priv

mysql.user

**查询权限：**show grants for '用户'@'主机' 	如：show grants for root@localhost;

**创建用户**：create user '用户'@'主机'  identified by '密码'	  如：create user zxl@% identified by '11111';

%表示所有主机 

**分配权限：**grant 权限列表 on 库.表 to 用户名@主机 with grant option   如：grant all privileges on *.* to zxl@'%' with grant option;

**收回权限：**remoke 权限列表 on 库.表 to 用户名@主机 

**删除用户：**drop user '用户名'@'主机'

**993.数据备份和恢复**

1.逻辑备份：逻辑备份的最大优点是对于各种存储引擎，都可以用同样的方法备份；

1.1、备份所有数据库

mysqldump -uroot -p --all-database(数据库名称) > all.sql(备份的文件)

1.2、备份数据库

mysqldump -uroot -p  test(数据库名称) > test.sql

1.3、备份数据库下的某一张表

mysqldump -uroot -p  test(数据库) emp(表) > emp.sql 

1.4、备份数据库下的emp表和dept表

mysqldump -uroot -p test emp dept >emp_dept.sql

1.5、mysqldump -help

注：-l代表给表加锁	-F代表生成一个新的日志文件

2.数据的恢复：mysql -uroot - p dbname < bakfile(将备份作为输入即可)，这样恢复的数据并不完整，还需要将备份后的日志进行重做mysqlbinlog binlog | mysql -u root -p

完全恢复

进行备份：mysqldump -uroot -p -l -F test > test.dmp

进行恢复：mysqlbinlog binlog | mysql -u root -p

**994.触发器**

1.语法

CREATE TRIGGER trigger_name trigger_time trigger_event

ON table_name FOR EACH ROW trigger_stmt

trigger_time:触发时间【BEFORE/AFTER】

trigger_event:触发事件【INSERT,UPDATE,DELETE】

INSERT 是用OLD

DELETE是用NEW

UPDATE两者都可以用

2.INSERT TRIGGER

CREATE TRIGGER tri BEFORE INSERT

ON stu FOR EACH ROW

INSERT INTO my(beforeName) VALUES(NEW.Sname);

3.UPDATE TRIGGER

create trigger tri after update

ON stu FOR EACH ROW

INSERT INTO my(afterName,beforeName) VALUES(NEW.Sname,OLD.Sname);

4.DELETE TRIGGER

CREATE TRIGGER tri BEFORE DELETE

ON stu FOR EACH ROW

INSERT INTO my(beforeName) VALUES(OLD.Sname);

**995.****事物控制基本语法** 

  START TRANSACTION | BEGIN [WORK] 开始一项新的事务

  COMMIT [WORK] [AND [NO] CHAIN] [[NO] RELEASE] 用来提交事务

  ROLLBACK [WORK] [AND [NO] CHAIN] [[NO] RELEASE]  回滚事务

  备注：在没有commit之前的数据可以回滚，用SAVEPOINT+名称 来指定要回滚的事务，RELEASE  SAVEPOINT命令可以删除SAVEPOINT

  SET AUTOCOMMIT ={0 | 1}

  备注： CHAIN和RELEASE 子句分别用来定义在事务提交或者回滚之后的操作，CHAIN会立即启动一	   个新事物，并且和刚才的事务具有相同的隔离级别，RELWASE则会断开和客户端的连接

​           SET AUTOCOMMIT可以修改当前连接的提交方式，如果设置 了SET AUTOCOMMIT=0,则设置之后的所有事务都需要通过明确的命令进行提交 或者回滚

**996.索引**

**1.创建索引**

create unique index 索引名 on 表名(column_list)

create index 索引名 on 表名 (column_list)

**2.删除索引**

drop index 索引名 on 表名

**3.显示表的索引**

show index from [表名]

**997.修改表结构**

**1.增加列**

alter table [表名] add [字段名] [类型]  

如：alter table stu add name varchar(20);

**2.删除列**

alter table [表名] drop [列名]

如：alter table stu drop name; 

**3.改变列的类型**

alter table [表名] modify [列名] [类型];

如：alter table stu modify name varchar(50);

**4.更改列名**

alter table [表名] change [列名] [新列名] [类型]；

如：alter table stu change name name1 varchar(50);

**5.更改表名**

alter table [表名] rename [新表名] 

**6.增删主健**

alter table [表名] add primary key ([主健名称]);

alter table [表名] drop primary key;

**6.增删外健**

alter table 表名 add constraint [FK_ID:外健名称] foreign key(你的外键字段名) REFERENCES 外表表名(对应的表的主键字段名);

alter table 表名 drop foreign key 外健名

show create table 表名   查看所有结构

**998.创建表**.

**0.创建库**

CREATE DATABASE 数据库名;

mysqladmin -u root -p create RUNOO 安全一点

**1.创建表**

create table stu

(

  id int not null auto_increment,

  name varchar(20),

  sex tinyint check(sex=0 or sex=1),

  age mediumint default 0,

  period int check(period > 0), --

  test int, 

  primary key(id),

​    foreign key(外健) regerences 外健表(外健)

)ENGINE=InnoDB DEFAULT CHARSET=utf8;

**注：**ENGINE是设置存储引擎

**2.字段：**

**2.1.整型**

tinyint(1)、smallint(2)、mediumint(3)、int(4)、bigint(8)

**注：**括号里的数字表示占多少字节byte)

**2.2.浮点型**

float:浮点型，含字节数为4，32bit，数值范围为-3.4E38~3.4E38（7个有效位）

double:双精度实型，含字节数为8，64bit数值范围-1.7E308~1.7E308（15个有效位）

decimal:数字型，128bit，不存在精度损失，常用于银行帐目计算。（28个有效位

decimal(a,b)：表示小数点左边加右边存储的十进制数字的最大个数，b表示小数点右边可以存储十进制的最大个数

money：含字节数为8，常用于货币类型

**2.3.日期和时间**

year：年

date：年-月-日

time：时:分:秒

datetime：年-月-日 时:分:秒

timestamp：年-月-日 时:分:秒

**注：datetime和timestamp的区别**

**1、存储方式不同**

datetime设置的是什么时间就是什么时间

timestamp则是把客户端插入的时间从当前时区转化为UTC(世界标准时间)进行存储，查询时又将其转化为客户端当前时区进行返回

**2、存储范围不同**

timestamp存储的范围为：’1970-01-01 00:00:01.000000’ 到 ‘2038-01-19 03:14:07.999999’；

datetime  存储的范围为：’1000-01-01 00:00:00.000000’ 到 ‘9999-12-31 23:59:59.999999’。

**3、timestamp不能为空，且增改跟操作时间一致**

**2.4.字符串类型**

字符串：char(1-255)、varchar(1-255)、

文本：tinytext(1-255)、text(1-65535)、mediumtext(1-16777215)、longtext(1-4294967295)

二进制：tinyblob(1-255)、blob(1-65535)、mediumblob(1-16777215)、longblob(1-4294967295)



# **999.查询相关信息**

## **999.1.查询库**

show databases;



## **999.2.查询表**

show tables;



## **999.3.显示表结构**

show columns from tables;



## **999.4.查看编码**

show variables like 'character%';



## **999.5.查看日志**

show variables like 'log_%';



## **999.6.连接查询**

select.concat(str1,str2);



## **999.7.查询结果进入新表**

CREATE TABLE test (a int not null auto_increment,primary key (a), key(b)) SELECT b,c from test2;

create table test select * from tb;

create table new_table select name as a from pet;  ##生成 的表里面的属性就是a



## **999.8.分页查找**

select * from 表名 where 条件 limit [开始],[结尾]

## **999.9.随机查找**

select * from 表名 order by rand() limit  [开始],[结尾]



## **999.10.时间查找**

当前日期：select curdate(); //yyyy-mm-dd

当前时间：select curtime();

日期时间：select now();

时间格式化：date_format(时间,'%Y-%m-%d');



## **999.11.表连接查询**

内连接: 只连接匹配的行

左外连接: 包含左边表的全部行（不管右边的表中是否存在与它们匹配的行），以及右边表中全部匹配的行

右外连接: 包含右边表的全部行（不管左边的表中是否存在与它们匹配的行），以及左边表中全部匹配的行

全外连接: 包含左、右两个表的全部行，不管另外一边的表中是否存在与它们匹配的行。

交叉连接 生成笛卡尔积－它不使用任何匹配或者选取条件，而是直接将一个数据源中的每个行与另一个数据源的每个

行都一一匹配

举个例子吧。

表A

id  name 

1  张

2  李

3  王

表B

id  address  A_id

1  北京   1

2  上海   3

3  南京   10

包容性:A表包容B表,左连接左表是全的.(left join 或 left outer join )

SQL语句如下：

SELECT A.name, B.address

FROM A

LEFT JOIN B ON A.id = B.A_id

查询结果为：

name   address

张   北京

李   NULL

王   上海

包容性:B表包容A表,右连接右表是全的.(right join 或 right outer join )

SQL语句如下：

SELECT A.name, B.address

FROM A

RIGHT JOIN B ON A.id = B.A_id

查询结果为：

name   address

张   北京

王   上海

NULL   南京

排他性:A,B表中至少有1个匹配时，才返回行。两表的交集

SQL语句如下：

select A.name,B.address from A

inner join B

on A.id = B.A_id

查询结果为：

name   address

张   北京

王   上海

inner join 内连接等价于下面的sql:

SELECT A.name, B.address

FROM A, B

WHERE A.id = B.A_id

注释:全外连接返回参与连接的两个数据集合中的全部数据，无论它们是否具有与之相匹配的行。在功能上，它等价于

对这两个数据集合分别进行左外连接和右外连接，然后再使用消去重复行的并操作将上述两个结果集合并为一个结果集

。(full join 或 full outer join )

SQL语句如下：

select * from A

full join B

查询结果为：

id   name   id   address A_id

1   张   1   北京   1

2   李   1   北京   1

3   王   1   北京   1

1   张   2   上海   3

2   李   2   上海   3

3   王   2   上海   3

1   张   3   南京   10

2   李   3   南京   10

3   王   3   南京   10

注释：返回3*3=9条记录，即笛卡尔积

SQL语句如下：

SELECT * FROM A

CROSS JOIN B

查询结果为：

id   name   id   address A_id

1   张   1   北京   1

2   李   1   北京   1

3   王   1   北京   1

1   张   2   上海   3

2   李   2   上海   3

3   王   2   上海   3

1   张   3   南京   10

2   李   3   南京   10

3   王   3   南京   10

CROSS JOIN等价于:

select * from A,B

注意:

\1. on A.id = B.id 等同于 using(id)//这里字段名要相同

\2. 当 MySQL 在从一个表中检索信息时，你可以提示它选择了哪一个索引。 

如果 [EXPLAIN](http://hi.baidu.com/woaidelphi/blog/item/b1adb40f02cc3a216159f340.html) 显示 MySQL 使用了可能的索引列表中错误的索引，这个特性将是很有用的。 

通过指定 USE INDEX (key_list)，你可以告诉 MySQL 使用可能的索引中最合适的一个索引在表中查找记录行。 

可选的二选一句法 IGNORE INDEX (key_list) 可被用于告诉 MySQL 不使用特定的索引。  

效率问题:

1.inner join比left join快

注:inner join 内连接等价于下面的sql: SELECT A.name, B.address FROM A, B WHERE A.id = B.A_id

所以一般要用一般的连接就可以了.

2.连接字段建索引

多表外连接

select  A.*,B.f1,B.f2,B.fn,C.f1,C.f2,C.fn  from  A   

 left  join  B  on  A.id=B.id   

 left  join  C  on  C.id=A.id  

 where .......



# **1000.配置**及测试数据

## **1000.1.启动与停止**

sudo /usr/local/mysql/support-files/mysql.server start/stop

## **1000.2.修改初始密码**

禁用验证： ./mysqld_safe --skip-grant-tables &

输入命令： ./mysql

 FLUSH PRIVILEGES;

修改密码：alter user 'root'@'localhost' identified by '密码'

**测试数据：**

SET SQL_SAFE_UPDATES = 0;

sex enum('m','f')

create table student

(

​	Sno char(4) not null primary key,

​	Sname varchar(20) not null,

​	Ssex char(2) check(Ssex='男' or Ssex='女'),

​	Sage int check(Sage between 0 and 120),

​	Sdept varchar(20),

 	createTime timestamp default current_timestamp comment '创建时间',

  	updateTime timestamp default current_timestamp on update current_timestamp comment '更新时间'

);

create table course

(

​	Cno varchar(10) primary key,

​	Cname varchar(20) not null,

​	Ccredit int check(Sctedit > 0),-- 学分

​	Semester int check(Semester>0),-- 学期

​	Period int check(Period > 0) -- 学时

);

create table SC

(

​	Sno varchar(7) ,

​	Cno varchar(10),

​	Grade int check(Grade between 0 and 100),

​	foreign key (Sno) references student(Sno),

​	foreign key (Cno) references course(Cno),

​	primary key (Sno,Cno)

);

insert into student(Sno,Sname,Ssex,Sage,Sdept) values

('0001','令狐冲','男',15,'信息管理'),

('0002','刘备','男',26,'网络工程'),

('0003','小龙女','女',20,'会计'),

('0004','张飞','男',34,'信息管理'),

('0005','秋香','女',32,'计算机'),

('0006','关羽','男',56,'计算机'),

('0007','西施','女',18,'会计'),

('0008','子龙','男',22,'计算机'),

('0009','孔明','男',20,'信息管理');

insert into course(Cno,Cname,Ccredit,Semester,Period) values

('c01','计算机基础',3,1,36);

insert into course(Cno,Cname,Ccredit,Semester,Period) values('c02','高数',4,1,42);

insert into course(Cno,Cname,Ccredit,Semester,Period) values('c03','面向对象',3,1,36);

insert into course(Cno,Cname,Ccredit,Semester,Period) values('c04','可视化编程',3,1,24);

insert into course(Cno,Cname,Ccredit,Semester,Period) values('c05','会计',2,2,10);

insert into course(Cno,Cname,Ccredit,Semester,Period) values('c06','web应用开发',3,1,36);

insert into course(Cno,Cname,Ccredit,Semester,Period) values('c07','数据库结构',4,2,38);

insert into SC(Sno,Cno,Grade) values('0001','c01',62);

insert into SC(Sno,Cno,Grade) values('0001','c02',60);

insert into SC(Sno,Cno,Grade) values('0001','c03',59);

insert into SC(Sno,Cno,Grade) values('0001','c04',91);

insert into SC(Sno,Cno,Grade) values('0001','c05',86);

insert into SC(Sno,Cno,Grade) values('0002','c01',75);

insert into SC(Sno,Cno,Grade) values('0002','c02',32);

insert into SC(Sno,Cno,Grade) values('0002','c03',0);

insert into SC(Sno,Cno,Grade) values('0003','c01',88);

insert into SC(Sno,Cno,Grade) values('0003','c02',78);

insert into SC(Sno,Cno,Grade) values('0004','c03',68);



# 10000.遇见的问题

## **10000.1.navicate远程连接问题**

问题：Authentication plugin 'caching_sha2_password' cannot be loaded: dlopen(../Frameworks/caching_sha2_password.so, 2): image not found

解决：

1.修改帐户密码加密规则并更新用户密码

ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;  #修改加密规则 

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';  #更新一下用户的

2.刷新权限

flush privileges;

3.重置密码

alter user 'root'@'localhost' identified by 'adsafa';