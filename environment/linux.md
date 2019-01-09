# LINUX

## 信息获取

### cpu和内存使用率查看

  [pstree](https://wangchujiang.com/linux-command/c/pstree.html) 显示进程树，判断当前运行进程和子线程数量

  top -M 查看实时状态。

​    M  按内存使用率排序

### 统计字数并排序

  awk '{print $1}' ip.log | sort | uniq -c | sort -n -r