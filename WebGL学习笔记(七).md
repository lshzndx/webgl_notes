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

更一般的情况下，顶点shader中的代码是这样的\(&lt;"从视点看上去的"旋转后的顶点坐标&gt; = &lt;视图矩阵&gt;✖️&lt;模型矩阵&gt;✖️&lt;原始顶点坐标&gt;\)：

```js
const VSHADER_SOURCE = `
    uniform mat4 u_ViewMatrix;
    uniform mat4 u_ModelMatrix;
    gl_Position = u_ViewMatrix * u_ModelMatrix * a_Position;
`
```

> 这里有一个优化点。就是u\_ViewMatrix \* u\_ModelMatrix，如果放在shader中运算这两个矩阵，每一个顶点都将重复运算一遍，一个模型通常有成千上万甚至几十万个顶点，增加了GPU很多无谓的运算，更好的方式是先在JavaScript中算好，传递给shader。

---

## 关于webgl的坐标系

### 默认情况

默认情况下，webgl没有“左手”、“右手”的概念，只是按照点的顺序进行绘制，即不关系z坐标值。

### 加入裁剪坐标系

当启用隐藏面消除时（gl.enable\(gl.DEPTH\_TEST\)），就用到了裁剪坐标系。而裁剪坐标系本身是左手坐标系的。

### 加入可视空间矩阵

以正交投影（矩阵）为例：

![](/assets/impo1rt.png)

即，gl\_Position = u\_MvpMatrix \* a\_Position;

一般情况下far &gt; near，因此3行3列处的值为负，这个值代表z方向的缩放变换，至此，webgl最终变成了“右手坐标系”。

---

### 共治一炉

```js
// u_MvpMatrix * a_Position;
gl_Position = u_ProjMatrix * u_ViewMatrix * u_ModelMatrix * a_Position;
```

---

![](/assets/imp21ort.png)

