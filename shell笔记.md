# shell笔记


### 获取时间戳
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

### 当日零点时间戳

```
nowtime_stamp=$(date +%s)
today_seconds=$(date -d@${nowtime_stamp}  '+%H:%M:%S'|awk -F":" '{print $1*3600+$2*60+$3}')
diff=`expr $nowtime_stamp - $today_seconds`
echo $diff
```

### md5加密

```
// echo -n  取消自带换行符
md5=$(echo -n "joql${diff}"|md5sum |cut -d ' ' -f1)
echo $md5
```

