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



