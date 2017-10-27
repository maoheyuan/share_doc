# MengoDB学习总结


### Redis的简介

Redis是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。<br>
它通常被称为数据结构服务器，因为值（value）可以是 字符串(String), 哈希(Map), 列表(list), 集合(sets) 和 有序集合(sorted sets)等类型。<br>

### Redis的常用命令


mongodb://localhost<br>

使用用户 admin 使用密码 123456 连接到本地的 MongoDB 服务上<br>
mongodb://admin:123456@localhost/<br>


使用用户名user，密码password登录localhost的users数据库。<br>
mongodb://user:password@localhost/users<br>

MongoDB 创建数据库的语法格式<br>
use DATABASE_NAME<br>


以下实例我们创建了数据库 users:<br>
> use users<br>
switched to db users<br>
> db<br>
users<br>
><br>
如果你想查看所有数据库，可以使用 show dbs 命令：<br>
> show dbs<br>
local  0.078GB<br>
test   0.078GB<br>
><br>

MongoDB 删除数据库的语法格式如下：<br>

db.dropDatabase()<br>




以下实例我们删除了数据库 users。<br>
首先，查看所有数据库：<br>
> show dbs<br>
local   0.078GB<br>
users  0.078GB<br>
test    0.078GB<br>
接下来我们切换到数据库 users：<br>
> use users<br>
switched to db runoob<br>
>

执行删除命令：<br>

> db.dropDatabase()<br>
{ "dropped" : "users", "ok" : 1 }<br>


最后，我们再通过 show dbs 命令数据库是否删除成功：<br>


> show dbs<br>
local  0.078GB<br>
test   0.078GB<br>
>



集合删除语法格式如下：<br>

db.collection.drop()<br>



以下实例删除了 users 数据库中的集合 member：<br>
> use users<br>
switched to db users<br>
> show tables<br>
member<br>
> db.member.drop()<br>
true<br>
> show tables<br>
>


MongoDB 使用 insert() 或 save() 方法向集合中插入文档 如下：<br>
db.COLLECTION_NAME.insert(document)<br>



实例<br>

以下文档可以存储在 MongoDB 的 test 数据库 的 table 集合中：<br>

>db.table.insert({title: 'MongoDB 教程',<br>
    description: 'MongoDB 是一个 Nosql 数据库',<br>
    by: '教程',<br>
    tags: ['mongodb', 'database', 'NoSQL'],<br>
    likes: 100<br>
})
以上实例中 table 是我们的集合名，如果该集合不在该数据库中， MongoDB 会自动创建该集合并插入文档。<br>
查看已插入文档：<br>
> db.table.find()<br>

{ "_id" : ObjectId("56064886ade2f21f36b03134"),<br>
 "title" : "MongoDB 教程",<br>
 "description" : "MongoDB 是一个 Nosql 数据库",<br>
  "by" : "教程",<br>
  "tags" : [ "mongodb", "database", "NoSQL" ],<br>
  "likes" : 100<br>
}<br>
><br>

我们也可以将数据定义为一个变量，如下所示：<br>

> document=({title: 'MongoDB 教程',<br>
    description: 'MongoDB 是一个 Nosql 数据库',<br>
    by: '教程',<br>
    tags: ['mongodb', 'database', 'NoSQL'],<br>
    likes: 100<br>
});<br>
>docoument
执行后显示结果如下：<br>
{<br>
        "title" : "MongoDB 教程",<br>
        "description" : "MongoDB 是一个 Nosql 数据库",<br>
        "by" : "教程",<br>
        "tags" : [<br>
                "mongodb",<br>
                "database",<br>
                "NoSQL"<br>
        ],<br>
        "likes" : 100<br>
}<br>
<br>
执行插入操作：<br>
> db.test.insert(document)<br>
WriteResult({ "nInserted" : 1 })<br>
><br>

插入文档你也可以使用 db.test.save(document) 命令。如果不指定 _id 字段 save() 方法类似于 insert() 方法。如果指定 _id 字段，则会更新该 _id 的数据。<br>







MongoDB 更新文档<br>
MongoDB 使用 update() 和 save() 方法来更新集合中的文档。接下来让我们详细来看下两个函数的应用及其区别。<br>
update() 方法<br>
update() 方法用于更新已存在的文档。语法格式如下：<br>

db.collection.update(<br>
   <query>,<br>
   <update>,<br>
   {<br>
     upsert: <boolean>,<br>
     multi: <boolean>,<br>
     writeConcern: <document><br>
   }<br>
)<br>
参数说明：<br>
query : update的查询条件，类似sql update查询内where后面的。<br>
update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的<br>
upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。<br>
multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。<br>
writeConcern :可选，抛出异常的级别。<br>


实例<br>
我们在集合 col 中插入如下数据：<br>

>db.test.insert({<br>
    title: 'MongoDB 教程',<br>
    description: 'MongoDB 是一个 Nosql 数据库',<br>
    by: '教程',<br>
    tags: ['mongodb', 'database', 'NoSQL'],<br>
    likes: 100<br>
})<br>
接着我们通过 update() 方法来更新标题(title):<br>

>db.test.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})<br>

WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })   # 输出信息<br>

> db.test.find().pretty()<br>
{<br>
        "_id" : ObjectId("56064f89ade2f21f36b03136"),<br>
        "title" : "MongoDB",<br>
        "description" : "MongoDB 是一个 Nosql 数据库",<br>
        "by" : "教程",<br>
        "tags" : [<br>
                "mongodb",<br>
                "database",<br>
                "NoSQL"<br>
        ],<br>
        "likes" : 100<br>
}<br>
><br>
可以看到标题(title)由原来的 "MongoDB 教程" 更新为了 "MongoDB"。<br>
以上语句只会修改第一条发现的文档，如果你要修改多条相同的文档，则需要设置 multi 参数为 true。<br>


>db.test.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})<br>
save() 方法<br>
save() 方法通过传入的文档来替换已有文档。语法格式如下：<br>


db.collection.save(<br>
   <document>,<br>
   {<br>
     writeConcern: <document><br>
   }<br>
)<br>
参数说明：<br>
document : 文档数据。<br>
writeConcern :可选，抛出异常的级别。<br>
实例<br>
以下实例中我们替换了 _id 为 56064f89ade2f21f36b03136 的文档数据：<br>
>db.test.save({<br>
    "_id" : ObjectId("56064f89ade2f21f36b03136"),<br>
    "title" : "MongoDB",<br>
    "description" : "MongoDB 是一个 Nosql 数据库",<br>
    "by" : "gggg",<br>
    "tags" : [<br>
            "mongodb",<br>
            "NoSQL"<br>
    ],<br>
    "likes" : 110<br>
})
替换成功后，我们可以通过 find() 命令来查看替换后的数据<br>
>db.test.find().pretty()<br>

{
        "_id" : ObjectId("56064f89ade2f21f36b03136"),<br>
        "title" : "MongoDB",<br>
        "description" : "MongoDB 是一个 Nosql 数据库",<br>
        "by" : "Runoob",<br>
        "url" : "http://www.runoob.com",<br>
        "tags" : [<br>
                "mongodb",<br>
                "NoSQL"<br>
        ],<br>
        "likes" : 110<br>
}<br>
><br>


更多实例<br>
只更新第一条记录：<br>
db.test.update( { "count" : { $gt : 1 } } , { $set : { "test2" : "OK"} } );<br>
全部更新：<br>
db.test.update( { "count" : { $gt : 3 } } , { $set : { "test2" : "OK"} },false,true );<br>
只添加第一条：<br>
db.test.update( { "count" : { $gt : 4 } } , { $set : { "test5" : "OK"} },true,false );<br>
全部添加加进去:<br>
db.test.update( { "count" : { $gt : 5 } } , { $set : { "test5" : "OK"} },true,true );<br>
全部更新：<br>
db.test.update( { "count" : { $gt : 15 } } , { $inc : { "count" : 1} },false,true );<br>
只更新第一条记录：<br>
db.test.update( { "count" : { $gt : 10 } } , { $inc : { "count" : 1} },false,false );<br>



在3.2版本开始，MongoDB提供以下更新集合文档的方法：<br>
db.collection.updateOne() 向指定集合更新单个文档<br>
db.collection.updateMany() 向指定集合更新多个文档<br>
首先我们在test集合里插入测试数据<br>
use test<br>
db.test.insert( [<br>
{"name":"abc","age":"25","status":"zxc"},<br>
{"name":"dec","age":"19","status":"qwe"},<br>
{"name":"asd","age":"30","status":"nmn"},<br>
] )<br>
更新单个文档<br>
> db.test.updateOne({"name":"abc"},{$set:{"age":"28"}})<br>
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }<br>
> db.test_collection.find()<br>
{ "_id" : ObjectId("59c8ba673b92ae498a5716af"), "name" : "abc", "age" : "28", "status" : "zxc" }<br>
{ "_id" : ObjectId("59c8ba673b92ae498a5716b0"), "name" : "dec", "age" : "19", "status" : "qwe" }<br>
{ "_id" : ObjectId("59c8ba673b92ae498a5716b1"), "name" : "asd", "age" : "30", "status" : "nmn" }<br>
><br>
更新多个文档<br>
> db.test.updateMany({"age":{$gt:"10"}},{$set:{"status":"xyz"}})<br>
{ "acknowledged" : true, "matchedCount" : 3, "modifiedCount" : 3 }<br>
> db.test_collection.find()<br>
{ "_id" : ObjectId("59c8ba673b92ae498a5716af"), "name" : "abc", "age" : "28", "status" : "xyz" }<br>
{ "_id" : ObjectId("59c8ba673b92ae498a5716b0"), "name" : "dec", "age" : "19", "status" : "xyz" }<br>
{ "_id" : ObjectId("59c8ba673b92ae498a5716b1"), "name" : "asd", "age" : "30", "status" : "xyz" }<br>
><br>





MongoDB 删除文档<br>

MongoDB remove()函数是用来移除集合中的数据。<br>

MongoDB数据更新可以使用update()函数。在执行remove()函数前先执行find()命令来判断执行的条件是否正确。<br>
语法<br>
remove()<br>
db.collection.remove(<br>
   <query>,<br>
   <justOne><br>
)<br>

如果你的 MongoDB 是 2.6 版本以后的，语法格式如下：<br>
db.collection.remove(<br>
   <query>,<br>
   {<br>
     justOne: <boolean>,<br>
     writeConcern: <document><br>
   }<br>
)<br>

参数说明：<br>
query :（可选）删除的文档的条件。<br>
justOne : （可选）如果设为 true 或 1，则只删除一个文档。<br>
writeConcern :（可选）抛出异常的级别。<br>
实例<br>
以下文档我们执行两次插入操作：<br>
>db.test.insert({title: 'MongoDB 教程',<br>
    description: 'MongoDB 是一个 Nosql 数据库',<br>
    by: '教程',<br>
    tags: ['mongodb', 'database', 'NoSQL'],<br>
    likes: 100<br>
})<br>
使用 find() 函数查询数据：<br>
<br>
> db.test.find()<br>
{ "_id" : ObjectId("56066169ade2f21f36b03137"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程",  "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }<br>
{ "_id" : ObjectId("5606616dade2f21f36b03138"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程",  "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }<br>
<br>
接下来我们移除 title 为 'MongoDB 教程' 的文档：<br>

>db.test.remove({'title':'MongoDB 教程'})<br>

WriteResult({ "nRemoved" : 2 })           # 删除了两条数据<br>
>db.test.find()<br>
……                                        # 没有数据<br>

如果你只想删除第一条找到的记录可以设置 justOne 为 1，如下所示：<br>

>db.COLLECTION_NAME.remove(DELETION_CRITERIA,1)<br>
如果你想删除所有数据，可以使用以下方式（类似常规 SQL 的 truncate 命令）：<br>

>db.test.remove({})<br>
>db.test.find()<br>
><br>



emove() 方法已经过时了，现在官方推荐使用 deleteOne() 和 deleteMany() 方法。<br>
如删除集合下全部文档：<br>
db.inventory.deleteMany({})<br>
删除 status 等于 A 的全部文档：<br>
db.inventory.deleteMany({ status : "A" })<br>
删除 status 等于 D 的一个文档：<br>
db.inventory.deleteOne( { status: "D" } )<br>





MongoDB 查询文档<br>
<br>
find() 方法以非结构化的方式来显示所有文档。<br>
语法<br>
MongoDB 查询数据的语法格式如下：<br>
db.collection.find(query, projection)<br>
query ：可选，使用查询操作符指定查询条件<br>
projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。<br>
如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：<br>
>db.test.find().pretty()<br>

pretty() 方法以格式化的方式来显示所有文档。<br>
实例<br>

以下实例我们查询了集合 col 中的数据：<br>
> db.test.find().pretty()<br>
{<br>
        "_id" : ObjectId("56063f17ade2f21f36b03133"),<br>
        "title" : "MongoDB 教程",<br>
        "description" : "MongoDB 是一个 Nosql 数据库",<br>
        "by" : "教程",<br>
        "tags" : [<br>
                "mongodb",<br>
                "database",<br>
                "NoSQL"<br>
        ],<br>
        "likes" : 100<br>
}<br>
除了 find() 方法之外，还有一个 findOne() 方法，它只返回一个文档。<br>




MongoDB AND 条件<br>
MongoDB 的 find() 方法可以传入多个键(key)，每个键(key)以逗号隔开，及常规 SQL 的 AND 条件。<br>
语法格式如下：<br>
>db.test.find({key1:value1, key2:value2}).pretty()<br>
<br>
实例<br>
以下实例通过 by 和 title 键来查询 教程 中 MongoDB 教程 的数据<br>
<br>
> db.test.find({"by":"教程", "title":"MongoDB 教程"}).pretty()<br>
{<br>
        "_id" : ObjectId("56063f17ade2f21f36b03133"),<br>
        "title" : "MongoDB 教程",<br>
        "description" : "MongoDB 是一个 Nosql 数据库",<br>
        "by" : "教程",<br>
        "tags" : [<br>
                "mongodb",<br>
                "database",<br>
                "NoSQL"<br>
        ],<br>
        "likes" : 100<br>
}<br>
<br>
以上实例中类似于 WHERE 语句：WHERE by='教程' AND title='MongoDB 教程'<br>
MongoDB OR 条件<br>
MongoDB OR 条件语句使用了关键字 $or,语法格式如下：<br>
>db.test.find(<br>
   {<br>
      $or: [<br>
         {key1: value1}, {key2:value2}<br>
      ]<br>
   }<br>
).pretty()<br>
实例<br>
以下实例中，我们演示了查询键 by 值为 教程 或键 title 值为 MongoDB 教程 的文档。<br>
<br>
>db.col.find({$or:[{"by":"教程"},{"title": "MongoDB 教程"}]}).pretty()<br>
{<br>
        "_id" : ObjectId("56063f17ade2f21f36b03133"),<br>
        "title" : "MongoDB 教程",<br>
        "description" : "MongoDB 是一个 Nosql 数据库",<br>
        "by" : "教程",<br>
        "url" : "http://www.runoob.com",<br>
        "tags" : [<br>
                "mongodb",<br>
                "database",<br>
                "NoSQL"<br>
        ],<br>
        "likes" : 100<br>
}<br>
><br>


AND 和 OR 联合使用<br>
以下实例演示了 AND 和 OR 联合使用，类似常规 SQL 语句为： 'where likes>50 AND (by = '教程' OR title = 'MongoDB 教程')'<br>
>db.col.find({"likes": {$gt:50}, $or: [{"by": "教程"},{"title": "MongoDB 教程"}]}).pretty()<br>
{<br>
        "_id" : ObjectId("56063f17ade2f21f36b03133"),<br>
        "title" : "MongoDB 教程",<br>
        "description" : "MongoDB 是一个 Nosql 数据库",<br>
        "by" : "教程",<br>
        "tags" : [<br>
                "mongodb",<br>
                "database",<br>
                "NoSQL"<br>
        ],<br>
        "likes" : 100<br>
}<br>
<br>
<br>
<br>
MongoDB 条件操作符<br>
<br>
描述<br>
条件操作符用于比较两个表达式并从mongoDB集合中获取数据。<br>
<br>
MongoDB中条件操作符有：<br>
(>) 大于 - $gt<br>
(<) 小于 - $lt<br>
(>=) 大于等于 - $gte<br>
(<= ) 小于等于 - $lte<br>
我们使用的数据库名称为"runoob" 我们的集合名称为"test"，以下为我们插入的数据。<br>
为了方便测试，我们可以先使用以下命令清空集合 "test" 的数据：<br>
db.test.remove({})<br>
插入以下数据<br>
>db.test.insert({<br>
    title: 'PHP 教程',<br>
    description: 'PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。',<br>
    by: '教程',<br>
    tags: ['php'],<br>
    likes: 200<br>
})<br>
>db.test.insert({title: 'Java 教程',<br>
    description: 'Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。',<br>
    by: '教程',<br>
    tags: ['java'],<br>
    likes: 150<br>
})<br>
>db.test.insert({title: 'MongoDB 教程',<br>
    description: 'MongoDB 是一个 Nosql 数据库',<br>
    by: '教程',<br>
    tags: ['mongodb'],<br>
    likes: 100<br>
})<br>
使用find()命令查看数据：<br>
> db.test.find()<br><br>
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程",  "tags" : [ "php" ], "likes" : 200 }<br>
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程",  "tags" : [ "java" ], "likes" : 150 }<br>
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程",  "tags" : [ "mongodb" ], "likes" : 100 }<br>
MongoDB (>) 大于操作符 - $gt<br>
如果你想获取 "test" 集合中 "likes" 大于 100 的数据，你可以使用以下命令：<br>
db.test.find({"likes" : {$gt : 100}})<br>
类似于SQL语句：<br>
Select * from col where likes > 100;<br>
输出结果：<br>
> db.test.find({"likes" : {$gt : 100}})<br>
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程",  "tags" : [ "php" ], "likes" : 200 }<br>
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程",  "tags" : [ "java" ], "likes" : 150 }<br>
><br>
MongoDB（>=）大于等于操作符 - $gte<br>
如果你想获取"test"集合中 "likes" 大于等于 100 的数据，你可以使用以下命令：<br>
db.test.find({likes : {$gte : 100}})<br>
类似于SQL语句：<br>
Select * from test where likes >=100;<br>
输出结果：<br>
> db.test.find({likes : {$gte : 100}})<br>
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程",  "tags" : [ "php" ], "likes" : 200 }<br>
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程",  "tags" : [ "java" ], "likes" : 150 }<br>
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "tags" : [ "mongodb" ], "likes" : 100 }<br>
><br>
MongoDB (<) 小于操作符 - $lt<br>
如果你想获取"test"集合中 "likes" 小于 150 的数据，你可以使用以下命令：<br>
db.test.find({likes : {$lt : 150}})<br>
类似于SQL语句：<br>
Select * from test where likes < 150;<br>
输出结果：<br>
> db.test.find({likes : {$lt : 150}})<br>
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程",  "tags" : [ "mongodb" ], "likes" : 100 }<br>
MongoDB (<=) 小于操作符 - $lte<br>
如果你想获取"test"集合中 "likes" 小于等于 150 的数据，你可以使用以下命令：<br>
db.test.find({likes : {$lte : 150}})<br>
类似于SQL语句：<br>
Select * from test where likes <= 150;<br>
输出结果：<br>
> db.test.find({likes : {$lte : 150}})<br>
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "tags" : [ "java" ], "likes" : 150 }<br>
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "tags" : [ "mongodb" ], "likes" : 100 }<br>
MongoDB 使用 (<) 和 (>) 查询 - $lt 和 $gt<br>
如果你想获取"test"集合中 "likes" 大于100，小于 200 的数据，你可以使用以下命令：<br>
<br>
db.test.find({likes : {$lt :200, $gt : 100}})<br>
类似于SQL语句：<br>
Select * from test where likes>100 AND  likes<200;<br>
输出结果：<br>
> db.test.find({likes : {$lt :200, $gt : 100}})<br>
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程", "tags" : [ "java" ], "likes" : 150 }<br>




MongoDB $type 操作符<br>
$type操作符是基于BSON类型来检索集合中匹配的数据类型，并返回结果。<br>
MongoDB 中可以使用的类型如下表所示：<br>
我们使用的数据库名称为"test" 我们的集合名称为"test"，以下为我们插入的数据。<br>
简单的集合"test"：<br>
>db.test.insert({<br>
    title: 'PHP 教程',<br>
    description: 'PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。',<br>
    by: '教程',<br>
    tags: ['php'],<br>
    likes: 200<br>
})<br>
>db.col.insert({title: 'Java 教程',<br>
    description: 'Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。',<br>
    by: '教程',<br>
<br>
    tags: ['java'],<br>
    likes: 150<br>
})<br>
>db.col.insert({title: 'MongoDB 教程',<br>
    description: 'MongoDB 是一个 Nosql 数据库',<br>
    by: '教程',<br>
<br>
    tags: ['mongodb'],<br>
    likes: 100<br>
})<br>
使用find()命令查看数据：<br>
> db.col.find()<br>
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程",  "tags" : [ "php" ], "likes" : 200 }<br>
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程",  "tags" : [ "java" ], "likes" : 150 }<br>
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程",  "tags" : [ "mongodb" ], "likes" : 100 }<br>
MongoDB 操作符 - $type 实例<br>
如果想获取 "col" 集合中 title 为 String 的数据，你可以使用以下命令：<br>
db.col.find({"title" : {$type : 2}})<br>
输出结果为：<br>
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程",  "tags" : [ "php" ], "likes" : 200 }<br>
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程","tags" : [ "java" ], "likes" : 150 }<br>
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程", "tags" : [ "mongodb" ], "likes" : 100 }<br>







MongoDB Limit与Skip方法<br>
如果你需要在MongoDB中读取指定数量的数据记录，可以使用MongoDB的Limit方法，limit()方法接受一个数字参数，该参数指定从MongoDB中读取的记录条数。<br>
语法<br>
limit()方法基本语法如下所示：<br>
>db.COLLECTION_NAME.find().limit(NUMBER)<br>
实例<br>
集合 test 中的数据如下：<br>
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程", "tags" : [ "php" ], "likes" : 200 }<br>
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程",  "tags" : [ "java" ], "likes" : 150 }<br>
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程",  "tags" : [ "mongodb" ], "likes" : 100 }<br>
以上实例为显示查询文档中的两条记录：<br>
> db.test.find({},{"title":1,_id:0}).limit(2)<br>
{ "title" : "PHP 教程" }<br>
{ "title" : "Java 教程" }<br>
><br>
注：如果你们没有指定limit()方法中的参数则显示集合中的所有数据。<br>
MongoDB Skip() 方法<br>
我们除了可以使用limit()方法来读取指定数量的数据外，还可以使用skip()方法来跳过指定数量的数据，skip方法同样接受一个数字参数作为跳过的记录条数。<br>
语法<br>
skip() 方法脚本语法格式如下：<br>
>db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)<br>
实例<br>
以上实例只会显示第二条文档数据<br>
>db.test.find({},{"title":1,_id:0}).limit(1).skip(1)<br>
{ "title" : "Java 教程" }<br>
><br>
注:skip()方法默认参数为 0 。<br>





MongoDB 排序<br>
MongoDB sort()方法<br>
在MongoDB中使用使用sort()方法对数据进行排序，sort()方法可以通过参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而-1是用于降序排列。<br>
语法<br>
sort()方法基本语法如下所示：<br>
>db.COLLECTION_NAME.find().sort({KEY:1})<br>
实例<br>
col 集合中的数据如下：<br>
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "教程","tags" : [ "php" ], "likes" : 200 }<br>
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "教程","tags" : [ "java" ], "likes" : 150 }<br>
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "教程",  "tags" : [ "mongodb" ], "likes" : 100 }<br>
以下实例演示了 col 集合中的数据按字段 likes 的降序排列：<br>
>db.col.find({},{"title":1,_id:0}).sort({"likes":-1})<br>
{ "title" : "PHP 教程" }<br>
{ "title" : "Java 教程" }<br>
{ "title" : "MongoDB 教程" }<br>
><br>




MongoDB 索引<br>
索引通常能够极大的提高查询的效率，如果没有索引，MongoDB在读取数据时必须扫描集合中的每个文件并选取那些符合查询条件的记录。<br>
这种扫描全集合的查询效率是非常低的，特别在处理大量的数据时，查询可以要花费几十秒甚至几分钟，这对网站的性能是非常致命的。<br>
索引是特殊的数据结构，索引存储在一个易于遍历读取的数据集合中，索引是对数据库表中一列或多列的值进行排序的一种结构<br>
ensureIndex() 方法<br>
MongoDB使用 ensureIndex() 方法来创建索引。<br>
语法<br>
ensureIndex()方法基本语法格式如下所示：<br>
>db.COLLECTION_NAME.ensureIndex({KEY:1})<br>
语法中 Key 值为你要创建的索引字段，1为指定按升序创建索引，如果你想按降序来创建索引指定为-1即可。<br>
实例<br>
>db.test.ensureIndex({"title":1})<br>
><br>
ensureIndex() 方法中你也可以设置使用多个字段创建索引（关系型数据库中称作复合索引）。<br>
>db.test.ensureIndex({"title":1,"description":-1})<br>
><br>