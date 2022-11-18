# NX-剖视图PMI无法WAVE链接的解决方法 - 知乎
[NX-剖视图PMI无法WAVE链接的解决方法​mp.weixin.qq.com/s/bEljACSUTZQpNrz468HUGg![](https://pic1.zhimg.com/v2-6d686c3eb7baaf7d2b2cea4154efcf10_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/bEljACSUTZQpNrz468HUGg)

作者：倪海 审校：陈林生

适用版本：NX11及以上版本

一、概述

NX PMI模块是贯穿MBD技术的基石，从上游的产品设计到下游的制造生产，无不需要PMI标注进行穿针引线。尤其在零件三维工艺的制作过程中，需要将产品模型剖视图中的PMI继承到工序模型中。本篇介绍了一种行之有效的方法。

二、功能说明

1、首先检查剖视图中PMI是否关联到剖面线中。关闭剖视图生成截面线功能，此时剖视图存在如下图所示失效的PMI，需要修改关联对象和重设PMI原点。

![](https://pic1.zhimg.com/v2-98ef5c51949db87094df60a21326131c_b.jpg)

图1

2、修改PMI的标注的参考对象：通过边上相切点的方式选择边上的截面处断点作为标注参考对象。

![](https://pic3.zhimg.com/v2-3fd876cd604339995fc9e0006deb490a_b.jpg)

图2

3、设置好PMI的参考对象，还需要重新设置PMI的原点，否则PMI还会呈现失效状态。设置PMI原点的方法如下图所示，弹出【原点工具】对话框，点击确定按钮重设原点。

![](https://pic3.zhimg.com/v2-e21a84bdb75729eb9bc5b285f01e8e9a_b.jpg)

图3

三、总结

本篇通过修改参考对象以及重设原点的方法修改剖视图中关联剖面线的PMI，实现PMI能够支持继承到其他模型中，例如工序模型中。