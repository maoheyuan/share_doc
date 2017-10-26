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
$current_value = $memcache->increment('money');//money 加上1
$current_value = $memcache->increment('money'，3);//money 加上3

$new_value = $memcache_obj->decrement('test_item');//money 减去 1
$new_value = $memcache_obj->decrement('test_item', 3);//money 减去 3

