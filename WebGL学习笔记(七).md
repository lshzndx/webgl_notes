# WebGL学习笔记\(七\)

## 摄像机的本质

```js
const VSHADER_SOURCE = `
    uniform mat4 u_ViewMatrix;
    gl_Position = u_ViewMatrix * a_Position;
`
```



