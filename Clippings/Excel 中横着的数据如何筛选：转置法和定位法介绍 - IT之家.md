# Excel 中横着的数据如何筛选：转置法和定位法介绍 - IT之家
[Excel 中横着的数据如何筛选：转置法和定位法介绍 - IT之家](https://www.ithome.com/0/674/312.htm) 

 原文标题：《谁说横着的数据不能筛选？！2 种方法，5 秒搞定，so easy！》

昨天距离下班还有俩小时，我伸了伸懒腰，想着晚上吃点什么好。

却冷不丁听到领导在叫我：小兰，我刚发了一份表格给你，你处理一下，将每月的销售额给我整理出来，尽快给我！

我：好的好的。

打开表格前：不就是**筛选每月销售额**吗，立马就能搞定，耽误不了我下班。

打开表格后：OMG，这表格是谁设计的？！

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/338704e0-8b1d-4886-9bf2-d2250a5aa9b0.png?raw=true)

（后面还有数据。）

想要找出每月的销售额，我们可以用筛选。

但是这里需要横着筛，常规的筛选只能竖着筛，怎么办呢？

我们先沿着常规的思路来想一下：

**既然数据需要竖着筛，而现在的数据都是横着的，那我们把表格竖过来不就好了？**

就像这样：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/9a174345-72f4-4e1d-9775-3ea72d80ea18.gif?raw=true)

不不不，放错了，应该是这样：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/df7e79f2-3a67-426c-820f-80cac2106f5b.png?raw=true)

下面我们就来看看怎么操作吧~

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/cbec4876-d2f3-4abb-b5f9-6eb07106ceee.png?raw=true)

**方法一：转置法**
-----------

想要像上图那样，将表格的行与列互换，把它竖起来，怎么做呢？

这里有 2 种方法：**复制粘贴转置和公式转置，统称为转置法。** 

先来看看第 1 种方法。

**👉 复制粘贴转置：** 

选中 A1 单元格，按【Ctrl+A】全选，然后复制粘贴，单击数据右下角的浮动图标，选择「转置」。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/29407c5c-f660-436d-81c4-aa1bc2920972.gif?raw=true)

之后就可以使用【筛选】功能筛选出每月的销售额了。

❶ 选中 A11 单元格，按【Ctrl+A】全选，按【Ctrl+T】一键转化为「超级表」，勾选「包含表标题」；

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/f55d731f-ba1e-4091-b99b-8148adcb4d7f.png?raw=true)

❷ 单击「列 2」筛选按钮，取消勾选「数量」-【确定】。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/6becac8d-6f72-460b-b2bd-b4a8ba83f7f5.png?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/2d5c9566-c80b-4cff-a5d0-b981907c059f.png?raw=true)

这样就完成啦！

但是这种转置方法有个缺点，如果源数据中增加或删减了数据，就需要再次复制粘贴。

那么，**怎样才能让目标区域的数据，随着源区域数据的变动而更新呢？**

使用函数公式转置就 OK 了。

**👉 公式转置：** 

❶ 在新区域中选中和源区域对应的数据范围，保持选中状态。

比如这里的源区域是 9 行 13 列，那新区域就需要选择 13 行 9 列。

（其实就是将行数与列数换一下~）

❷ 输入公式：

```
=TRANSPOSE(A1:M9)
```

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/f821988b-eeda-436e-ab5d-9a1bf993bcb2.png?raw=true)

❸ 按组合键【Ctrl+Shift+Enter】就好啦~

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/c6f6f2b8-6d71-4442-81cf-50b298349e9d.png?raw=true)

❹ 像之前一样正常筛选即可！

另外，这里说一下哈！

如果觉得源数据和转置后的数据，放在同一个工作表会显得很乱。

可以新建工作表，转置时将数据放在新的工作表里面。如下：

**👉 复制粘贴转置：** 

操作方法：

新建一个工作表（Sheet6），在 Sheet2 中按【Ctrl+A】全选表格，复制；

切换到 Sheet6，粘贴，单击数据右下角的浮动图标，选择「转置」即可。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/64d4c3b5-da8e-4690-8120-afe975c5dadf.gif?raw=true)

**👉 公式转置：** 

操作方法：

在新建的工作表（Sheet7）中选中和源区域对应的数据范围，保持选中状态；

然后输入函数：

```
=TRANSPOSE
```

单击源数据所在工作表（Sheet1），选择源数据，输入「)」，按组合键【Ctrl+Shift+Enter】就可以啦。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/4543e29e-f1e2-47e7-82f9-df30cf05f064.gif?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/0a827cc4-2e94-4977-92ec-4b19ea564613.png?raw=true)

**方法二：定位法**
-----------

转置法需要先改变表格结构，那么，如果想直接横向筛选，不改变表格结构呢？

当然也是可以的，定位**「****行内容差异单元格」**就行。

**操作方法：** 

❶ 选中 B2 单元格，按【Ctrl+Shift+→】选中当前行，按【Tab】键移动到 C2 单元格（销售额）；

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/ec6543e8-96ae-4bc3-80b1-1bc2a82d510d.png?raw=true)

❷ 按【Ctrl+G】键调出【定位窗口】，单击「定位条件」；

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/444b927d-e27c-4d61-8c20-8c5b491053eb.png?raw=true)

❸ 在弹出的窗口中，选中「行内容差异单元格」，单击【确定】。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/f5b8dcc5-70a9-43c0-8dbe-6919c7cd7777.png?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/ff8992e6-bfdd-4d0a-944f-19065efdc8ee.png?raw=true)

❹ 按【Ctrl+)】键隐藏其他列。

PS：中文「）」或英文「)」都可以。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/00814383-239e-4a55-a0a8-9ce17f13096d.png?raw=true)

每月销售额就都筛选出来了。

相较于转置法，这种方法更简单，省了使用【筛选】功能对数据进行筛选这一步。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-19%2010-19-47/f5e39279-85e8-433c-83d2-69b7407e4268.png?raw=true)

**总结**
------

以上就是「横向筛选」的解决方法，总的来说，有 2 种方法：

❶ **转置法：复制粘贴转置、公式转置；**

❷ **定位法：定位「行内容差异单元格」。** 

本文来自微信公众号：[秋叶 Excel （ID：excel100）](https://mp.weixin.qq.com/s/q5cLEaXC53OcdzFv_GQZnQ)，作者：竺兰