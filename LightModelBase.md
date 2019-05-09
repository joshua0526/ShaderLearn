# LightingModel
## 光源对物体照明的分类
* 间接照明  
间接光照是光在物体之间传播后，最终又对物体形成照明。对于实时计算，间接照明一般作为一个常量。
* 直接照明  
不考虑光线在物体间的传播，也不考虑光线在物体内部的传播，光线对物体的直接照明。  
我们把照明结果分为漫反射和镜面反射两种，镜面反射会形成强烈的高光。直接照明是实时渲染时的计算重点。
## 照明的计算方式：光照模型
* 漫反射和Lambert  
如果我们用L表示单位长度的入射光线，用C表示到达此点的光线的强度和颜色，用N表示此点的法线，那么，物体表面此点的亮度Lum就可以用下面的公式来表示：   
  >Lum=C*max(0,cos<L,N>)  
<L,N>表示的是方向矢量L和N的夹角，其cos值也就是这两个方向矢量的点积，在实时计算时，通过Cg标准库dot(L,N)来完成。L和N的夹角大于180时是背光状态，cos的值为负，所以用max对结果进行控制0表示没有受到光照影响。

  上面的光照计算模型就是Lambert。Unity的Surface Shader中内置两个Lighting Model函数，分别为LightLambert()和LightingLambert_PrePass(),分别表示了Forward和Deferred渲染路径下的这种简单的照明方式。
* 镜面高光和Phong  
如果用R表示光线在此点的单位长反射方向向量，V表示视线的单位方向向量，那么高光部分Spec的计算方式表示为：
  >Spec=pow(max(0,cos<R,V>), gloss)  
<R,V>表示方向矢量R和V之间的夹角,gloss表示其表面的镜面光滑程度。  
  
  再加上前面在Lambert部分提到的对漫反射的计算，就可以得到一个被渲染的高光物体。
* 半角向量和BlinnPhong  
一种更简单，更易于调节的方法，使用入射光线和视线的中间平均值，即半角向量，然后使用此半角向量和法线计算出一个和视角相关的高光，这种高光计算即为BlinnPhong。  
Unity的Surface Shader提供了BlinnPhong的两个实现对应于Forward和Deferred渲染路径。LightingBlinnPhong()和LightingBlinnPhong_PrePass()。
