### Swoole：面向生产环境的 PHP 异步网络通信引擎


#### 安装
Linux 用户 <br/>

#!/bin/bash <br/>
pecl install swoole <br/>
#### Mac 用户

#!/bin/bash <br/>
brew install swoole <br/>

#### HTTP Server

<?php <br/>
$http = new swoole_http_server("127.0.0.1", 9501); <br/>

$http->on("start", function ($server) { <br/>
    echo "Swoole http server is started at http://127.0.0.1:9501\n"; <br/>
});<br/>

$http->on("request", function ($request, $response) { <br/>
    $response->header("Content-Type", "text/plain"); <br/>
    $response->end("Hello World\n"); <br/>
}); <br/>

$http->start(); <br/>



#### WebSocket Server
<?php <br/>
$server = new swoole_websocket_server("127.0.0.1", 9502); <br/>

$server->on('open', function($server, $req) { <br/>
    echo "connection open: {$req->fd}\n"; <br/>
}); <br/>

$server->on('message', function($server, $frame) { <br/>
    echo "received message: {$frame->data}\n"; <br/>
    $server->push($frame->fd, json_encode(["hello", "world"])); <br/>
});<br/>

$server->on('close', function($server, $fd) { <br/>
    echo "connection close: {$fd}\n"; <br/>
}); <br/>

$server->start(); <br/>

可以结合js的Socket.IO.js 一起使用<br/>



#### TCP Server
<?php<br/>
$server = new swoole_server("127.0.0.1", 9503);<br/>
$server->on('connect', function ($server, $fd){<br/>
    echo "connection open: {$fd}\n";<br/>
});<br/>
$server->on('receive', function ($server, $fd, $reactor_id, $data) {<br/>
    $server->send($fd, "Swoole: {$data}");<br/>
    $server->close($fd);<br/>
});<br/>
$server->on('close', function ($server, $fd) {<br/>
    echo "connection close: {$fd}\n";<br/>
});<br/>
$server->start();<br/>




#### TCP Client
<?php<br/>
$client = new swoole_client(SWOOLE_SOCK_TCP, SWOOLE_SOCK_ASYNC);<br/>
$client->on("connect", function($cli) {<br/>
    $cli->send("hello world\n");<br/>
});<br/>
$client->on("receive", function($cli, $data){<br/>
    echo "received: {$data}\n";<br/>
});<br/>
$client->on("error", function($cli){<br/>
    echo "connect failed\n";<br/>
});<br/>
$client->on("close", function($cli){<br/>
    echo "connection close\n";<br/>
});<br/>
$client->connect("127.0.0.1", 9501, 0.5);<br/>




#### Async Redis / HTTP / WebSocket Client
<?php<br/>
$redis = new Swoole\Redis;<br/>
$redis->connect('127.0.0.1', 6379, function ($redis, $result) {<br/>
    $redis->set('test_key', 'value', function ($redis, $result) {<br/>
        $redis->get('test_key', function ($redis, $result) {<br/>
            var_dump($result);<br/>
        });<br/>
    });<br/>
});<br/>

$cli = new Swoole\Http\Client('127.0.0.1', 80);<br/>
$cli->setHeaders(array('User-Agent' => 'swoole-http-client'));<br/>
$cli->setCookies(array('test' => 'value'));<br/>

$cli->post('/dump.php', array("test" => 'abc'), function ($cli) {<br/>
    var_dump($cli->body);<br/>
    $cli->get('/index.php', function ($cli) {<br/>
        var_dump($cli->cookies);<br/>
        var_dump($cli->headers);<br/>
    });<br/>
});<br/>




#### Tasks
<?php<br/>
$server = new swoole_server("127.0.0.1", 9502);<br/>
$server->set(array('task_worker_num' => 4));<br/>
$server->on('receive', function($server, $fd, $reactor_id, $data) {<br/>
    $task_id = $server->task("Async");<br/>
    echo "Dispath AsyncTask: [id=$task_id]\n";<br/>
});<br/>
$server->on('task', function ($server, $task_id, $reactor_id, $data) {<br/>
    echo "New AsyncTask[id=$task_id]\n";<br/>
    $server->finish("$data -> OK");<br/>
});<br/>
$server->on('finish', function ($server, $task_id, $data) {<br/>
    echo "AsyncTask[$task_id] finished: {$data}\n";<br/>
});<br/>
$server->start();<br/>