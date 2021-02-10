# WebGL学习笔记\(四\)

## 变换矩阵

顶点的变换（平移、旋转、缩放等）可以通过数学表达式来实现，但是借助矩阵这种数学工具能够更简便的实现。

![](/assets/translate.png)

> 平移变换矩阵

![](/assets/rotate.png)

> 旋转变换矩阵

![](/assets/transform.png)

> 缩放变换矩阵

---

## 矩阵在数组中的存储方式

![](/assets/matrix_arr.png)

> 矩阵在数组中的存储方式有两种，一种按行主序，一种按列主序。webgl中是按列主序存储的。

例如上图在数组中存储是这样的：\[a, e, i, m, b, f, j, n, c, g, k, o, d, h, l, p\]。

