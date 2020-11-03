# **1.了解redus**

redis是一个开源的使用ANSI C语



# **2.常用命令**

编译测试:：sudo make test

编译安装： sudo make install

启动：redis-server  需要去usr/local/redis.../src

使用：redis-cli 或 redis-cli --raw(有时中文乱码)

远程使用：redis-cli -h host -p port -a password

查看运行情况：lsof -i tcp:6379

修改配置：config set name value



# **3.数据类型**

## **3.1.String(字符串)**

述：最基本的类型，可以包含任命数据，比如图片或者序列化的对象

max：512MB

set：set key value

get：get key



## **3.2.Hash(哈希)**

述：是一个string类型的映射表，适用于存储对象

max：2的32次方 -1 键值对（40多亿）

set： hmset myhash a "111" b "2222"

get：hget myhash a



## **3.3.List(列表)**

述：最简单的字符串列表，按照插入顺序排序，可添加元素到头和尾

max：2的32次方 -1 键值对（40多亿）

set：lpush [name] [value1]    lpush [name]  [value2]

get：lrange runoob 0 10



## **3.4.Set(集合)**

述：是string类型的无序集合，集合是通过哈希表实现的，rnnful删查的复杂度都是O(1)

注：如果是重复值会返回0，并且不会set进去，成功就返回1，错误就返回错误信息

max：2的32次方 -1 键值对（40多亿）

set：sadd key member

get：smembers key



## **3.5.zSet(有序集合)**

述：1.zset和set一样也是string类型元素的集合，且不允许重复的成员，

​       2.不同的是每个元素都会关联一个double类型的分数，redis下是通过分数来为集合中的成员进行从小到大的排序

​       3.zset的成员是唯一的，但分数却可以重复

set：zadd key score member

get：zrangebyscore member 0 1000

![](.\images\202010050003.png)



# **1000.遇见的错误**

## **1000.1.强制关闭redis快照导致不能持久化**

error：MISCONF Redis is configured to save RDB snapshots, but it is currently not able to persist on disk. Commands that may modify the data set are disabled, because this instance is configured to report errors during writes if RDB snapshotting fails (stop-writes-on-bgsave-error option). Please check the Redis logs for details about the RDB error.

解决：config set stop-writes-on-bgsave-error no



## **1000.2.关闭**

redis-cli -h 127.0.0.1 -p 6379 shutdown



## **1000.3**

Cannot get Jedis connection; nested exception is redis.clients.jedis.exceptions.JedisConnectionException: Could not get a resource from the pool

明明有设置密码，但还是出现cause error client send AUTH

解决方法：./src/redis-server redis.conf



## **1000.4.make test错误：**

error：You need tcl 8.5 or newer in order to run the Redis test

solve：

下载tcl ：http://downloads.sourceforge.net/tcl/tcl8.6.1-src.tar.gz 

配置：cd  tcl/unix/  -> ./configure   -> make -> make install