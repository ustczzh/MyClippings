# NX二次开发-如何使用TC集成环境的文件浏览器 - 知乎
作者：黄盛益 审校：薛剑腾

适用版本：NX 7.5以上

一、概述

在NX二次开发中，我们经常使用BlockUI来设计界面，使用文件选择控件（File Selection with Browse）可以选择本地文件，但是不可以在集成NX环境下选择TC上的文件。本文介绍在集成NX环境下，使用“NXMgrFileBrowser”控件选在TC上的部件文件。

二、功能说明

首先用记事本打开“..\\UGII\\menus\\styler\_blocks.pax”配置文件（在NX11及以上的版本，如果是中文环境，打开的配置文件应该是同目录下的“styler\_blocks\_simpl\_chinese.pax”配置文件），在最后添加如下内容并保存文件：

<PaletteEntry id="NXMgrFileBrowser">

<ObjectData class="NewStylerItem">

<NewStylerItem>

<item class="UGS::UI::Comp::NXMgrFileBrowser" icon="report\_in\_folder.bmp"/>

</NewStylerItem>

</ObjectData>

<Presentation name="NX Manager File Browser" category="Special" description="NX Manager File Browser"/>

</PaletteEntry>

图1

然后重新进入块UI样式编辑器，在左侧块目录特殊（Special）选项卡内就可找到NXMgrFileBrowser，如下图，这样用户可以自行添加到自定义BlockUI对话框中，其他操作与一般控件的用法相同。

![](https://pic1.zhimg.com/v2-b5806dfa93f16368181a2855e3700bfc_b.jpg)

![](https://pic3.zhimg.com/v2-08d7f94888b9f9768ac307becfe96cde_b.jpg)

![](https://pic1.zhimg.com/v2-73773ebac1211ad41566adc55d460f84_b.jpg)

图2

最后可以在代码中获取选择到的文件名称和路径，获取方法如下：

var propertyList = nXMgrFileBrowser0.GetProperties();

//获取文件名

string partName = propertyList.GetString("PartName");

//获取路径

string path = propertyList.GetString("Path");

propertyList.Dispose();

图3

三、总结

NX内部包含大量方便的Block控件，只是默认没有显示出来，我们不能使用，只需要通过修改配置即可使用NX自带的内部控件，这样不需要通过自绘实现，便于应用型开发人员只需专注应用方面的开发。