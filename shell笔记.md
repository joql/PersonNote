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