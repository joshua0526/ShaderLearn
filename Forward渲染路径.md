# Forward渲染路径
## 渲染物体-ForwardBase和ForwardAdd
ForwardBase和ForwardAdd是专门为在Forward渲染路径下渲染物体而设计的两种Pass，ForwardBase会先于ForwardAdd被执行。
* VertexLit渲染模式下，Forward都不会被执行。
* LightMode为ForwardAdd都不会被单独执行，必须跟ForwardBase一起使用。
## Forward渲染路径下的重要光源
在LightMode=ForwardBase、LightMode=ForwardAdd的Pass内，_WorldSpaceLightPos0只会含有Pixel光源。在ForwardBase Pass内，只有场景中存在RenderMode为Important的Pixel平行光时，才会含有有效的_WorldSpaceLightPos0和_LightColor0的组合。
## 重要光源在ForwardAdd内的执行