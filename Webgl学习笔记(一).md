# WebGL学习笔记（一）

webgl是通过JavaScript api对显卡的操作。

---

```js
void main() {
  gl_Position = vec4(0.0, 0.0, 0.0, 1.0);
  gl_PointSize = 10.0;
}
```

```js
void main() {
  gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0);
}
```

> 最简单的顶点shader与片元shader。

---

颜色缓冲区是GPU中的内存区域，对应显示器上的每一个像素点。

gl.clearColor\(\)就是用某种颜色设置这块缓冲区。

gl.clear\(gl.COLOR\__BUFFER\_BIT_\)就是用上面设置的颜色填充这块缓冲区。

---

gl.drawArrays\(gl.POINTS, 0, count\);

当执行这句代码时，顶点shader会被执行count次，然后将顶点处理结果（位置、pointsize等）送入片元着色器，片元着色器输出每个片元的颜色到帧缓冲区（对应屏幕的像素点）。

> 顶点shader被每个顶点执行一次，如果有一次性的运算放在该shader中，无疑会增加GPU很多无谓的运算。这是webgl应用的优化点之一。

---

attribute变量和uniform变量开辟在GPU内存中，用于从CPU向GPU传递数据。

> 从CPU向GPU传递数据是需要时间的。这是webgl应用的优化点之一。

例如：

```js
const gl = getWebGLContext(canvas);
// 拿到GPU中attribute变量地址
const a_Position = gl.getAttribLocation(gl.program, 'a_Position');
// 向GPU中传递数据
gl.vertexAttrib3f(a_Position, 0.5, 0.0, 0.0);
// 画点
gl.drawArrays(gl.POINTS, 0, 1);
```

---

齐次坐标（由4个分量组成的矢量）能够提高处理三维数据的效率，因而在三维图形系统中被广泛使用。

齐次坐标\(x, y, z, w\)等价于三维坐标\(x/w, y/w, z/w\)，w必须大于等于0。

齐次坐标的存在使得用矩阵乘法来描述顶点变换成为可能。

---

attribute变量：只在顶点shader中使用；片元shader中可以使用uniform、varying变量。

具体：attribute传递与顶点相关的数据，uniform传递对于所有顶点都相同（或与顶点无关）的数据。

---

顶点shader进行的是逐顶点操作，片元shader进行的是逐片元操作。

---

## 静态绘制三部曲

```js
// 1
var canvas = document.getElementById('webgl');
// 2
var gl = getWebGLContext(canvas);
// 3
initShaders(gl, VSHADER_SOURCE, FSHADER_SOURCE);
// 4
var n = initVertexBuffers(gl);
// 5
gl.clearColor(0, 0, 0, 1);
gl.clear(gl.COLOR_BUFFER_BIT);
gl.drawArrays(gl.TRIANGLE_STRIP, 0, n);
```









