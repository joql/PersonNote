发卡系统开发

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

- Couldn't find bootstrap

    ```
    //原因：application 中 application.directory 没有配置
    //解决方案:
    application.directory 修改成当前目录
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



## 需求日志

### 20190124

- 代理用户
  - [x] 用户管理，分普通用户，代理用户。
  - [x] 商品可设置代理价。
  - [x] 前台登录可选代理用户，登陆后显示代理价，可使用代理价购物。
- 用户中心
  - [x] 上传头像
  - 充值记录页，包括余额显示及余额充值入口。
  - [x] 消费记录页。
- 余额支付
  - [x] 增加余额支付类型，提交订单时可选余额支付，订单结算页，自动扣除余额，支付状态验证接口。
  - 余额充值
    - [x] 余额充值表构建及余额充值页面。
    - [x] 充值订单提交接口，支付二维码生成接口，支付回调更新表状态接口。



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

## 支付参考

### 幻兮支付

[官网](https://www.zhapay.com)

测试账号   **17633398721  ** **123456789a**

```
#跳转支付宝链接
https://qr.alipay.com/fkx01449rzweotl0a8lxe80?t=1548054987301
https://qr.alipay.com/fkx04560x0ccx8hsop02mc6?t=1548055143534

```



### 逻辑UI参考

- [用户界面UI参考](http://www.xx1q.com/)  账户 52249557  123456789a
    - [视多发卡，](http://demo.sdfaka.cc/admin.php/index/index.html)  后台账号密码 admin  普通用户账号密码 1234567
- [自动发卡系统](http://www.yxa1024.com/)

