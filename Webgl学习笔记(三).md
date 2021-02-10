# WebGl学习笔记（三）

## 缓冲区对象（buffer object）

GPU中的一块内存区域。可以一次性向缓冲区中对象中填充大量顶点数据，然后将这些数据保存其中，供顶点shader使用。

缓冲区可以设置多个，也可以使用一个携带非顶点数据，利用步进和偏移参数取用非顶点数据。

当非顶点数据包含颜色值时，因为顶点颜色在片元shader中，这时候就要用到varying变量，用来从顶点shader向片元shader传递数据。

varying最简单示例：

```js
// 顶点shader
const VSHADER_SOURCE = `
    attribute vec4 a_Color;
    varying vec4 v_Color;
    void main() {
        v_Color = a_Color;
    }
`
```

```js
// 片元shader
const FSHADER_SOURCE = `
    #ifdef GL_ES
    precision mediump float;
    #endif
    
    varying vec4 v_Color;
    void main() {
        gl_FragColor = v_Color;
    }
`
```

---

## 类型化数组

Int8Array、Uint8Array、Int16Array、Uint16Array、Int32Array、Uint32Array、Float32Array、Float64Array等都属于类型化数组。

类型化数组是ArrayBuffer的View。

ArrayBuffer直接在CPU内存中开辟连续的空间，用于存放底层的二进制数据。

JavaScript中调用webgl api时使用类型化数组，可直接操作底层字节数据，并直接送入显卡，省去了中间格式转化的麻烦，提高了性能。

![](/assets/typedarray.png)

> ArrayBuffer与TypedArray关系。

---

## Webgl中可以绘制的基本图形

![](/assets/types.png)

> 对应drawArrays\(\)中第一个参数mode。



