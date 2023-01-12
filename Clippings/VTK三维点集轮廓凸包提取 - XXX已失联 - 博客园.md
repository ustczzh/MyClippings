# VTK三维点集轮廓凸包提取 - XXX已失联 - 博客园
[VTK三维点集轮廓凸包提取 - XXX已失联 - 博客园](https://www.cnblogs.com/21207-ihome/p/6694490.html) 

 　　碰撞检测问题在虚拟现实、计算机辅助设计与制造、游戏及机器人等领域有着广泛的应用，甚至成为关键技术。而包围盒算法是进行碰撞干涉初步检测的重要方法之一。包围盒算法是一种求解离散点集最优包围空间的方法。基本思想是用体积稍大且特性简单的几何体（称为包围盒）来近似地代替复杂的几何对象。为物体添加包围体的目的是快速的进行碰撞检测或者进行精确的碰撞检测之前进行过滤（即当包围体碰撞，才进行精确碰撞检测和处理）。包围体类型包括球体、轴对齐包围盒（AABB/Axis-aligned bounding box）、有向包围盒（OBB/Oriented bounding box）以及凸壳/凸包（Convex Hull）等。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-12%2009-02-24/6a5cae2d-aeab-4c55-8b32-cf172145b935.png?raw=true)

　　AABB是应用最早的包围盒。它被定义为包含该对象，且边平行于坐标轴的最小六面体。故描述一个AABB，仅需六个标量。AABB构造比较简单，存储空间小，但紧密性差，尤其对不规则几何形体，冗余空间很大，当对象旋转时，无法对其进行相应的旋转。

　　包围球被定义为包含该对象的最小的球体。确定包围球，首先需分别计算组成对象的基本几何元素集合中所有元素的顶点的x，y，z坐标的均值以确定包围球的球心，再由球心与三个最大值坐标所确定的点间的距离确定半径r。包围球的碰撞检测主要是比较两球间半径和与球心距离的大小。

　　OBB是较为常用的包围盒类型。它是包含该对象且相对于坐标轴方向任意的最小的长方体。OBB最大特点是它的方向的任意性，这使得它可以根据被包围对象的形状特点尽可能紧密的包围对象，但同时也使得它的相交测试变得复杂。OBB包围盒比AABB包围盒和包围球更加紧密地逼近物体，能比较显著地减少包围体的个数，从而避免了大量包围体之间的相交检测。但OBB之间的相交检测比AABB或包围球体之间的相交检测更费时。

* * *

 　　在多维空间中有一群散布各处的点，凸包是包覆这群点的所有外壳当中，表表面积容积最小的一个外壳，而最小的外壳一定是凸的。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-12%2009-02-24/edf0f337-74ac-4881-a6d7-173fbf260ea5.png?raw=true)

　　二维平面上的凸包是一个凸多边形，在所有点的外围绕一圈即得凸包。另外，最顶端、最底端、最左端、最右端的点，一定是凸包上的点。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-12%2009-02-24/9b3d1941-64ae-4416-b368-fd48caa8b322.png?raw=true)

　　在许多物理引擎或仿真软件中进行碰撞检测和接触力计算时，会需要物体具有一个能代表其碰撞属性的碰撞体（Each object must have a _Collision Shape_）。碰撞体的形状可以是其真实的三维网格，但这样会影响计算的实时性和效果。这时可以用更简单的几何形状来代表要发生碰撞或接触的物体，比如立方体、球体等。下面是几种碰撞体形状，从左到右为：球体、立方体、凸包、原始网格。前三种包围体和原始网格相比显得不那么精确，但是计算效率更高。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-12%2009-02-24/0bb40fdb-6946-48ef-ba6d-817f80af70ab.png?raw=true)

　　VTK提取凸包使用类vtkPointsProjectedHull，该类可以获取任意点集的最小凸包。输入为点集，输出为包围该点集的最小凸包轮廓点集。下面代码以原点为球心随机生成40个三维点，然后提取凸包并投影到Y-Z平面上（projection along the x axis）

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-12%2009-02-24/ded38a1b-42a6-4262-8de1-04f085b33376.gif?raw=true)

import vtk
 
pointSource = vtk.vtkPointSource() # By default location of the points is random within the sphere
pointSource.SetNumberOfPoints(40)
pointSource.Update() #Create a mapper and actor
mapper = vtk.vtkPolyDataMapper()
mapper.SetInputConnection(pointSource.GetOutputPort())
pointActor = vtk.vtkActor()
pointActor.SetMapper(mapper) # change the size of actor's points
pointActor.GetProperty().SetPointSize(5) 

points = vtk.vtkPointsProjectedHull()
points.DeepCopy(pointSource.GetOutput().GetPoints()) # Returns the number of points in the convex hull of the projection of the points down the positive x-axis 
xSize = points.GetSizeCCWHullX() print "xSize: " , xSize
 
pts = 2 * xSize * \[0\] # Returns the coordinates (y,z) of the points in the convex hull of the projection of the points down the positive x-axis
points.GetCCWHullX(pts, xSize)
 
xHullPoints = vtk.vtkPoints() for i in range(xSize):
    yval = pts\[2*i\]
    zval = pts\[2*i + 1\] print "(y,z) value of point " , i , " : (" , yval, " , " , zval , ")" xHullPoints.InsertNextPoint(0.0, yval, zval) # Insert the first point again to close the loop
xHullPoints.InsertNextPoint(0.0, pts\[0\], pts\[1\]) # Display the x hull
xPolyLine = vtk.vtkPolyLine()
xPolyLine.GetPointIds().SetNumberOfIds(xHullPoints.GetNumberOfPoints()) for i in range(xHullPoints.GetNumberOfPoints()): 
    xPolyLine.GetPointIds().SetId(i, i) # Create a cell array to store the lines in and add the lines to it
cells = vtk.vtkCellArray()
cells.InsertNextCell(xPolyLine) # Create a polydata to store everything in
polyData = vtk.vtkPolyData() # Add the points to the dataset
polyData.SetPoints(xHullPoints) # Add the lines to the dataset
polyData.SetLines(cells) # Setup actor and mapper
xHullMapper = vtk.vtkPolyDataMapper()
xHullMapper.SetInputData(polyData)

xHullActor = vtk.vtkActor()
xHullActor.SetMapper(xHullMapper) #Create a renderer, render window, and interactor
renderer = vtk.vtkRenderer()
renderWindow = vtk.vtkRenderWindow()
renderWindow.AddRenderer(renderer)
renderWindowInteractor = vtk.vtkRenderWindowInteractor()
renderWindowInteractor.SetRenderWindow(renderWindow) # Add the actor to the scene
renderer.AddActor(xHullActor)
renderer.AddActor(pointActor)
axesActor = vtk.vtkAxesActor() 
axesActor.SetConeRadius(0)
renderer.AddActor(axesActor) # Rotate camera
renderer.GetActiveCamera().ParallelProjectionOn()
renderer.GetActiveCamera().Azimuth(90)
renderer.ResetCamera() # Render and interact
renderWindow.Render()
style = vtk.vtkInteractorStyleTrackballCamera()
renderWindowInteractor.SetInteractorStyle(style)
renderWindowInteractor.Start()

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-12%2009-02-24/0fd81a5b-2b54-4a90-bd12-ab5b1dc7239f.gif?raw=true)

输出结果如下：

xSize: 12  
(y,z) value of point 0 : ( -0.0582492426038 , -0.422939538956 )  
(y,z) value of point 1 : ( 0.105404652655 , -0.416797012091 )  
(y,z) value of point 2 : ( 0.384681999683 , -0.139498367906 )  
(y,z) value of point 3 : ( 0.403645426035 , -0.0483181998134 )  
(y,z) value of point 4 : ( 0.312736421824 , 0.160078570247 )  
(y,z) value of point 5 : ( 0.0731690451503 , 0.45936870575 )  
(y,z) value of point 6 : ( 0.0130856623873 , 0.45393627882 )  
(y,z) value of point 7 : ( -0.090303093195 , 0.434131532907 )  
(y,z) value of point 8 : ( -0.292154729366 , 0.340970009565 )  
(y,z) value of point 9 : ( -0.374829471111 , -0.0632947012782 )  
(y,z) value of point 10 : ( -0.348517596722 , -0.259341210127 )  
(y,z) value of point 11 : ( -0.256385087967 , -0.352375864983 )

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-12%2009-02-24/ae42c51d-9397-46e3-a0ac-4ae826d9c96b.png?raw=true)

参考：

[VTK/Examples/Cxx/PolyData/PointsProjectedHull](http://www.vtk.org/Wiki/VTK/Examples/Cxx/PolyData/vtkPointsProjectedHull)

[方向包围盒(OBB)碰撞检测](http://www.cnblogs.com/iamzhanglei/archive/2012/06/07/2539751.html)

[AABB包围盒算法,在2D碰撞检测中的实现](https://segmentfault.com/a/1190000006802081)

[计算几何之凸包(一) {卷包裹算法}](http://www.cnblogs.com/Booble/archive/2011/02/28/1967179.html)

[3D碰撞检测](http://www.cnblogs.com/xiangshancuizhu/p/3679004.html)

[Picking with a physics library](http://www.opengl-tutorial.org/miscellaneous/clicking-on-objects/picking-with-a-physics-library/)

[Quickhull](https://en.wikipedia.org/wiki/Quickhull)

[Video Game Physics Tutorial - Part I: An Introduction to Rigid Body Dynamics](https://www.toptal.com/game/video-game-physics-part-i-an-introduction-to-rigid-body-dynamics)

[Video Game Physics Tutorial - Part II: Collision Detection for Solid Objects](https://www.toptal.com/game/video-game-physics-part-ii-collision-detection-for-solid-objects)

[Video Game Physics Tutorial - Part III: Constrained Rigid Body Simulation](https://www.toptal.com/game/video-game-physics-part-iii-constrained-rigid-body-simulation)