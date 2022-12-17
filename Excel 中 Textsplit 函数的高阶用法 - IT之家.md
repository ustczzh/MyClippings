# Excel 中 Textsplit 函数的高阶用法 - IT之家
[Excel 中 Textsplit 函数的高阶用法 - IT之家](https://www.ithome.com/0/661/833.htm) 

 俗话说，有分就有合。

那么在 Excel 函数中，**合就是 Textjoin，分就是 Textsplit****。** 

Textsplit=Text（文本）+Split（拆分），顾名思义，它是用来对文本进行拆分的函数，既可以按照列拆分，又可以按照行拆分。

它有五个参数：

```
\=TEXTSPLIT(text,col\_delimiter,\[row\_delimiter\],\[ignore\_empty\],\[pad\_with\]
```

> 第一参数 text：需要拆分的文本；
> 
> 第二参数 col\_delimiter：列分隔符；
> 
> 第三参数 \[row\_delimiter\]：\[行分隔符\]；
> 
> 第四参数 \[ignore\_empty\]：\[是否忽略空单元格\]；
> 
> 第五参数 \[pad\_with\])：\[出错时填充的值\]。

将 Textsplit 搭配其他函数，有更多更神奇的玩法

下面跟着我，一起来体验一下吧~

我会用三个案例，完成下图中，从左表到右表的数据整理。

![](https://img.ithome.com/newsuploadfiles/2022/12/69da0f0f-d85d-4187-9f75-f4870b4cf4ec.png)

![](https://img.ithome.com/newsuploadfiles/2022/12/906d8027-cb5f-487f-b4fc-4b80cb76c415.png)

**统计科目数**
---------

**案例一：统计每个人购买的课程科目数。** 

如下图，在 C2 单元格输入公式并向下填充：

```
\=COUNTA(TEXTSPLIT(B2"、"))
```

![](https://img.ithome.com/newsuploadfiles/2022/12/5e0db185-999c-41a6-a2c2-9bc468093a40.png)

**思路解析：** 

我们先使用 Textsplit 函数，将字符串按照分隔符「、」进行拆分。

最后利用 Counta 函数，统计非空单元格的个数，就是科目个数。

![](https://img.ithome.com/newsuploadfiles/2022/12/5199945a-57f9-4a66-96c9-508a8d6f8a8c.png?x-bce-process=image/format,f_auto)

是不是很简单~ 下面我们来看第二个案例。

![](https://img.ithome.com/newsuploadfiles/2022/12/af102a76-7198-4fd9-a6ef-2f6d93691172.png?x-bce-process=image/format,f_auto)

**重复姓名的指定次数**
-------------

**案例二：将姓名按照指定科目数整理为一列。** 

如下图，在 D2 单元格，输入如下公式：

```
\=TEXTSPLIT(CONCAT(REPT(A2:A6&"、",C2:C6))"、",TRUE)
```

![](https://img.ithome.com/newsuploadfiles/2022/12/5e2f488c-c97d-4406-aba0-8339aa663801.png?x-bce-process=image/format,f_auto)

**思路解析：** 

我们先将需要重复次数的姓名与分隔符进行拼接，接着利用 Rept 函数重复对应姓名的个数。

> Rept 函数的作用是根据指定次数重复文本；
> 
> \=Rept (文本，次数)

如下图：

![](https://img.ithome.com/newsuploadfiles/2022/12/809ca32a-0e56-4259-9d53-b363baffe040.png?x-bce-process=image/format,f_auto)

然后利用 Concat 函数合并。

> Concat 的功能是连接列表或文本字符串区域，即：=Concat (文本区域)

![](https://img.ithome.com/newsuploadfiles/2022/12/ea09d259-4d73-4702-82bf-0c1be2a63fd3.png?x-bce-process=image/format,f_auto)

最后将合并后的文本，利用 Textsplit 拆分到行，就是我们想要的结果。

![](https://img.ithome.com/newsuploadfiles/2022/12/3f1a3bcd-2858-41b3-b10a-a047269f96d8.png?x-bce-process=image/format,f_auto)

继续来看下一个案例~

![](https://img.ithome.com/newsuploadfiles/2022/12/7f272731-61c9-4dbd-99ef-6aa7213ebf02.png?x-bce-process=image/format,f_auto)

**拆分分隔符到行**
-----------

**案例三：将购买科目列按照分隔符拆分到一列。** 

如下图，在 E2 单元格中，输入如下公式：

```
\=TEXTSPLIT(TEXTJOIN("、",TRUE,B2:B6)"、")
```

![](https://img.ithome.com/newsuploadfiles/2022/12/7bcef81f-eae9-4cb5-92c5-e0a16156b915.png?x-bce-process=image/format,f_auto)

**思路解析：** 

我们先将购买科目列的数据区域，用 Textjoin 函数按照「、」进行合并。

> Textjoin 函数，可以将单元格区域按照指定分隔符进行合并；
> 
> \=Textjoin (分隔符，是否忽略空值，数组 / 单元格区域)；
> 
> 如果要忽略空值就填 True，不忽略空值就填 False。

如下图：

![](https://img.ithome.com/newsuploadfiles/2022/12/c18080de-58bd-4474-8c55-797dc0864ff1.png?x-bce-process=image/format,f_auto)

最后将合并后的文本，使用 Textsplit 函数进行拆分到行，就直接搞定！！！

![](https://img.ithome.com/newsuploadfiles/2022/12/5b9d45e5-8506-4d49-a660-e47bf82223e0.png?x-bce-process=image/format,f_auto)

细心的你，一定不难发现。

拆分指定字符，并整理为一维表，其实就是上面案例的合并。

![](https://img.ithome.com/newsuploadfiles/2022/12/3bbce858-c117-44ff-9df8-1a7d0d96f693.png)

这样整理成一维表的好处是，**方便我们后续做数据透视表分析统计。** 

![](https://img.ithome.com/newsuploadfiles/2022/12/23994e8f-f3df-497b-8b81-c5eb516f48c3.png?x-bce-process=image/format,f_auto)

这下，大家体会到 Textsplit 函数的强大之处了吧？

![](https://img.ithome.com/newsuploadfiles/2022/12/ddf7281a-3a33-4f57-ba63-e11f603a8a0a.png)

**总结一下**
--------

本文介绍了 **Textsplit 函数的高阶用法，即搭配其他函数可以发挥出更强大的数据整理能力。** 其中提到了三个案例：

❶ **统计科目数** (Textsplit，Counta)

❷ **重复指定次数** (Rept，Concat，Textsplit)

❸ **拆分分隔符到行** (Textjoin，Textsplit)

Textsplit 函数这么好用，那么要不要学呢？

不建议学，因为......

**需要 OFFICE 365 才支持！**

更低版本 Excel 和 WPS 目前还不能使用~

本文来自微信公众号：[秋叶 Excel （ID：excel100）](https://mp.weixin.qq.com/s/nwfy2d5iB48VBw4Z0JnB2w)，作者：小爽，编辑：小音、竺兰