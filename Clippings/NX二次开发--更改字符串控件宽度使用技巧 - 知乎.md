# NX二次开发--更改字符串控件宽度使用技巧 - 知乎
[NX二次开发--更改字符串控件宽度使用技巧​mp.weixin.qq.com/s/vsfnqyFDqq23TdkgyRXqUg![](https://pic4.zhimg.com/v2-f233a109c7b417327bcb213ae36e0ab7_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/vsfnqyFDqq23TdkgyRXqUg)

作者：谢买粮 审校：陈建红

适用版本：NX6.0以上

1.  **简介**

BlockUI是开发NX对话框的可视化工具,生成的对话框能与NX集成,让用户更方便、更高效地与NX进行交互操作；字体串控件是常用控件之一,字符串控件有四种形式,其中Combo形式，既可下拉选择，也可以手工输入，在应用中，有时候下拉的内容过长，而控件默认宽度是固定的，导致不能完整显示，用户体验降低，下面将演示通过调用内部API的方法实现修改控件宽度。

**二、举例说明**

首先，使用反编译工具，查看libuifw.dll内部函数，流程如图1，注意32位NX和64位NX是不一样的，需分开查看;

![](https://pic4.zhimg.com/v2-4997be6314eeb7d416f1fcdb062fcd97_b.jpg)

图1

然后写代码调用，示例代码如图2.

![](https://pic1.zhimg.com/v2-c86cbba2c1ba7daf3e058dff02adeeac_b.jpg)

图2

最后测试设置效果，如图3，注意：要在“initialize\_cb”方法内调用才有效。

![](https://pic2.zhimg.com/v2-7f84d15708b14e23f6f620ddacba81a9_b.jpg)

图3

**三、总结**

本文讲解了修改NX字符串控件Combo类型控件宽度的方法，开发人员可根据实际需要来进行采用。