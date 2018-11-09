# easyswoole笔记


### server信息
  通过 $this->request()->getSwooleRequest()->server 获取
  ```
	var_dump($this->request()->getSwooleRequest()->server);
	{
	    "query_string": "a=11",
	    "request_method": "GET",
	    "request_uri": "/",
	    "path_info": "/",
	    "request_time": 1493204184,
	    "request_time_float": 1493204199.6719,
	    "server_port": 9501,
	    "remote_port": 55411,
	    "remote_addr": "127.0.0.1",
	    "server_protocol": "HTTP/1.1",
	    "server_software": "swoole-http-server"
	}
  ```

### 服务器热重启
```
#!/bin/bash

DIR=$1
changeTime=1

if [ ! -n "$DIR" ] ;then
    echo "you have not choice Application directory !"
    exit
fi

php easyswoole stop
php easyswoole start --d


fswatch -r $DIR | while read file
do
   current=`date "+%Y-%m-%d %H:%M:%S"`
   nowTimeStamp=`date -d "$current" +%s`
   val=`expr $nowTimeStamp - $changeTime`
   if [ 6 -lt $val ]; then
       changeTime=$nowTimeStamp
       echo "${file} was modify" >> ./Temp/reload.log 2>&1
       php easyswoole reload
   fi
done

```