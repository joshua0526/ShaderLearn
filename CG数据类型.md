# CG数据类型
## CG基本数据类型
1. float ： 32 位浮点数据，一个符号位。浮点数据类型被所有的 profile 支持（但是 DirectX8 pixel profiles 在一些操作中降低了浮点数的精度和范围）； 
2. half ： 16 为浮点数据； 
3. int ： 32 位整形数据，有些 profile 会将 int 类型作为 float 类型使用； 
4. fixed ： 12 位定点数，被所有的 fragment profiles 所支持； 
5. bool： 布尔数据，通常用于 if 和条件操作符（ ?: ），布尔数据类型被所有的profiles 支持； 
6. sampler* ：纹理对象的句柄（ the handle to a texture object ），分为 6 类：sampler, sampler1D, sampler2D, sampler3D, samplerCUBE, 和 samplerRECT 。DirectX profiles 不支持 samplerRECT 类型，除此之外这些类型被所有的 pixelprofiles和 NV40 vertex program profile 所支持。 
7. string ：字符类型，该类型不被当前存在的 profile 所支持，实际上也没有必要在 Cg 程序中用到字符类型，但是你可以通过 Cg runtime API 声明该类型变量，并赋值；因此，该类型变量可以保存 Cg 文件的信息。

## 额外的数据类型
除了上面的基本数据类型外， Cg还提供了内置的向量数据类型 (built-in vector data types) ，内置的向量数据类型基于基础数据类型。例如：float4，表示 float 类型的 4 元向量；bool4，表示 bool类型 4 元向量。 
注意：向量最长不能超过 4 元，即在 Cg 程序中可以声明 float1 、 float2 、float3 、float4 类型的数组变量，但是不能声明超过 4 元的向量，例如：
> float5 array;//编译报错

向量初始化的一般方式：
> float4 array = float4(1.0,2.0,3.0,4.0);

较长的向量还可以通过较短的向量构建：
> float2 a = float2(1.0, 1.0);  
  float4 b = float4(a, 0.0, 0.0);

此外，Cg还提供矩阵数据类型，不过最大的维数不能超过4*4阶。例如：
>float1x1 matrix1;  
float2x3 matrix2;  
float4x4 matrix3;

初始化方式为：
>float2x3 matrix4 = {1.0,2.0,3.0,4.0,5.0,6.0};
## Cg数组类型
在着色程序中，数组通常的使用目的是：作为从外部应用程序传入大量参数到 Cg 的顶点程序中的形参接口，例如与皮肤形变相关的矩阵数组，或者光照参数数组等。简而言之，数组数据类型在 Cg 程序中的作用是：作为函数的形参，用于大量数据的转递。 
Cg 中声明数组变量的方式和 C 语言类似：例如：
>float a[10];  
float4 b[10];

对数组初始化：
>float a[4] = {1.0,2.0,3.0,4.0};

获取数组长度有length属性：
>float a[10];  
int length = a.length;

声明多维数组：
>float b[2][3] = {{1.0,2.0,3.0},{1.0,2.0,3.0}};

数组和矩阵有些类似，但是并不是相同。 例如 4*4 阶数组的的声明方式为：float M[4][4];4 阶矩阵的声明方式为： float4x4 M 。前者是一个数据结构，包含 16个 float 类型数据，后者是一个 4 阶矩阵数据。 float4x4 M[4] ，表示一个数组，包含 4 个 4 阶矩阵数据。进行数组变量声明时，一定要指定数组长度，除非是作为函数参数而声明的形参数组。并且在当前的 profiles 中，数组的长度和所引用的数组元素的地址必须在编译时就知道。
## Cg结构类型
Cg 语言支持结构体（ structure ），实际上 Cg 中的结构体的声明、使用和 C++非常类似（只是类似，不是相同）。一个结构体相当于一种数据类型，可以定义该类型的变量。引入结构体机制，赋予了 Cg 语言一丝面向对象的色彩。不过目前的 Cg 语言中的结构体以展现 “ 封装 ” 功能为主，并不支持继承机制。

结构体的声明以关键字 struct 开始，然后紧跟结构体的名字，接下来是一个大括号，并以分号结尾（不要忘了分号）。大括号中是结构体的定义，分为两大类：成员变量和成员函数。例如，定义一个名为 myAdd 的结构体，包含一个成员变量，和一个执行相加功能的成员函数，然后声明一个该结构体类型的变量， 
代码为：
>struct myAdd  
{  
    &emsp;float val;  
    &emsp;float add(float x)  
    &emsp;{  
        &emsp;&emsp;return val + x;  
    &emsp;}  
};  
myAdd s;
## Cg语言表达式和控制语句
### 操作符
* 关系操作符  
<,<=,!=,==,>=,>
* 逻辑操作符  
&&,||,!
* 数学操作符  
*，/，-，+，%，++，--，*=，/=，+=，-=
* 位移操作符  
<<,>>只能对int类型操作
* Swizzle操作符  
可以使用 Cg 语言中的 swizzle 操作符（ . ）将一个向量的成员取组成一个新的向量。 swizzle 操作符被 GPU 硬件高效支持。 swizzle 操作符后接 x 、 y 、 z 、 w ，分别表示原始向量的第一个、第二个、第三个、第四个元素； swizzle 操作符后接r 、 g 、 b 和 a 的含义与前者等同。不过为了程序的易读性，建议对于表示颜色值的向量，使用 swizzle 操作符后接 r 、 g 、 b 和 a 的方式。 
举例如下：
> float4(a,b,c,d).xyz等价于float3(a,b,c)

注意：swizzle操作符只能对结构体和向量使用。
* 条件操作符
> float3 h = float3(-1.0,1.0,1.0);  
float3 i = float3(1.0,0.0,0.0);  
float3 g = float3(1.0,1.0,0.0);  
float3 k;  
k = (h < float3(0.0,0.0,0.0))?(i):(g);  
k = float3(1.0,1.0,0.0);

三元向量 h 与 float3(0.0, 0.0, 0.0) 做比较运算后结果为（ true, false, false ） , 所 
以 i 的第一个数据赋值给 K 的第一个数据， g 的第二个和第三个数据赋值给 k 的第二 
个和第三个数据， K 的值为 (1.0, 1.0, 0.0) 。 

