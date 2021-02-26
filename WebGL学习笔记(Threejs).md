# WebGL学习笔记\(Threejs\)

## 关于WebGLRenderer.render\(scene, camera\)

### 在每一帧的更新流程

#### 更新顶点的matrix

```js
// 更新所有顶点的matrix
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
}
```



