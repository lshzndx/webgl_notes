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



