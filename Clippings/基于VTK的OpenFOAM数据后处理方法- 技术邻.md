# 基于VTK的OpenFOAM数据后处理方法- 技术邻
来源：多相流在线

作者：吴玉欣

OpenFOAM数据后处理通常使用ParaView等可视化绘图工具，在处理大量计算数据时存在效率低下的问题，本篇主要介绍基于VTK的OpenFOAM数据后处理方法，该方法通过Python程序调用VTK函数库自动执行数据场的3D图形化渲染，大幅提高了数据后处理效率，并可以此为基础开发可自定义的新型OpenFOAM后处理程序。

OpenFOAM\[1\]数据后处理通常使用ParaView\[2\]、Ensight和Tecplot360等可视化绘图工具，以上工具依赖手动操作的方式生成数据场的分布云图，因此在处理多组计算数据时存在效率低下的问题，且难以添加其他自定义功能。为解决以上问题，有必要开发程序自动完成数据场的3D图形化渲染输出，同时满足可添加自定义功能的需求。

为实现以上功能，首先需要寻找支持读取OpenFOAM数据文件的工具，同时该工具要支持3D图形化渲染功能。

VTK（visualization toolkit）为免费开源的软件系统\[3\]，可实现三维模型的计算机图形可视化，被广泛应用于计算流体数据分析、医学建模成像等多个领域（图1）。

VTK的阅读器可直接读取OpenFOAM的数据文件，并通过数据流的方式实现数据场的3D图形化渲染，因此成为开发OpenFOAM后处理程序的理想选择。

![](https://img.jishulink.com/201906/imgs/4fdbddc054fd433ca30989999782f282)

图1 VTK在计算流体、医学等领域的应用

VTK实质上是一个C++的标准库，采用面向对象的方式构建，包含600多个类，支持Java、Python等高级语言的调用。KitwarePublic\[4\]及PENGUINITIS\[5\]网站上提供了诸多关于Python程序如何调用VTK函数库绘图的示例。在调研了现有基于VTK的OpenFOAM数据后处理方法基础上，重点介绍如何利用Python语言调用VTK函数库直接读取OpenFOAM数据，自动执行数据场的3D图形化渲染。通过该方法可实现多组计算数据的批处理绘图，大大提高数据后处理效率，并可在此基础上开发可自定义的新型后处理程序。

**1****VTK的工作原理**

VTK系统采用数据流的方法，实现数据信息与图形信息之间的转变。图2为基于VTK的3D图形绘制流程\[6\]。

![](https://img.jishulink.com/201906/imgs/7731b67519434976a04fd8d135b397fb)

图2 VTK的主要模块及3D绘图流程

绘图流程：

通过Reader读取数据，采用Filter对数据进行格式转换；

通过Mapper建立数据与图形元素间的映射关系，将数据映射到Actor对象；

通过渲染器Renderer和RendererWindows建立可视化场景和窗口，同时基于Interactor实现窗口交互功能。

VTK库中支持读取的数据类型包括_vtkStructuredPoints_、_vtkRectilinearGrid_、_vtkStructuredGrid_、_vtkUnstructuredGrid_和_vtkPolyData_几大类，OpenFOAM的数据格式对应VTK中的_vtkCompositePolyData_数据类型，为_vtkPolyData_数据类型的一种，可通过vtk库中的_vtkOpenFOAMReader_函数读取。

**2****基于VTK的OpenFOAM后处理方法**

以OpenFOAM6中的顶盖驱动流算例（cavity）为例，详细介绍基于VTK的OpenFOAM后处理程序开发思路。cavity算例为OpenFOAM的经典算例之一，采用不可压缩层流流动求解器icoFoam求解。计算完成后，输入以下命令依据速度向量场"U"计算得到速度模的分布场"mag(U)"。

![](https://img.jishulink.com/201906/imgs/c588a699dda84b7b9e2d662ec1d9c725)

这里将介绍的**ofvtk\_reader.py**程序，可实现cavity算例计算结果的3D图形化渲染。该程序可运行于Python3.6+VTK8.2.0环境，VTK库可采用Python的pip工具通过以下命令安装：

![](https://img.jishulink.com/201906/imgs/6fa757230bef4f1d9cdd5c555f57b18f)

基于VTK的3D图形化渲染过程需要遵循图2所示的工作流程，下面就图2中各个VTK模块在ofvtk\_reader.py程序中的实现过程进行了详细介绍。

*   #### **Reader**
    

该模块通过VTK库中的_vtkOpenFOAMReader_函数实现OpenFOAM格式数据的读取。

![](https://img.jishulink.com/201906/imgs/a33a0d6c69aa463ca259eba78f8c7e33)

其中_filename_参数为算例中_controlDict_文件的路径，_latest\_time_表示当前算例运行的最新时间步。运行以上代码，可以将当前最新时间步的数据读入程序中。我们要绘制的标量场存储在网格节点上，通过以下代码实现网格上标量场"mag(U)"的读取。

_data\_type_参数设置为"cell"，表示程序读取的是网格体中心上的数据。至此，网格几何信息存储在_block_变量中，标量场"mag(U)"储存在矩阵变量_array_中，其名称则储存在字符串变量_array\_name_里。

*   #### **Filter**
    

采用_vtkGeometryFilter_转换读入的网格数据信息，并将其存储在_geom\_filter_变量中。

*   #### **Mapper**
    

基于_vtkCompositePolyDataMapper_以及_SetInputConnection_函数，建立网格信息与图形元素之间的映射关系。通过_SelectColorArray_指定网格在图形中的显示颜色由_array\_name_变量对应的参数场决定。

*   #### **Actor**
    

通过_vtkActor_函数建立图形化对象_actor_，采用_SetMapper_函数选择之前建立的映射器_mapper_将网格数据信息映射到对象actor上。

*   #### **Renderer**
    

采用渲染器_vtkRenderer_建立场景_ren_，运行_AddActor_函数将对象_actor_添加至_ren_场景中。

*   #### **RenderWindow**
    

通过窗口渲染器_vtkRenderWindow_建立窗口对象_ren\_win_，将场景_ren_添加至_ren\_win_中。

![](https://img.jishulink.com/201906/imgs/53a4175cfe63467d9697859cd7e3218b)

*   #### **Interactor**
    

通过_vtkRenderWindowInteractor_窗口交互控件可实现通过鼠标旋转图形方向等交互功能。

除此之外，程序中还包括了定义颜色条_scalar\_bar_和输出文本信息等模块，详情参见附件中的程序代码。

将此程序拷贝至cavity算例文件夹所在的目录中，运行后即可生成速度模"mag(U)"的3D分布云图，可通过鼠标拖拽调整网格显示的方向（图3）。

![](https://img.jishulink.com/201906/imgs/8ca84af3d1d347cc99bbd527467af55f)

图3 cavity算例采用VTK渲染生成的速度模3D分布云图

**3****总结**

本文以ofvtk\_render.py程序为例，介绍了如何利用Python语言调用VTK函数库读取OpenFOAM数据并实现数据场的3D图形化渲染。基于该方法可实现多组计算数据的批处理绘图，提高数据的后处理效率。同时可在此基础上结合PyQT5模块，开发新型的OpenFOAM图形化后处理工具，实现任意自定义的后处理功能。

参考文献

\[1\] Jasak H, Jemcov A, Tukovic Z. OpenFOAM: A C++ library for complex physics simulations\[C\]//International workshop on coupled methods in numerical dynamics. IUC Dubrovnik Croatia, 2007, 1000: 1-20.

\[2\] Ahrens J, Geveci B, Law C. Paraview: An end-user tool for large data  visualization\[J\]. The visualization handbook, 2005, 717.

\[3\] Schroeder W J, Lorensen B, Martin K. The visualization toolkit: an object-oriented approach to 3D graphics\[M\]. Kitware, 2004.

\[4\] VTK/Examples. KitwarePublic. https://vtk.org/Wiki/VTK/Examples/Python

\[5\] VTK メモ. PENGUINITIS. http://penguinitis.g1.xrea.com/computer/programming/Python/VTK/VTK.html

\[6\] Stefano Perticoni. Introduction to Visualization ToolKit. 13th summer school on scientific visualization.