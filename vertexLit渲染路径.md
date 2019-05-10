# VertexLit渲染路径
## 顶点照明
* 什么是顶点照明  
逐顶点计算的Vertex Light就是基于物体的每一个顶点进行光的照明计算，然后根据顶点的光照信息对面片上的像素光照进行线性插值。
* 存取光源的变量
    * float4 _WorldSpaceLightPos0矢量，和它对应的是float4 _LightColor0，这是表示在世界坐标系的光源位置的矢量或者方向矢量。w为0表示平行光，w为1表示点光源。
    * float4 unity_4LightPosX0,float4 unity_4LightPosY0,float4 unity_4LightPosZ0是4个点光源的x,y,z坐标。  
    float4 unity_4LightAtten0表示4个点光源的衰减。  
    float4 unity_LightColor[4]是对应的点光源的颜色。
    * float4 unity_LightPosition[4],表示4个点光源位置或者平行光的方向矢量。
## 顶点照明和Unity存放光源的第一种方式
在LightMode=Vertex的Pass内，Unity没有提供数据到unity_4LightPos[X,Y,Z]0中。
## 顶点照明和Unity存放光源的第二种方式
在LightMode=Vertex的Pass内，Unity没有提供光源数据到_WorldSpaceLightPos0和_LightColor0中。
## 顶点照明和Unity存放光源的第三种方式
在LightMode=Vertex时，unity为unity_LightPosition[4]和unity_LightColor[4]中提供了光源数据。
* float4 unity_LightPosition[4]中存储的是视空间中的光源信息。
* float4 unity_LightPosition[4]中可以存储平行光，并且在数组中更靠前。
## 总结
* 对于LightMode=Vertex的Pass来说，有效存取光源的方法是读取unity_LightPosition[4]和unity_LightColor[4]，这两个数组可以保证在Unity的3个RenderingPath下都有效工作。  
* unity_LightPosition[4]中的点光源的位置向量和平行光的方向向量都处于视空间中。
* 使用UnityCG.cginc中的ShadeVertexLights可以完成一个只逐Vertex照明的shader。
