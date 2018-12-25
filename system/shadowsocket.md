

```


//debian
//安装python
apt-get install python

//下载pip
wget https://bootstrap.pypa.io/get-pip.py --no-check-certificate

//安装pip
 python get-pip.py 
 
//centos
yum install python-pip
//如果缺失python-pip包
rpm -ivh http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm


//pip安装shadowsockets
pip install shadowsocks
 
 //配置文件

vi /etc/shadowsocks.json 

{
    "server":"ip",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "port_password":{
    "4443":"password"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}


//启动
ssserver -c /etc/shadowsocks.json -d start


//备注
如果不通，查看防护墙状态
/sbin/iptables -I INPUT -p tcp --dport 4443 -j ACCEPT #4443 ss
```

