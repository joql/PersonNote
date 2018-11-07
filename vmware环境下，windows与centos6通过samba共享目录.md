# vmware环境下，windows与centos6通过samba共享目录

## 安装
    yum install samba -y

## 配置smb.conf

//新增

;共享文件名
[share]
comment = this is linux share directory
;共享路径
path = /home/samba/share
available = yes
browseable = yes
public = yes
writable = yes
;共享访问账号，必须是系统内存在的
user = www


## 设置samba登陆密码

// www为用户名，必须是系统内存在的，设置的密码专用于登陆samba
smbpasswd -a www


## 运行
service smb start

## windows连接
ctrl+r
\\192.168.137.128
![Alt text](https://raw.githubusercontent.com/joql/PersonNote/master/public/img/samba-1.webp)

## 注意
  1. 无法连接时尝试关闭防火墙