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

###学习练习


//$connection=new MongoClient();<br>

//$db=$connection->shop->users;<br>

//批量添加文档: (只能循环一条一条加)<br>
/*for($i=0;$i<100;$i++){<br>
    $db->insert(array("id"=>$i,"username"=>$i,"addtime"=>time()));<br>
}*/<br>


//查找集合所有数据<br>
/*$userList=$db->find();*/<br>

//查找集合所有数据<br>
/*$userList=$db->find();<br>
//显示结果<br>
foreach (  $userList  as  $id  =>  $value  ) {<br>
   var_dump (  $value  );<br>
}*/<br>


//查找条件为username=1的<br>
/*$userList=$db->find(array("username"=>1));<br>
foreach($userList as $id=>$value){<br>
    var_dump($value);<br>
}*/<br>


//查找条件为username=3的<br>
/*$userInfo=$db->findOne(array("username"=>3));<br>
var_dump($userInfo);*/<br>


//清空一个表中的所有数据<br>
/*var_dump($db->remove());*/<br>
/*var_dump($db->remove());<br>
for($i=0;$i<100;$i++){<br>
    $username="username".$i;<br>
    $db->insert(array("id"=>$i,"money"=>$i,"username"=>"$username","addtime"=>time()));<br>
}*/<br>



//查找所有的记录，不过只要money 与username 二个字段的数据<br>
/*$userList=$db->find(array(),array("money"=>1,"username"=>1));<br>
foreach($userList as $id=>$value){<br>
    var_dump($value);<br>
}*/<br>


/*$userList=$db->find(array("money"=>99));<br>
foreach($userList as $key=>$value){<br>
    var_dump($value);<br>
}*/<br>


//查找money 大于90的所有数据<br>
//$userList=$db->find(array("money" => array('$gt' => 90)));<br>
/*$userList=$db->find(array("money"=>array('$gt'=>90)));<br>
foreach($userList as $key=>$value){<br>
    var_dump($value);<br>
}*/<br>

//查找money 大于1 且小于5 的所有数据<br>
/*<br>
$userList=$db->find(array("money"=>array('$gt'=>1,'$lt'=>5)));<br>
<br>
foreach($userList as $key=>$value){<br>
    var_dump($value);<br>
}*/<br>


//查找money 在1 5 6 三个值的所有数据<br>
/*$userList=$db->find(array("money"=>array('$in'=>array(1,5,6))));<br>
foreach($userList as $key=>$value){<br>
 var_dump($value);<br>
}*/<br>

//查找money 大于1 且小于5  且 值在2,3,4 的所有数据<br>
/*$userList=$db->find(array("money"=>array('$gt'=>1,'$lt'=>10,'$nin'=>array(2,3,4))));<br>
foreach ($userList as $key=>$value){<br>
    var_dump($value);<br>
}*/<br>


//查找(money 等于2 或 等于5 或username 等于username3)  且 值在2 的所有数据<br>
/*$userList=$db->find(<br>
    array(<br>
        "money"=>2,<br>
        '$or'=>array(<br>
            array("money"=>2),<br>
            array("money"=>5),<br>
            array("username"=>"username3")<br>
        )<br>
    )<br>
);<br>
foreach ($userList as $key=>$value){<br>
    var_dump($value);<br>
}*/<br>
<br>
//查找username like  %me1% 的所有数据<br>
/*<br>
$userList=$db->find(array("username"=>new MongoRegex("/me1/")));<br>
foreach($userList as $key=>$value){<br>
    var_dump($value);<br>
}*/<br>
/*<br>

//查找 从 10开始的后10条数据<br>
$userList=$db->find()->limit(10)->skip(10);<br>

foreach($userList as $key=>$value){<br>
    var_dump($value);<br>
}*/<br>


//查找 满足条件的数据数量<br>

/*$userList=$db->find(array("money"=>array('$gt'=>50)))->count();<br>
var_dump($userList);*/<br>


//查找 一条记录<br>
/*$userInfo=$db->findOne(array("money"=>1));<br>
var_dump($userInfo);<br>
// 更新money等于1的数据<br>
$result=$db->update(array("money"=>1),array('$set'=>array("username"=>"sbs")));<br>
var_dump($result);<br>
$userInfo=$db->findOne(array("money"=>1));<br>
var_dump($userInfo);*/<br>

// 更新money等于1的数据清空原来的数据，更新后数据只要 a=1<br>
/*$db->update(array("money"=>1), array("a" => 1));<br>

$userInfo=$db->findOne( array("a" => 1));<br>
var_dump($userInfo);*/<br>


/*$result=$db->remove(array("money"=>array('$gt'=>10,'$lt'=>20)));<br>

var_dump($result);*/<br>


/*$userList=$db->find(array("money"=>array('$gt'=>10,'$lt'=>20)));<br>

foreach($userList as $key=>$value){<br>
    var_dump($value);<br>
}*/<br>


/*$userList=$db->find(array("money"=>array('$gt'=>10,'$lt'=>20)))->count();<br>
var_dump($userList);*/<br>

