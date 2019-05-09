# ShaderSpace
## 模型空间
模型创建的时候独属于自己的坐标空间。
## 世界空间
编辑器里，多个模型可以交互的空间。多个模型用统一的世界坐标系。
* 模型空间转换成世界空间，在unity代码中使用Transform.localToWorldMatrix。
* 在shader中使用mul(_Object2World,verts)
## 视空间
视空间，又可以叫做相机空间，是为了方便表达以相机为世界的中心时，所有物体的相互关系的一个空间。
* 世界空间转换到视空间，在unity中使用camera.worldToCameraMatrix
* Shader中可以直接从模型空间到视空间mul(UNITY_MATRIX_MV,verts)
## 视锥体
相机视角本身是锥形，设定了视空间的Near和Far，确定视空间的可视范围。
专业术语叫做Culling。
## 裁剪空间
* 投影：我们要把视锥体变换为Far为底长方体，这次变换vertex Shader任务结束。引擎会自动处理后面的事情，把坐标转换到NDC（Normalized Device Coordinates），OpenGL的值域是（-1，-1，-1）到（1，1，1），Direct3D的值域(-1,0,-1)到(1,1,1)。
* 在unity脚本中，投影操作调用Camera.projectionMatrix。
* 在Shader中则是mul(UNITY_MATRIX_MVP,verts)
## NDC之后
在vertex Shader中的点被变换到了NDC空间，几何体的顶点在经过变换之后有序的被发送到GPU，然后进行几何体的组装，这时候会进行Culling操作，也就是判断三角面是否在视锥体中，再进行栅格化操作。
* 把顶点数据有序的发送到GPU。
* 做几何体的组装。
* 进行Culling操作，排除不在视锥体内的三角面。
* 栅格化操作。
* 三角面3个顶点线性插值，为每个像素产生相关的UV、法线、Z深度等信息。
* 然后执行fragment函数。
* GPU做Z测试、Blend操作。