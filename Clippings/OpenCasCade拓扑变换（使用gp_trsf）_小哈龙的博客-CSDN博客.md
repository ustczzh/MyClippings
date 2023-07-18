# OpenCasCade拓扑变换（使用gp_trsf）_小哈龙的博客-CSDN博客
[(3条消息) OpenCasCade拓扑变换（使用gp_trsf）_小哈龙的博客-CSDN博客](https://blog.csdn.net/qq_22642239/article/details/102608607) 

 **在OCC中如果要实现一个拓扑（TopoDS\_Shape）的变换（平移，（点，轴，面）镜像，旋转，缩放，移位，形状），那么gp\_trsf，gp_GTrsf是一个很好的媒介。深入理解其中的含义，可以组合起来使用，实现复杂的拓扑变换功能，要想熟练使用，必须在实践中体会**

**在此使用到的几何知识可以参考：** [几何变换之仿射变换](https://blog.csdn.net/qq_22642239/article/details/102612207)

在OpenCaseCade6.8.0源代码中有介绍：

**gp_trsf（平移，（点，轴，面）镜像，旋转，缩放，移位）**

//! Geometric transformation on a [shape](https://so.csdn.net/so/search?q=shape&spm=1001.2101.3001.7020).  
//! The transformation to be applied is defined as a  
//! gp_Trsf transformation, i.e. a transformation which does  
//! not modify the underlying geometry of shapes.  
//! The transformation is applied to:  
//! \-   all curves which support edges of a shape, and  
//! \-   all surfaces which support its faces.  
//! A Transform object provides a framework for:  
//! \-   defining the geometric transformation to be applied,  
//! \-   implementing the transformation algorithm, and  
//! \-   consulting the results.

谷歌翻译：

在3D空间中定义非持久变换。 实现了以下转换：。 平移，旋转，缩放。 关于点，线，平面的对称性。 复杂的转换可以通过使用矩阵“乘”方法组合先前的基本转换来获得。 转换可以表示如下：  
//！ V1 V2 V3         T          XYZ      XYZ  
//！ | a11 a12 a13 a14 |     | x |        | x'|  
//！ | a21 a22 a23 a24 |     | y |        | y'|  
//！ | a31 a32 a33 a34 |     | z |   =   | z'|  
//！ | 0 0 0 1 |                     | 1 |        | 1 |  
//！ 其中{V1，V2，V3}定义转换的矢量部分，T定义转换的平移部分。 这种转换永远不会改变对象的性质。

**gp_GTrsf（形状变换）**

//! Defines a non-persistent transformation in 3D space.  
//! This transformation is a general transformation.  
//! It can be a Trsf from gp, an affinity, or you can define  
//! your own transformation giving the matrix of transformation.  
//!  
//! With a Gtrsf you can transform only a triplet of coordinates  
//! XYZ. It is not possible to transform other geometric objects  
//! because these transformations can change the nature of non-  
//! elementary geometric objects.  
//! The transformation GTrsf can be represented as follow :  
//!  
//! V1   V2   V3       T          XYZ      XYZ  
//! | a11  a12  a13   a14 |    | x |       | x'|  
//! | a21  a22  a23   a24 |    | y |       | y'|  
//! | a31  a32  a33   a34 |    | z |   =  | z'|  
//! |  0    0    0     1  |            | 1 |       | 1 |  
//!  
//! where {V1, V2, V3} define the vectorial part of the  
//! transformation and T defines the translation part of the  
//! transformation.  
//! Warning  
//! A GTrsf transformation is only applicable to  
//! coordinates. Be careful if you apply such a  
//! transformation to all points of a geometric object, as  
//! this can change the nature of the object and thus  
//! render it incoherent!  
//! Typically, a circle is transformed into an ellipse by an  
//! affinity transformation. To avoid modifying the nature of  
//! an object, use a gp_Trsf transformation instead, as  
//! objects of this class respect the nature of geometric objects.

谷歌翻译：

在3D空间中定义非持久变换。此转换是一般转换。它可以是来自gp的Trsf，相似性，也可以定义自己的转换以提供转换矩阵。使用Gtrsf，您只能变换坐标XYZ的三元组。无法变换其他几何对象，因为这些变换可以改变非基本几何对象的性质。转换GTrsf可以表示为：  
//！ V1 V2 V3         T           XYZ    XYZ  
//！ | a11 a12 a13 a14 |      | x |      | x'|  
//！ | a21 a22 a23 a24 |      | y |      | y'|  
//！ | a31 a32 a33 a34 |      | z | =   | z'|  
//！ | 0 0 0 1 |                      | 1 |      | 1 |  
//！  
 其中{V1，V2，V3}定义转换的矢量部分，T定义转换的平移部分。  
 警告  
 GTrsf转换仅适用于坐标。如果将这样的变换应用于几何对象的所有点，请小心，因为这会改变对象的性质，从而使其变得不连贯！  
通常，通过亲和力转换将圆转换为椭圆。为避免修改对象的性质，请改用gp_Trsf转换，因为此类的对象尊重几何对象的性质。

**下面介绍参考自网络资料，稍有改动，Occ6.8.0版，其它版本可能会有稍许变动。** 

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-7-18%2008-51-11/e0212199-9bd5-440c-9fc2-38630fadefc0.png?raw=true)

**1.BRepBuilderAPI_Transform**
------------------------------

**（类，Occ标准头文件BRepBuilderAPI_Transform.hxx）**

### **(1)**功能说明：拓扑变换

此对象与gp_Trsf相关联进行变换

### **(2)**构造函数：

```null
BRepBuilderAPI_Transform(const gp_Trsf& T);BRepBuilderAPI_Transform(const TopoDS_Shape& S, const gp_Trsf& T, const Standard_Boolean Copy = Standard_False);
```

### **(3)**参数说明：

T：要进行的变换

S：进行变换的拓扑图形

Copy：是否用副本进行变换

### **(4)**备注：

Perform是该对象的一个方法。

```null
BRepBuilderAPI_Transform(gp_Trsf T) 与public void Perform(TopoDS_Shape S, bool Copy)BRepBuilderAPI_Transform(TopoDS_Shape S, gp_Trsf T, bool Copy)。
```

### **(5)**实例：

**例1：点对称**

 OCC源码：

```null
Makes the transformation into a symmetrical transformation.void SetMirror (const gp_Pnt& P) ;
```

  使用示例：

```null
gp_Trsf theTransformation = new gp_Trsf();gp_Pnt PntCenterOfTheTransformation = new gp_Pnt(110, 60, 60);theTransformation.SetMirror(PntCenterOfTheTransformation);BRepBuilderAPI_Transform myBRepTransformation =new BRepBuilderAPI_Transform(S,theTransformation, false);TopoDS_Shape S2 = myBRepTransformation.Shape();  
```

**例2：轴对称**

OCC源码：

```null
Standard_EXPORT   void SetMirror (const gp_Ax1& A1) ;
```

使用实例：

```null
gp_Trsf theTransformation = new gp_Trsf();gp_Ax1 axe = new gp_Ax1(new gp_Pnt(110, 60, 60), new gp_Dir(0.0, 1.0, 0.0));                   theTransformation.SetMirror(axe);BRepBuilderAPI_Transform myBRepTransformation =new BRepBuilderAPI_Transform(S, theTransformation, false);TopoDS_Shape S2 = myBRepTransformation.Shape(); 
```

**例3：面对称**

OCC源代码：

```null
Standard_EXPORT   void SetMirror (const gp_Ax2& A2) ;
```

使用示例：

```null
TopoDS_Shape S = new BRepPrimAPI_MakeWedge(60.0, 100.0, 80.0, 20.0).Shape();        gp_Trsf theTransformation = new OCgp_Trsf();gp_Ax2 axe2 = new gp_Ax2(new gp_Pnt(0, 0, 0), new gp_Dir(1, 0, 0));theTransformation.SetMirror(axe2);BRepBuilderAPI_Transform myBRepTransformation =new BRepBuilderAPI_Transform(S, theTransformation, false);TopoDS_Shape S2 = myBRepTransformation.Shape();  
```

**关于面对称，需要说明一下：** 

gp_Ax1（3D空间中的一个轴）

gp_Ax2（右手坐标系）

gp_Ax3（左手坐标系）

此示例中使用的是gp_Ax2右手坐标系，面对称会以该定义的坐标系中x轴y轴所构成的面作为对称的面来转换。

**例4：旋转变换**

OCC源代码：

```null
Standard_EXPORT   void SetRotation (const gp_Ax1& A1, const Standard_Real Ang) ;Standard_EXPORT   void SetRotation (const gp_Quaternion& R) ;
```

使用示例：

```null
gp_Trsf theTransformation = new gp_Trsf();gp_Ax1 axe = new gp_Ax1(new gp_Pnt(200, 60, 60), new gp_Dir(0.0, 1.0, 0.0));theTransformation.SetRotation(axe, 30 * System.Math.PI / 180); BRepBuilderAPI_Transform myBRepTransformation =new BRepBuilderAPI_Transform(S, theTransformation, false);TopoDS_Shape S2 = myBRepTransformation.Shape();
```

**例5：缩放变换**

OCC源代码：

```null
Standard_EXPORT   void SetScale (const gp_Pnt& P, const Standard_Real S) ;
```

使用示例：

```null
gp_Trsf theTransformation = new gp_Trsf();gp_Pnt theCenterOfScale = new gp_Pnt(100, 60, 60);  theTransformation.SetScale(theCenterOfScale, 0.3);  BRepBuilderAPI_Transform myBRepTransformation =new BRepBuilderAPI_Transform(S, theTransformation, false);TopoDS_Shape S2 = myBRepTransformation.Shape();
```

**例6：平移变换**

OCC源代码：

```null
void SetTranslation (const gp_Vec& V) ;void SetTranslation (const gp_Pnt& P1, const gp_Pnt& P2) ;
```

使用示例：

```null
gp_Trsf theTransformation = new gp_Trsf();gp_Vec theVectorOfTranslation = new gp_Vec(-6, -6, 6);  theTransformation.SetTranslation(theVectorOfTranslation);BRepBuilderAPI_Transform myBRepTransformation =new BRepBuilderAPI_Transform(S, theTransformation, false);TopoDS_Shape S2 = myBRepTransformation.Shape();
```

**例7：移位（Displacement）变换**

OCC源代码：

```null
Standard_EXPORT   void SetDisplacement (const gp_Ax3& FromSystem1, const gp_Ax3& ToSystem2) ;
```

使用示例：  
 

```null
gp_Trsf theTransformation = new gp_Trsf();gp_Ax3 ax3_1 = new gp_Ax3(new OCgp_Pnt(0, 0, 0), new gp_Dir(0, 0, 1));  gp_Ax3 ax3_2 = new OCgp_Ax3(new gp_Pnt(60, 60, 60), new gp_Dir(1, 1, 1));theTransformation.SetDisplacement(ax3_1, ax3_2);BRepBuilderAPI_Transform myBRepTransformation =new BRepBuilderAPI_Transform(S, theTransformation, false);TopoDS_Shape TransformedShape = myBRepTransformation.Shape();
```

**例8：组合变换**
-----------

**功能：可以将上述多个变换组合在一起，组合方式即：每一个gp_Trsf对象相乘即可（OCC提供对应方法与重载的*运算符）**

**OCC源码：** 

```null
gp_Trsf Multiplied (const gp_Trsf& T)  const;   //两个变换gp_Trsf相乘的函数gp_Trsf operator * (const gp_Trsf& T)  const     //重载的*运算符，可以直接使用两个对象相乘//! Computes the transformation composed with <me> and T.Standard_EXPORT   void Multiply (const gp_Trsf& T) ;  //如注释<me> = <me> * Tvoid operator *= (const gp_Trsf& T)                   //重载的*=  运算符
```

**使用示例：** 

假如要实现旋转加平移，直接将旋转的**gp_Trsf**对象与平移的**gp_Trsf**对象相乘即可其它操作类似，虽然是相乘但是实际是OCC内部对转换矩阵平移部分与旋转部分做了矩阵运算，构成了一个新的变换矩阵，将旋转部分与平移部分结合到了一起，详细矩阵变化请查阅文章开头仿射变换 文章介绍的各种图形变换的转换矩阵规则与运算。

**2.BRepBuilderAPI_GTransform**
-------------------------------

（类，Occ标准头文件BRepBuilderAPI_GTransform.hxx）

### **(1)**功能说明：拓扑变换

此对象与gp_GTrsf相关联进行变换

### **(2)**构造函数：

```null
Standard_EXPORT BRepBuilderAPI_GTransform(const gp_GTrsf& T);Standard_EXPORT BRepBuilderAPI_GTransform(const TopoDS_Shape& S, const gp_GTrsf& T, const Standard_Boolean Copy = Standard_False);
```

### **(3)**参数说明：

T：要进行的变换

S：进行变换的拓扑图形

Copy：是否用副本进行变换

### **(4)**实例：

**例1：变形（deform）变换**

OCC源代码：

```null
void SetVectorialPart (const gp_Mat& Matrix) ;Standard_EXPORT   void SetTranslationPart (const gp_XYZ& Coord) ;
```

使用示例：

```null
gp_GTrsf theTransformation = new gp_GTrsf();gp_Mat rot = new gp_Mat(1, 0, 0, 0, 0.5, 0, 0, 0, 1.5);  theTransformation.SetVectorialPart(rot);theTransformation.SetTranslationPart(new gp_XYZ(5, 5, 5)); BRepBuilderAPI_GTransform myBRepTransformation =new BRepBuilderAPI_GTransform(S, theTransformation, false);TopoDS_Shape S2 = myBRepTransformation.Shape(); 
```

参考资料：

[http://www.cppblog.com/eryar/archive/2015/01/22/209612.html](http://www.cppblog.com/eryar/archive/2015/01/22/209612.html)

网络文档资料