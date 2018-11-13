# php常用函数

## 目录
#### xml转array
#### ajax返回
#### 获取系统类型
#### 图片保存 base64
#### curl
 
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


