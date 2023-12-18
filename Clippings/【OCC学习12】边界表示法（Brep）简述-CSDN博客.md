# 【OCC学习12】边界表示法（Brep）简述-CSDN博客
[【OCC学习12】边界表示法（Brep）简述-CSDN博客](https://blog.csdn.net/loveoobaby/article/details/126450490?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-126450490-blog-102851005.235%5Ev38%5Epc_relevant_sort_base1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-126450490-blog-102851005.235%5Ev38%5Epc_relevant_sort_base1&utm_relevant_index=2) 

 #### 一、边界表示法简述

边界表示（[Boundary](https://so.csdn.net/so/search?q=Boundary&spm=1001.2101.3001.7020) Representation）也称为BRep表示，它是几何造型中最成熟、无二义的表示法。**实体Solid用一组封闭的面组成，而每个面又由它所在的曲面的定义加上其边界来表示，面的边界是边的并集，而边又是由点来表示的。** 

边界表示的一个重要特征是描述形体的信息包括几何信息（Geometry）和拓朴信息（Topology）两个方面。拓朴信息描述形体上的顶点、边、面的连接关系，它形成物体边界表示的“骨架”。形体的几何信息犹如附着在“骨架”上的肌肉。例如，形体的某个面位于某一个曲面上，定义这一曲面方程的数据就是几何信息。此外，边的形状、顶点在三维空间中的位置（点的坐标）等都是几何信息，一般来说，几何信息描述形体的大小、尺寸、位置和形状等。

在边界表示法中，边界表示就按照体－面－环－边－点的层次，详细记录构成形体的所有几何元素的几何信息及其相互连接的拓朴关系。这样，在进行各种运算和操作中，就可以直接取得这些信息。

下面是由两个面组成的壳(shell):

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-18%2009-27-32/a5e24a79-6dfd-4a2f-b33a-173c8b3f9b14.png?raw=true)

上图所示的形状表示为TS， 面TF1和TF2，有七条边TE1~TE7和六个顶点TV1~TV6。

环TW1引用边TE1~TE4；环TW2引用TE4~TE7 。边引用的顶点如下：TE1（TV1，TV4），TE2（TV1，TV2），TE3（TV2，TV3），TE4（TV3，TV4），TE5（TV4，TV5），TE6（TV5，TV6），TE7（TV3，TV6）。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-18%2009-27-32/358e034c-460e-4694-b02c-b3f50de6a642.png?raw=true)

####  二、BRep in OpenCascade

[OCC](https://so.csdn.net/so/search?q=OCC&spm=1001.2101.3001.7020)中拓扑对象都继承至TopoDS_Shape, 根据其派生类可推看出其数据结构。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-18%2009-27-32/be7c0f26-95fa-43d6-8f21-d72cf75d5e84.png?raw=true)

TopoDS\_Shape中包含三个成员变量：myLocation、myOrient、myTShape。其中myTShape是TopoDS\_TShape类型变量，其继承关系如下：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-18%2009-27-32/d0c1be78-fce7-4d0e-8a5e-6fca0fbdbec9.png?raw=true)

TopsDS\_TShape中存在TopoDS\_ListOfShape的列表，用于存储子TopoDS\_Shape对象。 TopsDS\_TShape中有几何数据的对象只有三种：Vertex、Edge、Face，其他类型都是这三种类型间接拼接而成。这样几何与拓扑之间就产生了关联。