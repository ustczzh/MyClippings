# VTK基本工作原理 - CodeAntenna
* * *

转载自：[http://blog.sina.com.cn/s/blog\_63f6ddae010126de.html](http://blog.sina.com.cn/s/blog_63f6ddae010126de.htmlu)  

          http://blog.csdn.net/jayxbja/article/details/5701084

仅作学习用途

VTK有两个子系统组成：一个是编译生成的C++类库，一个是通过[Java](http://lib.csdn.net/base/javaee "Java EE知识库")、Tcl、[Python](http://lib.csdn.net/base/python "Python知识库")语言来使用这些类的解释包。 

VTK通过数据流实现变信息为图形数据的。

数据流一般为：source—filter——mapper——actor——render——renderwindow——interactor。

Actor用来在场景中表现一个可视化实体，也可以成为3D图形的描绘实现，主要用足对图形的绘制。它还包含一些属性，用来对显示对象的设置；如Actor->GetProperty()->SetColor（1,1,1）表示白色，也就是将绘制的对象着色为白色。

Camera在VTK中可以理解为视点（FocalPoint），即观察者的位置，或者称为虚拟照相机实现3D视图。

操作方法比较简单，强调的使另外两个重要的方法：   
Azimuth（150）表示Camera的视点位置沿着顺时针旋转150角度；   
Elevation（60）表示Camera的视点位置沿着向上方向旋转60角度。

Filter是一种数据处理机制，有个或者多个输入，但仅有一个输出。其目的是对图形图像数据进行处理，以便得到我们期望的数据。

要理解工作原理，首先明确几个类型：   
1.vtkSource(数据源) 这个就好比一个剧本里面的角色，让演员知道要演的是什么人物。数据源有：vtkConeSourcevtkSphereSource,vtkOutlineSource…等等。   
它们都继承与vtkPolyDataAlgorithm类，该类用于提供不同的类型的数据源

2.vtkMapper(映射器) 它就像是一个剧本，应该如何塑造角色的装扮   
映射器有：vtkDataSetMapper,vtkMultiGroupPolyDataMapper,vtkPolyDataMapper。   
它们都继承于vtkMapper类。所有的数据对象都要通过映射器Mapper映射到vtkActor中。

3.vtkActor(演员） 有了剧本，有了角色，得找个真人来演出这个剧本了。

4.vtkRenderer(渲染器) 这个过程就相当于对演员进行化妆并布置场景；   
该类继承于vtkViewport,有2个子类：vtkMesaRender,vtkOpenGLRender.   
该类另外一个作用是设置窗口vtkRenderWindow的背景.

5.vtkRenderWindow(窗口) 这个就相当于个舞台 ，把准备好的演员放进去，准备表演了；

6.vtkRenderWindowInteractor(窗口交互器) 这个像摄像机，用于捕捉演员的动作，然后传给导演看   
该类的继承关系在vtkRenderWindowInteractor文章中已给出。

7.vtkInteractorObserver(观察者) 有点导演的意思，导演通过观看录像后，做出一系列调整   

该类的继承关系在vtkRenderWindowInteractor文章中已给出

**Example:**

vtkConeSource \* s=vtkConeSource::New();                //棱锥数据源，用于构造描述棱锥的点、线、面  
vtkPolyDataMapper \* m=vtkPolyDataMapper::New();   //映射器，可以根据点、线、面构造图形  
m->SetInput(s->GetOutput());                                      //完成点、线、面的传递  
vtkActor \* a=vtkActor::New();                                        //创建一个物体（演员）  
a->SetMapper(m);                                                         //设置描述物体的图形  
vtkRenderer \* r=vtkRenderer::New();                            //绘制器，能够绘制物体  
r->AddActor(a);                                                             //把物体交给绘制器绘制  
vtkRenderWindow \* w=vtkRenderWindow::New();         //绘制窗，提供绘制的平面  
w->AddRenderer(r);                                                     //设置绘制器工作的平面  
for (int i=0;i<65535;i++)                                                  //循环绘制  
   w->Render();

vtkRenderWindowInteractor \*iren=vtkRenderWindowInteractor::New();//交互器  
iren->SetRenderWindow(w);

vtkInteractorStyleTrackballCamera \*style = vtkInteractorStyleTrackballCamera::New();//观察者  
iren->SetInteractorStyle(style);

vtkMyCallback \*callback = vtkMyCallback::New();                             //对观察到的事件响应  
iren->AddObserver(vtkCommand::InteractionEvent, callback);  
iren->Initialize();  
iren->Start();

  

更多相关推荐
------

* * *

[VTK的工作原理](https://codeantenna.com/a/AVkPKr2HB1)
------------------------------------------------

要理解VTK的工作原理，首先应明确几个类型：1.vtkSource(数据源)  这个就好比一个剧本里面的角色，让演员知道要演的是什么人物。       数据源有：vtkConeSource，vtkSphereSource,vtkOutlineSource...等等。       ...

* * *

[VTK学习之一（基本介绍、一个简单的VTK例子）](https://codeantenna.com/a/nv1TPRdrhU)
----------------------------------------------------------------

这篇主要讲一个VTK的简单例子，加深自己对VTK的理解。前记    学习vtk就是觉得pcl中封装的PCLVisualizer功能有限。学习vtk主要是看《VTK图形图像开发进阶》张晓东、罗火灵编著这本书，还有就是水灵的视频。水灵的视...

* * *

[vtk三维场景基本要素](https://codeantenna.com/a/JJmGTlH7sm)
---------------------------------------------------

1.灯光2.相机1.相机位置:vtkCamera::SetPosition()2.相机焦距:vtkCamera::setFocusPoint()3.朝上方向:4.投影方向:5.投影方法:6.视角:7.前后裁剪平面:8.相机运动:3.颜色4.纹理映射

* * *

[【VTK】VTK Cilpping](https://codeantenna.com/a/KPhT27NBbC)
---------------------------------------------------------

原文：http://glbook.gamedev.net/GLBOOK/glbook.gamedev.net/moglgp/advclip.htmlAdvancedClippingTechniques by AndreiGradinariSometimesthereisaneedtoshowwhatisinsideofanobject.Forexample,let'ssayyouwantto...

* * *

[VTK基本概念之VTK智能指针](https://codeantenna.com/a/7YS1ljgIu2)
-------------------------------------------------------

1、概念　　如果很多对象有相同的值，在程序里没有必要将这个值存储多次。更好的办法是让所有的对象共享这个值。这么做不但节省内存，而且可以是程序运行得更快，因为不需要构造和析构这个值得副本。引用计数就是这...

* * *

[VTK 学习----VTK对象-窗口类](https://codeantenna.com/a/3Wxt9PwDoS)
-----------------------------------------------------------

4.2窗口对象4.2.1vtkRenderWindowvtkRenderWindowvtkRenderWindow是一个抽象对象，用于指定呈现窗口的行为。渲染窗口是图形用户界面中的窗口，渲染器在其中绘制图像。提供了用于同步渲染过程，设置窗口大小和控制双...

* * *

[vtk学习记录（二）——新建vtk工程及Qt配置vtk](https://codeantenna.com/a/0AuL3xU0Sw)
-------------------------------------------------------------------

前言前面vtk学习记录（一）——vtk工程配置与生成我们已经生成了vtk的类库，下面就是新建自己的工程来实现业务功能了。配置Qt下载安装Qt的过程就不赘述了，只是一个安装过程。在我们之前生成的类库目录下复制QVTKWidge...

* * *

[【VTK】cmake编译VTK](https://codeantenna.com/a/kUbBe1qxAX)
-------------------------------------------------------

使用Cmkae编译VTK源码1.安装Cmake   下载安装包：http://www.cmake.org/download/   选择.exe文件下载后，安装到本地计算机。2.获得VTK源码   2.1  源码网址：http://www.vtk.org/download/   下载VTK-6.2.0.zip（vt...

* * *

[VTK 学习----VTK对象-算法类-vtkDataObjectAlgorithm](https://codeantenna.com/a/jh4dJJDayw)
----------------------------------------------------------------------------------

4.5.2vtkDataObjectAlgorithmvtk3DLinearGridPlaneCutter3D线性单元的vtkUnstructuredGrid的切割平面。vtk3DLinearGridPlaneCutter是一个专门的过滤器，它可以切割一个由三维线性单元(四面体、六面体、体素、金字塔...

* * *

[VTK简介](https://codeantenna.com/a/wT3Fuk3Ws6)
---------------------------------------------

VTK简介1、简介2、特点3、历史4、应用5、应用领域1、简介Vtk，（visualizationtoolkit）是一个开源的免费软件系统，主要用于三维计算机图形学、图像处理和可视化。Vtk是在面向对象原理的基础上设计和实现的，它的内...

* * *

© 2021-2022 All rights reserved by CodeAntenna.com.