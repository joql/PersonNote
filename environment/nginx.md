[TOC]

## 资料

1. [nginx中文文档](http://www.nginx.cn/doc/general/overview.html)

## 安装

> [安装流程](http://www.nginx.cn/install)

### 库功能

1. pcer库 提供 rewrite
2. zlib库 提供 gzip
3. ssl库 提供 https

## 变量

###   自定义变量

  变量在启动时创建，全局通用，但变量值不共享。变量在每次请求中赋值。

#### 创建

  set 变量名  变量值

```
location /bar {
        set $foo 32;
        echo "foo = [${foo}]";
}
```

#### 

### 内建变量

1. $arg_xxx 获取get中的xxx变量
2. $binary_remote_addr 客户二进制ip地址
3. $content_length Content-Length
4. $content-Type Content-Type
5. $cookie_xxx 获取Cookie中xxx
6. $document_root 网站根目录
7. $nginx_version 当前运行nginx版本号
8. $args,\$query_string get字符串
9. $remote_addr,\$remote_port,\$remote_user 客户端ip，端口号，用户信息
10. $scheme http,https
11. 当前url = $scheme://\$server_name/\$query_string

## 模块

### http core

1. **alias** ,重新命名路径，/spool/w3/images/ 代替 /i/。不支持正则。

   ```
   location  /i/ {
     alias   /spool/w3/images/;
   }
   
   #The request "/i/top.gif" will return the file "/spool/w3/images/top.gif".
   ```

2. **error_page**,配置错误页面。用法：error_page 错误码 错误提示页地址

3. location ,根据规则匹配uri，从而提供不同配置. 精准匹配和非正则情况下会停止匹配检测。

   1. 忽略大小写正则匹配使用前缀 ~*
   2. 大小写敏感正则匹配使用前缀 ~
   3. 精准匹配使用前缀 = 。例如 location = /
   4. 非正则使用前缀 ^~ 

4. server 用于区分域名

   1. server_name ，可采用通配符或正则方式匹配。
   2. 

5. server_tokens, 发送nginx版本号。

### http upstream

  采用轮询的方式实现负载均衡,ip_hash可以确保同一个用户尽可能链接到同一台服务器

```
upstream backend  {
  ip_hash; 
  server backend1.example.com weight=5;
  server backend2.example.com:8080;
  server unix:/tmp/backend3;
}
 
server {
  location / {
    proxy_pass  http://backend;
  }
}
```

### http authBasic

  访问验证，两种方式。1.密码验证。2.密码文件验证，可设置多个密码，密码必须用crpyt(3)加密

### http autoIndex

 用于显示目录，自动生成索引，

1. autoindex on  启动配置。
2. *autoindex_exact_size* 开启后显示文件准确大小，单位bytes。关闭后，显示大概大小，m、g等。

###  http gzip

  实施压缩数据流



## 语法

## 解决方案

### ws通过nginx转发为wss

  http://114.215.168.91:7273 为原ws地址,**ORIGIN**头用来过HTTP_ORIGIN认证

```nginx
location /wss {
        proxy_pass http://114.215.168.91:7273;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header ORIGIN "http://example.com";
    }
```

### 访问密码

  htpasswd命令提示不存在时通过 **yum -y install httpd** 安装

```nginx
location / {
    	 auth_basic "NY";
         auth_basic_user_file /www/conf/passwd.db;
    }
```

  **htpasswd -c /www/conf/passwd.db baoxy** 然后输入密码

### 反向代理

```
location / {
		proxy_http_version 1.1;
        proxy_set_header Connection "keep-alive";
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header server_name $server_name;
        proxy_pass http://localhost:6501;
    }
```

