# Webgl学习笔记（二）功能实现篇

## 将鼠标点击坐标转换到webgl系统中

### 获取鼠标位置

```js
let x = ev.clientX;
let y = ev.clientY;
```

> 鼠标点击坐标是在“浏览器客户区”中的坐标，而不是canvas中坐标。

![](/assets/client.png)

### 将鼠标位置转换为canvas中坐标

```js
let rect = ev.target.getBoundingClientRect();

// rect.left、rect.top是<canvas>原点在浏览器客户区中的坐标。
x = x - rect.left;
y = y - rect.top;
```

### 将&lt;canvas&gt;中坐标转换到webgl系统中

```js
x = (x - canvas.width / 2) / (canvas.width / 2);
y = (canvas.height / 2 - y) / (canvas.height / 2);
```

![](/assets/canvas_webgl.png)

### 事件绑定代码

```js
canvas.onmousedown = function(ev) {

}
```



