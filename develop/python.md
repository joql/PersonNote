# python

## 部署

### python3.6

#### 安装

```bash
#ius源安装 具体看/environment/linux
#安装python3.6
yum -y install python36u
#创建快捷方式
ln -s /bin/python3.6 /bin/python3
#安装pip3
yum -y install python36u-pip
#创建快捷方式
ln -s /bin/pip3.6 /bin/pip3
## 检测
python3.6 -V
pip3.6 -V
```

