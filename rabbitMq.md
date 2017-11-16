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
yum install xmlto

## 安装rabbitmq服务端
wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.5.7/rabbitmq-server-3.5.7.tar.gz
tar zxvf rabbitmq-server-3.5.7.tar.gz
cd rabbitmq-server-3.5.7/
make
make install TARGET_DIR=/usr/local/rabbitmq SBIN_DIR=/usr/local/rabbitmq/sbin MAN_DIR=/usr/local/rabbitmq/man DOC_INSTALL_DIR=/usr/local/rabbitmq/doc

## 配置hosts
vim /etc/hosts
## 增加一行内容
## 当前IP地址   绑定HOSTNAME名称(vim /etc/sysconfig/network)
192.168.2.208 localhost.localdomain

## 这种会提示错误(Warning: PID file not written; -detached was passed.)
/usr/local/rabbitmq/sbin/rabbitmq-server -detached 启动rabbitmq
/usr/local/rabbitmq/sbin/rabbitmqctl status 查看状态
/usr/local/rabbitmq/sbin/rabbitmqctl stop 关闭rabbitmq

## 目前我自己使用
/usr/local/rabbitmq/sbin/rabbitmq-server start & 启动rabbitmq
/usr/local/rabbitmq/sbin/rabbitmqctl status 查看状态
/usr/local/rabbitmq/sbin/rabbitmqctl stop 关闭rabbitmq
## 启用管理插件
mkdir /etc/rabbitmq
/usr/local/rabbitmq/sbin/rabbitmq-plugins list 查看插件列表
/usr/local/rabbitmq/sbin/rabbitmq-plugins enable rabbitmq_management  (启用插件)
/usr/local/rabbitmq/sbin/rabbitmq-plugins disable rabbitmq_management (禁用插件)

## 重启rabbitmq
## 访问 http://127.0.0.1:15672/

## 如果有iptables
vim /etc/sysconfig/iptables

## 增加一下内容
-A INPUT -m state --state NEW -m tcp -p tcp --dport 15672 -j ACCEPT

## 重启动iptable
service iptables restart
开机自启动配置

## !/bin/sh
## start rabbitMq
sudo /usr/local/rabbitmq/sbin/rabbitmq-server & > /usr/local/rabbitmq/logs/rabbitmq.log 2>&1
#RabbitMQ PHP扩展安装
## 安装rabbitmq-c依赖包
yum install libtool autoconf

## 安装rabbitmq-c ( 最好下载 0.5的，0.6安装可能会报错)
## 版本下载：https://github.com/alanxz/rabbitmq-c/releases/tag/v0.5.0
wget https://github.com/alanxz/rabbitmq-c/releases/download/v0.5.0/rabbitmq-c-0.5.0.tar.gz
tar -zxvf v0.5.0
cd rabbitmq-c-0.5.0/
autoreconf -i
./configure --prefix=/usr/local/rabbitmq-c
make
make install

## 安装PHP扩展 amqp
wget http://pecl.php.net/get/amqp-1.6.1.tgz
tar zxvf amqp-1.6.1.tgz
cd amqp-1.6.1
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config --with-amqp --with-librabbitmq-dir=/usr/local/rabbitmq-c
make
make install

## 编辑php.ini文件，增加amqp扩展支持
vim /usr/local/php/etc/php.ini

## 增加下面内容
; rabbitmq扩展支持
extension=amqp.so

## 重启php-fpm
/etc/init.d/php-fpm restart
## 验证是否成功 phpinfo()查看下是否支持amqp扩展