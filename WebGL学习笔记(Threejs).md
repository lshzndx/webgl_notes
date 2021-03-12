# WebGL学习笔记\(Threejs\)

## 关于WebGLRenderer.render\(scene, camera\)

### 在每一帧的更新流程

#### 更新顶点的matrix

```js
// 更新所有顶点的matrixWorld
scene.updateMatrixWorld();
Object3D.updateMatrixWorld = function(force) {
    if ( this.matrixAutoUpdate ) this.updateMatrix();
    // matrixWorldNeedsUpdate可以手动设置，也可以在上一行的updateMatrix里被置为true
    if ( this.matrixWorldNeedsUpdate || force ) {

        if ( this.parent === null ) {

            this.matrixWorld.copy( this.matrix );

        } else {

            this.matrixWorld.multiplyMatrices( this.parent.matrixWorld, this.matrix );

        }

        this.matrixWorldNeedsUpdate = false;

        force = true;
    }    

    const children = this.children;

    for ( let i = 0, l = children.length; i < l; i ++ ) {

      children[ i ].updateMatrixWorld( force );

    }
}

// 将当前的position、quaternion、scale复合成matrix
Object3D.updateMatrix = function() {
    this.matrix.compose( this.position, this.quaternion, this.scale );

    this.matrixWorldNeedsUpdate = true;    
}

// 最终是将Object3D的matrixWorld送入shader
function setProgram() {
    p_uniforms.setValue( _gl, 'modelViewMatrix', object.modelViewMatrix );
    p_uniforms.setValue( _gl, 'normalMatrix', object.normalMatrix );
    p_uniforms.setValue( _gl, 'modelMatrix', object.matrixWorld );

    return program;
}
```

> matrix 为Object3D的本地坐标；matrixWorld为世界坐标。
>
> Object3D的position、quaternion、scale会复合成matrix，matrix被计算进matrixWorld，最终matrixWorld被送入shader。
>
> 从这里寻找优化点（待续 即matrixAutoUpdate）。

### 优化

关闭场景的自动更新（scene.autoUpdate = false）。

```js
WebGLRenderer.render = function(scene, camera) {
    if ( scene.autoUpdate === true ) scene.updateMatrixWorld();
}
```



