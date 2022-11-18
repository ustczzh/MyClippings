# NX装配加载选项默认设置应用技巧 - 知乎
作者：张磊 审校： 刘卫民

适用版本：NX所有版本

使用NX软件进行产品设计时，很多工程师习惯将一个产品中所有的零组件根据类型存放在不同的文件夹中，如图1，用NX打开产品总装模型时，经常要先设置NX的装配加载选项，如图2，将其设置为按照保存的或者是从文件夹搜索。所以很多工程师就提出了如何通过设置，使装配加载选项默认设置为打开不同文件夹下的装配零组件。对于以上问题如何解决，今天就把方法介绍给大家。

![](https://pic1.zhimg.com/v2-41ae1cbae62b8e65721ef9f76cdf4694_b.jpg)

![](https://pic4.zhimg.com/v2-73a6edc215993938b47c9130a7cdec7b_b.jpg)

详细操作步骤如下：

Step1：、打开NX软件→菜单→文件→选项→装配选项；

![](https://pic3.zhimg.com/v2-af68217b84a05e7d87274877cc31b28e_b.jpg)

Step2:弹出“装配加载选项”对话框，在部件版本加载项中选择“按照保存的”；在“已保存的加载选项”中可以选择“保存至文件”。

![](https://pic2.zhimg.com/v2-838b52921752e7baabf7636155e6847d_b.jpg)

Step3：弹出“保存加载选项文件”对话框后，将“load\_options.def”文件可以保存到NX安装目录的UGII文件夹下。；

![](https://pic3.zhimg.com/v2-e8c40f0b13950efeae834ff6a21c9a4e_b.jpg)

Step4：找到NX安装目录UGII文件夹下的“ugii\_env.dat”文件，用记事本的方式打开；

![](https://pic2.zhimg.com/v2-320b9520e5ab0a1d8abd1a75175cb5b5_b.jpg)

Step5：找到NX安装目录UGII文件夹下的“ugii\_env.dat”文件，用记事本的方式打开，输入“UGII\_LOAD\_OPTIONS=D:\\Siemens\\NX11\\UGII\\load\_options.def”，保存退出。

![](https://pic3.zhimg.com/v2-9b362f851f5423ced539bb5aca085466_b.jpg)

完成以上设置，NX装配加载选项默认设置就成功了，希望在以后的建模工作中能够给大家提供帮助。