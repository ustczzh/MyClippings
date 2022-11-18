# NX二次开发-创建必检符号 - 知乎
[NX二次开发-创建必检符号​mp.weixin.qq.com/s/slMgGKazjZ2mST6z9PySEA![](https://pic2.zhimg.com/v2-bf9a8a62b6be68faa3e1279044f1f675_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/slMgGKazjZ2mST6z9PySEA)

作者：张晓峰 审校：徐涛 适用版本：NX6以上

一、概述

在NX二次开发的时候，我们经常会用到使用代码去创建必检符号。在NX的GC工具箱中可以直接手动创建，但本篇技巧讲述的是如何使用NXOpen的方式去创建必检符号。

二、功能说明

必检符号，本质上来说就是自定义符号。通过创建自定义符号，并将相应的一些属性写入到指定的对象中。

三、实现方法

![](https://pic1.zhimg.com/v2-3c3fbf7bd6c7742a4461968f2f2c3bb4_b.jpg)

3.1、根据“UGII\_BASE\_DIR”环境变量，找到自定义符号的存储路径，并创建自定义符号对象，如图1所示。

图1 创建自定义符号

3.2、获取自定义符号的Handle值，并写入到Note中，如图2所示。

![](https://pic2.zhimg.com/v2-122284ce7796b8dc8070198cbc47857d_b.jpg)

图2 创建Note

![](https://pic3.zhimg.com/v2-00cec9643ae5d16b41d168126e9a5826_b.jpg)

3.3、将自定义符号的序号以及Note的Handle值写入到尺寸的属性中，并设置与尺寸的显示视图一致，如图3所示。

图3 设置属性和视图

3.4、实际效果如图4所示。

![](https://pic4.zhimg.com/v2-6e601f69b332db1dcfb1739d378cb073_b.jpg)

图4 实际效果

四、总结

通过使用代码的方式创建必检符号，可以极大地减少用户手动创建的时间，提升效率。