# WebGL学习笔记\(Threejs\)

## 关于WebGLRenderer.render\(scene, camera\)

### 在每一帧的更新流程

#### 更新顶点的matrix

```js
// 更新所有顶点的matrix
scene.updateMatrixWorld();
Object3D.updateMatrixWorld = function() {
	if ( this.matrixAutoUpdate ) this.updateMatrix();
	
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









