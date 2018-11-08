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