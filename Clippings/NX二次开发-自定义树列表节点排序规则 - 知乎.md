# NX二次开发-自定义树列表节点排序规则 - 知乎
作者：黄盛益 审校：张季

适用版本：NX7.5以上

一、概述

树列表是一种在NX二次开发之中很常用的控件。在往树列表中插入节点后，有时候需要对其节点进行排序，在涉及首字母、数字排序时，可以采用树列表自身的排序方法，但在需要一些特殊的排序条件时，其自身自带的排序方法便存在很大的局限性，本文主要介绍如何自定义树列表节点的排序规则。

二、功能说明

要自定义树列表排序规则，需要先找到树列表中自定义排序规则的回调函数，该回调函数如下图所示：

![](https://pic1.zhimg.com/v2-565f55bc901579b3eb593d4868132210_b.jpg)

再来，创建回调函数，在该回调函数中定义树列表排序的具体规则。例如，自定义树列表节点根据节点颜色进行排序，排序顺序依次是：原色-蓝色-祖母绿-红色。在该回调函数中，返回值可能为0、正值、负值，返回值为0时表示节点1和节点2相同，无需更改位置；返回值为正值时表示节点1大于节点2，即节点1在节点2前面；返回值为负值时表示节点1小于节点2，即节点1在节点2后面。依照此逻辑，我们可以根据比较的优先级，对其节点的颜色进行排序，具体代码如下图所示：

![](https://pic2.zhimg.com/v2-9396f446c84963a95f6eb33ad57ae3d9_b.jpg)

最后在界面初始化回调函数中让树列表的排序规则回调引用该回调函数即可，具体代码如下图所示：

![](https://pic2.zhimg.com/v2-e9ae4e2dcd1aba06a0b95480be00457d_b.jpg)

最终效果如下图所示：

![](https://pic3.zhimg.com/v2-8db8cb3b69e71a99eed55ac8fd0201ba_b.jpg)

三、总结

树列表的自定义排序规则回调函数是一种自由度很高的方法，用户可根据自身需求对其树列表节点进行自定义排序。