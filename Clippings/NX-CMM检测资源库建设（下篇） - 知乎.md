# NX-CMM检测资源库建设（下篇） - 知乎
[NX-CMM检测资源库建设（下篇）​mp.weixin.qq.com/s/aw\_OWVui5Pa6nwzTQjUpuw![](https://pic2.zhimg.com/v2-d398d5fc06b50d6f1e81c7969353a6ed_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/aw_OWVui5Pa6nwzTQjUpuw)

作者：倪海 审校：徐涛

适用版本：NX11及以上版本

一、概述

NX CMM三次元检测资源库建设包含机床库、机头库、探针库、样例程序，将这四种资源组合才能形成一个完整的资源库。

由于篇幅太长，本文分上、下两篇，分别介绍机床库、机头库以及探针库的建设。

本篇承接上篇，作为下篇，仅介绍探针库的建设。

二、功能说明

1、建立探针库

探针作为跟工件实际接触的零件，其尺寸和实际形状对仿真最为关键。

探针库的建立步骤如下：

(1)实物测绘探针3D模型图；

(2)进入应用模块“机床构建器”；

(3)如下图所示建立两个坐标系，一个坐标系为探针在机头上的安装位置，一个坐标系为探针的圆心；

![](https://pic4.zhimg.com/v2-6b6c7c0222ac044f04828371cc868f1b_b.jpg)

图1

(4)探针3D图设定完成后，用insp\_tool\_database.dat在NX软件内注册；

![](https://pic3.zhimg.com/v2-210dc9961dd84862cc99ec545d98e26a_b.jpg)

图2

(5)探针的注册名字必需与探针3D图文件名一致。

![](https://pic4.zhimg.com/v2-3c3783ee0d98254c2199ba819fa731e7_b.jpg)

图3

三、总结

本篇通过图文并茂的方式详细介绍了探针库的建设。通过上下两篇技巧详细介绍了NX CMM检测资源库的建设，实现机床资源的重用，提高检测编程效率。