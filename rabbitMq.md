# RabbitMQ+PHP 消息队列环境配置

## 依赖包安装
yum install ncurses-devel unixODBC unixODBC-devel<br>

##erlang环境
wget http://erlang.org/download/otp_src_18.1.tar.gz<br>
tar -zxvf otp_src_18.1.tar.gz<br>
cd otp_src_18.1<br>
./configure --prefix=/usr/local/erlang<br>
make<br>
make install<br>


## 配置erlang环境变量<br>
vim /etc/profile<br>
## 增加内容：<br>
export PATH="$PATH:/usr/local/erlang/bin"<br>

## 保存退出，并刷新变量<br>
source /etc/profile<br>


## 测试erlang是否安装成功<br>
 安装完成以后，执行erl看是否能打开eshell，用’halt().’退出，注意后面的点号，那是erlang的结束符。<br>
[root@localhost src]# erl<br>
Erlang/OTP 17 [erts-6.1] [source] [64-bit] [async-threads:10] [hipe] [kernel-poll:false]<br>

Eshell V6.1  (abort with ^G)<br>
2> 9+3.<br>
12<br>
3> halt().<br>




#安装rabbitmq依赖文件,安装rabbitmq
## 安装rabbitmq依赖包
yum install xmlto<br>

## 安装rabbitmq服务端
wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.5.7/rabbitmq-server-3.5.7.tar.gz<br>
tar zxvf rabbitmq-server-3.5.7.tar.gz<br>
cd rabbitmq-server-3.5.7/<br>
make<br>
make install TARGET_DIR=/usr/local/rabbitmq SBIN_DIR=/usr/local/rabbitmq/sbin MAN_DIR=/usr/local/rabbitmq/man DOC_INSTALL_DIR=/usr/local/rabbitmq/doc<br>

## 配置hosts
vim /etc/hosts<br>
## 增加一行内容
## 当前IP地址   绑定HOSTNAME名称(vim /etc/sysconfig/network)
192.168.2.208 localhost.localdomain<br>

## 这种会提示错误(Warning: PID file not written; -detached was passed.)
/usr/local/rabbitmq/sbin/rabbitmq-server -detached 启动rabbitmq<br>
/usr/local/rabbitmq/sbin/rabbitmqctl status 查看状态<br>
/usr/local/rabbitmq/sbin/rabbitmqctl stop 关闭rabbitmq<br>

## 目前我自己使用
/usr/local/rabbitmq/sbin/rabbitmq-server start & 启动rabbitmq<br>
/usr/local/rabbitmq/sbin/rabbitmqctl status 查看状态<br>
/usr/local/rabbitmq/sbin/rabbitmqctl stop 关闭rabbitmq<br>
## 启用管理插件
mkdir /etc/rabbitmq<br>
/usr/local/rabbitmq/sbin/rabbitmq-plugins list 查看插件列表<br>
/usr/local/rabbitmq/sbin/rabbitmq-plugins enable rabbitmq_management  (启用插件)<br>
/usr/local/rabbitmq/sbin/rabbitmq-plugins disable rabbitmq_management (禁用插件)<br>

## 重启rabbitmq
## 访问 http://127.0.0.1:15672/

## 如果有iptables
vim /etc/sysconfig/iptables<br>

## 增加一下内容
-A INPUT -m state --state NEW -m tcp -p tcp --dport 15672 -j ACCEPT<br>

## 重启动iptable
service iptables restart<br>
开机自启动配置<br>

## !/bin/sh
## start rabbitMq
sudo /usr/local/rabbitmq/sbin/rabbitmq-server & > /usr/local/rabbitmq/logs/rabbitmq.log 2>&1<br>
#RabbitMQ PHP扩展安装
## 安装rabbitmq-c依赖包
yum install libtool autoconf<br>

## 安装rabbitmq-c ( 最好下载 0.5的，0.6安装可能会报错)
## 版本下载：https://github.com/alanxz/rabbitmq-c/releases/tag/v0.5.0
wget https://github.com/alanxz/rabbitmq-c/releases/download/v0.5.0/rabbitmq-c-0.5.0.tar.gz<br>
tar -zxvf v0.5.0<br>
cd rabbitmq-c-0.5.0/<br>
autoreconf -i<br>
./configure --prefix=/usr/local/rabbitmq-c<br>
make<br>
make install<br>

## 安装PHP扩展 amqp
wget http://pecl.php.net/get/amqp-1.6.1.tgz<br>
tar zxvf amqp-1.6.1.tgz<br>
cd amqp-1.6.1<br>
/usr/local/php/bin/phpize<br>
./configure --with-php-config=/usr/local/php/bin/php-config --with-amqp --with-librabbitmq-dir=/usr/local/rabbitmq-c<br>
make<br>
make install<br>

## 编辑php.ini文件，增加amqp扩展支持
vim /usr/local/php/etc/php.ini<br>

## 增加下面内容
; rabbitmq扩展支持<br>
extension=amqp.so<br>

## 重启php-fpm
/etc/init.d/php-fpm restart<br>
## 验证是否成功 phpinfo()查看下是否支持amqp扩展





##相关配置
hostname mq        // 设置hostname名称<br>
vim /etc/sysconfig/network    // 设置hostname<br>
vim /etc/hosts    // 编辑hosts<br>
./rabbitmqctl add_user admin admin     // 添加用户<br>
./rabbitmqctl set_user_tags admin administrator        // 添加admin 到 administrator分组<br>
./rabbitmqctl set_permissions -p / admin "*." "*." "*."    // 添加权限<br>
##创建配置文件
在/usr/rabbitmq/sbin/rabbitmq-defaults 查看config文件路径<br>
 ##创建配置文件
touch/usr/rabbitmq/sbin<br>
vm_memory_high_watermark 内存低水位线，若低于该水位线，则开启流控机制,阻止所有请求，默认值是0.4，即内存总量的40%,<br>
vm_memory_high_watermark_paging_ratio 内存低水位线的多少百分比开始通过写入磁盘文件来释放内存<br>
vi /usr/rabbitmq/sbin/rabbitmq.config 输入<br>
[<br>
{rabbit, [{vm_memory_high_watermark_paging_ratio, 0.75},<br>
         {vm_memory_high_watermark, 0.7}]}<br>
].<br>
##创建环境文件
touch /etc/rabbitmq/rabbitmq-env.conf<br>
输入<br>
    RABBITMQ_NODENAME=FZTEC-240088 节点名称<br>
    RABBITMQ_NODE_IP_ADDRESS=127.0.0.1 监听IP<br>
    RABBITMQ_NODE_PORT=5672 监听端口<br>
    RABBITMQ_LOG_BASE=/data/rabbitmq/log 日志目录<br>
    RABBITMQ_PLUGINS_DIR=/data/rabbitmq/plugins 插件目录<br>
    RABBITMQ_MNESIA_BASE=/data/rabbitmq/mnesia 后端存储目录<br>
##操作命令
#### 查看exchange信息
          /usr/rabbitmq/sbin/rabbitmqctl list_exchanges name type durable auto_delete arguments<br>

#### 查看队列信息
          /usr/rabbitmq/sbin/rabbitmqctl list_queues name durable auto_delete messages consumers me<br>
####  查看绑定信息
          /usr/rabbitmq/sbin/rabbitmqctl list_bindings<br>
#### 查看连接信息
          /usr/rabbitmq/sbin/rabbitmqctl list_connections<br>
##php的server端脚本
<?php<br>
$routingkey='key';<br>
//设置你的连接<br>
$conn_args = array('host' => 'localhost', 'port' => '5672', 'login' => 'guest', 'password' => 'guest');<br>
$conn = new AMQPConnection($conn_args);<br>
if ($conn->connect()) {<br>
    echo "Established a connection to the broker \n";<br>
}<br>
else {<br>
    echo "Cannot connect to the broker \n ";<br>
}<br>
//你的消息<br>
$message = json_encode(array('Hello World3!','php3','c++3:'));<br>
//创建channel<br>
$channel = new AMQPChannel($conn);<br>
//创建exchange<br>
$ex = new AMQPExchange($channel);<br>
$ex->setName('exchange');//创建名字<br>
$ex->setType(AMQP_EX_TYPE_DIRECT);<br>
$ex->setFlags(AMQP_DURABLE);<br>
//$ex->setFlags(AMQP_AUTODELETE);<br>
//echo "exchange status:".$ex->declare();<br>
echo "exchange status:".$ex->declareExchange();<br>
echo "\n";
for($i=0;$i<100;$i++){<br>
        if($routingkey=='key2'){<br>
                $routingkey='key';<br>
        }else{<br>
                $routingkey='key2';<br>
        }<br>
        $ex->publish($message,$routingkey);<br>
}<br>
/*
$ex->publish($message,$routingkey);<br>
创建队列<br>
$q = new AMQPQueue($channel);<br>
设置队列名字 如果不存在则添加<br>
$q->setName('queue');<br>
$q->setFlags(AMQP_DURABLE | AMQP_AUTODELETE);<br>
echo "queue status: ".$q->declare();<br>
echo "\n";<br>
echo 'queue bind: '.$q->bind('exchange','route.key');<br>
将你的队列绑定到routingKey<br>
echo "\n";<br>

$channel->startTransaction();<br>
echo "send: ".$ex->publish($message, 'route.key'); //将你的消息通过制定routingKey发送<br>
$channel->commitTransaction();<br>
$conn->disconnect();<br>
*/
##php客户端脚本
<?php<br>
$bindingkey='key2';<br>
//连接RabbitMQ<br>
$conn_args = array( 'host'=>'127.0.0.1' , 'port'=> '5672', 'login'=>'guest' , 'password'=> 'guest','vhost' =>'/');<br>
$conn = new AMQPConnection($conn_args);<br>
$conn->connect();<br>
//设置queue名称，使用exchange，绑定routingkey<br>
$channel = new AMQPChannel($conn);<br>
$q = new AMQPQueue($channel);<br>
$q->setName('queue2');<br>
$q->setFlags(AMQP_DURABLE);<br>
$q->declare();<br>
$q->bind('exchange',$bindingkey);<br>
//消息获取<br>
$messages = $q->get(AMQP_AUTOACK) ;<br>
if ($messages){<br>
var_dump(json_decode($messages->getBody(), true ));<br>
}<br>
$conn->disconnect();<br>
?><br>




###翻译了部分mq常量设置,不正确的地方,大家以试验为准
/**<br>
 * Passing in this constant as a flag will forcefully disable all other flags.<br>
 * Use this if you want to temporarily disable the amqp.auto_ack ini setting.<br>
 * 传递这个参数作为标志将完全禁用其他标志,如果你想临时禁用amqp.auto_ack设置起效<br>
 */<br>
define('AMQP_NOPARAM', 0);<br>

/**<br>
 * Durable exchanges and queues will survive a broker restart, complete with all of their data.<br>
 * 持久化交换机和队列,当代理重启动后依然存在,并包括它们中的完整数据<br>
 */<br>
define('AMQP_DURABLE', 2);<br>

/**<br>
 * Passive exchanges and queues will not be redeclared, but the broker will throw an error if the exchange or queue does not exist.<br>
 * 被动模式的交换机和队列不能被重新定义,但是如果交换机和队列不存在,代理将扔出一个错误提示<br>
 */<br>
define('AMQP_PASSIVE', 4);<br>

/**<br>
 * Valid for queues only, this flag indicates that only one client can be listening to and consuming from this queue.<br>
 * 仅对队列有效,这个人标志定义队列仅允许一个客户端连接并且从其消费消息<br>
 */<br>
define('AMQP_EXCLUSIVE', 8);<br>

/**<br>
 * For exchanges, the auto delete flag indicates that the exchange will be deleted as soon as no more queues are bound<br>
 * to it. If no queues were ever bound the exchange, the exchange will never be deleted. For queues, the auto delete<br>
 * flag indicates that the queue will be deleted as soon as there are no more listeners subscribed to it. If no<br>
 * subscription has ever been active, the queue will never be deleted. Note: Exclusive queues will always be<br>
 * automatically deleted with the client disconnects.<br>
 * 对交换机而言,自动删除标志表示交换机将在没有队列绑定的情况下被自动删除,如果从没有队列和其绑定过,这个交换机将不会被删除.<br>
 * 对队列而言,自动删除标志表示如果没有消费者和你绑定的话将被自动删除,如果从没有消费者和其绑定,将不被删除,独占队列在客户断<br>
 * 开连接的时候将总是会被删除<br>
 */
define('AMQP_AUTODELETE', 16);<br>

/**
 * Clients are not allowed to make specific queue bindings to exchanges defined with this flag.<br>
 * 这个标志标识不允许自定义队列绑定到交换机上<br>
 */
define('AMQP_INTERNAL', 32);<br>

/**
 * When passed to the consume method for a clustered environment, do not consume from the local node.<br>
 * 在集群环境消费方法中传递这个参数,表示将不会从本地站点消费消息<br>
 */
define('AMQP_NOLOCAL', 64);<br>

/**
 * When passed to the {@link AMQPQueue::get()} and {@link AMQPQueue::get()} methods as a flag,<br>
 * the messages will be immediately marked as acknowledged by the server upon delivery.<br>
 * 当在队列get方法中作为标志传递这个参数的时候,消息将在被服务器输出之前标志为acknowledged (已收到)<br>
 */
define('AMQP_AUTOACK', 128);<br>

/**<br>
 * Passed on queue creation, this flag indicates that the queue should be deleted if it becomes empty.<br>
 * 在队列建立时候传递这个参数,这个标志表示队列将在为空的时候被删除<br>
 */<br>
define('AMQP_IFEMPTY', 256);<br>

/**<br>
 * Passed on queue or exchange creation, this flag indicates that the queue or exchange should be<br>
 * deleted when no clients are connected to the given queue or exchange.<br>
 * 在交换机或者队列建立的时候传递这个参数,这个标志表示没有客户端连接的时候,交换机或者队列将被删除<br>
 */
define('AMQP_IFUNUSED', 512);<br>

/**<br>
 * When publishing a message, the message must be routed to a valid queue. If it is not, an error will be returned.<br>
 * 当发布消息的时候,消息必须被正确路由到一个有效的队列,否则将返回一个错误<br>
 */
define('AMQP_MANDATORY', 1024);<br>
<br>
/**<br>
 * When publishing a message, mark this message for immediate processing by the broker. (High priority message.)<br>
 * 当发布消息时候,这个消息将被立即处理.<br>
 */<br>
define('AMQP_IMMEDIATE', 2048);<br>

/**<br>
 * If set during a call to {@link AMQPQueue::ack()}, the delivery tag is treated as "up to and including", so that multiple<br>
 * messages can be acknowledged with a single method. If set to zero, the delivery tag refers to a single message.<br>
 * If the AMQP_MULTIPLE flag is set, and the delivery tag is zero, this indicates acknowledgement of all outstanding<br>
 * messages.<br>
 * 当在调用AMQPQueue::ack时候设置这个标志,传递标签将被视为最大包含数量,以便通过单个方法标示多个消息为已收到,如果设置为0<br>
 * 传递标签指向单个消息,如果设置了AMQP_MULTIPLE,并且传递标签是0,将所有未完成消息标示为已收到<br>
 */
define('AMQP_MULTIPLE', 4096);<br>

/**<br>
 * If set during a call to {@link AMQPExchange::bind()}, the server will not respond to the method.The client should not wait<br>
 * for a reply method. If the server could not complete the method it will raise a channel or connection exception.<br>
 * 当在调用AMQPExchange::bind()方法的时候,服务器将不响应请求,客户端将不应该等待响应,如果服务器无法完成该方法,将会抛出一个异常<br>
 */<br>
define('AMQP_NOWAIT', 8192);<br>

/**<br>
 * If set during a call to {@link AMQPQueue::nack()}, the message will be placed back to the queue.<br>
 * 如果在调用AMQPQueue::nack方法时候设置,消息将会被传递回队列<br>
 */<br>
define('AMQP_REQUEUE', 16384);<br>

/**<br>
 * A direct exchange type.<br>
 * direct类型交换机<br>
 */<br>
define('AMQP_EX_TYPE_DIRECT', 'direct');<br>

/**
 * A fanout exchange type.<br>
 * fanout类型交换机<br>
 */<br>
define('AMQP_EX_TYPE_FANOUT', 'fanout');<br>

/**<br>
 * A topic exchange type.<br>
 * topic类型交换机<br>
 */<br>
define('AMQP_EX_TYPE_TOPIC', 'topic');<br>

/**<br>
 * A header exchange type.<br>
 * header类型交换机<br>
 */<br>
define('AMQP_EX_TYPE_HEADERS', 'headers');<br>

/**<br>
 * socket连接超时设置<br>
 */<br>
define('AMQP_OS_SOCKET_TIMEOUT_ERRNO', 536870947);<br>
