# php常用函数

## 目录
1. xml转array
2. ajax返回
3. 获取系统类型
4. 图片保存 base64
5. curl
6. 获得当日凌晨的时间戳
7. server数组
 
### xml转array
```
	/**
     * use for:xml转array
     * auth: Joql
     * @param $xml
     * @return mixed
     * date:2018-11-13 14:59
     */
    function xml2Array($xml)
    {
        $xml = simplexml_load_string($xml, 'SimpleXMLElement', LIBXML_NOCDATA);
        return json_decode(json_encode((array)$xml), true);
    }
```

### ajax返回
```
function returnAjax($code, $msg = '', $data = array()){
    header('Content-Type:application/json; charset=utf-8');
    exit(json_encode(array('code' => $code, 'data' => $data, 'msg' => $msg)));
}
```

### 获取系统类型
```
function getOS(){
    $agent = strtolower($_SERVER['HTTP_USER_AGENT']);
    $arr = array();
    if(strpos($agent, 'windows nt')) {
        $arr['platform'] = 'windows';
        $arr['id']=1;
    } elseif(strpos($agent, 'macintosh')) {
        $arr['platform'] = 'mac';
        $arr['id']=2;
    } elseif(strpos($agent, 'ipod')) {
        $arr['platform'] = 'ipod';
        $arr['id']=3;
    } elseif(strpos($agent, 'ipad')) {
        $arr['platform'] = 'ipad';
        $arr['id']=4;
    } elseif(strpos($agent, 'iphone')) {
        $arr['platform'] = 'iphone';
        $arr['id']=5;
    } elseif (strpos($agent, 'android')) {
        $arr['platform'] = 'android';
        $arr['id']=6;
    } elseif(strpos($agent, 'unix')) {
        $arr['platform'] = 'unix';
        $arr['id']=7;
    } elseif(strpos($agent, 'linux')) {
        $arr['platform'] = 'linux';
        $arr['id']=8;
    } else {
        $arr['platform'] = 'other';
        $arr['id']=9;
    }
    return $arr;
}
```
### 图片保存 base64
```
/**
 * TODO: 图片保存 base64
 * @param  String base64string 'data:image/png;base64'开头的图片数据
 * @param  String savePath 图片保存路径，不带文件名
 * @param  String old_img 原图片路径，传入则删除原图片
 * @param  String img_name 存储的图片名称，如为空，则使用uniqid自动生成
 * @param  Array  allowType 允许的图片类型，一维数组
 * @param  int    maxSize 限制图片的最大大小，单位Kb
 * @param  bool   type  值不为空则图片返回路径不以/开头
 * @return String savePath 图片保存路径,失败返回错误代码
 * /
function saveBase64Img($base64string,$savePath,$old_img = '',$img_name='',$allowType='jpg,bmp,gif,png',$maxSize=1024,$type=''){
    if(empty($base64string)){
        return 'EMPTY';
    }        
    // 处理存储地址，该地址不能以‘/’开头
    $savePath = trim($savePath,'/').'/';
     // 数据以逗号分隔，第二部分为图片数据，第一部分为图片mine信息
    $img_arr = explode(',', $base64string);
    
    $mime = $img_arr[0]; // data:image/jpeg;base64          
    $tmp = explode(';', $mime);
	// 获取加密类型
	$mime_info['encry'] = $tmp[1];
    // 获取mine类型
    $tmp1 = explode(':', $tmp[0]);
    $mime = $tmp1[1];
    // 获取类型/子类型
    $mime_info['mime'] = $mime;
    switch ($mime) {
        case 'image/bmp':
            $mime_info['suffix'] = 'bmp';
            break;
        case 'image/gif':
            $mime_info['suffix'] = 'gif';
            break;
        case 'image/jpeg':
            $mime_info['suffix'] = 'jpg';
            break;
        case 'image/png':
            $mime_info['suffix'] = 'png';
            break;
        default:
            
            break;
    }
    /* 如果后缀名为空，或者不在允许的类型之中，返回false */
    if(empty($mime_info['suffix']) || !in_array($mime_info['suffix'], explode(',', $allowType))){
        return 'NOT_ALLOW_TYPE';
    }
    // 计算图片流大小
    $strLength = strlen($img_arr[1]);
    $fileLength_byte = $strLength-($strLength/8)*2; // Byte
    $fileLength = ceil($fileLength_byte/1024); // Kb
    /* 如果文件大小大于限制的大小，返回false */
    if($fileLength > $maxSize){
        return 'MAX_SIZE';
    }
    // 检测目标文件夹是否存在
    if(!file_exists($savePath)){
        mkdir($savePath);
    }
    // 取出图片源数据
    $img = $img_arr[1];
    // 将编码过的数据转换为图片对象
    $img = base64_decode($img);
    // 设置存储的图片名称
    if(empty($img_name)){
        $img_name = uniqid();
    }
    // 对图片重命名，并存储到指定路径
    $img_path = $savePath.$img_name.'.'.$mime_info['suffix'];
    $res = file_put_contents($img_path, $img);
    if(!empty($old_img)){
        unlink(trim($old_img,'/'));
    }
    // 处理返回的地址，以 / 开头，方便存入数据库以及其它地方调用
    if($type){
        $img_path = $img_path;
    }else{
        $img_path = '/'.$img_path;
    }
    return $img_path;
}
```

### curl
```
 function curl($url,array $array ,$type = 'post') {
    $ch = curl_init();
    if($type == 'get'){
        if(is_array($array)) {
            $query = http_build_query($array);
            $url = $url . '?' . $query;
        }
    }
    if(stripos($url, "https://") !== false) {
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
        curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);
    }
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1 );
    if($type == 'post'){
        curl_setopt($ch, CURLOPT_POST, true);
        curl_setopt($ch, CURLOPT_POSTFIELDS, $array);
    }
    $content = curl_exec($ch);
    $status = curl_getinfo($ch);
    curl_close($ch);
    if(intval($status["http_code"]) == 200) {
        return $content;
    } else {
        echo $status["http_code"];
        return false;
    }
}
```

### 获得当日凌晨的时间戳
```
$today = strtotime(date("Y-m-d"),time());
```
### server数组
```
array(34) {
  ["USER"]=>
  string(3) "www"
  ["HOME"]=>
  string(9) "/home/www"
  ["FCGI_ROLE"]=>
  string(9) "RESPONDER"
  ["SCRIPT_FILENAME"]=>
  string(32) "/www/wwwroot/test.cn/install.php"
  ["QUERY_STRING"]=>
  string(0) ""
  ["REQUEST_METHOD"]=>
  string(3) "GET"
  ["CONTENT_TYPE"]=>
  string(0) ""
  ["CONTENT_LENGTH"]=>
  string(0) ""
  ["SCRIPT_NAME"]=>
  string(12) "/install.php"
  ["REQUEST_URI"]=>
  string(12) "/install.php"
  ["DOCUMENT_URI"]=>
  string(12) "/install.php"
  ["DOCUMENT_ROOT"]=>
  string(20) "/www/wwwroot/test.cn"
  ["SERVER_PROTOCOL"]=>
  string(8) "HTTP/1.1"
  ["REQUEST_SCHEME"]=>
  string(4) "http"
  ["GATEWAY_INTERFACE"]=>
  string(7) "CGI/1.1"
  ["SERVER_SOFTWARE"]=>
  string(12) "nginx/1.12.2"
  ["REMOTE_ADDR"]=>
  string(12) "171.11.0.162"
  ["REMOTE_PORT"]=>
  string(5) "11359"
  ["SERVER_ADDR"]=>
  string(12) "47.96.40.249"
  ["SERVER_PORT"]=>
  string(2) "80"
  ["SERVER_NAME"]=>
  string(7) "test.cn"
  ["REDIRECT_STATUS"]=>
  string(3) "200"
  ["PATH_INFO"]=>
  string(0) ""
  ["HTTP_HOST"]=>
  string(12) "47.96.40.249"
  ["HTTP_CONNECTION"]=>
  string(10) "keep-alive"
  ["HTTP_UPGRADE_INSECURE_REQUESTS"]=>
  string(1) "1"
  ["HTTP_USER_AGENT"]=>
  string(115) "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36"
  ["HTTP_ACCEPT"]=>
  string(85) "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8"
  ["HTTP_ACCEPT_ENCODING"]=>
  string(13) "gzip, deflate"
  ["HTTP_ACCEPT_LANGUAGE"]=>
  string(14) "zh-CN,zh;q=0.9"
  ["HTTP_COOKIE"]=>
  string(290) "serverType=nginx; order=id%20desc; memSize=2006; uploadSize=1073741824; rank=a; Path=/www/wwwroot/WeiXinKaiFa; BT_PANEL=c9854f04c616c20675a10fe704561624e909880d; backup_path=/www/backup; memRealUsed=566; mem-before=566/2006%20%28MB%29; upNet=0.51; downNet=0.28; apacheVersion=; phpVersion=0"
  ["PHP_SELF"]=>
  string(12) "/install.php"
  ["REQUEST_TIME_FLOAT"]=>
  float(1542288330.0927)
  ["REQUEST_TIME"]=>
  int(1542288330)
}
```
