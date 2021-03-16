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

---

### 运动物体的光照效果

法则：用法向量乘以模型矩阵的逆转置矩阵，就可以求得变换后的法向量。

### 点光源

对于点光源，如果采用逐顶点计算光照，中间的片元通过颜色差值计算出来，效果不够逼真。

而采用逐片元计算光照的方式会更加逼真。

> 逐片元计算光照方法：（1）由顶点着色器将世界坐标系下的顶点v\_Position、v\_Normal通过varying传给片元着色器；（2）片元着色器就得到了经过差值后的每个片元的v\_Position、v\_Normal（世界坐标系下）。具体计算方法：

```js
// 片元着色器
void main() {
    vec3 normal = normalize(v_Normal);
    vec3 lightDirection = normalize(u_LightPosition - v_Position);
    float nDotL = max(dot(lightDirection, normal), 0.0);
    vec3 diffuse = u_LightColor * v_Color.rgb * nDotL;
    vec3 ambient = u_AmbientLight * v_Color.rgb;
    gl_FragColor = vec4(diffuse + ambient, v_Color.a);
}
```



