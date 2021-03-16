# WebGL学习笔记\(九\)功能实现篇

## HUD\(平视显示器\)

实现HUD效果的原理非常简单，只要两个&lt;canvas&gt;的两个位置重叠，浏览器自动将HUD内容和webgl内容混合起来。因为默认情况下&lt;canvas&gt;的背景色是透明的。

示例：

![](/assets/import.png)

