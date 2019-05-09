# ShaderLearn
* [常用函数](##-常用函数)
* [语义](##-语义)
* [变量](##-变量)
* [摄像机和屏幕参数](##-摄像机和屏幕参数)
* [光照的函数和变量](#-光照的函数和变量)
## 常用函数
* [float3 WorldSpaceViewDir(float4 v)](###-float3-WorldSpaceViewDir(float4-v))
* [float3 ObjSpaceViewDir(float4 v)](###-float3-ObjSpaceViewDir(float4-v))
* [float3 WorldSpaceLightDir(float4 v)](###-float3-WorldSpaceLightDir(float4-v))
* [float3 UnityWorldSpaceLightDir(float4 v)](###-float3-UnityWorldSpaceLightDir(float4-v))
* [float3 ObjSpaceLightDir(float4 v)](###-float3-ObjSpaceLightDir(float4-v))
* [float3 UnityObjectToWorldNormal(float3 norm)](###-float3-UnityObjectToWorldNormal(float3-norm))
* [float3 UnityObjectToWorldDir(in float3 dir)](###-float3-UnityObjectToWorldDir(in-float3-dir))
* [float3 UnityWorldToObjectDir(float3 dir)](###-float3-UnityWorldToObjectDir(float3-dir))

### float3 WorldSpaceViewDir(float4 v)
输入一个模型空间（本地坐标系）中的顶点位置，返回世界空间（世界坐标系）中从该点到摄像机的观察方向。
### float3 ObjSpaceViewDir(float4 v)
输入一个模型空间中的顶点位置，返回模型空间中从该点到摄像机的观察方向。
### float3 WorldSpaceLightDir(float4 v)
仅用于前向渲染（ForwardBase），输入一个模型空间中的顶点位置，返回世界空间中从该点到光源的光照方向，没有被归一化。
### float3 UnityWorldSpaceLightDir(float4 v)
仅可用于前向渲染中，输入一个世界空间的顶点位置，返回世界空间从该点到光源的光照方向，没有被归一化。
### float3 ObjSpaceLightDir(float4 v)
仅可用于前向渲染中，输入一个模型空间中的顶点位置，返回模型空间中从该点到光源的光照方向，没有被归一化。
### float3 UnityObjectToWorldNormal(float3 norm)
把法线从模型空间转换到世界空间中。
### float3 UnityObjectToWorldDir(in float3 dir)
把方向矢量从模型空间转换到世界空间中。
### float3 UnityWorldToObjectDir(float3 dir)
把方向矢量从世界空间转换到模型空间
### float3 Shader4PointLights(...)
仅可用于前向渲染中，计算四个点光源的光照，它的参数是已经打包进矢量的光照数据。前向渲染通常会使用这个函数来计算逐顶点光照。
## 语义
### position
模型空间中的顶点位置，通常是float4类型。
### normal
顶点法线，通常是float3类型。
### tangent
顶点法线，通常是float4类型。
### texcoordn
该顶点的纹理坐标，texcoord0表示第一组纹理坐标，...，通常是float2或者float4类型。
### color
顶点颜色，通常是fixed4或者float4类型。
### sv_position
裁剪空间中的顶点坐标，结构体中必须包含一个用该语义修饰的变量。
### color0-colorn
输出多组第一到n组顶点颜色。
### sv_target
输出值将会存储到渲染目标(render target)中。
## 变量
### UNITY_MATRIX_MVP
当前的模型 观察 投影矩阵，用于将顶点/方向矢量从模型空间转换到裁剪空间。
### UNITY_MATRIX_MV
当前的模型 观察矩阵，用于将顶点/方向矢量从模型空间转换到观察空间。
### UNITY_MATRIX_V
当前的观察矩阵，用于将顶点/方向矢量从世界空间转换到观察空间。
### UNITY_MATRIX_P
当前的投影矩阵，用于将顶点/方向矢量从观察空间转换到裁剪空间。
### UNITY_MATRIX_VP
当前的观察 投影矩阵，用于将顶点/方向矢量从世界空间转换到裁剪空间。
### UNITY_MATRIX_T_MV
UNITY_MATRIX_MV的转置矩阵
### UNITY_MATRIX_IT_MV
UNITY_MATRIX_MV的逆转置矩阵，用于将发现从模型空间转换到观察空间，也可以用于得到UNITY_MATRIX_MV的逆矩阵。
### unity_ObjectToWorld(_Object2World)
当前的模型矩阵，用于将顶点/方向矢量从模型空间变换到世界空间。
### unity_WorldToObject(_World2Object)
用于将顶点/方向矢量从世界空间转换到模型空间。
## 摄像机和屏幕参数
### float3 _WorldSpaceCameraPos
该摄像机在世界空间中的位置。
### float4 _projectionParams
>* x=1.0(或-1.0，如果正在使用一个反转的投影矩阵进行渲染)
>* y=Near
>* z=Far
>* w=1.0+1.0/Far

其中Near和Far分别是近裁剪平面和远裁剪平面到摄像机的距离。
### float4 _ScreenParams
>* x=width
>* y=height
>* z=1.0+1.0/width
>* w=1.0+1.0/height

其中width和height分别是该摄像机的渲染目标的像素宽度和高度。
### float4 _ZBufferParams
>* x=1-Far/Near
>* y=Far/Near
>* z=x/Far
>* w=y/Far

该变量用于线性化Z缓存中的深度值
### float4 unity_OrthoParams
>* x=width
>* y=height
>* z没有定义
>* w=1.0(正交相机)或w=0.0(透视相机)

其中width和height是正交摄像机的宽度和高度。
### float4x4 unity_CameraProjection
该摄像机的投影矩阵
### float4x4 unity_CameraInvProjection
该摄像机的投影矩阵的逆矩阵
### float4 unity_CameraWorldClipPlanes[6]
该摄像机的6个裁剪平面在世界空间下的等式，按左 右 下 上 近 远裁剪平面。
# 光照的函数和变量
* [内置的光照变量](##-内置的光照变量)
* [LightMode标签支持的渲染路径设置选项](##-LightMode标签支持的渲染路径设置选项)
* [顶点照明渲染路径中可以使用的内置变量](##-顶点照明渲染路径中可以使用的内置变量)
* [顶点照明渲染路径中可使用的内置函数](##-顶点照明渲染路径中可使用的内置函数)
## 内置的光照变量
### float4 _LightColor0
该pass处理的逐像素光源的颜色。
### float4 _WorldSpaceLightPos0
_WorldSpaceLightPos0.xyz是该pass处理的逐像素光源的位置。如果该光源是平行光，那么_WorldSpaceLightPos0.w是0，其他光源类型是1。
### float4x4 _LightMatrix0
从世界空间到光源空间的变换矩阵，可以用于采样cookie和光强衰减纹理。
### float4 unity_4LightPosX0,unity_4LightPosY0,unity_4LightPosZ0
仅用于BasePass，前4个非重要的点光源在世界空间中的位置。
### float4 unity_4LightAtten()
仅用于BasePass，存储了前4个非重要的点光源的衰减因子。
### half4[4] unity_LightColor
仅用于BasePass，存储了前4个非重要的点光源的颜色。
## LightMode标签支持的渲染路径设置选项
### Always
不管使用哪种渲染路径，该pass总会被渲染，但是不会计算任何光照。
### ForwardBase
用于前向渲染，该pass会计算环境光，最重要的平行光，逐顶点/SH光源和LightMaps
### ForwardAdd
用于前向渲染，该pass会计算额外的逐像素光源，每个pass对应一个光源。
### Deferred
用于延迟渲染，该pass会渲染G缓冲（G_buffer）。
### ShadowCaster
把物体的深度信息渲染到阴影映射纹理或一张深度纹理中。
### PrepassBase
用于遗留延迟渲染，该Pass会渲染法线和高光反射的指数部分。
### PerpassFinal
用于遗留延迟渲染，该pass通过合并纹理 光照 自发光来渲染得到的最后的颜色。
### Vertex，VertexLMRGBM和VertextLM
用于遗留的顶点照明渲染。
## 顶点照明渲染路径中可以使用的内置变量
### half4[8] unity_LightColor
光源颜色
### float4[8] unity_LightPostion
xyz分量是视角空间中的光源位置，如果光源是平行光，那么z分量值为0，其他光源类型z分量值为1。
### half4[8] unity_LightAtten
光源衰减因子，如果光源是聚光灯，x分量是cos(spotAngle/2),y分量是1/cos(spotAngle/4);如果是其他光源，x分量是-1，y分量是1，z分量是衰减的平分，w分量是光源范围开根号的结果。
### float4[8] unity_SpotDirection
如果光源是聚光灯的话，值为视角空间的聚光灯的位置，如果是其他类型的光源，值为(0,0,1,0)。
## 顶点照明渲染路径中可使用的内置函数
### float3 ShadeVertexLights(float4 vertex, float3 normal)
输入模型空间中的顶点位置和法线，计算四个顶点光源的光照以及环境光。
### float3 ShadeVertexLightsFull(float4 vertex, float3 normal, int lightCount, bool spotLight)
输入模型空间中的顶点位置和法线，计算lightCount个光源的光照以及环境光，如果SpotLight值为true，那么这些光源会被当成聚光灯来处理，虽然结果更精确，但计算更加耗时，否则，按照点光源处理。
