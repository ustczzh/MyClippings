# NX工程图之局部放大图标签设置 - 知乎
[NX工程图之局部放大图标签设置​mp.weixin.qq.com/s/mK47GrXjdbrzXyyt4icGbg![](https://pic4.zhimg.com/v2-2b3ed7b48facdb26c207246988910833_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/mK47GrXjdbrzXyyt4icGbg)

作者：彭军华 审校： 黄健泳 适用版本：NX9.0

在NX工程图中，做完局部放大图后，默认的局部放大图标签是带有字母前缀的上下结构（**图1**）。

![](https://pic4.zhimg.com/v2-7cc22eda4714da3ba18d0fdf44954e4f_b.jpg)

图1

按机械制图标准，局部放大图标签是不需要字母前缀的，只需在视图标签和比例之间添加符号“**—**”（**图2**）。

![](https://pic1.zhimg.com/v2-21dfdd79dda77a008a588b9b3bad5ac4_b.jpg)

图2

针对这种情况，通常我们在做完局部放大图后，会对局部放大图的标签进行设置，在视图比例前添加前缀“**<A>**”，以达到我们需要的效果（**图3**）。

![](https://pic1.zhimg.com/v2-4095dc0b6b5f478c35c9cdbff6b6b604_b.jpg)

图3

但是，在NX9.0版本中，当我们设置完局部放大图标签后，并不会出现我们需要的效果，而是在视图名称和比例之间出现符号“**二**”（**图4**）。

![](https://pic4.zhimg.com/v2-fa414fc00827490635f442b2aca6181f_b.jpg)

图4

为了解决这个问题，我们可以对视图标签做进一步修改。将视图比例的“**前缀字符高度因子**”提高（如**1.5**）。这样修改完成以后，视图标签与比例之间就不会出现符号“**二**”，而是会出现我们需要的效果（**图5**）。

![](https://pic1.zhimg.com/v2-74c8b4e7df2c5e6a71447a774a2716e8_b.jpg)

图5