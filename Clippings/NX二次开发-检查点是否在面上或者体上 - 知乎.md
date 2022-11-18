# NX二次开发-检查点是否在面上或者体上 - 知乎
[NX二次开发-检查点是否在面上或者体上​mp.weixin.qq.com/s/2UN2umiXbb5\_knO5x2agyQ![](https://pic1.zhimg.com/v2-390b2b282279dc4816d83a28638380ac_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/2UN2umiXbb5_knO5x2agyQ)

作者：凌俊 审校：倪海

适用版本：NX7.5以上版本

在基于NX软件，通过二次开发实现某些功能的过程中，需要知道某些点是否在面上或者体上，从而来解决某些问题。例如，用Block UI选择点的时候，找到该点所在的面或者边。可以利用UF函数中提供的方法：UF\_MODL\_ask\_point\_containment实现。在C#中是AskPointContainment。

![](https://pic2.zhimg.com/v2-39d3be0748424fa43ca8c9bace35c449_b.jpg)

该方法通过输入的点坐标和某个具体的对象，输出点相对于该对象的位置关系。具体对象包括：面、边、实体、片体；位置关系包括点在对象里面、点在对象上和点在对象外面。只有实体存在点在对象里面。

下面举一个在一个部件中查找到某个点所在的面的例子，详细情况如下：

1、首先，通过该方法查找点所在的体上，位置关系为点在对象上

![](https://pic4.zhimg.com/v2-33e2564d05a5dc93fa41dbba6993eaf3_b.jpg)

2、再通过该方法，找到点所在面，并输出面

![](https://pic2.zhimg.com/v2-32503a33ffca4c320b10a421bc9cf525_b.jpg)