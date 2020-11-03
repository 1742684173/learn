# **1、常用语法**

## **1.1、进入记事本编写**

输入 ED A 回车会弹出一个记事本框，在里面进行输入sql语法保存关掉，最后输入@ A(注意两个字符没有大小与之分，但中间有空格)

## **1.1、显示系统日期**

select sysdate from dual;

## **1.2、输入出字符串**

set serveroutput on  --首先要输入这一串，输入一次，后面就不用再输入也行

begin

dbms_output.put_line('hello wolrd');

end;

/   --结束符号



# **2、创建用户**

## **2.1、在sys和system的区别**

sys:拥有dba、sysdba、sysoper（系统操作员 ）角色或权限，是Oracle权限最高的用户，只能以sysdba或sysoper登录，不能以normal形式登录。

System:拥有dba、sysdba权限或角色，可以以普通用户的身份登录。

•【sysdba、sysoper、DBA区别】

Sysdba用户: 可以改变字符集、创建删除数据库、登录之后用户是SYS（shutdown、startup）

Sysoper:用户不可改变字符集、不能创、删数据库、登陆之后用户是PUBLIC （shutdown、startup）

DBA用户：只有在启动数据库后才能执行各种管理工作。

## **2.2、创建/更改用户**

create user 用户名 identified by 密码; 注意：没有引号

alter user 用户名 identify by 新密码

## **2.3、授权角色**

oracle为兼容以前版本，提供三种标准角色（role）:connect/resource和dba.

1》. connect role(连接角色)

--临时用户，特指不需要建表的用户，通常只赋予他们connect role. 

--connect是使用oracle简单权限，这种权限只对其他用户的表有访问权限，包括select/insert/update和delete等。

--拥有connect role 的用户还能够创建表、视图、序列（sequence）、簇（cluster）、同义词(synonym)、回话（session）和其他 数据的链（link）

2》. resource role(资源角色)

--更可靠和正式的数据库用户可以授予resource role。

--resource提供给用户另外的权限以创建他们自己的表、序列、过程(procedure)、触发器(trigger)、索引(index)和簇(cluster)。

3》. dba role(数据库管理员角色)

--dba role拥有所有的系统权限

--包括无限制的空间限额和给其他用户授予各种权限的能力。system由dba用户拥有

授权命令：grant connect ,resource to 用户名;

撤消权限：revoke connect,resource from 用户名；

# **3、SQL语言的组成与功能**

## **3.1、数据定义语言(DDL-Data Definition Language)**

DDL用于定义和修改数据模式(如基本表)；定义外模式(如视图)和内模式(如索引)；主要语句如下：

create/drop/alter table 

create/drop view

create/drop index

创建表语法：CRETE TABLE table_name(column_nmae typ [CONSTRAINT constraint_def][DEFAULT default_exp],...);

CONSTRAINT约束条件   constraint_def可以是primary key主键

DEFAULT默认值

例：

```
alter table tb_name add colName colType;

COMMENT ON COLUMN CAR_INSTALLMENT_INFO.total_price IS '装修总价';

只删除一列
alter table RENOVATION_INSTALLMENT_INFO drop column product_name
删除多列
alter table RENOVATION_INSTALLMENT_INFO drop (product_name,...)
```



## **3.2、数据查询语言(SELECT-Query Statements)**

SELECT用于按指定的条件从表或视图中检索数据行

**3.2.0、**count(*)统计行的总数，count(column)列不为空的行数，count(distinct column)

**3.2.1、**TO_CHAR(date,'格式 如 YYYY-MM-DD')转换成日期格式

**3.2.2、**DISTINCT 避免重复

**3.2.3、**AS 设计别名 注意：别名必需要使用双引号 where后面的条件要用单引号

**3.2.4、**||  连接符号

**3.2.5、**函数NVL(ex1,ex2)，如果ex1为空时，返回时ex2的值，否则返回ex1的值，注意ex1和ex2的类型一致

**3.2.6、**函数NVL2(ex1,ex2,ex3),如果ex1为空，返回ex3的值，否则返回ex2的值，oracle 9i开始引入的，几个参数类型必需一致

**3.2.7、**函数COALESCE(ex1,ex2,ex3),返回参数列表中的第一个非空值，如果都是空值，则返回一个空值

**3.2.8、**通配符：%(百分号)表示0个或多个字符，_(下划线)表示单个字符

**3.2.9、**is null和is not null用来查询是否空的，不能使用=null来进行查询

**3.2.10、**not主要与between...and、like、in、is null混合使用

**3.2.11、**逻辑运算符AND、OR、NOT中，NOT优先级最高，AND其次，OR最低。并且他们的优先级低于任何一种比较操作符。如果改变优先级，需要使用括号

**3.2.12、**order by用来排序，select * from table order by 列1，列2，列3；asc指定按升序排序(默认),desc按降序排序

【select 表达式 from 表名 [where 语句] [order by 表达式 [ asc | desc ]]】

**3.2.13、**分组查询group by格式

【select  表达式 from 表名 [where 语句] [group by 语句] [having 条件]】

avg(column)平均值 stddev(column)列的标准偏差  variance(column)列的方差

**3.2.14、**多行子查询。当在where子句中使用多行子查询时，必需要使用多选比较符IN，ANY或ALL.

IN：匹配于子查询结果的任意一个值

ANY：只要符合子查询的任意一个值

ALL：必需要符合子查询结果的所有值

**3.2.15、**相关子查询：exists、not exists 用来记录行是否存在 、in、not in

**3.2.16、**过程名和函数

select object_name,status from user_objects where object_type=’PROCEDURE’;‘function’

select text from all_source where owner=user and name=upper(‘&plsql_name’);//查询源码

**3.2.17、**查询前几千

select * from 表名 where rownum=行数

**3.3、数据操纵语言(DML-Date Manipulation Language)**

DML用于对数据库中的内容进行更新，从表中完成插入、修改和删除数据行的数据操作功能

insert  update delete

**3.4、事务控制语言(TC-Transaction Control Statements)**

oracle处理DML语句的结果时，以事务为单位进行，一个事务为一个工作的逻辑单位，是一组SQL语句序列。在执行每一条DML语句时，所有的操作都在内存中完成 ，所以在执行完DML语句后都应该执行事务控制语句，以决定是否将内存中的数据 永久地保留到外部数据库中。建议用户在应用程序中用事务控制语句COMMIT或ROLLBACK显示地结束第一事务。

COMMIT 把当前事务所做的更改写入外存数据库，该语句的作用是结束当前事务，使当前的事务所执行的全部修改永久化，同时该命令还删除事务所设置的全部保留点，释放该事务的封锁。

ROLLBACK撤消上次使用COMMIT语句以来的当前事务中所做的全部或部分更改。同时命令还删除事务所设置的保留点，释放该事务的封锁。

**3.5、数据控制语言(DCL-Date Control Language)**

数据控制语言用于规定数据库用户的各种权限。

GRANT语句将权限或角色授予用户或其它角色，用于指定操作权限

REVOKE语句从用户或数据库角色收回权限

# **4、PL/SQL程序语言的组成与功能**

PL/SQL程序由块结构构成，在PL/SQL程序中含有变量、各种不同的的程序控制结构、异常处理模块、子程序(过程、函数、包)、触发器等

## **4.1、存储过程**

创建：create or replace procedure 过程名(参数) as 声明变量 begin .... end;

调用：exec 过程名

参数的类型：in(输入参数，默认为in，宝参数可以有默认值)，out(输出参数)，in out(输入输出参数)，注：不能宝参数的长度

## **4.2、游标使用**

显示游标分为：普通游标、参数化游标和游标变量



# 999、初次使用

## 999.1、进入命令行

cmd -> sqlplus /nolog -> conn /as sysdba ->alter user system identified by root

在本地hosts文件中加入了：127.0.0.1 localhost

https://localhost:1158/em



帐号： system
密码：root

## 999.2、配置sqlplus连接

查看当前的sid: select instance_name from v$instance

```
APPORCL =
  (DESCRIPTION = 
    (ADDRESS_LIST = 
      (ADDRESS = (PROTOCOL = TCP)(HOST = [ip])(PORT = 1521))
    )
    (CONNECT_DATA = 
      (SERVICE_NAME = [sid])
    )
  )
```

APPORCL : 连接名，连接数据库的别名

SERVICE_NAME ：数据库名



## 999.3、创建库

```
cmd -> sqlplus /nolog -> conn /as sysdba
创建库并扩展大小自动管理：create tablespace [库名] datafile 'E:\workspace\db\oracel\education\data.dbf' size 100m extent management local autoallocate;
删除表空间：drop tablespace [库名] including contents and datafiles;

创建用户：create user [用户] identified by [密码]  例： create user education identified by education 
查询用户：select username from dba_users where 条件; 例：select username from dba_users where username= 'EDUCATION';
删除用户：drop user [用户名] 例：drop user education 
删除用户及其相关：drop user [用户名] cascade
```

## 999.4、授权

```
conn sys/as sysdba
授权用户连接和开发权限：grant connect,resource to [用户]
登录测试：conn [用户]/[密码]
创建角色：create role [角色名] identified by [密码]; 例：create role admin identified by admin;
授于所有权限：grant all on [表] to [权限];
授权角色：grant create view,create table to  [角色名]
表权限：grant [权限] on [表] to [角色]  例：grant select, insert, delete, update on user to admin;
查询角色：select * from role_sys_privs where role=[角色名];  例： select * from role_sys_privs where role='ADMIN';
将角色授权给用户：grant [角色名] to [用户]
权限收回：revoke [权限] on [表] from [角色] 例：revoke select on emp from admin;
```



# 1000、遇见的问题

## **1000.1.ora-00911: 无效字符 oracle**

解决方法：LiveBos\FormBuilder\WEB-INF\classes下的system.properties里面的DatabaseType=ORACLE





## 1000.2.ORA-01109: 数据库未打开**

由于贪图方便，直接把创建的表空间用360强行删掉，后面再创建表空间，就遇到ORA-01109: 数据库未打开

解决方法：

第一步：把刚才删除的表空间文件drop掉

ALTER DATABASE DATAFILE 'D:\Oracle\oradata\orcl\nc63_data01.dbf' OFFLINE DROP;

ALTER DATABASE DATAFILE 'D:\Oracle\oradata\orcl\nc63_data01.dbf' OFFLINE DROP;

第二步： 打开数据库

ALTER DATABASE OPEN;

第三步： 删除表空间

DROP TABLESPACE NC63_DATA01 INCLUDING CONTENTS;

DROP TABLESPACE NC63_INDEX01 INCLUDING CONTENTS;



## 1000.3.报oracle数据库的字符集问题

解决：

1.在电脑系统变量中新增名为NLS_LANG ，值为american_america.AL32UTF8



## 1000.4.数据误删除delete

```
insert into set_sy_group_tb 
select * from set_sy_group_tb as of timestamp to_timestamp('2020-10-21 11:30:00','yyyy-mm-dd hh24:mi:ss') 
where group_name='信用卡分期风险调查岗'
```



## 1000.5.表误删drop

```
恢复删除的表数据
select original_name,dropscn from recyclebin where original_name='test'
flashback table test to before drop
```





# **1001.创建储存过程例子**

## **1000.1、基本使用**

如果有错误用show error来显示错误

**示例1：简单输出**

CREATE OR REPLACE PROCEDURE PBB_SJKQDK_SJHQ

AS

v_total number(5);

BEGIN

  select count(*) into v_total from cs;

  dbms_output.put_line('total:'||v_total);

END;

**示例2：输入输出参数**

CREATE OR REPLACE PROCEDURE PBB_SJKQDK_SJHQ(

​       id number,

​       yg varchar2 default '默认值',

​       result out varchar2

)

AS

v_total number(16);

v_result varchar2(20) default 'total:';

BEGIN

 	insert into cs(id,yg) values(id,yg);

 select count(*) into v_total from cs;

 	result :=v_result||v_total;

END;

var result varchar2(20);

exec PBB_SJKQDK_SJHQ(3,'logo',:result );

注意：当第一个输入参数有默认值，第二个没有，调用方法可以exex 过程名(第二个参数名=>值)

**示例3：存储过程内部块**

create or replace procedure PBB_SJKQDK_SJHQ

as

  result1 varchar2(20);

begin

  result1 := '一层';

  dbms_output.put_line(result1);

  declare

​    result2 varchar2(20) default 'begin';

  begin

​     result2 := '二层'+1;//这里是错误的写法，主要是想测异常

​     dbms_output.put_line(result2);

​     exception

​       when others then dbms_output.put_line('error');

  end;

end;

**示例4：存储过程输出对象集合**

create or replace procedure test(

 o_cur out sys_refcursor

)as

begin

  open o_cur for 'select id,yg from cs';

end;

**示例3：存储过程输出对象**

declare

refc sys_refcursor;

v_id number(20);

v_yg varchar2(30);

begin

  test(o_cur=>refc);

  loop

​    fetch refc into v_id,v_yg;

​          dbms_output.put_line(v_id||' '||v_yg);

​    exit when refc%notfound;

  end loop;

end;

**1000.2、游标使用**

**示例1：常用的几种游标**

create or replace procedure PBB_SJKQDK_SJHQ(

 p number

)as

 v_rownum number(10) := p;//将输入的参数赋给v_rownum

 cursor c1 is select yg from cs where rownum=1;//直接把数据定死

 cursor c2 is select yg from cs where rownum=v_rownum;//动态的查询

 cursor c3(p_rownum number) is select yg from cs where rownum=p_rownum;//动态查询，可读性没有上面的好

 type cursor_type is ref cursor;//定义一个游标类型

 c4 cursor_type;//根据创建的游标类型创建游标4

 v_yg varchar2(20);

begin

  open c1;

  fetch c1 into v_yg;

​    dbms_output.put_line('c1:'||v_yg);

  close c1;

  

  open c2;

  fetch c2 into v_yg;

​    dbms_output.put_line('c2:'||v_yg);

  close c2;

  

  open c3(1);

  fetch c3 into v_yg;

​    dbms_output.put_line('c3:'||v_yg);

  close c3;

  

  open c4 for select yg from cs where rownum=1;

  fetch c4 into v_yg;

​    dbms_output.put_line('c4:'||v_yg);

  close c4;

  

end;

**示例2：游标的几种循环**

create or replace procedure PBB_SJKQDK_SJHQ(

 in_rownum number

)as

 cursor c1 is select id,yg from cs where rownum<in_rownum;

 v_id number(16);

 v_yg varchar(30);

begin

  

 --方法1

 dbms_output.put_line('--方法1');

 open c1;

​    loop

​        fetch c1 into v_id,v_yg;

​        exit when c1%notfound;--一定要跟在fetch之后

​        dbms_output.put_line(v_id||' '||v_yg);

​    end loop;

 close c1;

 

 --方法2

 dbms_output.put_line('--方法2');

 open c1;

​    fetch c1 into v_id,v_yg;

​    while c1%found loop

​      dbms_output.put_line(v_id||' '||v_yg);

​    fetch c1 into v_id,v_yg;

​    end loop;

 close c1;

 

 --方法3

 dbms_output.put_line('--方法3');

 for v_pos in c1 loop

   v_id := v_pos.id;

   v_yg := v_pos.yg;

   dbms_output.put_line(v_id||' '||v_yg);

   end loop;

end;

**1000.3、异常**

create or replace proceduer procexception2(

p varchar2

) as

v_postype varchar2(20);

begin

  begin

​    select pos_type into v_postype from pos_type_tb1 where rownum < 5;

  exception

​    when no_data_found then

​      v_postype := null;

​    when too_many_rows then

​      raise_application_error(-20000,'对v_postype赋值是,找到多条数据');

  end;

  dbms_output.put_line(v_postype);

end;