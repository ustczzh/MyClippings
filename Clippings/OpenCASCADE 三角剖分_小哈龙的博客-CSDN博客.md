# OpenCASCADE 三角剖分_小哈龙的博客-CSDN博客
[(3条消息) OpenCASCADE 三角剖分_小哈龙的博客-CSDN博客](https://study-life.blog.csdn.net/article/details/131223528) 

 本文转载自：[https://www.cnblogs.com/opencascade/p/IncementalMesh.html](https://www.cnblogs.com/opencascade/p/IncementalMesh.html "https://www.cnblogs.com/opencascade/p/IncementalMesh.html")

感谢原作者分享，对自己帮助很大

eryar@163.com

Abstract. [OpenCASCADE](https://so.csdn.net/so/search?q=OpenCASCADE&spm=1001.2101.3001.7020) IncrementalMesh is used to build the mesh of a shape with respect of their correctly triangulated parts. The blog focus on the deflection control of the algorithm.

Key Words. [Mesh](https://so.csdn.net/so/search?q=Mesh&spm=1001.2101.3001.7020), Visualization

### 1. Introduction

Mesh是生成三维模型显示数据的关键算法。OpenCASCADE的TKMesh提供了网格剖分算法，用于生成BREP体的显示数据。原来的一些文章对网格剖分的算法及其用法进行过说明，本文主要对网格剖分的核心部分进行深入挖掘，理解其剖分精度控制原理。感兴趣的读者可以结合源码，学习其实现方法。当理解其算法原理后，也可以自己实现一套结合实际需求的高性能网格剖分库。

网格剖分的主要流程如下：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-7-18%2013-15-28/f2ca5258-2be6-4dde-8a77-95cc39d92170.png?raw=true)

遍历TopoDS\_Shape的TopoDS\_Face，对于每个TopoDS\_Face，遍历其TopoDS\_Wire，对于每个TopoDS\_Wire遍历其TopoDS\_Edge，在根据Edge和Face得到PCurve。因为TopoDS_Wire是闭合的，所以[Wire](https://so.csdn.net/so/search?q=Wire&spm=1001.2101.3001.7020)的PCurve是在参数空间闭合区域。对PCurve围成的参数区域进行三角剖分，将三角剖分的结果映射到三维空间，最终生成每个Face的网格剖分。这个流程很好理解，但是如何对网格剖分的质量进行控制呢？即用相对少的三角网格来更好地表示三维模型呢？

### 2. Mesh Deflection Control

OpenCASCADE对BRep体进行三角剖分网格化的类是BRepMesh_IncrementalMesh，此类有两个主要的选项来控制三角网格化：线性偏差Linear deflection和角度偏差Angular deflection。

三角网格剖分第一步是将所有的边Edge进行离散，即根据一定的精度生成多段线；

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-7-18%2013-15-28/755371ca-c9e1-41a3-a612-7de599802b4a.png?raw=true)

线性偏差限制离散的多段线与曲线之间的距离；角度偏差限制每段线段端部切线的夹角。

第二步是对面进行三角剖分。线性偏差也限制离散的三角形中点到曲线的距离。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-7-18%2013-15-28/9864875c-7385-4684-b745-0fa34fb6809b.png?raw=true)

应用程序应该提供适当的偏差参数以达到生成满意的三角网格。角度偏差Angular deflection比较简单且允许使用一个默认值（12~20度）。线性偏差Linear deflection有绝对的含义，需要由程序来给定正确的值。给一个很小的线性偏差值会导致网格剖分过密，消耗大量内存及影响显示效率；但是值太大得到的网格效果就是显示失真。所以对于LOD的网格来说，需要根据模型尺寸来设置相应的线性偏差值。

上面对网格剖分的参数设置进行了介绍，下面对网格剖分的实现原理进行说明。因为曲线曲面是三维的，而对曲面进行剖分的底层三角剖功能是个二维三角剖分库，所以网格剖分总的思路是对曲线在二维参数空间进行剖分，将参数空间剖分的结果通过曲面参数方程映射回到三维空间。通过对pcurve围成的参数空间闭合区域进行二维三角剖分，即可对三维曲面进行剖分。类BRepMesh\_FastDiscretFace是对每个TopoDS\_Face进行离散，其中函数control()是用来控制生成网格的质量的。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-7-18%2013-15-28/49165758-5ad6-4387-819b-7e9f1cc1652b.png?raw=true)

最多迭代次数是11次。在每一次迭代过程中，检查生成的所有三角形在参数空间中心点处与曲面的距离是否满足线性偏差，如果不满足，则插入新的点以便下次迭代。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-7-18%2013-15-28/b0ac0730-0bf9-482d-aa2a-9367a531a14d.png?raw=true)

从上面的代码可以看出其实现思路与其类名Incremental还是很贴切的，即增量法。

### 3. Conclusion

OpenCASCADE的网格剖分中网格质量控制是相对重要的核心功能。在理解其原理后，可以自己实现一个更清晰的网格剖分库。

2018年就过结束了，这一年收获颇丰，其中最大的收获就是有了自己的小宝宝。

分享创建价值。虽然OpenCASCADE不是完美的，但是她是目前世界上唯一一款功能相对完善的开放的几何造型库。OpenCASCADE的开放分享，给她带来生机。当他人因为我的blog的分享的文章或代码联系我时，他们的一声感谢，我都会觉得很高兴。

2019年马上就要到来，希望大家在新的一年里，创造、创新，突破自我，更上一层楼！