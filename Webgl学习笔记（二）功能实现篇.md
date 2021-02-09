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

---

```js
function click(ev, gl, canvas, a_Position) {
  var x = ev.clientX;
  var y = ev.clientY;
  var rect = ev.target.getBoundingClientRect();

  x = ((x - rect.left) - canvas.width / 2) / (canvas.width / 2);
  y = (canvas.height / 2 - (y - rect.top)) / (canvas.height / 2);

  g_points.push(x, y);


  /**
  *  注掉这句代码后，鼠标点击后背景变为了透明。原因是绘制完点后，颜色缓冲区被webgl系统重置
  *  成了默认的(0.0, 0.0, 0.0, 0.0)。
  *  所以这句代码可理解为“填充背景色”。
  */
  gl.clear(gl.COLOR_BUFFER_BIT);

  var len = g_points.length;
  for(let i = 0; i < len; i += 2) {
    gl.vertexAttrib3f(a_Position, g_points[i], g_points[i + 1], 0.0);

    gl.drawArrays(gl.POINTS, 0, 1);
  }

}
```



