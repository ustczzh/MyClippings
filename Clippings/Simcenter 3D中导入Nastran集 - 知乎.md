# Simcenter 3D中导入Nastran集 - 知乎
作者：周涛 审校： 冒小萍

适用版本：NX/Simcenter 3D / Nastran任何版本

集（SET）在NX/Simcenter Nastran中是一个常用的也是非常重要的功能，它能够快速的对多个目标对象进行控制，以便于后续的操作。例如在定义边界条件、输出请求，或动力学分析时，集可以快速选中目标。集也可以重复利用，方便于其它的分析。在Simcenter 3D前后处理界面中，集的表现形式是组（gruops）。

本文描述如何在Simcenter 3D中导入已有的 Nastran集：

我们可以将有被调用的集导入到Simcenter 3D中。方法是：文件下拉菜单—导入—仿真，选择求解器Simcenter Nastran，输入文件栏指定dat文件的目录，确定。下图中可以看出，此时SET1和SET3被输出请求调用，但是SET2没有被调用，所以在导入到Simcenter 3D中以后，SET2并不会出现在前后处理界面的组中。

![](https://pic2.zhimg.com/v2-b4dbca6198491c2ef9b32f122c2510e9_b.jpg)

图1 被调用集的导入

如果想导入没有被调用的集，可以在该集的上方加入相应的注释行，注释行必须以$\*开头，后跟两个空格，然后是关键字 Group(elements)或Group(nodes)。注意大小写。例如$\* Group (elements): 2 Name: 支架安装点。如图2：

![](https://pic4.zhimg.com/v2-81f8a48251c982768b491a8ef0f93457_b.jpg)

图2集的注释

在导入的集中，我们会经常发现有的组是红色的，并且在状态行显示：组中没有可显示的实体，这是由于该集中包含了节点对象（nodes），节点在simcenter 3D中是默认不显示的，所以会有红色的提示，此时只需要点击上边框条的独立显示和隐藏节点命令即可。如图3：

![](https://pic1.zhimg.com/v2-207e60a07865276fd299c8e0a36f5b08_b.jpg)

图3节点集的显示

如果网格模型已经导入到simcenter 3D，可以通过附加功能将集合并到已有的网格模型中。方法是：文件下拉菜单-附加，注意：行为选项须选择附加/合并，如下图：

![](https://pic1.zhimg.com/v2-3595c5f3c94c4626b53cd0f2c128bd90_b.jpg)

图4集的附加合并