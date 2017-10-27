# memgodb-php学习总结



## memgodb-php的常用命令


转自 http://blog.csdn.net/black_ox/article/details/22678747 <br>
命令也可以在参考http://www.jb51.net/article/51601.htm<br>
这个 文档也不错http://www.cnblogs.com/yuechaotian/archive/2013/02/04/2891457.html<br>
mongo操作 php 官方网站  http://php.net/manual/zh/mongo.core.php<br>
连接mongo<br>
$connection  = new  MongoClient ();  // 连接到 localhost:27017<br>
$connection  = new  MongoClient (  "mongodb://example.com"  );  // 连接到远程服务器 （使用默认端口： 27017）<br>
$connection  = new  MongoClient (  "mongodb://example.com:65432"  );  // 链接到远程服务器，使用自定义的端口<br>
这个驱动使用了持久连接，并会在下次试图链接到同一服务器时重用它。<br>
验证<br>
复制代码<br>
//指定用户名和密码连接URI（首选）<br>
$m  = new  MongoClient ( "mongodb:// ${ username } : ${ password } @localhost" );<br>
// 指定的用户名和密码，选择array<br>
$m  = new  MongoClient ( "mongodb://localhost" , array( "username"  =>  $username ,  "password"  =>  $password ));<br>
// 在连接URI中指定身份验证数据库（首选）<br>
$m  = new  MongoClient ( "mongodb:// ${ username } : ${ password } @localhost/myDatabase" );<br>
//通过指定的数据库身份验证选项array<br>
$m  = new  MongoClient ( "mongodb:// ${ username } : ${ password } @localhost" , array( "db"  =>  "myDatabase" ));<br>
复制代码<br>
多个服务器<br>
$m  = new  MongoClient ( "mongodb://mongos1.example.com:27017,mongos2.example.com:27017" ));<br>
CURD总结<br>
复制代码<br>
$connection  = new  MongoClient (); //mongo 连接<br>
$db  =  $connection -> dbname -> runoob;   //选择数据库 -> 选择集合<br>
$db->find(); //查找集合所有数据<br>
$db->findOne();//查找一条数据<br>
$db->find(array(), array("a" => 1, "b" => 1)); // 返回a,b字段<br>
$db->find(array("age" => 33)); //查找where age=33 的数据<br>
$db->find(array("age" => array('$gt' => 33))); //$gt:大于  $gte:大于等于   $lt:小于   $lte:小于等于   $ne:不等于<br>
$db->find(array("number"=>array('$gt' => 1,'$lt' => 9))); //大于1,小于9<br>
$db->find(array("number"=>array('$in' => array(1,2,9)))); //等于哪些值<br>
$db->find(array("number"=>array('$nin' => array(1,2,9)))); //不等于哪些值<br>
$db->find(array('$or' => array(array('number'=>2),array('number'=>9))); //或<br>
$db->find(array("name" => new MongoRegex("/Joe/"))); // name LIKE "%Joe%"<br>
$db->find()->limit(10)->skip(20); //LIMIT 10 SKIP 20<br>
$db->find(array("z" => 3))->explain(); //explain<br>
$db->sort(array("name" => 1)); //1 是正序ASC  -1是倒序 DESC<br>
$db->count(); //集合的总数<br>
$db->count({"a"=>2}); //a=2集合的总数<br>
$db->find()->limit(5)->skip(0)->count(true) //返回条件查询的总数<br>
$db->update(array("b" => "q"), array("a" => 1)); // 将整条更新为 {a:1} 其他的数据都会消失<br>
$db->update(array("b" => "q"), array('$set' => array("a" => 1))); //$set 只更新数据中 a 字段<br>
$db->update(array("b" => "q"), array('$inc' => array("a" => 2))); //UPDATE dbname SET a=a+2 WHERE b='q'<br>
$db->remove(array("z" => "abc"));//删除字段z='abc'的数据<br>
$db->remove(array("z" => "abc"), array("justOne" => true)); //justOne 删除一条<br>
复制代码<br>
批量添加文档: (只能循环一条一条加)<br>
for (  $i  =  0 ;  $i  <  100 ;  $i ++ ) {<br>
     $collection -> insert ( array(  'i'  =>  $i ,  "field { $i } "  =>  $i  *  2  ) );<br>
}<br>
 返回结果处理<br>
$cursor  =  $collection -> find ();<br>
foreach (  $cursor  as  $id  =>  $value  ) {<br>
    var_dump (  $value  );<br>
}  <br>