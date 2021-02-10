# WebGL学习笔记\(六\)

## 图形装配与光栅化

![](/assets/guangshanhua.png)

在顶点着色器和片元着色器之间有这两个过程：图形装配和光栅化。

片元的数目就是这个三角形显示在屏幕上所覆盖的像素数。如果将drawArrays\(mode, start, count\)中mode参数改为LINE\_LOOP，就会光栅化出一个空心的三角形（不产生中间的像素）。

对于每个片元，片元着色器计算出该片元的颜色，并写入颜色缓冲区。

> 关于varying变量的说明。顶点shader中的v\_Color变量在传递给片元shader之前，经过了内插过程，所以片元shader中的varying变量和顶点shader中的varying变量实际上并不是一回事。

除顶点之外，三角形表面上其他片元的颜色值都是内插出来的。

简单的内插示例：

![](/assets/neicha.png)

---

## 纹理

### 纹理坐标

![](/assets/texture_coord.png)



