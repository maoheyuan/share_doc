### php 利用activeMq+stomp实现消息队列

### Activemq+PHP演示demo


### 1、先安装php扩展stomp
cd /usr/local/src<br>
wget http://pecl.php.net/get/stomp-1.0.9.tgz<br>
tar zxvf stomp-1.0.9.tgz<br>
cd stomp-1.0.9<br>
/usr/local/php/bin/phpize<br>
./configure --enable-stomp --with-php-config=/usr/local/php/bin/php-config<br>
make<br>
make install<br>
### 修改php配置文件php.ini添加刚装好的stomp
 #vim /usr/local/php/etc/php.ini
添加 extension=stomp.so<br>
重启php<br>

### 2、生产者publisher.php（每秒发送一次当前时间）
<?php<br>
$queue  = '/topic/phptest';<br>
$msg    = 'bar';<br>

try {<br>
    $stomp = new Stomp('tcp://localhost:61613');<br>

    while (true) {<br>
      $stomp->send($queue, $msg." ". date("Y-m-d H:i:s"));<br>
      sleep(1);<br>
    }<br>

} catch(StompException $e) {<br>
    die('Connection failed: ' . $e->getMessage());<br>
}<br>
###3、消费者consumer.php
<?php<br>
$queue  = '/topic/phptest';<br>

try {<br>
    $stomp = new Stomp('tcp://localhost:61613');<br>
    $stomp->subscribe($queue);<br>

    while (true) {<br>
       if ($stomp->hasFrame()) {<br>
           $frame = $stomp->readFrame();<br>
           if ($frame != NULL) {<br>
               print "Received: " . $frame->body . " - time now is " . date("Y-m-d H:i:s"). "\n";
               $stomp->ack($frame);<br>
           }<br>
       } else {<br>
           print "No frames to read\n";<br>
       }<br>
    }<br>

} catch(StompException $e) {<br>
    die('Connection failed: ' . $e->getMessage());<br>
}<br>

###4、通过执行 php producer.php 和 php consumer.php来进行mq消息存取。