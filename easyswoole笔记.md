# easyswoole笔记


### 安装

  0. 基础环境  
    ```
    curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
    yum makecache
    yum update -y
    yum install -y gcc zlib zlib-dev xml2 xml2-dev vim wget openssl openssl-devel curl-devel libjpeg.x86_64 libpng.x86_64 freetype.x86_64 libjpeg-devel.x86_64 libpng-devel.x86_64 freetype-devel.x86_64 bzip2-devel.x86_64 php-mcrypt  libmcrypt  libmcrypt-devel libxml2 libxml2-devel gcc-c++ icu libicu libicu-devel ntpdate
    ntpdate ntp.api.bz
    ```

  1. 安装php7.2  
    ```
    wget http://cn2.php.net/distributions/php-7.2.12.tar.gz
    mv php-7.2.12.tar.gz /home/
    tar -xvf php-7.2.12.tar.gz
    cd php-7.2.12
    ./configure --prefix=/home/php72 --with-config-file-path=/home/php72/etc --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-iconv-dir --with-zlib  --disable-rpath  --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring --enable-intl --with-openssl --with-mhash  --enable-sockets --with-mysql
    make && make install

    // /etc/profile 添加环境变量
    // 拷贝 php.ini
    cp /home/php-7.2.12/php.ini-production /home/php72/etc/php.ini
    ```
  2. 安装redis  
    ```
    wget http://download.redis.io/releases/redis-4.0.9.tar.gz
    tar -xvf redis-4.0.9.tar.gz
    cd redis-4.0.9
    make PREFIX=/home/redis install

    //修改redis.conf   daemonize yes
    //后台启动  ./redis-server ./redis.conf
    cp /home/redis-4.0.9/redis.conf ./bin/
    ```
  3. 安装hiredis  
    ```
    wget --no-check-certificate https://github.com/redis/hiredis/archive/v0.14.0.tar.gz
    tar -zxvf v0.14.0.tar.gz
    cd hiredis-0.14.0/
    make && make install
    mkdir /usr/lib/hiredis
    cp libhiredis.so /usr/lib/hiredis
    mkdir /usr/include/hiredis
    cp hiredis.h /usr/include/hiredis
    echo '/usr/local/lib' >>/etc/ld.so.conf
    ldconfig
    ```

  4. 安装phpiredis  
    ```
    wget --no-check-certificate https://github.com/nrk/phpiredis/archive/v1.0.0.tar.gz
    tar -xvf v1.0.0.tar.gz
    cd phpiredis-1.0.0
    phpize && ./configure --enable-phpiredis
    make && make install
    ```
  5. 安装nghttp2  
    ```
    wget --no-check-certificate https://github.com/nghttp2/nghttp2/releases/download/v1.30.0/nghttp2-1.30.0.tar.bz2
    tar -jxvf nghttp2-1.30.0.tar.bz2
    ./configure && make && make install
    ```
  6. 安装swoole  
    ```
    wget https://github.com/swoole/swoole-src/archive/master.tar.gz
    phpize
    make clean &&
    ```


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


### nginx 转发
```
location / {
        proxy_http_version 1.1;
        proxy_set_header Connection "keep-alive";
        proxy_set_header x-forwarded-for $remote_addr;
        proxy_set_header host $server_name;
        //允许cros跨域访问
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' "POST, GET, OPTIONS";
        add_header 'Access-Control-Allow-Headers' "Origin, Authorization, Accept, auth";
        add_header 'Access-Control-Allow-Credentials' true;
        if (!-e $request_filename){
            proxy_pass http://localhost:9502;
        }
    }

```