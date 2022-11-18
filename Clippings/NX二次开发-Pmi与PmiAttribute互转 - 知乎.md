# NX二次开发-Pmi与PmiAttribute互转 - 知乎
作者：徐涛 审校：倪海

适用版本：NX所有版本

**一、概述**

在使用UG进行三维软件进行设计时，PMI是一个非常重要的模块，本文主要讲Pmi对象和PmiAttribute对象，这两个对象可以互相转化。

**二、功能说明**

Pmi对象是不可见的，是原始数据，元对象，只有一个，一个Pmi对象可以有多个PmiAttribute对象，它直接属于NXObjec。

PmiAttribute是可视对象，父类是Annotation，Annotation是DisplayableObject类的子类。PmiAttribute是Pmi在不同视图中的显示结果，它们相当于是父子关系，一个父与若干个子。

Pmi有默认的序号（在部件导航器-模型视图-各个视图中显示的PMI节点后面的括号里），但PmiAttribute没有。

这两者可以互相转化：

通过Pmi获得各个视图中的PmiAttribute：

![](https://pic3.zhimg.com/v2-c9a26bf816935755e6f8433cb8775516_b.jpg)

图1

通过PmiAttribute获得Pmi

![](https://pic2.zhimg.com/v2-4d40df7090aebd51c1ee356a677b61dd_b.jpg)

图2

**三、总结**

通过uf的cycle函数去遍历获取各个视图中的pmi，其实这里获取的都是PmiAttribute，想要获得PMI需要通过上面的方法转化。另一种方法就是直接通过部件的PmiManager直接获取所有PMI。