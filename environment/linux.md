# LINUX

## 安装

### ius源安装

  [详细参考地址](https://www.cnblogs.com/f-ck-need-u/p/8494992.html#auto_id_0)

```shell
[root@xuexi ~]# vim /etc/yum.repos.d/ius.repo
[ius]
name=iusrepo
baseurl=https://mirrors.tuna.tsinghua.edu.cn/ius/stable/CentOS/6/$basearch
gpgcheck=0
enable=1
```



## 信息获取

### cpu和内存使用率查看

  [pstree](https://wangchujiang.com/linux-command/c/pstree.html) 显示进程树，判断当前运行进程和子线程数量

  top -M 查看实时状态。

​    M  按内存使用率排序

### 统计字数并排序

  awk '{print $1}' ip.log | sort | uniq -c | sort -n -r



## 报错

### 网卡启动

```linux
//错误信息: device eth0 does not seem to be present, delaying initialization.
//原因： mac地址出现问题
//方案：删除旧的，重新绑定即可

//打开后注释掉mac地址
vim /etc/sysconfig/network-scripts/ifcfg-eth0

//删除绑定文件
rm -f /etc/udev/rules.d/70-persistent-net.rules

//重启
reboot
```

