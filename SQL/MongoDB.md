* MongoDB是一个基于分布式文件存储的数据库。由C++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。
* MongoDB是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。它支持的数据结构非常松散，是类似json的bson格式，因此可以存储比较复杂的数据类型。Mongo最大的特点是它支持的查询语言非常强大，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还支持对数据建立索引。


### mongod命令和mongo命令的区别
* mongod命令用于启动数据库服务，即搭建并开启服务器，可以通过端口被访问（27017）<br>
启动mongodb服务时需要先确定数据库文件存放的位置，否则系统不会自动创建，启动会不成功。
```bash
mongod -dbpath "D:\Applications\Environment\MongoDB\data"
```
* mongo命令是连接数据库服务，即连接服务器，可以通过端口进行访问（27017）<br>
在执行mongod命令启用MongoDB进程（服务器）的基础上，再使用mongo 对其进行连接操作。

### mongod数据库常用语法
```bash
// 如果数据库不存在，则创建数据库，否则切换到指定数据库。
use DATABASE_NAME

// 查看所有数据库
show dbs
```
* 示例如下：
```bash
> db
mtest
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
mtest   0.000GB
test    0.000GB
> use mtest
switched to db mtest
```