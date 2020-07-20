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

// 删除当前数据库。
db.dropDatabase()

// 查看当前数据库中的集合
show tables
show collections    // 相比更为准确

// 创建集合collections
db.createCollection("c")

// 删除a集合
db.a.drop()

// MongoDB 使用 insert() 或 save() 方法向集合中插入文档
// 如果COLLECTION_NAME集合不在该数据库中， MongoDB 会自动创建该集合并插入文档。
db.COLLECTION_NAME.insert(document)

// 查看已插入文档：
db.COLLECTION_NAME.find()

// MongoDB使用update()函数更新数据
// criteria : update的查询条件，类似sql update查询内where后面的。
// objNew : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的。结果会将objNew全部替换原有的文档
// upsert : 这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
// multi : mongodb默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
db.COLLECTION_NAME.update( criteria, objNew, upsert, multi )

// MongoDB remove()函数是用来移除集合中的数据。
// query :（可选）删除的文档的条件。
// justOne : （可选）如果设为 true 或 1，则只删除一个文档。
// writeConcern :（可选）抛出异常的级别。
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)

// find() 方法以非结构化的方式来显示所有文档。
db.COLLECTION_NAME.find()
// 如果你需要以易读的方式来读取数据，可以使用 pretty() 方法
db.COLLECTION_NAME.find().pretty()
// 除了 find() 方法之外，还有一个 findOne() 方法，它只返回一个文档。它不可以使用pretty()函数
db.COLLECTION_NAME.findOne()
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
>
> use mtest
switched to db mtest
>
> db.dropDatabase()
{ "dropped" : "mtest", "ok" : 1 }
>
> use t
switched to db t
>
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
test    0.000GB
>
> db.t.insert({"name":"wkk", "age":12})
WriteResult({ "nInserted" : 1 })
>
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
t       0.000GB
test    0.000GB
>
> db.stats()
{
        "db" : "t",
        "collections" : 1,
        "views" : 0,
        "objects" : 1,
        "avgObjSize" : 49,
        "dataSize" : 49,
        "storageSize" : 20480,
        "numExtents" : 0,
        "indexes" : 1,
        "indexSize" : 20480,
        "scaleFactor" : 1,
        "fsUsedSize" : 125843591168,
        "fsTotalSize" : 375674368000,
        "ok" : 1
}
>
> show collections
t
>
> db.createCollection("c")
{ "ok" : 1 }
>
>show collections
c
t
>
> db.t.drop()
true
>
> db.createCollection("used")
{ "ok" : 1 }
>
> db.used.insert({"name":"wukeke", "age":22})
WriteResult({ "nInserted" : 1 })
>
> db.used.find()
{ "_id" : ObjectId("5f100c38b699cbfca074571d"), "name" : "wukeke", "age" : 22 }
>
> db.used.find()
{ "_id" : ObjectId("5f100c38b699cbfca074571d"), "name" : "wukeke", "age" : 22 }
{ "_id" : ObjectId("5f101e14b699cbfca074571e"), "name" : "张三", "age" : 20 }
{ "_id" : ObjectId("5f101e39b699cbfca074571f"), "name" : "张三", "age" : 20 }
>
> db.used.update({"name":"张三"}, {"name":"李四"}))
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.used.find()
{ "_id" : ObjectId("5f100c38b699cbfca074571d"), "name" : "wukeke", "age" : 22 }
{ "_id" : ObjectId("5f101e14b699cbfca074571e"), "name" : "李四" }
{ "_id" : ObjectId("5f101e39b699cbfca074571f"), "name" : "张三", "age" : 20 }
>
```
* MongoDB 的操作

|操作       |格式                   |
|---        |---                    |
|等于       |{<key>:<value>}        |
|小于       |{<key>:{$lt:<value>}}  |
|小于或等于 |{<key>:{$lte:<value>}}	|
|大于       |{<key>:{$gt:<value>}}  |
|大于或等于 |{<key>:{$gte:<value>}}	|
|不等于     |{<key>:{$ne:<value>}}  |
* MongoDB AND 条件：MongoDB 的 find() 方法可以传入多个键(key)，每个键(key)以逗号隔开，及常规 SQL 的 AND 条件。
```
db.col.find({key1:value1, key2:value2}).pretty()
```
* MongoDB OR 条件: MongoDB OR 条件语句使用了关键字 $or
```
// 查询键 by 值为 w3cschool 或键 title 值为 MongoDB 教程 的文档。
db.col.find({$or:[{"by":"w3cschool"},{"title": "MongoDB 教程"}]}).pretty()
```
* MongoDB $type 操作符:MongoDB 中可以使用的类型如下表所示：

|类型	                        |数字       |备注       |
|----                           |----       |----       |
|Double	                        |1	        |           | 
|String	                        |2	        |           |
|Object	                        |3	        |           |
|Array	                        |4	        |           |
|Binary data                    |5	        |           |
|Undefined                  	|6	        |已废弃。    |
|Object id                  	|7	        |           |
|Boolean                    	|8	        |           |
|Date	                        |9	        |           |
|Null	                        |10	        |           |
|Regular Expression         	|11	        |           |
|JavaScript                 	|13	        |           |
|Symbol	                        |14	        |           |
|JavaScript (with scope)	    |15	        |           |
|32-bit integer	                |16	        |           |
|Timestamp	                    |17	        |           |
|64-bit integer             	|18	        |
|Min key	                    |255	    |Query with -1. |
|Max key	                    |127	    |           |
```bash
// 如果想获取 "col" 集合中 title 为 String 的数据，你可以使用以下命令：
db.col.find({"title" : {$type : 2}})
```
* MongoDB Limit() 方法：如果你需要在MongoDB中读取指定数量的数据记录，可以使用MongoDB的Limit方法，limit()方法接受一个数字参数，该参数指定从MongoDB中读取的记录条数。
```bash
db.COLLECTION_NAME.find().limit(NUMBER)
```
* MongoDB Skip() 方法：我们除了可以使用limit()方法来读取指定数量的数据外，还可以使用skip()方法来跳过指定数量的数据，skip方法同样接受一个数字参数作为跳过的记录条数。
```bash
>db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```
* MongoDB sort()方法：在MongoDB中使用使用sort()方法对数据进行排序，sort()方法可以通过KEY参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而-1是用于降序排列。
```bash
>db.COLLECTION_NAME.find().sort({KEY:1})
```
* MongoDB 索引:索引通常能够极大的提高查询的效率，如果没有索引，MongoDB在读取数据时必须扫描集合中的每个文件并选取那些符合查询条件的记录。
这种扫描全集合的查询效率是非常低的，特别在处理大量的数据时，查询可以要花费几十秒甚至几分钟，这对网站的性能是非常致命的。索引是特殊的数据结构，索引存储在一个易于遍历读取的数据集合中，索引是对数据库表中一列或多列的值进行排序的一种结构<br>
MongoDB使用 ensureIndex() 方法来创建索引。
```bash
// 语法中 Key 值为要创建的索引字段，1为指定按升序创建索引，如果想按降序来创建索引指定为-1即可。
db.COLLECTION_NAME.ensureIndex({KEY:1})

// ensureIndex() 方法中你也可以设置使用多个字段创建索引（关系型数据库中称作复合索引）。
db.mycol.ensureIndex({"title":1,"description":-1})

db.values.ensureIndex({open: 1, close: 1}, {background: true})
```
* ensureIndex()接收可选参数，可选参数列表如下：

|Parameter	    |Type	            |Description            |
|---            |---                |---                    |
|background	    |Boolean	        |建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 "background" 可选参数。 <br>"background" 默认值为false。|
|unique	        |Boolean	        |建立的索引是否唯一。指定为true创建唯一索引。默认值为false. |
|name	        |string	            |索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称。  |
|dropDups	    |Boolean	        |在建立唯一索引时是否删除重复记录,指定 true 创建唯一索引。默认值为 false.           |
|sparse	        |Boolean	        |对文档中不存在的字段数据不启用索引；这个参数需要特别注意，如果设置为true的话，在索引字段中不会查询出不包含对应字段的文档.。默认值为 false. |
|expireAfterSeconds	|integer	    |指定一个以秒为单位的数值，完成 TTL设定，设定集合的生存时间。       |
|v	            |index<br>version	|索引的版本号。默认的索引版本取决于mongod创建索引时运行的版本。     |
|weights	    |document	        |索引权重值，数值在 1 到 99,999 之间，表示该索引相对于其他索引字段的得分权重。      |
|default_language	|string	        |对于文本索引，该参数决定了停用词及词干和词器的规则的列表。 默认为英语              |
|language_override	|string	        |对于文本索引，该参数指定了包含在文档中的字段名，语言覆盖默认的language，默认值为 language.|

* MongoDB 聚合：MongoDB中聚合(aggregate)主要用于处理数据(诸如统计平均值,求和等)，并返回计算后的数据结果。有点类似sql语句中的 count(*)。<br>
$group 进行分布查询操作。这个有点类似于我们在 SQL 中的 group by 语法，但是这个可以操作的内容多一些。<br>
aggregate() 方法的基本语法格式如下所示：
```bash
// https://blog.csdn.net/qq_18948359/article/details/88777066
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)
```

// 出现的问题：
1，http://localhost:28017 来访问MongoDB的web用户界面。不成功
2，使用ensureIndex建立索引：do.used.ensureIndex({"name":1})不成功

