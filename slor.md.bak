#使用 PHP 和 Apache Solr 实现企业搜索
原文链接：http://www.ibm.com/developerworks/cn/opensource/os-php-apachesolr/

　　　　   http://blog.csdn.net/hzcyclone/article/details/7006354

###1、安装solr（下载地址：http://www.apache.org/dyn/closer.lua/lucene/solr/6.0.0）

###2、安装php的solr扩展（下载地址：http://pecl.php.net/package/solr）

###3、下载phpsolrclient代码（最健壮的实现是 Donovan Jimenez 的 PHP Solr Client，下载地址：http://code.google.com/p/solr-php-client/downloads/list）

###4、使用代码示例：

复制代码<br>
<?php<br>
include("Apache/Solr/Service.php");<br>

//连接Solr服务器<br>
$solr = new Apache_Solr_service('localhost' , '8983' ,'/solr');<br>
if( !$solr->ping() ) {<br>
    echo'Solr server not responding';<br>
   exit;<br>
}<br>

$data = array(<br>
array(<br>
'id' => 'EN80922032',<br>
'name' => '男士打磨直筒休闲牛仔裤',<br>
'brand' => 'ENERGIE',<br>
'cat' => '牛仔裤',<br>
'price' => '1870.00'<br>
),<br>
array(<br>
'id' => 'EN70906025',<br>
'name' => '品牌LOGO翻领拉链外套',<br>
'brand' => 'ENERGIE',<br>
'cat' => '外套',<br>
'price' => '1680.00'<br>
),<br>
);<br>

//添加索引数据
$documents = array();<br>
foreach($data as $key => $value) {<br>
    $part =new Apache_Solr_Document();<br>
   foreach($value as $key2 =>$value2) {<br>
       $part->$key2 =$value2;<br>
    }<br>
    
   $documents[] = $part;<br>
}<br>

$solr->addDocuments( $documents );<br>
$solr->commit();<br>
$solr->optimize();<br>

//查询索引 $solr->search(字段:关键字 , 开始 ,每页显示,排序)<br>
$offset = 0;<br>
$limit = 10;<br>
$sort = 'price asc';<br>

$rs = $solr->search("brand:ENERGIE" , $offset ,$limit,array('sort' => $sort));<br>
if($rs->response->numFound> 0) {<br>
   foreach($rs->response->docs as $doc) {<br>
       echo $doc->id.'|'.$doc->name.'|'.$doc->brand.'|'.$doc->price.'<br>';<br>
    }<br>
}<br>
?><br>