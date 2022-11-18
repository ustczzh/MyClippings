# NX二次开发-外部开发环境配置 2 - 知乎
作者：金雷 审校：纪新杭

适用版本：NX6.0及以上

一、概述

在NX开发过程中，有的时候想要在不打开NX的情况下，导出NX模型中的信息。之前有分享过NX是本地的状况下。现在随着集成化越来越高，与TC集成的环境下，外部开发模式的要求也逐步显现出来。

本文将主要介绍在TC集成的环境下如何配置NX二次开发外部开发的环境。功能目标为导出PDF。

二、功能说明

本篇以TC11（四层）+NX11为例进行介绍，其他版本与此版本配置方式几乎相同。

首先需要创建一个文件夹，然后将需要引用的“.dll”文件放在该文件夹中，如图1所示。

![](https://pic3.zhimg.com/v2-74e420e8633f2fe31d727a570205bd1a_b.jpg)

图1

接着编写对应的导出PDF的程序。需要注意的是，在入口函数，需要传入参数，和本地环境不同，TC集成的环境要求下，还需要传入TC的参数，用于初始化NX集成环境，参数分别为：

*   环境：-pim=yes——代表NX的运行环境为TC集成环境；
*   账号：-u=infodba
*   密码：-p=infodba

参考代码如下图2所示。

![](https://pic1.zhimg.com/v2-deb27586bd6d2ae9bc3c9a28ca826450_b.jpg)

图2

然后创建“.bat”文件，编写内容如下图３所示。主体的思想是：设置TC和NX的目录，然后设置好TC数据路径，执行程序并输入参数。

![](https://pic4.zhimg.com/v2-03fe63738b0267ab70a904c8b5de82e3_b.jpg)

图3

图3中参数“UGII\_UGMGR\_HTTP\_URL=[http://SFServer:7001/tc](https://link.zhihu.com/?target=http%3A//SFServer%3A7001/tc)”，该http路径查找方式为：

3-1：查找相对路径“D:\\Siemens\\Teamcenter11\\install”下的“configuration.xml”文件，用记事本打开，找到对应的行（<tcUrl value="[http://SFServer:7001/tc](https://link.zhihu.com/?target=http%3A//SFServer%3A7001/tc)" />）。如图3-1所示：

![](https://pic4.zhimg.com/v2-07d62ae29d3834377ce2e37189f89d7f_b.jpg)

图3-1

3-2：打开“环境管理器”

![](https://pic3.zhimg.com/v2-a5186ef693039f919c11c5efea42fb72_b.jpg)

，依次点击“配置管理器”->“维护现有配置”->“下一步”->“功能部件维护”中选择“修改四层结构服务器设置”如图3-2：

![](https://pic2.zhimg.com/v2-2da864f83c7a98d771f232e7cf2ead4d_b.jpg)

图3-2

再次点击下一步时，显示的即为“url”地址，如图3-3。需要注意的是，若地址中如3-3没有对应的计算机名称的，需要手动补齐，格式如：“[http://SFServer:7001/tc](https://link.zhihu.com/?target=http%3A//SFServer%3A7001/tc)”

![](https://pic2.zhimg.com/v2-4ba4f130f8aada440c11a0588351e521_b.jpg)

图3-3

关闭该“.bat”文件，启动TC服务。然后双击执行该“.bat”文件，程序将执行，并导出PDF，如图4所示。

![](https://pic3.zhimg.com/v2-2b2dce58a8b1196017ae89518cbbd8aa_b.jpg)

图4

三、总结

NX集成环境下提供了这个方法，方便了用户的操作。但是需要注意的是，NX本身还是需要安装，并且，如果电脑配置较低，程序执行起来时间会稍微有点久。