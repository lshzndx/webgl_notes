# Webgl学习笔记（二）功能实现篇

## 将鼠标点击坐标转换到webgl系统中

### 获取鼠标位置

```js
let x = ev.clientX;
let y = ev.clientY;
```

> 鼠标点击坐标是在“浏览器客户区”中的坐标，而不是canvas中坐标。

![](/assets/client.png)

