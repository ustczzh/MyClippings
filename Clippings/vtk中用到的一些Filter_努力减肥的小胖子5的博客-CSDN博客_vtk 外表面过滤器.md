# vtk中用到的一些Filter_努力减肥的小胖子5的博客-CSDN博客_vtk 外表面过滤器
[vtk中用到的一些Filter(1)_努力减肥的小胖子5的博客-CSDN博客_vtk 外表面过滤器](https://blog.csdn.net/yuxing55555/article/details/123473761) 

 > [vtk](https://so.csdn.net/so/search?q=vtk&spm=1001.2101.3001.7020) 示例：https://kitware.github.io/vtk-examples/site/Cxx/

vtkTubeFilter
-------------

> 示例：https://kitware.github.io/vtk-examples/site/Cxx/PolyData/TubeFilter/

filter that generates tubes around lines  
在线路周围生成管道的过滤器

vtkTubeFilter is a filter that generates a tube around each input line. The tubes are made up of triangle strips and rotate around the tube with the rotation of the line normals. (If no normals are present, they are computed automatically.) The radius of the tube can be set to vary with scalar or vector value. If the radius varies with scalar value the radius is linearly adjusted. If the radius varies with vector value, a mass flux preserving variation is used. The number of sides for the tube also can be specified. You can also specify which of the sides are visible. This is useful for generating interesting striping effects. Other options include the ability to cap the tube and generate texture coordinates. Texture coordinates can be used with an associated texture map to create interesting effects such as marking the tube with stripes corresponding to length or time.  
vtkTubeFilter 是一个过滤器，它在每个输入线周围生成一个管子。管子由三角形条带组成，并随着线法线的旋转围绕管子旋转。 （如果不存在法线，则会自动计算它们。）管的半径可以设置为随标量或矢量值而变化。如果半径随标量值变化，则半径是线性调整的。如果半径随矢量值变化，则使用质量通量保持变化。也可以指定管的边数。您还可以指定哪些边是可见的。这对于生成有趣的条纹效果很有用。其他选项包括盖住管子和生成纹理坐标的能力。纹理坐标可以与关联的纹理贴图一起使用，以创建有趣的效果，例如用对应于长度或时间的条纹标记管。

This filter is typically used to create thick or dramatic lines. Another common use is to combine this filter with vtkStreamTracer to generate streamtubes.  
此滤镜通常用于创建粗线条或戏剧性线条。另一个常见的用途是将此过滤器与 vtkStreamTracer 结合以生成流管。

Warning  
The number of tube sides must be greater than 3. If you wish to use fewer sides (i.e., a ribbon), use vtkRibbonFilter.  
The input line must not have duplicate points, or normals at points that are parallel to the incoming/outgoing line segments. (Duplicate points can be removed with vtkCleanPolyData.) If a line does not meet this criteria, then that line is not tubed.  
警告  
管边的数量必须大于 3。如果您希望使用更少的边（即色带），请使用 vtkRibbonFilter。  
输入线不能有重复的点，或平行于输入/输出线段的点的法线。 （可以使用 vtkCleanPolyData 删除重复的点。）如果一条线不符合此标准，则该线不是管状的。

vtkWindowedSincPolyDataFilter
-----------------------------

> 示例：https://kitware.github.io/vtk-examples/site/Cxx/Meshes/WindowedSincPolyDataFilter

adjust point positions using a windowed sinc function interpolation kernel  
使用窗口 sinc 函数插值内核调整点位置

vtkWindowedSincPolyDataFiler adjust point coordinate using a windowed sinc function interpolation kernel. The effect is to “relax” the mesh, making the cells better shaped and the vertices more evenly distributed. Note that this filter operates the lines, polygons, and triangle strips composing an instance of vtkPolyData. Vertex or poly-vertex cells are never modified.  
vtkWindowedSincPolyDataFiler 使用窗口化 sinc 函数插值内核调整点坐标。效果是“放松”网格，使单元格的形状更好，顶点分布更均匀。请注意，此过滤器操作构成 vtkPolyData 实例的线、多边形和三角形带。顶点或多顶点单元永远不会被修改。

The algorithm proceeds as follows. For each vertex v, a topological and geometric analysis is performed to determine which vertices are connected to v, and which cells are connected to v. Then, a connectivity array is constructed for each vertex. (The connectivity array is a list of lists of vertices that directly attach to each vertex.) Next, an iteration phase begins over all vertices. For each vertex v, the coordinates of v are modified using a windowed sinc function interpolation kernel. Taubin describes this methodology is the IBM tech report RC-20404 (#90237, dated 3/12/96) “Optimal Surface Smoothing as Filter Design” G. Taubin, T. Zhang and G. Golub. (Zhang and Golub are at Stanford University).  
该算法如下进行。对于每个顶点v，进行拓扑和几何分析以确定哪些顶点连接到v，以及哪些单元连接到v。然后，为每个顶点构造一个连通性数组。 （连通性数组是直接连接到每个顶点的顶点列表的列表。）接下来，迭代阶段开始于所有顶点。对于每个顶点 v，v 的坐标使用窗口化的 sinc 函数插值内核进行修改。 Taubin 在 IBM 技术报告 RC-20404（#90237，日期为 3/12/96）“Optimal Surface Smoothing as Filter Design”G. Taubin、T. Zhang 和 G. Golub 中描述了这种方法。 （Zhang 和 Golub 在斯坦福大学）。

This report discusses using standard signal processing low-pass filters (in particular windowed sinc functions) to smooth polyhedra. The transfer functions of the low-pass filters are approximated by Chebyshev polynomials. This facilitates applying the filters in an iterative diffusion process (as opposed to a kernel convolution). The more smoothing iterations applied, the higher the degree of polynomial approximating the low-pass filter transfer function. Each smoothing iteration, therefore, applies the next higher term of the Chebyshev filter approximation to the polyhedron. This decoupling of the filter into an iteratively applied polynomial is possible since the Chebyshev polynomials are orthogonal, i.e. increasing the order of the approximation to the filter transfer function does not alter the previously calculated coefficients for the low order terms.  
本报告讨论了使用标准信号处理低通滤波器（特别是加窗 sinc 函数）来平滑多面体。低通滤波器的传递函数由切比雪夫多项式近似。这有助于在迭代扩散过程中应用过滤器（与核卷积相反）。应用的平滑迭代越多，多项式逼近低通滤波器传递函数的程度就越高。因此，每次平滑迭代将切比雪夫滤波器近似的下一个较高项应用于多面体。由于切比雪夫多项式是正交的，因此可以将滤波器解耦为迭代应用的多项式，即增加滤波器传递函数的近似阶不会改变先前计算的低阶项的系数。

Note: Care must be taken to avoid smoothing with too few iterations. A Chebyshev approximation with too few terms is an poor approximation. The first few smoothing iterations represent a severe scaling and translation of the data. Subsequent iterations cause the smoothed polyhedron to converge to the true location and scale of the object. We have attempted to protect against this by automatically adjusting the filter, effectively widening the pass band. This adjustment is only possible if the number of iterations is greater than 1. Note that this sacrifices some degree of smoothing for model integrity. For those interested, the filter is adjusted by searching for a value sigma such that the actual pass band is k\_pb + sigma and such that the filter transfer function evaluates to unity at k\_pb, i.e. f(k_pb) = 1  
注意：必须注意避免使用太少的迭代进行平滑。项太少的切比雪夫近似是一个较差的近似。前几次平滑迭代代表了数据的严重缩放和转换。随后的迭代使平滑的多面体收敛到对象的真实位置和比例。我们试图通过自动调整滤波器来防止这种情况，有效地扩大通带。仅当迭代次数大于 1 时才可能进行此调整。请注意，这会牺牲一定程度的平滑以保证模型的完整性。对于那些感兴趣的人，通过搜索值 sigma 来调整滤波器，使得实际通带为 k\_pb + sigma，并且滤波器传递函数在 k\_pb 处计算为统一，即 f(k_pb) = 1

To improve the numerical stability of the solution and minimize the scaling the translation effects, the algorithm can translate and scale the position coordinates to within the unit cube \[-1, 1\], perform the smoothing, and translate and scale the position coordinates back to the original coordinate frame. This mode is controlled with the NormalizeCoordinatesOn() / NormalizeCoordinatesOff() methods. For legacy reasons, the default is NormalizeCoordinatesOff.  
为了提高解的数值稳定性并最小化缩放平移效应，算法可以将位置坐标平移和缩放到单位立方体\[-1, 1\]内，执行平滑，并将位置坐标平移和缩放回原始坐标系。此模式由 NormalizeCoordinatesOn() / NormalizeCoordinatesOff() 方法控制。由于遗留原因，默认值为 NormalizeCoordinatesOff。

This implementation is currently limited to using an interpolation kernel based on Hamming windows. Other windows (such as Hann, Blackman, Kaiser, Lanczos, Gaussian, and exponential windows) could be used instead.  
此实现目前仅限于使用基于汉明窗的插值内核。可以使用其他窗口（例如 Hann、Blackman、Kaiser、Lanczos、Gaussian 和指数窗口）代替。

There are some special instance variables used to control the execution of this filter. (These ivars basically control what vertices can be smoothed, and the creation of the connectivity array.) The BoundarySmoothing ivar enables/disables the smoothing operation on vertices that are on the “boundary” of the mesh. A boundary vertex is one that is surrounded by a semi-cycle of polygons (or used by a single line).  
有一些特殊的实例变量用于控制此过滤器的执行。 （这些 ivar 基本上控制可以平滑哪些顶点，以及创建连接数组。） BoundarySmoothing ivar 启用/禁用对网格“边界”上的顶点的平滑操作。边界顶点是由多边形的半环（或由单条线使用）包围的顶点。

Another important ivar is FeatureEdgeSmoothing. If this ivar is enabled, then interior vertices are classified as either “simple”, “interior edge”, or “fixed”, and smoothed differently. (Interior vertices are manifold vertices surrounded by a cycle of polygons; or used by two line cells.) The classification is based on the number of feature edges attached to v. A feature edge occurs when the angle between the two surface normals of a polygon sharing an edge is greater than the FeatureAngle ivar. Then, vertices used by no feature edges are classified “simple”, vertices used by exactly two feature edges are classified “interior edge”, and all others are “fixed” vertices.  
另一个重要的 ivar 是 FeatureEdgeSmoothing。如果启用此 ivar，则内部顶点被分类为“简单”、“内部边缘”或“固定”，并进行不同的平滑处理。 （内部顶点是由多边形循环包围的多个顶点；或由两个线单元使用。）分类基于附加到 v 的特征边的数量。当多边形的两个表面法线之间的角度时，就会出现特征边共享边大于 FeatureAngle ivar。然后，没有特征边使用的顶点被分类为“简单”，恰好两个特征边使用的顶点被分类为“内部边缘”，而所有其他的顶点都是“固定”顶点。

Once the classification is known, the vertices are smoothed differently. Corner (i.e., fixed) vertices are not smoothed at all. Simple vertices are smoothed as before . Interior edge vertices are smoothed only along their two connected edges, and only if the angle between the edges is less than the EdgeAngle ivar.  
一旦知道分类，就会对顶点进行不同的平滑处理。角落（即固定）顶点根本不平滑。像以前一样平滑简单的顶点。内部边缘顶点仅沿其两个连接的边缘进行平滑处理，并且仅当边缘之间的角度小于 EdgeAngle ivar 时。

The total smoothing can be controlled by using two ivars. The NumberOfIterations determines the maximum number of smoothing passes. The NumberOfIterations corresponds to the degree of the polynomial that is used to approximate the windowed sinc function. Ten or twenty iterations is all the is usually necessary. Contrast this with vtkSmoothPolyDataFilter which usually requires 100 to 200 smoothing iterations. vtkSmoothPolyDataFilter is also not an approximation to an ideal low-pass filter, which can cause the geometry to shrink as the amount of smoothing increases.  
总平滑可以通过使用两个 ivars 来控制。 NumberOfIterations 确定最大平滑次数。 NumberOfIterations 对应于用于逼近加窗 sinc 函数的多项式的次数。通常需要十或二十次迭代。将此与通常需要 100 到 200 次平滑迭代的 vtkSmoothPolyDataFilter 进行对比。 vtkSmoothPolyDataFilter 也不是理想低通滤波器的近似值，它会导致几何形状随着平滑量的增加而缩小。

The second ivar is the specification of the PassBand for the windowed sinc filter. By design, the PassBand is specified as a doubleing point number between 0 and 2. Lower PassBand values produce more smoothing. A good default value for the PassBand is 0.1 (for those interested, the PassBand (and frequencies) for PolyData are based on the valence of the vertices, this limits all the frequency modes in a polyhedral mesh to between 0 and 2.)  
第二个 ivar 是窗口 sinc 滤波器的通带规范。根据设计，PassBand 被指定为 0 到 2 之间的双倍点数。较低的 PassBand 值会产生更多的平滑度。 PassBand 的一个很好的默认值是 0.1（对于那些感兴趣的人，PolyData 的 PassBand（和频率）基于顶点的化合价，这将多面体网格中的所有频率模式限制在 0 和 2 之间。）

There are two instance variables that control the generation of error data. If the ivar GenerateErrorScalars is on, then a scalar value indicating the distance of each vertex from its original position is computed. If the ivar GenerateErrorVectors is on, then a vector representing change in position is computed.  
有两个实例变量控制错误数据的生成。如果 ivar GenerateErrorScalars 打开，则计算一个标量值，指示每个顶点与其原始位置的距离。如果 ivar GenerateErrorVectors 打开，则计算表示位置变化的向量。

Warning  
The smoothing operation reduces high frequency information in the geometry of the mesh. With excessive smoothing important details may be lost. Enabling FeatureEdgeSmoothing helps reduce this effect, but cannot entirely eliminate it.  
警告  
平滑操作减少了网格几何形状中的高频信息。过度平滑可能会丢失重要的细节。启用 FeatureEdgeSmoothing 有助于减少这种影响，但不能完全消除它。

```cpp
  vtkNew<vtkWindowedSincPolyDataFilter> smoother;
  smoother->SetInputConnection(sphereSource->GetOutputPort());
  smoother->SetNumberOfIterations(15);
  smoother->BoundarySmoothingOff();
  smoother->FeatureEdgeSmoothingOff();
  smoother->SetFeatureAngle(120.0);
  smoother->SetPassBand(.001);
  smoother->NonManifoldSmoothingOn();
  smoother->NormalizeCoordinatesOn();
  smoother->Update();

```

vtkSmoothPolyDataFilter
-----------------------

> 示例：https://kitware.github.io/vtk-examples/site/Cxx/PolyData/SmoothPolyDataFilter/

adjust point positions using Laplacian smoothing  
使用拉普拉斯平滑调整点位置

vtkSmoothPolyDataFilter is a filter that adjusts point coordinates using Laplacian smoothing. The effect is to “relax” the mesh, making the cells better shaped and the vertices more evenly distributed. Note that this filter operates on the lines, polygons, and triangle strips composing an instance of vtkPolyData. Vertex or poly-vertex cells are never modified.  
vtkSmoothPolyDataFilter 是一个使用拉普拉斯平滑调整点坐标的过滤器。效果是“放松”网格，使单元格的形状更好，顶点分布更均匀。请注意，此过滤器对构成 vtkPolyData 实例的线、多边形和三角形条带进行操作。顶点或多顶点单元永远不会被修改。

The algorithm proceeds as follows. For each vertex v, a topological and geometric analysis is performed to determine which vertices are connected to v, and which cells are connected to v. Then, a connectivity array is constructed for each vertex. (The connectivity array is a list of lists of vertices that directly attach to each vertex.) Next, an iteration phase begins over all vertices. For each vertex v, the coordinates of v are modified according to an average of the connected vertices. (A relaxation factor is available to control the amount of displacement of v). The process repeats for each vertex. This pass over the list of vertices is a single iteration. Many iterations (generally around 20 or so) are repeated until the desired result is obtained.  
该算法如下进行。对于每个顶点v，进行拓扑和几何分析以确定哪些顶点连接到v，以及哪些单元连接到v。然后，为每个顶点构造一个连通性数组。 （连通性数组是直接连接到每个顶点的顶点列表的列表。）接下来，迭代阶段开始于所有顶点。对于每个顶点v，v的坐标根据连接顶点的平均值进行修改。 （松弛因子可用于控制 v 的位移量）。对每个顶点重复该过程。遍历顶点列表是一次迭代。重复多次迭代（通常大约 20 次左右），直到获得所需的结果。

There are some special instance variables used to control the execution of this filter. (These ivars basically control what vertices can be smoothed, and the creation of the connectivity array.) The BoundarySmoothing ivar enables/disables the smoothing operation on vertices that are on the “boundary” of the mesh. A boundary vertex is one that is surrounded by a semi-cycle of polygons (or used by a single line).  
有一些特殊的实例变量用于控制此过滤器的执行。 （这些 ivar 基本上控制可以平滑哪些顶点，以及创建连接数组。） BoundarySmoothing ivar 启用/禁用对网格“边界”上的顶点的平滑操作。边界顶点是由多边形的半环（或由单条线使用）包围的顶点。

Another important ivar is FeatureEdgeSmoothing. If this ivar is enabled, then interior vertices are classified as either “simple”, “interior edge”, or “fixed”, and smoothed differently. (Interior vertices are manifold vertices surrounded by a cycle of polygons; or used by two line cells.) The classification is based on the number of feature edges attached to v. A feature edge occurs when the angle between the two surface normals of a polygon sharing an edge is greater than the FeatureAngle ivar. Then, vertices used by no feature edges are classified “simple”, vertices used by exactly two feature edges are classified “interior edge”, and all others are “fixed” vertices.  
另一个重要的 ivar 是 FeatureEdgeSmoothing。如果启用此 ivar，则内部顶点被分类为“简单”、“内部边缘”或“固定”，并进行不同的平滑处理。 （内部顶点是由多边形循环包围的多个顶点；或由两个线单元使用。）分类基于附加到 v 的特征边的数量。当多边形的两个表面法线之间的角度时，就会出现特征边共享边大于 FeatureAngle ivar。然后，没有特征边使用的顶点被分类为“简单”，恰好两个特征边使用的顶点被分类为“内部边缘”，而所有其他的顶点都是“固定”顶点。

Once the classification is known, the vertices are smoothed differently. Corner (i.e., fixed) vertices are not smoothed at all. Simple vertices are smoothed as before (i.e., average of connected vertex coordinates). Interior edge vertices are smoothed only along their two connected edges, and only if the angle between the edges is less than the EdgeAngle ivar.  
一旦知道分类，就会对顶点进行不同的平滑处理。角落（即固定）顶点根本不平滑。像以前一样平滑简单顶点（即，连接顶点坐标的平均值）。内部边缘顶点仅沿其两个连接的边缘进行平滑处理，并且仅当边缘之间的角度小于 EdgeAngle ivar 时。

The total smoothing can be controlled by using two ivars. The NumberOfIterations is a cap on the maximum number of smoothing passes. The Convergence ivar is a limit on the maximum point motion. If the maximum motion during an iteration is less than Convergence, then the smoothing process terminates. (Convergence is expressed as a fraction of the diagonal of the bounding box.)  
总平滑可以通过使用两个 ivars 来控制。 NumberOfIterations 是最大平滑通道数的上限。 Convergence ivar 是对最大点运动的限制。如果迭代期间的最大运动小于收敛，则平滑过程终止。 （收敛性表示为边界框对角线的一小部分。）

There are two instance variables that control the generation of error data. If the ivar GenerateErrorScalars is on, then a scalar value indicating the distance of each vertex from its original position is computed. If the ivar GenerateErrorVectors is on, then a vector representing change in position is computed.  
有两个实例变量控制错误数据的生成。如果 ivar GenerateErrorScalars 打开，则计算一个标量值，指示每个顶点与其原始位置的距离。如果 ivar GenerateErrorVectors 打开，则计算表示位置变化的向量。

Optionally you can further control the smoothing process by defining a second input: the Source. If defined, the input mesh is constrained to lie on the surface defined by the Source ivar.  
或者，您可以通过定义第二个输入来进一步控制平滑过程：源。如果已定义，输入网格将被限制在 Source ivar 定义的曲面上。

Warning  
The Laplacian operation reduces high frequency information in the geometry of the mesh. With excessive smoothing important details may be lost, and the surface may shrink towards the centroid. Enabling FeatureEdgeSmoothing helps reduce this effect, but cannot entirely eliminate it. You may also wish to try vtkWindowedSincPolyDataFilter. It does a better job of minimizing shrinkage.  
警告  
拉普拉斯运算减少了网格几何结构中的高频信息。过度平滑可能会丢失重要的细节，并且表面可能会向质心收缩。启用 FeatureEdgeSmoothing 有助于减少这种影响，但不能完全消除它。您可能还希望尝试 vtkWindowedSincPolyDataFilter。它在最大限度地减少收缩方面做得更好。

vtkTransformPolyDataFilter
--------------------------

> 示例：https://kitware.github.io/vtk-examples/site/Cxx/Filtering/TransformPolyData/

transform points and associated normals and vectors for polygonal dataset  
多边形数据集的变换点和相关的法线和向量

vtkTransformPolyDataFilter is a filter to transform point coordinates and associated point and cell normals and vectors. Other point and cell data is passed through the filter unchanged. This filter is specialized for polygonal data. See vtkTransformFilter for more general data.  
vtkTransformPolyDataFilter 是一个过滤器，用于变换点坐标以及相关的点和单元法线和向量。 其他点和单元格数据不变地通过过滤器。 此过滤器专门用于多边形数据。 有关更一般的数据，请参阅 vtkTransformFilter。

An alternative method of transformation is to use vtkActor’s methods to scale, rotate, and translate objects. The difference between the two methods is that vtkActor’s transformation simply effects where objects are rendered (via the graphics pipeline), whereas vtkTransformPolyDataFilter actually modifies point coordinates in the visualization pipeline. This is necessary for some objects (e.g., vtkProbeFilter) that require point coordinates as input.  
另一种转换方法是使用 vtkActor 的方法来缩放、旋转和平移对象。 这两种方法的区别在于 vtkActor 的转换只是影响对象的渲染位置（通过图形管道），而 vtkTransformPolyDataFilter 实际上修改了可视化管道中的点坐标。 这对于需要点坐标作为输入的某些对象（例如 vtkProbeFilter）是必需的。

vtkBooleanOperationPolyDataFilter
---------------------------------

> 示例：https://kitware.github.io/vtk-examples/site/Cxx/VisualizationAlgorithms/BandedPolyDataContourFilter/

Computes the boundary of the union, intersection, or difference volume computed from the volumes defined by two input surfaces.  
计算由两个输入曲面定义的体积计算的并集、交集或差体积的边界。

The two surfaces do not need to be manifold, but if they are not, unexpected results may be obtained. The resulting surface is available in the first output of the filter. The second output contains a set of polylines that represent the intersection between the two input surfaces.  
这两个表面不需要是流形的，但如果不是，则可能会获得意想不到的结果。 生成的表面在过滤器的第一个输出中可用。 第二个输出包含一组多段线，表示两个输入表面之间的交点。

Warning  
This filter is not designed to perform 2D boolean operations, and in fact relies on the inputs having no co-planar, overlapping cells.  
警告  
该过滤器不是为执行 2D 布尔运算而设计的，实际上依赖于没有共面重叠单元的输入。

vtkConnectivityFilter 和 vtkPolyDataConnectivityFilter
-----------------------------------------------------

### vtkConnectivityFilter

> 示例：https://kitware.github.io/vtk-examples/site/Cxx/Filtering/ConnectivityFilter/

extract data based on geometric connectivity  
基于几何连通性提取数据

vtkConnectivityFilter is a filter that extracts cells that share common points and/or meet other connectivity criterion. (Cells that share vertices and meet other connectivity criterion such as scalar range are known as a region.) The filter works in one of six ways: 1) extract the largest connected region in the dataset; 2) extract specified region numbers; 3) extract all regions sharing specified point ids; 4) extract all regions sharing specified cell ids; 5) extract the region closest to the specified point; or 6) extract all regions (used to color the data by region).  
vtkConnectivityFilter 是一个过滤器，用于提取共享公共点和/或满足其他连通性标准的单元格。 （共享顶点并满足其他连通性标准（例如标量范围）的单元称为区域。）过滤器以六种方式之一工作：1）提取数据集中最大的连通区域； 2) 提取指定区域编号； 3) 提取所有共享指定点 id 的区域； 4）提取所有共享指定小区ID的区域； 5）提取离指定点最近的区域；或 6) 提取所有区域（用于按区域对数据进行着色）。

vtkConnectivityFilter is generalized to handle any type of input dataset. If the input to this filter is a vtkPolyData, the output will be a vtkPolyData. For all other input types, it generates output data of type vtkUnstructuredGrid. Note that the only Get_Output() methods that will return a non-null pointer are GetUnstructuredGridOutput() and GetPolyDataOutput() when the output of the filter is a vtkUnstructuredGrid or vtkPolyData, respectively.  
vtkConnectivityFilter 被泛化为处理任何类型的输入数据集。如果此过滤器的输入是 vtkPolyData，则输出将是 vtkPolyData。对于所有其他输入类型，它会生成类型为 vtkUnstructuredGrid 的输出数据。请注意，当过滤器的输出分别为 vtkUnstructuredGrid 或 vtkPolyData 时，将返回非空指针的唯一 Get_Output() 方法是 GetUnstructuredGridOutput() 和 GetPolyDataOutput()。

The behavior of vtkConnectivityFilter can be modified by turning on the boolean ivar ScalarConnectivity. If this flag is on, the connectivity algorithm is modified so that cells are considered connected only if 1) they are geometrically connected (share a point) and 2) the scalar values of one of the cell’s points falls in the scalar range specified. This use of ScalarConnectivity is particularly useful for volume datasets: it can be used as a simple “connected segmentation” algorithm. For example, by using a seed voxel (i.e., cell) on a known anatomical structure, connectivity will pull out all voxels “containing” the anatomical structure. These voxels can then be contoured or processed by other visualization filters.  
vtkConnectivityFilter 的行为可以通过打开布尔 ivar ScalarConnectivity 来修改。如果此标志打开，则连接性算法将被修改，以便仅在以下情况下才认为单元格是连接的：1) 它们在几何上连接（共享一个点）和 2) 单元格点之一的标量值落在指定的标量范围内。 ScalarConnectivity 的这种使用对于体积数据集特别有用：它可以用作简单的“连接分割”算法。例如，通过在已知解剖结构上使用种子体素（即细胞），连通性将拉出所有“包含”该解剖结构的体素。然后可以通过其他可视化过滤器对这些体素进行轮廓化或处理。

If the extraction mode is set to all regions and ColorRegions is enabled, The RegionIds are assigned to each region by the order in which the region was processed and has no other significance with respect to the size of or number of cells.  
如果提取模式设置为所有区域并启用 ColorRegions，则 RegionIds 按区域处理顺序分配给每个区域，并且对于单元格的大小或数量没有其他意义。

### vtkPolyDataConnectivityFilter

> 示例：https://kitware.github.io/vtk-examples/site/Cxx/Filtering/ConnectivityFilterDemo/

extract polygonal data based on geometric connectivity  
基于几何连通性提取多边形数据

vtkPolyDataConnectivityFilter is a filter that extracts cells that share common points and/or satisfy a scalar threshold criterion. (Such a group of cells is called a region.) The filter works in one of six ways: 1) extract the largest (most points) connected region in the dataset; 2) extract specified region numbers; 3) extract all regions sharing specified point ids; 4) extract all regions sharing specified cell ids; 5) extract the region closest to the specified point; or 6) extract all regions (used to color regions).  
vtkPolyDataConnectivityFilter 是一个过滤器，用于提取共享公共点和/或满足标量阈值标准的单元格。 （这样的一组单元称为一个区域。）过滤器以以下六种方式之一工作：1）提取数据集中最大（最多点）的连接区域； 2) 提取指定区域编号； 3) 提取所有共享指定点 id 的区域； 4）提取所有共享指定小区ID的区域； 5）提取离指定点最近的区域；或 6) 提取所有区域（用于为区域着色）。

This filter is specialized for polygonal data. This means it runs a bit faster and is easier to construct visualization networks that process polygonal data.  
此过滤器专门用于多边形数据。这意味着它运行速度更快，并且更容易构建处理多边形数据的可视化网络。

The behavior of vtkPolyDataConnectivityFilter can be modified by turning on the boolean ivar ScalarConnectivity. If this flag is on, the connectivity algorithm is modified so that cells are considered connected only if 1) they are geometrically connected (share a point) and 2) the scalar values of the cell’s points falls in the scalar range specified. If ScalarConnectivity and FullScalarConnectivity is ON, all the cell’s points must lie in the scalar range specified for the cell to qualify as being connected. If FullScalarConnectivity is OFF, any one of the cell’s points may lie in the user specified scalar range for the cell to qualify as being connected.  
vtkPolyDataConnectivityFilter 的行为可以通过打开布尔 ivar ScalarConnectivity 来修改。如果此标志打开，则连接算法将被修改，以便仅在以下情况下才认为单元格连接：1) 它们在几何上连接（共享一个点）和 2) 单元格点的标量值落在指定的标量范围内。如果 ScalarConnectivity 和 FullScalarConnectivity 为 ON，则所有单元格的点必须位于为单元格指定的标量范围内，才有资格被连接。如果 FullScalarConnectivity 为 OFF，则单元格的任何一个点都可能位于用户指定的标量范围内，以使单元格有资格被连接。

This use of ScalarConnectivity is particularly useful for selecting cells for later processing.  
ScalarConnectivity 的这种使用对于选择单元格以供以后处理特别有用。