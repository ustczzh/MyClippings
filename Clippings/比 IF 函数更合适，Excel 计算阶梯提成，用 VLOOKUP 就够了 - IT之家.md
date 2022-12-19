# 比 IF 函数更合适，Excel 计算阶梯提成，用 VLOOKUP 就够了 - IT之家
[比 IF 函数更合适，Excel 计算阶梯提成，用 VLOOKUP 就够了 - IT之家](https://www.ithome.com/0/662/150.htm) 

 最近我在苦恼一个问题。

前阵子老板让我用 Excel 给公司同事们算提成，层层嵌套的 IF 函数把给我写伤了。

可那天瞥到新来的同事**只用了很简短的一串函数，就算好了提成。** 

为什么？这到底是为什么？

在我苦思冥想之下，终于…… 还是没想出来。

好吧，那我只有使出杀手锏了！

我：这位大哥，请问你是怎么算提成的呢？

他：提成？我用的 VLOOKUP 函数啊。

一起来看看吧👇👇👇

**👉 具体操作：** 

❶ 点击要计算提成的单元格，在编辑栏输入「=VLOOKUP (」。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-12-19%2021-48-30/5d262c19-4528-48ae-96ea-c1f6dc1edc0b.png?raw=true)

❷ 第一个参数是「查找目标」，我们点击对应的销售额，然后输入「,」。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-12-19%2021-48-30/0720cba8-27c9-4aa5-b405-5bb60137718c.gif?raw=true)

❸ 第二个参数是「查找范围」，我们选中提成表（不包含标题哦~），输入「,」。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-12-19%2021-48-30/d2fc77a2-cd97-4432-9ec1-ff80482b5c97.gif?raw=true)

❹ 第三个参数是「返回查找范围中的第几列」，我们输入「2」（这里提成在查找范围的第 2 列），同样输入「,」。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-12-19%2021-48-30/caeaa49f-badd-460c-a3ed-8a3e68cafb3e.gif?raw=true)

❺ 第四个参数是「匹配类型」，我们输入「1」（近似匹配）和「)」。

按下【Enter】键，在单元格右下角双击鼠标【左键】完成填充，最后在【开始】选项卡中点击「百分比样式」。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-12-19%2021-48-30/5b6be51d-2030-45c8-b9a9-f93aca549fd2.png?raw=true)

完整公式如下：

```
=VLOOKUP(B2,$A$11:$B$16,2,1)
```

怎么样，是不是比一层一层嵌套 IF 函数简单多啦？

俗话说的好，时间就是金钱，看来我又学会了一个省钱小妙招~

本文来自微信公众号：[秋叶 Excel （ID：excel100）](https://mp.weixin.qq.com/s/Qr84-HIL4LqO2oMpeqtwsA)，作者：小音，编辑：雅梨子