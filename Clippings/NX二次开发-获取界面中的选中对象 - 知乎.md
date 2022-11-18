# NX二次开发-获取界面中的选中对象 - 知乎
[NX二次开发-获取界面中的选中对象​mp.weixin.qq.com/s/aJluV50NK-KjK7uz\_rIhaQ![](https://pic2.zhimg.com/v2-7fb1d0f29d727a0f0f617f2ed6bab965_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/aJluV50NK-KjK7uz_rIhaQ)

作者：金雷 审校：纪新杭

适用版本：NX6.0及以上

一、概述

在NX开发过程中，有的时候只需要选择对象，即可满足程序运行要求。基于这种情况，为了简化用户操作，可不使用选择控件，用户在NX操作界面中选中高亮对象，点击菜单按钮，即可执行程序。

本文将主要介绍如何获取NX界面中的高亮对象。

二、功能说明

本篇以NX11为例，使用UF和NXOpen两种方式进行介绍。

方法一：UF函数

通过NX给的UF函数（UF\_UI\_ask\_global\_sel\_object\_list），可以获取到当前已经高亮的对象Tag，对应的解释如图1所示。

![](https://pic4.zhimg.com/v2-7b30daf1750552928e23ab7144e5ca67_b.jpg)

图1

根据该函数获取到的Tag值，获取到对应的TaggedObject，从而获取到对应的输入类型，参考代码如下图2所示。

![](https://pic1.zhimg.com/v2-ca19ab602b8bf5367a5443e822b07db4_b.jpg)

图2

方法二：NXOpen

通过NXOpen方法，获取到高亮对象的个数，通过遍历，获取到对象的TaggedObject，然后获取到对象的类型，参考代码如图3所示。

![](https://pic3.zhimg.com/v2-d617f8454b47cb24f9469d2e41665c4a_b.jpg)

图3

编译并执行程序，生成结果如图4所示：

![](https://pic4.zhimg.com/v2-48d98db9d7174960c08c82e8552f62cb_b.jpg)

图4

三、总结

NX提供了很多接口，我们需要从我们的实际需求出发，通过NX的接口，或者通过接口的配合，获取到我们需要的信息，然后进行进一步的操作。