# WebGL学习笔记\(八\)

## 光照原理

### 漫反射

![](/assets/impor221t.png)

> &lt;漫反射光颜色&gt; = &lt;入射光颜色&gt; ✖️ &lt;表面基底色&gt; ✖️ cosΘ

### 环境反射

![](/assets/impo3rt.png)

> &lt;环境反射光颜色&gt; = &lt;入射光颜色&gt; ✖️ &lt;表面基底色&gt;

### 漫反射和环境反射同时存在时

&lt;表面的反射光颜色&gt; = &lt;漫反射光颜色&gt; + &lt;环境反射光颜色&gt;

### cosΘ的计算

![](/assets/impo32rt.png)

> cosΘ = &lt;光线方向&gt; ✖️ &lt;法线方向&gt;



