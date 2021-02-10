# WebGL学习笔记\(五\)功能实现篇

## 均匀动画的实现

应用中基于浏览器提供的api requestAnimationFrame实现基本的动画。

### requestAnimationFrame的问题

浏览器会根据自身的状态在适当时机调用这个方法。不同的浏览器及同一浏览器不同状态下调用的时间间隔不同。

实现匀速动画的代码：

```js
let g_last = Date.now();
function animate(angle) {
  let now = Date.now();
  let elapsed = now - g_last;
  
  g_last = now;
  // 核心代码就这一句。
  let newAngle = angle + (ANGLE_STEP * elapsed) / 1000.0;
  
  return newAngle %= 360;
}
```



