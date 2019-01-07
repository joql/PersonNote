# js

[TOC]

## 函数

### 识别iso客户端

  **key: ios 识别**

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

  **key: 微信 识别**

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

### 复制到剪贴板

  **key: 复制**

```
function copy(el) {
	var range = document.createRange();
	var end = el.childNodes.length;
	range.setStart(el,0);
	range.setEnd(el,end);

	var selection = window.getSelection();
	selection.removeAllRanges();
	selection.addRange(range);
	document.execCommand("copy",false,null);
	selection.removeRange(range);
}

```



## 常用

### 监听video播放状态

  **key: video 监听-播放**

  $('#MsgBox video').bind("play",function(){console.log(456);})









## requirejs用途

> 1.实现js的异步加载
>
> 2.处理模块之间的依赖性

## 杂记

```
//函数定义后，函数名就是一个可用的对象变量

//var 定义全局变量 函数内可调用函数外的变量

//delegate可以监听未来元素，非冒泡类型的事件不能添加
```





