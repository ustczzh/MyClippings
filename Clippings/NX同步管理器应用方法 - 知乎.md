# NX同步管理器应用方法 - 知乎
[NX同步管理器应用方法​mp.weixin.qq.com/s/W5WTSIy2UId4Sq5oy64XSA![](https://pic3.zhimg.com/v2-50fde772c5fe80d7bdb98a352faae35e_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/W5WTSIy2UId4Sq5oy64XSA)

作者：黎芳勇 审校：陈林生

适用版本：NX所有版本

随着现代数控加工技术的发展，双通道机床在日常生产中的应用越来越多，NX 软件的同步控制器可以有效的解决双通道机床的等待与同步控制问题。本文就向大家介绍NX CAM提供的同步管理器的使用方法。

![](https://pic2.zhimg.com/v2-b98301231a4dbe3426c2d2f28e9eae99_b.jpg)

双通道车铣复合加工中心编程时，首先需要设置好两个通道。在机床视图中创建两个通道，如下图所示：

![](https://pic2.zhimg.com/v2-24f95c67c50ff836b4e11cf56d36b3bd_b.jpg)

在程序顺序视图中分别编制两个通道的刀具运动轨迹，如下图所示：

![](https://pic4.zhimg.com/v2-6d72cea6e796ad10dd252eb53b8c09db_b.jpg)

创建一个包含两个通道刀具运动轨迹的程序组，选择它并单击右键，选择【刀轨】-【同步】如下图所示：

![](https://pic2.zhimg.com/v2-c9d19ba31a366979f7e5e40aa5fa67c9_b.jpg)

在弹出的同步控制器对话框中，两个通道的程序被自动区分显示，如下图所示：

![](https://pic2.zhimg.com/v2-afc5e75478d8838e396cf1a7385602b1_b.jpg)

在同步控制器中选择要启动同步操作的刀具轨迹，在工具条中选择【同步标记】按钮，在弹出的同步标记对话框中选择主要通道和要标记同步的通道号，如下图所示：

![](https://pic1.zhimg.com/v2-eb9bef915648f7cb8838d17db8fc3d10_b.jpg)

添加好同步控制标记后，在同步控制器中显示如下图所示：

![](https://pic3.zhimg.com/v2-742caca53ac2737f196e3163446d80ca_b.jpg)

配合具有同步控制代码转换能力的后处理，就会自动生成双通道同步控制的NC代码，并且在NC代码的相对应的时间节点上输出同步指令。如下图所示：

![](https://pic3.zhimg.com/v2-64a35b4ccd6f430d940a845d77297abe_b.jpg)