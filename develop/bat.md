



### 批量修改文件

> 循环递增
>
> setlocal enabledelayedexpansion  //变量延迟的启动语句
>
> !a!  //双感叹号括起来调用

```
@echo off
set /a a=1
setlocal enabledelayedexpansion
for /f %%i in ('dir /b *.jpg') do (
    echo %%i
    echo "!a!.jpg"
    ren %%i "!a!.jpg"
    set /a a+=1
)
pause
```

