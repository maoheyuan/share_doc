# memcache学习总结


### memcache的简介

Memcached是一个自由开源的，高性能，分布式内存对象缓存系统。<br>
Memcached是一种基于内存的key-value存储，用来存储小块的任意数据（字符串、对象）。这些数据可以是数据库调用、API调用或者是页面渲染的结果。<br>
Memcached简洁而强大。它的简洁设计便于快速开发，减轻开发难度，解决了大数据量缓存的很多问题。它的API兼容大部分流行的开发语言。<br>
本质上，它是一个简洁的key-value存储系统。<br>


### memcache的常用命令

#### 连接服务器
$memcache = new Memcache;<br>
$memcache->addServer('127.0.0.1', 11211);<br>
$memcache->addServer('192.168.1.149', 11211);<br>
或者
$memcache = new Memcache;<br>
$memcache =$memcache->connect('127.0.0.1', 11211) <br>

或者
$memcache = new Memcache;<br>
$memcache =$memcache->pconnect('127.0.0.1', 11211) <br>

#### 关闭memcache连接
$memcache->close();<br>

#### 增加一个条目到缓存服务器
$memcache->add('key', 'value', false, 30);<br>
$memcache->set('key', 'value', false, 50);<br>
add 与set的区别在于如果原来就有key时 add不会新增 但是set 会覆盖原来的值，都不存在时都会新增<br>


#### 从服务端检回一个元素
$memcache->get('key');<br>
$memcache->set(array('key1', 'key2'));<br>
get方法可以接收一个字符串或一个数组<br>


#### 给定一个key加减 上1或加减上指定的数值
$current_value = $memcache->increment('money');//money 加上1 <br>
$current_value = $memcache->increment('money'，3);//money 加上3 <br>

$new_value = $memcache_obj->decrement('test_item');//money 减去 1 <br>
$new_value = $memcache_obj->decrement('test_item', 3);//money 减去 3 <br>


#### 替换已经存在的元素的值
$memcache->replace("key", "new value", false, 30);<br>


#### 从服务端删除一个元素

$memcache->delete('key_delete', 10);<br>

10 删除该元素的执行时间。如果值为0,则该元素立即删除，如果值为30,元素会在30秒内被删除。<br>



#### 清洗（删除）已经存储的所有的元素

$memcache->flush();<br>


#### 返回服务器版本信息

$memcache->getVersion() ;<br>


#### 用于获取一个服务器的在线/离线状态

$memcache->>getServerStatus('127.0.0.1', 11211);;<br>
返回一个服务器的状态，0表示服务器离线，非0表示在线。<br>


####  获取服务器统计信息【单个】

$memcache->>getStats('127.0.0.1', 11211);;<br>
返回关联数组表示的服务器统计信息 或者在失败时返回 FALSE。<br>

pid                       进程号<br>
uptime                    服务器运行时间<br>
time                    当前时间<br>
version                    版本号<br>
rusage_user                平均时间<br>
rusage_system            Accumulated system time for this process<br>
curr_items                Current number of items stored by the server<br>
total_items                Total number of items stored by this server ever since it started<br>
bytes                    当前用了多少内存<br>
curr_connections        当前的边接数<br>
total_connections        开始时多少连接<br>
connection_structures    最大允许的连接<br>
cmd_get                    get的总次数<br>
cmd_set                    set 的次数<br>
get_hits                命中的次数<br>
get_misses                没有命中的次数<br>
bytes_read                读取的大小<br>
bytes_written            写入大小<br>
limit_maxbytes            服务器可用的最大内存.<br>




####  缓存服务器池中所有服务器统计信息【多个】

$memcache_obj = new Memcache;
$memcache_obj->addServer('127.0.0.1', 11211);
$memcache_obj->addServer('192.168.1.149', 11211);


$stats = $memcache_obj->getExtendedStats();
print_r($stats);
返回一个二维关联数组的服务器统计信息或者在失败时返回FALSE。<br>


Array<br>
(<br>
    [127.0.0.1:11211] => Array<br>
        (
            [pid] => 3756<br>
            [uptime] => 603011<br>
            [time] => 1133810435<br>
            [version] => 1.1.12
            [rusage_user] => 0.451931<br>
            [rusage_system] => 0.634903<br>
            [curr_items] => 2483<br>
            [total_items] => 3079<br>
            [bytes] => 2718136<br>
            [curr_connections] => 2<br>
            [total_connections] => 807<br>
            [connection_structures] => 13<br>
            [cmd_get] => 9748<br>
            [cmd_set] => 3096<br>
            [get_hits] => 5976<br>
            [get_misses] => 3772<br>
            [bytes_read] => 3448968<br>
            [bytes_written] => 2318883<br>
            [limit_maxbytes] => 33554432<br>
        )<br>

    [192.168.1.149:11211] => false<br>
)<br>

    pid                       进程号<br>
    uptime                    服务器运行时间<br>
    time                    当前时间<br>
    version                    版本号<br>
    rusage_user                平均时间<br>
    rusage_system            Accumulated system time for this process<br>
    curr_items                Current number of items stored by the server<br>
    total_items                Total number of items stored by this server ever since it started<br>
    bytes                    当前用了多少内存<br>
    curr_connections        当前的边接数<br>
    total_connections        开始时多少连接<br>
    connection_structures    最大允许的连接<br>
    cmd_get                    get的总次数<br>
    cmd_set                    set 的次数<br>
    get_hits                命中的次数<br>
    get_misses                没有命中的次数<br>
    bytes_read                读取的大小<br>
    bytes_written            写入大小<br>
    limit_maxbytes            服务器可用的最大内存.<br>



####  运行时修改服务器参数和状态

<?php<br>

function _callback_memcache_failure($host, $port) {<br>
    print "memcache '$host:$port' failed";<br>
}<br>
$memcache = new Memcache;<br>
// 增加一台离线服务器<br>
$memcache->addServer('memcache_host', 11211, false, 1, 1, -1, false);<br>
// 使该服务器变为在线状态<br>
$memcache->setServerParams('memcache_host', 11211, 1, 15, true, '_callback_memcache_failure');<br>

?><br>




####  开启大值自动压缩

$memcache_obj = new Memcache;<br>
$memcache_obj->addServer('memcache_host', 11211);<br>
$memcache_obj->setCompressThreshold(20000, 0.2);<br>


20000<br>
控制多大值进行自动压缩的阈值。<br>

0.2<br>
指定经过压缩实际存储的值的压缩率，支持的值必须在0和1之间。默认值是0.2表示20%压缩率。<br>



总结，memcache 的命令不多，大多数可以记得了，如果记不了的可以查看一下手册，关键在其基础上设计出一套完整的合理的缓存方案，才能使系统的架构更合理，能承受更大的负载...