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

### 原因\(ViewerImpl.js:\)：![](/assets/impyort.png)爆炸

爆炸效果耗费性能，处理不好甚至导致页面卡死。

![](/assets/importb.png)

