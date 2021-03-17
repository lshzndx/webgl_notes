# BIMjs框架优化记录

## 视觉

### 开启对数缓存

ViewerImpl.js:

```js
const renderer = new THREE.WebGLRenderer({
  antialias: true,
  // alpha: true,
  preserveDrawingBuffer: true,
  logarithmicDepthBuffer: true
});
```

### 设置设备像素比

ViewerImpl.js:

```js
renderer.setPixelRatio(window.devicePixelRatio);
```

### 使用感光材质

BaseLoader.js:

![](/assets/imporct.png)

threejs中常用的3种材质

* **MeshBasicMaterial**：基础材质，不感光。

* **MeshLambertMaterial：**兰伯特材质，适用于表面比较粗糙的物体，如纸张、墙壁等。感光。

* **MeshPhongMaterial：**冯氏材质，适用于表面比较明亮的物体，如金属、玻璃等。感光。

### 设置灯光

目前设置了环境光及4条交错的平行光。

---

## 性能

### 构件的点选

得益于数据结构的设计，构件的点选实现了O\(1\)的复杂度，比小红砖及bimface快了一个数量级\(O\(n\)\)。

原因：

![](/assets/import4.png)

### 构件的透明及隐藏

虽然从严格的复杂度意义上讲与小红砖同阶\(O\(n\)\)，但从实际的应从场景来说，仍然比小红砖快出一个数量级。

原因\(ViewerImpl.js:\)：![](/assets/impyort.png)

### 爆炸

爆炸效果耗费性能，处理不好甚至导致页面卡死。![](/assets/importb.png)

> 1. 在计算机系统中，虽然new操作与赋值操作都是O\(1\)，但new操作花费的时间成本是赋值运算的100倍。
> 2. 上面这段代码，每一块构件都会执行一遍，因此，当构件有上千块时，提升的性能是可观的。

### 射线探测

射线探测同样较为耗费性能。通常情况下，模型只占整个viewpoint的一少部分，通过屏蔽掉模型以外的不必要的射线，无疑可以提升性能。

```js
// mouse 是否在模型的包围盒内,包围盒以外不进行探测
const {models} = this.viewImpl.modelManager
let _inbox = false;
for (let key in models) {
  const model = models[key]
  if (model.boundingBox) {
    if (this.isInbox({x: event.layerX, y: event.layerY}, model.boundingBox)) {
      _inbox = true;
      break;
    }
  }
}
if (!_inbox) return;
```

```js
// 鼠标是否在模型的包围盒内
isInbox(mouse, boudingBox, offset = 50) {
    const {min, max} = boudingBox;
    const canvas = this.viewImpl.three.renderer.domElement;
    const {x: x1, y: y1} = mouseToGL(mouse, canvas, window.devicePixelRatio);
    return (min.x - offset < x1 < max.x + offset) && (min.y - offset < y1 < max.y + offset)
}
```

```js
function mouseToGL(mouse, canvas, devicePixelRatio = 1) {
  let { x, y } = mouse;
  x = (x / (canvas.width / devicePixelRatio)) * 2 - 1;
  y = -(y / (canvas.height / devicePixelRatio)) * 2 + 1;
  // x = (x / window.innerWidth) * 2 - 1;
  // y = -(y / window.innerHeight) * 2 + 1;
  return { x, y };
}
```

### 静态更新场景





