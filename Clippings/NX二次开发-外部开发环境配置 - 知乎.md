# NX二次开发-外部开发环境配置 - 知乎
[NX二次开发-外部开发环境配置​mp.weixin.qq.com/s/HFfk\_ni8iFRX\_cA9Rsc64g![](https://pic4.zhimg.com/v2-504893388c4abe0ac9efe03df6d42f73_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/HFfk_ni8iFRX_cA9Rsc64g)

作者：金雷 审校：纪新杭

适用版本：NX6.0及以上

一、概述

在NX开发过程中，有的时候想要在不打开NX的情况下，导出NX模型中的信息。

本文将主要介绍如何配置NX二次开发外部开发的环境。功能目标为导出PDF。

二、功能说明

本篇以NX11为例进行介绍，其他版本与此版本配置方式几乎相同。

首先需要创建一个文件夹，然后将需要引用的“.dll”文件放在该文件夹中，如图1所示。

![](https://pic3.zhimg.com/v2-74e420e8633f2fe31d727a570205bd1a_b.jpg)

图1

接着编写对应的导出PDF的程序。需要注意的是，在入口函数，需要传入参数，参考代码如下图2所示。

![](https://pic2.zhimg.com/v2-38e076bb514314fa59c90c88c0b44879_b.jpg)

图2

然后创建“.bat”文件，编写内容如下图３所示。主体的思想是：将安装目录设置为工作目录，然后设置好NX路径，执行程序并输入参数（输入参数1为模型路径，输入参数2为导出的PDF路径）

![](https://pic2.zhimg.com/v2-58d4acc70991fb11898fc7cacefd564d_b.jpg)

图3

关闭该“.bat”文件，双击执行，程序将执行，并导出PDF，如图4所示。

![](https://pic1.zhimg.com/v2-54077d48d615b768df53ae41e1f3b404_b.jpg)

图4

三、总结

NX提供了这个方法，方便了用户的操作。但是需要注意的是，NX本身还是需要安装，并且，如果电脑配置较低，程序执行起来时间会稍微有点久。