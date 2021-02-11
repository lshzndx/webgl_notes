# WebGL学习笔记\(七\)

## 摄像机的本质

```js
const VSHADER_SOURCE = `
    uniform mat4 u_ViewMatrix;
    // 从这句代码就能看出来，改变摄像机状态，就是顶点的变换。
    gl_Position = u_ViewMatrix * a_Position;
`
```

> 摄像机的本质是一个坐标系，将所有的顶点映射到这个坐标系下。因此，改变摄像机的状态（平移、旋转等）与改变“整个世界的状态”，是等价的。



