# css

## 特效收集

### 地图类

[ddos攻击地图，流量图特效](http://www.damddos.com/networkattack.html#page2)

## 功能

### 禁用长按选择文字

```css
* {
  -webkit-touch-callout:none;
  -webkit-user-select:none;
  -khtml-user-select:none;
  -moz-user-select:none;
  -ms-user-select:none;
  user-select:none;
}
```

### 限制显示字数，超出隐藏

```css
max-width: 76px;
overflow: hidden;
white-space: nowrap;
text-overflow: ellipsis;
```

