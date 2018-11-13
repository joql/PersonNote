# js常用例子

### 识别iso客户端
```
var u = navigator.userAgent;
var isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
if (isiOS) {
	console.log(IOS);
}else{
	console.log(非IOS);
}
```

### 识别微信浏览器
```
var isWx = navigator.userAgent.toLowerCase().indexOf('micromessenger') > -1 || typeof navigator.wxuserAgent !== "undefined"; //wx
if (isWx) {
	console.log(Wx);
}else{
	console.log(非Wx);
}
```

### 判断数组存在某值
```
[1,2,3].indexOf(1)   // return 0
[1,2,3].indexOf(3)   // return 2

```