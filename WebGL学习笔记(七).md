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

更一般的情况下，顶点shader中的代码是这样的\(&lt;"从视点看上去的"旋转后的坐标&gt; = &lt;视图矩阵&gt;✖️&lt;模型矩阵&gt;✖️&lt;原始顶点坐标&gt;\)：

```js
const VSHADER_SOURCE = `
    uniform mat4 u_ViewMatrix;
    uniform mat4 u_ModelMatrix;
    gl_Position = u_ViewMatrix * u_ModelMatrix * a_Position;
`
```

> 这里有一个优化点。就是u\_ViewMatrix \* u\_ModelMatrix，如果放在shader中运算这两个矩阵，每一个顶点都将重复运算一遍，一个模型通常有成千上万甚至几十万个顶点，增加了GPU很多无谓的运算，更好的方式是先在JavaScript中算好，传递给shader。



