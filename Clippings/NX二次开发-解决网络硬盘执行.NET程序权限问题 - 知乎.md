# NX二次开发-解决网络硬盘执行.NET程序权限问题 - 知乎
作者：倪海 审校：徐涛

适用版本：NX5及以上版本

一、概述

基于NX软件进行二次开发，可以选择很多语言，如C、C++、C#、Java、Python等。相对来说，C#语言相比C和C++门槛较低，网络资源丰富，学习研究成本较低。C#相比Java更贴近桌面应用程序。所以，基于NX软件进行二次开发，很多人选择了使用C#。C#是一种托管语言，需要基于.NET平台运行。

通常我们会将基于NET开发好程序包放到服务器，通过网络硬盘加载程序，这种方式可以实现程序的共享。但往往会遇到如下问题：

![](https://pic2.zhimg.com/v2-abe1a6e6213a6e98e985e850a292f299_b.jpg)

图1

二、功能说明

如何解决上述问题呢？

首先，我们了解产生此问题的原因。由于.Net Framework设置了安全性管控，默认情况没有打开从网络位置加载程序集的权限。

那么，我们只需要开发权限就可以了。通过以下命令打开权限：caspol -q -m -cg LocalIntranet\_Zone FullTrust

启动命令行，进入对应NET版本路径，输入上述命令，执行效果如下图所示：

![](https://pic1.zhimg.com/v2-c64fd4184d86484103364228daaaa594_b.jpg)

图2

以上命令需要局域网内每台客户端电脑仅执行一次该命令即可开启权限。

三、总结

基于.NET框架开发的程序，通过网络硬盘共享执行时，会遇到上述权限问题，通过上述方法可以开启权限加载网络硬盘上程序集。