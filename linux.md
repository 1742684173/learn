查看系统是32还是64位：uname -i

直接下载：wget + 网站 (有网络的情况下)

查看ip：ifconfig

切换到root：su root

ctrc c 退出



## 9997.防火墙

关闭：systemctl stop firewalld 

状态：systemctl status firewalld



## **9998.java**

```
export JAVA_HOME=/usr/local/jdk1.8.0_241
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH

```

需要安装

yum install glibc.i686

## **9999.源码安装git**

/configure --prefix=/usr/local/git-2.16.1 && make && make install

配置bin目录

zlib安装

./configure --prefix=`pwd`/bintarget

make profix=/home/zxl/zlib

make install



## **10000.常用命令**

### 972、wget



### 972.yum安装

yum -y install lrzsz

yum install wget



![202011180001](.\images\202011180001.jpg)

修改/etc/sysconfig/network-scripts/ifcfg-**追加

DNS1=8.8.8.8

DNS2=4.2.2.2

ifup ifcfg-** 

systemctl restart network

### 973.下载和上传

yum -y install lrzsz

sz + 文件名  
rz  + 文件名



### 974.查询进程

如：ps -ef | grep java



### 975.tail、less、cat、more

#### 975.1.tail

tail -1000f  + 文件

#### 975.2.less

less查看文件内容 

/ 向下

？向上

ctrl+c 退出

shift+g 跳到最后

n 向上搜索

shift+n: 向下

### **976、useradd**

用于添加用户帐号或设置添加用户使用的默认的信息,语法 ：useradd [options][username]  

删除用户用userdel命令

1.1、 username是添加的新用户名名称

1.2、options是选项，可以为空

 -u  UID 指定新用户的 UID，默认为使用当前最大的UID 加 1。

 -g  GROUP 指定新用户 组。

 -G  GROUP1[,group2,...[,groupn]]指定新用户的附加组 。

 -d  HOME_DIR指定新用户的登录目。

 -s  SHELL指定新用户使的 Shell ，默认为 bash 。

 -m 在/home 创建用户 的目录 。



### **977、passwd**

用于设置用户的认证信息，包括用户密码、密码过期时间等。系统管理员则能用它管理系统用户的密码。只有系统管理员可以指定用户名称，一般用户只能更改自己的密码，语法

passwd [options] [username]

2.1、username是用户要修改的用户名，只有系统管理员可以指定用户名。

2.2、[options]是选项，可以为空，可是如下选项

-d 删除密码，仅有系统管理员才能使用

-f 强制执行

-k 设置只有在密码过期失效后，才能更新

-l 锁住密码

-s 列出密码的相关信息，仅有系统管理员才能使用

-u 解开已上锁的帐号



### **978、apt-get**

用于自动从互联网的软件仓库中搜索、安装、升级软件或操作系统，一般需要系统管理员权限，常用命令如下：

3.1、更新已安装的软件

sudo apt-get update

3.2、安装指定的软件

sudo apt-get install [software]

3.3、卸载指定的软件(保留配置文档)

sudo apt-get remove [software]

3.4、卸载指定的软件(不保留配置稳定)

sudo apt-get -purge remove [software]



###  **979、cd**

4.1、cd / 切换到当前用户的根目录

4.2、cd ~ 切换到当前用户的用户目录

4.3、cd 回到上一级



###  **980、ls**

列出目录下的文件，语法：ls [option] [directory]

5.1、directory 是指定的目录，列出目录下的文件

5.2、options是选项，可以为空，有以下的选项：

-a 各部门文件下所有的文件，包括 以"."开头的隐藏文件("."开头的是隐藏文件，".."代表存在着父目录)

-A 列出除"."和".."以外的所有文件

-l   列出文件的详细信息，如创建者、时间、文件的读写权限列表等

-R 将目录下所有的子目录的文件都列出来

-S 将文件的大小进行排序

-t  按时间进行文件的排序 



###  **981、mkdir**

用来创建指定名称的目录，要求用户在当前目录中具有写权限，并且指定的目录名不能是当前目录中已有的目录，语法： mkdir [option] [directory]

6.1、directroy 是要创建的目录，可以是绝对路径，也可以是相对路径

6.2、options是选项，可为空，有以下选项

-p 要创建的目录路径中如果有某一级目录 不存在，使用该选项将会创建不存在的上级目录

-v 在创建目录的过程中显示信息

 

### **982、touch**

用来创建一个文件，格式：touch [file path]



###  **983、mv**

用来移动文件或者更改文件名，经学用来备份文件或者目录

格式：mv [option] [source] [source file] [destination file]

8.1、source file是源文件或源目录

8.2、destination file 是目标文件名或目标目录。

8.3、options 是选项，可以为空，有以下可选项

-b 若需覆盖文件，则覆盖前先行备份

-f  强制覆盖，如果目标文件已经存在，不会询问而直接覆盖

-i  若目标文件已经存在时，会询问是否覆盖

-u 若目标文件已经存在，且源文件更新时，才会覆盖



###  **984、rm**

用于删除一个目录中的一个或多个文件或目录，也可以将某个目录及项下的所有文件及了目录均删除，格式 rm [option] [file]

补充说明：执行rm指令可删除文件或目录，如欲删除目录必须加上参数”-r”，否则预设仅会删除文件。 

9.1、option可选项参　　数：

　-d或–directory 直接把欲删除的目录的硬连接数据删成0，删除该目录。 

　-f或–force 强制删除文件或目录。 -f * 当前目录下所有文件

　-i或–interactive 删除既有文件或目录之前先询问用户。 

　-r或-R或–recursive 递归处理，将指定目录下的所有文件及子目录一并处理。 

　-v或–verbose 显示指令执行过程。 

   -rf 的时候一定要格外小心，linux没有回收站的



###  **985、cp**

复制文件或目录，格式：cp [option] [source file] [destination file]

10.1、source file是源文件， destination file是目的文件。source file可以是多个目录，在不带参数的情况下，destination file必须是已经存在的目录。

10.2、option可选项，可为空

-f 强行复制文件或目录，不论目的文件或目录是否已经存在。

-r 递归处理，将源目录下的文件与子目录一并复制

-i 覆盖文件之前先询问用户

-v 显示执行过程



###  **986、cat**

有三个功能：一个显示一个文件，二是创建一个文件，三是将多个文件合并为一个文件

格式：cat [option] [file]

11.1、option可选项，可为空

-A 显示所有内容 

-b 对输出的非空行进行编号

-n 对输出的所有行进行编号

-E 在每行以$结尾

注：

创建文件： cat >[file]

合并文件：cat [file1] [file2] ... >[file]  file是合并后的新文件

向指定文件中追加内容的命令格式如下：cat [file1] [filw2]..>>[file]  file是必须存在的



###  **987、ssh**

ssh即安全Shell(Secure Shell)，是由国际 互联网工程任务组IETF的Network Working Group所制定的一种建立在应用层和传输层基础上的安全协议。SSH传输的数据是经过加密和压缩的，可有效防止传输过程数据泄露，并加快速度。

ssh命令用于远程登录Linux主机，其格式如下：ssh [-l user] [-p port] [user$]hostname

其中：

[-l user] 表示用指定的用户名user登录

[user@]也表示用指定的用户user登录，user为指定的用户名

[-p port]表示用指定的端口port进行登录

hostname 为主机名，一般用IP地址表示



###  **988、dpkg**

用于操作套件管理系统Debian Packager，可以方便地进行软件的安装、更新及移除。

格式：dpkg [option] [software]

13.1、software 是软件包名

13.2、options 是可选项，可以为空，可选值

-l	查看当前系统中已经安装的软件包的信息

-L	查看系统中已经安装的软件文件的详细信息

-s	查看已经安装的指定软件包的详细信息

-S	查看系统中的某个文件属于那个软件包

-i	*.deb文件的安装

-r	*.deb文件的卸载

-p	彻底的卸载，包括软件的配置文件等



###  **989、|**

是管道命令,它的操作对象是两条命令,并将前一条命令的正确输出作为后一条命令的输入

格式 command1 | command2,  使用管道命令时需要注意两点

1.管道命令只处理前一个命令的正确输出,不处理错误输出

2.第二条命令必须能够接收标准输入流命令



###  **990、grep**

是文本搜索命令,其作用是在指定文件中所有匹配的文本输出,它支持正则表达式.,

格式为 grep [option] [file]

15.1、file是指定文件，可以是多个。

15.2、options是可选项，可以为空，可选值

-c 只输出匹配行的数量

-I 不区分大小写(只适用于单字符)

-h 查询多文件时不显示文件名

-I 查询多文件时只输出包含匹配字符的文件名

-n 显示匹配行及行号

-s 不显示不存在或无匹配文本的错误信息

-v 显示不包含匹配文本的所有行



###  **991、source**

可以让Shell命令讲稿指定的Shell程序文件，并依次执行文件中的所有语句。该命令通常用于重新执行刚修改的初始化文件，立即生效，而不必注销并重新登录。格式：source [file]



###  **992、echo**

作用是在显示器上显示一段文字，一般起到提示的作用，格式：echo [option] [content]

17.1、content 是要输出的内容，可以是字符串也可以是变量。如果 要输出变量，则需要在变量前面加$

17.2、option是可选项，可以为空，可选 值

-n 不要在最后 自动 换行

-help 显示帮助信息

-versuib 显示版本信息



### 993、chown/chrpg/chmod 

将指定文件的拥有者改为指定的用户或组，用户可以是用户名或者用户ID，组也可以是组名或者组ID，命令格式 ：chown/chrpg [option] [user/group] [file]

18.1、user/group是指定的用户或组

18.2、file是指定的文件

18.3、option是可选项，可以为空，可选值

-c	显示 更改的部分的信息

-f	忽略错误信息

-R	处理指定目录以及其子目录 下的所有文件

-v	显示详细的处理信息



chmod –R 777 * 



### **994、vi/vim的工作模式**

#### 1、分为三种模式

**一般模式**：以vi/vim打开 一个文件，就进入了一般模式(这是默认的工作模式)。在该模式下，可以使				用方向键来移动光标，也可以使用删除键和复制/粘贴来编辑文件内容，但是不能从键盘 进行输入

**编辑模式**：按下"i、I、o、O、a、A、r、R"中的任意一个字母后就进入 了编辑模式，些时在界面的左下通常会出现"INSERT"或“REPLACE”的字样。在该模式下，按"Esc"键，返回一般模式

**命令模式**：在一般模式下，输入 “： / ？”中的任意一个字符，就可进行命令模式，此时资材移动 到界面最底下，进入命令行，在该模式下，可以通过命令，进行读取文件、保存文件、批量替换、退出、显示行号等操作。



#### 2、常用命令

![./image/](.\images\202010050002.png)

如：gcc -o test test,c    ./test



### **995、tar解压**和压缩

tar -xzvf  文件名'

tar -zcvf test.tar.gz 路径



### 996、ll和权限

![./image/](.\images\202010050001.png)

第1列：第一个单词（文件类型【-为普通文件】【d为文件夹】）后面9位权限(rwx（当前用户）rwx（用户组）rwx(其他用户)，如果没有此权限的话用-代替)

 第2列：（数量）

第3列：文件所有者

第4列：文件所属用户组（root属于root组）

第5列：文件大小

chmod命令

1，详解：

  （相应用户） u(user) ：文件的所有者  g：文件的所属组用户   o(other)：其他用户  a(all)：所有用户

  （相应权限）r：文件的读权限（read）数字代号是4，w：文件的写权限（write）数字代号2，x：文件的执行权限数字代号是1，-：不具备任何权限，数字代号是0

 2，权限为rwxr-xr-x（hadoop用户具有读-r、写-w、执行-x权限（rwx）--------haddop组具有读-r、执行-x的权限(r-x) -------------其他用户具有读-r、执行-x的权限(r-x) )   

  增加权限：chmod o+w data  给其他用户增加data文件写的权限【其中参数o对应用户（比如要给组用户增加权限此处参数就是g），参数w对应权限(比如要增加读的权限此处参数就是r)】————注：加号代表增加权限

  删除权限：chmod o-w data  给其他用户删除data文件写的权限【其中参数o对应用户（比如要给组用户增加权限此处参数就是g），参数w对应权限(比如要增加读的权限此处参数就是r)】————注：减号号代表删除权限

  

 数字表示法：chmod  700 data  给data目录读-r、写-w、执行-x权限，组用户权限全部删除，其他用户的权限全部删除。——————注：7为r+w+x(4+2+1) ，比如要给data目录hadoop用户读、写、执行权限，组用户读、写权限，其他用户读的权限，只要执行命令chmod 764 data就OK了。



### 997、find、location、 whereis和 which的区别**

find 查找任何

location箅find一样，比find快，因为不会去查目录，直接查数据库

whereis程序名搜索

which查PATH配置的环境变量



### 998、ps、grep、history、netstat和 find的区别

ps显示进程

grep文本搜索

history查看历史命令

netstat查看端口

find查找文件



### **999.shell**

a="sh ./dex2jar-2.0/d2j-dex2jar.sh ./app/classes"

b=".dex"

for((i=1;i<=97;i++))

do 

​	result=${a}${i}${b}

​	eval $result

done

### 1000、搜索文字

选/ ,再输入字符串，回车。按N域n向前向后搜索文字