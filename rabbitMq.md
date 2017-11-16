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