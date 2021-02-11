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

### 将纹理图像粘贴在几何图形上

![](/assets/map_texture.png)

---

## 最简单的使用代码

### 单幅纹理

```js
const VSHADER_SOURCE = `
  attribute vec4 a_Position;
  attribute vec2 a_TexCoord;
  varying vec2 v_TexCoord;
  void main() {
    gl_Position = a_Position;
    v_TexCoord = a_TexCoord;
  }
`

const FSHADER_SOURCE = `
  #ifdef GL_ES
  precision mediump float;
  #endif

  uniform sampler2D u_Sampler;
  varying vec2 v_TexCoord;
  void main() {
    gl_FragColor = texture2D(u_Sampler, v_TexCoord);
  }
`

function main() {
  const canvas = document.getElementById('webgl')
  const gl = getWebGLContext(canvas)

  initShaders(gl, VSHADER_SOURCE, FSHADER_SOURCE)

  const n = initVertexBuffers(gl)

  gl.clearColor(0.0, 0.0, 0.0, 1.0);

  initTextures(gl, n);
}

function initVertexBuffers(gl) {
  const verticesTexCoords = new Float32Array([
    -0.5,  0.5,   0.0, 1.0,
    -0.5, -0.5,   0.0, 0.0,
    0.5,  0.5,   1.0, 1.0,
    0.5, -0.5,   1.0, 0.0,
  ])

  const n = 4;

  const vertexTexCoordBuffer = gl.createBuffer();

  gl.bindBuffer(gl.ARRAY_BUFFER, vertexTexCoordBuffer)
  gl.bufferData(gl.ARRAY_BUFFER, verticesTexCoords, gl.STATIC_DRAW);

  const FSIZE = verticesTexCoords.BYTES_PER_ELEMENT;

  const a_Position = gl.getAttribLocation(gl.program, 'a_Position');
  gl.vertexAttribPointer(a_Position, 2, gl.FLOAT, false, FSIZE * 4, 0);
  gl.enableVertexAttribArray(a_Position)

  // 将纹理坐标分配给a_TexCoord，并开启。
  const a_TexCoord = gl.getAttribLocation(gl.program, 'a_TexCoord');
  gl.vertexAttribPointer(a_TexCoord, 2, gl.FLOAT, false, FSIZE * 4, FSIZE * 2)
  gl.enableVertexAttribArray(a_TexCoord);

  return n;

}

function initTextures(gl, n) {
  // 创建一个纹理对象
  const texture = gl.createTexture();

  const u_Sampler = gl.getUniformLocation(gl.program, 'u_Sampler')

  const image = new Image();
  image.onload = function() {
    loadTexture(gl, n, texture, u_Sampler, image)
  }
  image.src = '../resources/sky.jpg';

  return true
}

function loadTexture(gl, n, texture, u_Sampler, image) {
  // 对纹理图像进行Y轴反转
  gl.pixelStorei(gl.UNPACK_FLIP_Y_WEBGL, 1);
  // 启用0号纹理单元
  gl.activeTexture(gl.TEXTURE0)
  // 绑定纹理对象
  gl.bindTexture(gl.TEXTURE_2D, texture)

  // 配置纹理参数
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.LINEAR);
  // 配置纹理图像
  gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGB, gl.RGB, gl.UNSIGNED_BYTE, image);

  // 将0号纹理单元传递给着色器中的取样器变量
  gl.uniform1i(u_Sampler, 0);

  gl.clear(gl.COLOR_BUFFER_BIT)
  gl.drawArrays(gl.TRIANGLE_STRIP, 0, n);
}
```

### 多幅纹理

```js
const FSHADER_SOURCE = `
  uniform sampler2D u_Sampler1;
  void main() {
    vec4 color1 = texture2D(u_Sampler1, v_TexCoord);
    gl_FragColor = color0 * color1;
  }
`


```















