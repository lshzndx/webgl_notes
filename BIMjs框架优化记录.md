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

threejs中常用的3中材质

* **MeshBasicMaterial**：基础材质，不感光。

* **MeshLambertMaterial：**兰伯特材质，适用于表面比较粗糙的物体，如纸张、墙壁等。感光。

* **MeshPhongMaterial：**冯氏材质，适用于表面比较明亮的物体，如金属、玻璃等。感光。



