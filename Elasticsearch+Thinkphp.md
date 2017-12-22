### elasticsearch在php中的使用

第一步，安装 java jdk；<br>
第二步，去elasticsearch 官网下载elasticsearch1.7.5 链接：https://www.elastic.co/downloads/past-releases 选相关的版本下载 ；<br>

第三步，安装插件：<br>
 elasticsearch-head安装<br>
	在cmd命令行中进入安装目录，再进入 bin目录，运行以下命令<br>
	plugin install mobz/elasticsearch-head<br>
然后浏览器http://localhost:9200/_plugin/head/ 就可以了 很简单<br>


第四步，安装elasticsearch-php 文件<br>

	1.PHP的版本在5.3.9以上，我用的是wamp php 5.5.12
	2.在项目中使用Composor来管理包，下载地址如下：http://www.phpcomposer.com/<br>
	3.在php.ini中开启curl和openssl  我的wamp集成环境，都已经开了<br>
	要使用elasticsearch，需要JDK的版本大于6，最好选择8吧，我的最新版<br>

	4.在项目里面放入一个命名为composer.json的文件，文件内容为：<br>

	{<br>
    		"require":{<br>
        		"elasticsearch/elasticsearch" : "~2.0"<br>
    		}<br>
	}<br>
	5.cmd 到项目根目录 composer insall 安装  elasticsearch-php<br>

第五步，安装thinkphp3.2<br>
	下载thinkphp3.2框架<br>
	index.php文件如下<br>

<?php<br>

// 检测PHP环境<br>
if(version_compare(PHP_VERSION,'5.3.0','<'))  die('require PHP > 5.3.0 !');<br>
// 开启调试模式 建议开发阶段开启 部署阶段注释或者设为false<br>
define('APP_DEBUG',True);<br>
/*define("REAL_PATH",dirname(__FILE__));*/<br>
// 定义应用目录<br>
define('APP_PATH','./mhyApp/');<br>
define('APP_NAME','mhyApp');<br>
define('RUNTIME_PATH','./Runtime/');<br>
// 引入ThinkPHP入口文件<br>

require './vendor/autoload.php';<br>
require './ThinkPHP3.2/ThinkPHP.php';<br>
// 亲^_^ 后面不需要任何代码了 就是如此简单<br>

第六步，修改 index 控制器内容，测试elasticsearch 文档中常用的方法 文件内容如下<br>


<?php<br>
namespace Home\Controller;<br>
use Think\Controller;<br>
class IndexController extends Controller {<br>
    public function _initialize()<br>
    {<br>
        $params['hosts'] = array(<br>
            '127.0.0.1:9200'<br>
        );<br>

        $this->client = \Elasticsearch\ClientBuilder::create()->build();<br>

    }<br>
    public function create_index()<br>
    {<br>
        $params = [<br>
            'index' => 'my_index',<br>
            'body' => [<br>
                'settings' => [<br>
                    "number_of_shards"=>"3",<br>
                    "number_of_replicas"=> "0"<br>
                ]<br>
            ]<br>
        ];<br>

        $response =  $this->client->indices()->create($params);<br>
        print_r($response);<br>

    }
    public function add_document()<br>
    {<br>
        $params = [<br>
            'index' => 'my_index',<br>
            'type' => 'my_type',<br>
            'id' => 'my_id1',<br>
            'body' => ['testField' => 'abc']<br>
        ];<br>

        $response = $this->client->index($params);<br>
        print_r($response);<br>

    }<br>
    public function delete_index()<br>
    {<br>
        $deleteParams = [<br>
            'index' => 'my_index'<br>
        ];<br>
        $response = $this->client->indices()->delete($deleteParams);<br>
        print_r($response);<br>
    }<br>
    public function delete_document()<br>
    {<br>
        $params = [<br>
            'index' => 'my_index',<br>
            'type' => 'my_type',<br>
            'id' => 'my_id'<br>
        ];<br>

        $response = $this->client->delete($params);<br>
        print_r($response);<br>
    }<br>
    public function update_document()<br>
    {<br>
        $updateParams = array();<br>
        $updateParams['index'] = 'my_index';<br>
        $updateParams['type'] = 'my_index';<br>
        $updateParams['id'] = 'my_id';<br>
        $updateParams['body']['doc']['asas']  = '111111';<br>
        $response = $this->client->update($updateParams);<br>

    }<br>
    public function search()<br>
    {<br>
        $params = [<br>
            'index' => 'my_index',<br>
            'type' => 'my_type',<br>
            'body' => [<br>
                'query' => [<br>
                    'match' => [ <br>
                        'testField' => 'abc'<br>

                    ]<br>
                ]<br>
            ]<br>
        ];<br>

        $response = $this->client->search($params);<br>
        print_r($response);<br>
    }<br>
    public function get_document()<br>
    {<br>
        $params = [<br>
            'index' => 'my_index',<br>
            'type' => 'my_type',<br>
            'id' => 'my_id'<br>
        ];<br>

        $response = $this->client->get($params);<br>
        print_r($response);<br>
    }<br>
}<br>


文档地址如下，注意不同elasticsearch-php的版本，文档是不一样的，同时不同版本的elasticsearch所有的到elasticsearch-php 版本也是不一样的<br>

https://www.elastic.co/guide/en/elasticsearch/client/php-api/2.0/index.html<br>

参考的文档：<br>
windows下elasticSearch以及elasticSearch-php安装及使用<br>
http://blog.csdn.net/kdchxue/article/details/51161276<br>
