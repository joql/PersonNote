# shell笔记

### 获取时间戳

[TOC]



```
current=`date "+%Y-%m-%d %H:%M:%S"`
timeStamp=`date -d "$current" +%s`
currentTimeStamp=$((timeStamp*1000+`date "+%N"`/1000000))

#10位时间戳
echo $timeStamp

#13位时间戳
echo $currentTimeStamp

```

### 双层循环时，循环内修改的变量，循环外不生效

------



### 当日零点时间戳

```
nowtime_stamp=$(date +%s)
today_seconds=$(date -d@${nowtime_stamp}  '+%H:%M:%S'|awk -F":" '{print $1*3600+$2*60+$3}')
diff=`expr $nowtime_stamp - $today_seconds`
echo $diff
```

------



### md5加密

```
// echo -n  取消自带换行符
md5=$(echo -n "joql${diff}"|md5sum |cut -d ' ' -f1)
echo $md5
```

------



### rm -rf 报错 argument list too long

**find /tmp/ -name "sess_*" | xargs -n 10 rm -f**

-n 10 代表每次删除10个

------

### word转pdf pdf转jpg

```
//安装 libreoffice
yum -y install libreoffice libreoffice-headless unoconv

//安装unoconv
wget http://ftp.tu-chemnitz.de/pub/linux/dag/redhat/el6/en/i386/rpmforge/RPMS/unoconv-0.5-1.el6.rf.noarch.rpm
rpm -Uvh unoconv-0.5-1.el6.rf.noarch.rpm


//word 2 pdf
//第一次转码好像会失败，之后就没事了  
//报错信息是 Error: Unable to connect or start own listener. Aborting.
unoconv -f pdf 1.doc


//pdf 2 jpg
yum install ImageMagick -y

//转码 输出格式 out-1.jpg  out-2.jpg
convert 1.pdf -quality 100 out/out.jpg
```



### [修改时区](https://www.jianshu.com/p/adefed4862a2)

```
tzselect
cp /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
```

