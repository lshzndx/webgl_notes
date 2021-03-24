# WebGL学习笔记\(九\)功能实现篇

## HUD\(平视显示器\)

实现HUD效果的原理非常简单，只要两个&lt;canvas&gt;的两个位置重叠，浏览器自动将HUD内容和webgl内容混合起来。因为默认情况下&lt;canvas&gt;的背景色是透明的。

示例：

![](/assets/import.png)

---

## 雾化效果

![](/assets/im1port.png)

> 最简单的一种实现方式：线性雾化

---

## 多边形偏移基本原理

```js
  // Enable the polygon offset function
  gl.enable(gl.POLYGON_OFFSET_FILL);
  // Draw the triangles
  gl.drawArrays(gl.TRIANGLES, 0, n/2);   // The green triangle
    gl.polygonOffset(1.0, 1.0);          // Set the polygon offset
  gl.drawArrays(gl.TRIANGLES, n/2, n/2); // The yellow triangle
```



