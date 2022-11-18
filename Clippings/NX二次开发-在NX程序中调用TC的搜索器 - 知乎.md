# NX二次开发-在NX程序中调用TC的搜索器 - 知乎
[NX二次开发-在NX程序中调用TC的搜索器​mp.weixin.qq.com/s/E63D5X7h5TSbLF6SJFw0kA![](https://pic1.zhimg.com/v2-2ef8745c7919ff55f629ced96f8409c0_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/E63D5X7h5TSbLF6SJFw0kA)

作者：金雷 审校：纪新杭

适用版本：NX8.5以上

一、概述

NX二次开发中，经常会有需要获取到TC中的一些数据集信息，尤其是搜索只能通过TC端搜索的内容。

本篇技巧将介绍如何实现这一需求。

二、功能说明

本篇以NX11（NX11.0.2.7），利用ID号，查找对象的名称为例。简述如下：

a) 在帮助文档中，存在“PdmSearchManger Class”类，类中有一个“NewPdmSearch()”方法，如图1所示。

![](https://pic1.zhimg.com/v2-bb9398d9760d8dc563bddf85ce81cdfc_b.jpg)

图1

b）在帮助文档中，存在“PdmSearch Class”类，类中有一个“Advanced Method (entries, values)”方法，如图2所示。

![](https://pic3.zhimg.com/v2-a7da221f904e4e6a6421af37dfdde51a_b.jpg)

图2

c）获取结果的方法：在“SearchResult Class”类中，有三种获取结果的方法（如图3），分别获取ID号，对象名称和对象的类型。

![](https://pic2.zhimg.com/v2-c505647604901e4bc9d421cc555c0ae9_b.jpg)

图3

d）参考代码如图4所示。

![](https://pic1.zhimg.com/v2-450aeb5fe6d661bdfde68ae603411d38_b.jpg)

图4

e）TC中的对象和最终的输出结果如图5所示。

![](https://pic1.zhimg.com/v2-7f470d529f115e1f977f6b7047ae7c24_b.jpg)

图5

三、总结

通过以上操作，就可调用TC中的搜索器，搜索到我们需要的部件信息。方法中的参数通过增加搜索规则，缩小搜索范围。