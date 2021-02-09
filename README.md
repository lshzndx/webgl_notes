# Webgl学习笔记（一）

webgl是通过JavaScript api对显卡的操作。

---

颜色缓冲区是GPU中的内存区域，对应显示器上的每一个像素点。

gl.clearColor\(\)就是用某种颜色设置这块缓冲区。

gl.clear\(gl.COLOR\__BUFFER\_BIT_\)就是用上面设置的颜色填充这块缓冲区。

---

gl.drawArrays\(gl.POINTS, 0, count\);

当执行这句代码时，顶点shader会被执行count次，然后将顶点处理结果（位置、pointsize等）送入片元着色器，片元着色器输出每个片元的颜色到帧缓冲区（对应屏幕的像素点）。

---

attribute变量和uniform变量开辟在GPU内存中，用于从CPU向GPU传递数据。

从CPU向GPU传递数据是需要时间的。

---



