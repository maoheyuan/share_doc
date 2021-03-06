# Redis学习总结


### Redis的简介

Redis是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。<br>
它通常被称为数据结构服务器，因为值（value）可以是 字符串(String), 哈希(Map), 列表(list), 集合(sets) 和 有序集合(sorted sets)等类型。<br>

### Redis的常用命令

#### 连接服务器
$redis=new Redis();<br>
$redis->connect("127.0.0.1",6379);<br>


#### Redis String类型

//普通set/get操作<br>
$redis->set('key', 'value');<br>
$str = $redis->get('key');<br>
echo $str; //显示 'value'<br>

//setex set一个存储时效<br>
$redis->setex('key', 10, 'value'); //表示存储有效期为10秒<br>

//setnx/msetnx相当于add操作,不会覆盖已有值<br>
$redis->setnx('foo',12); //true<br>
$redis->setnx('foo',38); //false<br>
$redis->msetnx(array("xiamogg"=>1,"ffgg"=>"fff"));//true<br>
$redis->msetnx(array("xiaomaogg"=>2,"ffgg"=>"ggfff"));//false<br>


//getset操作,set的变种,结果返回替换前的值<br>
$redis->getset('foo',56);//返回38<br>

// incrby/incr/decrby/decr 对值的递增和递减<br>
$redis->incr('foo'); //foo为57<br>
$redis->incrby('foo',2); //foo为59<br>
$redis->decr("foo"));foo为58<br>
$redis->decrBy("foo",2);foo为56<br>

//exists检测是否存在某值<br>
$redis->exists('foo');//true<br>

//del 删除<br>
$redis->del('foo');//true<br>

//type 类型检测,字符串返回string,列表返回 list,set表返回set/zset,hash表返回hash<br>
$redis->type('foo');//不存在,返回none<br>
$redis->set('str','test');<br>
$redis->type('str'); //字符串，返回string<br>

//append 连接到已存在字符串<br>

//返回累加后的字符串长度8,此进str为 'test_ggg'<br>
$redis->append('str','_ggg');<br>

//setrange 部分替换操作<br>

//返回3,参数2为0时等同于set操作<br>
$redis->setrange('str',0,'abc');<br>

//返回4,表示从第2个字符后替换,这时'str'为'abcd'<br>
$redis->setrange('str',2,'cd');<br>

//substr 部分获取操作<br>

$redis->substr('str',0,2);//表示从第0个起，取到第2个字符，共3个，返回'abc'<br>

//strlen 获取字符串长度<br>

$redis->strlen('str'); //返回4<br>

//setbit/getbit 位存储和获取<br>

$redis->setbit('binary',31,1);//表示在第31位存入1,这边可能会有大小端问题?不过没关系,getbit 应该不会有问题<br>
$redis->getbit('binary',31);//返回1<br>

//keys 模糊查找功能,支持*号以及?号(匹配一个字符)<br>
$redis->set('foo1',123);<br>
$redis->set('foo2',456);<br>
$redis->keys('foo*'); //返回foo1和foo2的array<br>
$redis->keys('f?o?');&nbsp; //同上<br>


//randomkey 随机返回一个key<br>

$redis->randomkey(); //可能是返回 'foo1'或者是'foo2'及其它任何一存在redis的key<br>

//rename/renamenx 对key进行改名,所不同的是renamenx不允许改成已存在的key<br>
$redis->rename('str','str2'); //把原先命名为'str'的key改成了'str2'<br>

//expire 设置key-value的时效性,ttl 获取剩余有效期,persist 重新设置为永久存储<br>
$redis->expire('foo', 1); //设置有效期为1秒<br>
$redis->ttl('foo'); //返回有效期值1s<br>
$redis->expire('foo'); //取消expire行为<br>

//dbsize 返回redis当前数据库的记录总数<br>
$redis->dbsize();<br>


#### Redis Hash表

//hset/hget 存取hash表的数据<br>
$redis->hset('hash1','key1','v1'); //将key为'key1' value为'v1'的元素存入hash1表<br>
$redis->hset('hash1','key2','v2');<br>
$redis->hget('hash1','key1');//取出表'hash1'中的key 'key1'的值,返回'v1'<br>

//hexists 返回hash表中的指定key是否存在<br>
$redis->hexists ('hash1','key1'); //true or false

//hdel 删除hash表中指定key的元素<br>
$redis->hdel('hash1','key2'); //true or false <br>

//hlen 返回hash表元素个数<br>
$redis->hlen('hash1'); //1 <br>

//hsetnx 增加一个元素,但不能重复
$redis->hsetnx('hash1','key1','v2'); //false <br>
$redis->hsetnx('hash1','key2','v2'); //true <br>

//hmset/hmget 存取多个元素到hash表 <br>
$redis->hmset('hash1',array('key3'=>'v3','key4'=>'v4')); <br>
$redis->hmget('hash1',array('key3','key4')); //返回相应的值 array('v3','v4') <br>

//hincrby 对指定key进行累加 <br>
$redis->hincrby('hash1','key5',3); //返回3 <br>
$redis->hincrby('hash1','key5',10); //返回13 <br>

//hkeys 返回hash表中的所有key <br>
$redis->hkeys('hash1'); //返回array('key1','key2','key3','key4','key5')

//hvals 返回hash表中的所有value <br>
$redis->hvals('hash1'); //返回array('v1','v2','v3','v4',13) <br>

//hgetall 返回整个hash表元素 <br>
$redis->hgetall('hash1'); //返回array('key1'=>'v1','key2'=>'v2','key3'=>'v3','key4'=>'v4','key5'=>13) <br>


####Redis list列表

//rpush/rpushx 有序列表操作,从队列后插入元素<br>
//lpush/lpushx 和rpush/rpushx的区别是插入到队列的头部,同上,'x'含义是只对已存在的key进行操作<br>
$redis->rpush('fooList', 'bar1'); //返回一个列表的长度1<br>
$redis->lpush('fooList', 'bar0'); //返回一个列表的长度2<br>
$redis->rpushx('fooList', 'bar2'); //返回3,rpushx只对已存在的队列做添加,否则返回0<br>
//llen返回当前列表长度<br>
$redis->llen('fooList');//3<br>

//lrange 返回队列中一个区间的元素<br>
$redis->lrange('fooList',0,1); //返回数组包含第0个至第1个共2个元素<br>
$redis->lrange('fooList',0,-1);//返回第0个至倒数第一个,相当于返回所有元素,注意redis中很多时候会用到负数,下同<br>

//lindex 返回指定顺序位置的list元素<br>
$redis->lindex('fooList',1); //返回'bar1'<br>

//lset 修改队列中指定位置的value<br>
$redis->lset('fooList',1,'123');//修改位置1的元素,返回true<br>

//lrem 删除队列中左起指定数量的字符<br>
$redis->lrem('fooList',1,'_'); //删除队列中左起(右起使用-1)1个字符'_'(若有)<br>

//lpop/rpop 类似栈结构地弹出(并删除)最左或最右的一个元素<br>
$redis->lpop('fooList'); //'bar0'<br>
$redis->rpop('fooList'); //'bar2'<br>

//ltrim 队列修改，保留左边起若干元素，其余删除<br>
$redis->ltrim('fooList', 0,1); //保留左边起第0个至第1个元素<br>

//rpoplpush 从一个队列中pop出元素并push到另一个队列<br>
$redis->rpush('list1','ab0');<br>
$redis->rpush('list1','ab1');<br>
$redis->rpush('list2','ab2');<br>
$redis->rpush('list2','ab3');<br>
$redis->rpoplpush('list1','list2');//结果list1 =>array('ab0'),list2 =>array('ab1','ab2','ab3')<br>
$redis->rpoplpush('list2','list2');//也适用于同一个队列,把最后一个元素移到头部list2 =>array('ab3','ab1','ab2')<br>

//linsert 在队列的中间指定元素前或后插入元素<br>
$redis->linsert('list2', 'before','ab1','123'); //表示在元素'ab1'之前插入'123'<br>
$redis->linsert('list2', 'after','ab1','456');&nbsp;&nbsp; //表示在元素'ab1'之后插入'456'<br>

//blpop/brpop 阻塞并等待一个列队不为空时，再pop出最左或最右的一个元素（这个功能在php以外可以说非常好用）<br>
//brpoplpush 同样是阻塞并等待操作，结果同rpoplpush一样
$redis->blpop('list3',10); //如果list3为空则一直等待,直到不为空时将第一元素弹出,10秒后超时；<br>

####Redis Set集合


//sadd 增加元素,返回true,重复返回false<br>
$redis->sadd('set1','ab');<br>
$redis->sadd('set1','cd');<br>
$redis->sadd('set1','ef');<br>

//srem 移除指定元素<br>
$redis->srem('set1','cd'); //删除'cd'元素<br> <br>

//spop 弹出首元素
$redis->spop('set1');<br>

//smove 移动当前set表的指定元素到另一个set表
$redis->sadd('set2','123');<br>
$redis->smove('set1','set2','ab');//移动'set1'中的'ab'到'set2',返回true or false<br>

//scard 返回当前set表元素个数<br>
$redis->scard('set2');//2<br>

//sismember 判断元素是否属于当前表<br>
$redis->sismember('set2','123'); //true or false<br>

//smembers 返回当前表的所有元素<br>
$redis->smembers('set2'); //array('123','ab');<br>

//sinter/sunion/sdiff 返回两个表中元素的交集/并集/补集<br>
$redis->sadd('set1','ab');<br>
$redis->sinter('set2','set1'); //返回array('ab')<br>

//sinterstore/sunionstore/sdiffstore 将两个表交集/并集/补集元素copy到第三个表中<br>
$redis->set('foo',0);<br>
$redis->sinterstore('foo','set1'); //这边等同于将'set1'的内容copy到'foo'中，并将'foo'转为set表<br>
$redis->sinterstore('foo',array('set1','set2')); //将'set1'和'set2'中相同的元素copy到'foo'表中,覆盖'foo'原有内容<br>

//srandmember 返回表中一个随机元素<br>
$redis->srandmember('set1');<br>


####Redis Zset集合

//sadd 增加元素,并设置序号,返回true,重复返回false<br>
$redis->zadd('zset1',1,'ab');<br>
$redis->zadd('zset1',2,'cd');<br>
$redis->zadd('zset1',3,'ef');<br>

//zincrby 对指定元素索引值的增减,改变元素排列次序<br>
$redis->zincrby('zset1',10,'ab');//返回11<br>

//zrem 移除指定元素<br>
$redis->zrem('zset1','ef'); //true or false<br>

//zrange 按位置次序返回表中指定区间的元素<br>
$redis->zrange('zset1',0,1); //返回位置0和1之间(两个)的元素<br>
$redis->zrange('zset1',0,-1);//返回位置0和倒数第一个元素之间的元素(相当于所有元素)<br>

//zrevrange 同上,返回表中指定区间的元素,按次序倒排<br>
$redis->zrevrange('zset1',0,-1); //元素顺序和zrange相反<br>

//zrangebyscore/zrevrangebyscore 按顺序/降序返回表中指定索引区间的元素<br>
$redis->zadd('zset1',3,'ef');<br>
$redis->zadd('zset1',5,'gh');<br>
$redis->zrangebyscore('zset1',2,9); //返回索引值2-9之间的元素 array('ef','gh')<br>
//参数形式
$redis->zrangebyscore('zset1',2,9,'withscores'); //返回索引值2-9之间的元素并包含索引值 array(array('ef',3),array('gh',5))<br>
$redis->zrangebyscore('zset1',2,9,array('withscores' =>true,'limit'=>array(1, 2))); //返回索引值2-9之间的元素,'withscores' =>true表示包含索引值; 'limit'=>array(1, 2),表示最多返回2条,结果为array(array('ef',3),array('gh',5))<br>

//zunionstore/zinterstore 将多个表的并集/交集存入另一个表中<br>
$redis->zunionstore('zset3',array('zset1','zset2','zset0')); //将'zset1','zset2','zset0'的并集存入'zset3'<br>
//其它参数
$redis->zunionstore('zset3',array('zset1','zset2'),array('weights' => array(5,0)));//weights参数表示权重，其中表示并集后值大于5的元素排在前，大于0的排在后<br>
$redis->zunionstore('zset3',array('zset1','zset2'),array('aggregate' => 'max'));//'aggregate' => 'max'或'min'表示并集后相同的元素是取大值或是取小值<br>

//zcount 统计一个索引区间的元素个数<br>
$redis->zcount('zset1',3,5);//2<br>
$redis->zcount('zset1','(3',5)); //'(3'表示索引值在3-5之间但不含3,同理也可以使用'(5'表示上限为5但不含5<br>

//zcard 统计元素个数<br>
$redis->zcard('zset1');//4<br>

//zscore 查询元素的索引<br>
$redis->zscore('zset1','ef');//3<br>

//zremrangebyscore 删除一个索引区间的元素<br>
$redis->zremrangebyscore('zset1',0,2); //删除索引在0-2之间的元素('ab','cd'),返回删除元素个数2<br>

//zrank/zrevrank 返回元素所在表顺序/降序的位置(不是索引)<br>
$redis->zrank('zset1','ef');//返回0,因为它是第一个元素;zrevrank则返回1(最后一个)<br>

//zremrangebyrank 删除表中指定位置区间的元素<br>
$redis->zremrangebyrank('zset1',0,10); //删除位置为0-10的元素,返回删除的元素个数2<br>