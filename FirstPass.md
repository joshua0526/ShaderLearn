# Pass解析
## 不同的LightMode被选择的顺序
* 渲染路径  
Unity支持3中渲染路径：VertexLit,Forward,Deferred Lighting。
* LightMode标签  
Pass中的LightMode标签：Vertex，ForwardBase，ForwardAdd，PrepassBase，PrepassFinal等。
* VertexLit渲染路径下Pass执行  
只执行LightMode=Vertex的Pass。
* Forward渲染路径下Pass的执行  
如果有包含Forward路径的Pass则不会执行其他，没有的话会执行针对vertex的Pass，绝对不会执行针对Deferred渲染路径下的Pass。
* Deferred渲染路径下Pass的执行
优先Deferred，然后Forward，最后Vertex。

Unity在一个时间只会执行一个渲染路径下的Pass，它并不会将全部可执行的Pass都渲染。
## 3个渲染路径之外
* 当LightMode标签被设为Always  
只在vertex和Forward渲染模式下才会执行，deferred模式下不执行。
* 没有设置LightMode或是默认  
跟LightMode为Always的结果保持一致。