# NX工程图模板中解决材料填写的问题 - 知乎
作者：黄玮玮 审校：陈昂

适用版本：NX所有版本

在机械设计手册第一卷第1章【机械工程材料】里面有说明，不同的材质（比如A3和45），不同的形式（比如棒材和板材），不同的状态（比如薄板和厚板）等等都不一样。以下为标记标准：如图1，也有可能像图2一样，只标注一行。我们怎样在制图模板中一个单元格中同时解决两种情况呢？

![](https://pic4.zhimg.com/v2-5da76bb22aa304be919f182d0646785f_b.jpg)

图1

![](https://pic2.zhimg.com/v2-cab1354da29b48a763d15069a0aedda1_b.jpg)

图2

以下就是解决方案：

1.  分析两种标准发现，图1中的格式是A又B分之C，图2的格式就是一个A，分别给A.B.C三个属性，假如：A:材料名称;B:材料上标；C:材料下标三个属性，填写到NX的属性中。

![](https://pic3.zhimg.com/v2-23ff3fba4cd8f866305a0b7310a4d516_b.jpg)

![](https://pic2.zhimg.com/v2-92f97fe6cfc1f2be08415ac82d767db1_b.jpg)

图3

1.  在制图模块下，在单元格中导入属性:材料名称，如图4（按序号操作）；

![](https://pic3.zhimg.com/v2-b6b82651afda42a4ef28ce8463b5b1ba_b.jpg)

![](https://pic4.zhimg.com/v2-91c7b43bc9734b896670beb61d56e143_b.jpg)

图4

1.  在单元格中导入属性:材料名称；导入后会显示：<WRef1\*0@材料名称>，如图5。

![](https://pic2.zhimg.com/v2-ccf2c1992b661531f191ea64b7b67879_b.jpg)

图5

1.  选中单元格，点击“编辑文本”命令，按照对应的序号进行操作，如图6，但这个时候只会显示出如“图7”的样式，这个时候复制“<WRef1\*0@材料名称>”替换材料上标，材料下标，注意修改WRef后面的数字，可按顺序修改，结果如图8

![](https://pic3.zhimg.com/v2-e76b673f81bbe1dfeb4d6ec47373ffae_b.jpg)

图6

![](https://pic1.zhimg.com/v2-300ea240a26d8ee5ddd62140ce086eec_b.jpg)

图7

![](https://pic2.zhimg.com/v2-d7ceda160ea5143967dfc23c8f54b7e9_b.jpg)

图8

1.  点击确定后，就设置好了。对属性进行填写，然后进行测试，如图9。图10是填写一行的材料测试结果。

![](https://pic4.zhimg.com/v2-785fdab3cf8a93ef8e81eafe34cfa1e3_b.jpg)

图9

![](https://pic4.zhimg.com/v2-4187df75360e2d55198033e8b01b6147_b.jpg)

图10

以上就是工程图模板中解决材料填写的方法，希望能对大家有所帮助。