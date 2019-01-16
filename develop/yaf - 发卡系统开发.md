# 发卡系统开发

## 项目概述

### 项目地址

[git](https://github.com/joql/SendVitrualCard)

### 安装

### 伪静态

```nginx
if (!-e $request_filename) {
    rewrite ^/(.*)  /index.php?$1 last;
}
```

### 报错

- 找不到Yaf\Loader类
    [yaf配置项参考文件](http://www.laruence.com/manual/yaf.ini.html)

  ```
  //错误信息： Class 'Yaf\Loader' not found in
  //原因：Yaf需要使用命名空间方式注册自己的类, 比如Yaf_Application将会变成Yaf\Application
  //解决方案：php.ini文件中加入 yaf.use_namespace=1
  ```

  

## 开发手册

### 模板引擎

变量

```php
<?php echo $data;?>
```

#### if

```php
<?php if():?>
<?php elseif():?>
<?php else:?>
<?php endif;?>
```

foreach

```php
<?php foreach($addons AS $k=>$v):?>
<?php endforeach;?>
```



## 参考

### 开发参考

#### 弹框特效

  [notice弹框特效](http://www.htmleaf.com/jQuery/Tooltips/201801204941.html), 用于商品详情页中查看批发价。弹框宽度noticejs.css中设置，text属性中可加标签，timeout时间貌似不太准

```html
<link href="dist/noticejs.css" rel="stylesheet" type="text/css">
<script src="dist/noticejs.js"></script>
<link href="css/animate.css" rel="stylesheet" type="text/css">  
new NoticeJs({
    text: text,
    position: 'middleCenter',
    timeout: 60,
    animation: {
    open: 'animated bounceInRight',
    close: 'animated bounceOutLeft'
    }
}).show();    
```



### 逻辑UI参考

- [用户界面UI参考](http://www.xx1q.com/)  账户 52249557  123456789a
- [视多发卡，](http://demo.sdfaka.cc/admin.php/index/index.html)  后台账号密码 admin  普通用户账号密码 1234567
- [自动发卡系统](http://www.yxa1024.com/)

