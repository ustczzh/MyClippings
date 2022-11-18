# NX二次开发-设置装配加载选项 - 知乎
[NX二次开发-设置装配加载选项​mp.weixin.qq.com/s/IzZzxOTlryQ2Qmex-7zxxA![](https://pic1.zhimg.com/v2-a4b130f4c3bb7b1b55487d6f59e33c90_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/IzZzxOTlryQ2Qmex-7zxxA)

作者：吴亚 审校：王镭

适用版本：任何版本

概述
--

NX在打开装配时，因设计业务要求需要，可能会把零件放到多个文件夹，那么此装配拷贝到另一个工程师那打开，需要设置装配加载方式，否则会报无法加载的错误。当然我们可以通过手工方式去修改，但考虑到一些非常特殊的业务场景，需要频繁切换加载方式，那么使用开发方式可以很好的解决此问题。

步骤如下：
-----

1.  NX在打开装配时，需要设置加载方式，如下图所示：

![](https://pic4.zhimg.com/v2-a2d2782210dd02f8932a2cada405789f_b.jpg)

1.  设置装配【从搜索文件夹】或【从文件夹】或【按照保存的】打开：

![](https://pic3.zhimg.com/v2-22015324337458a06bbbbc9f79535db6_b.jpg)

1.  SetSearchDirectories方法只需要设置总目录就可以，不需要像NX界面那样每个子目录都加一遍。theUfSession封装的都是UFUN的方法，UFUN使用方法也是一样的。

![](https://pic1.zhimg.com/v2-23dbff3e3efb63b0bf0b1ba6b3d10b1c_b.jpg)

总结
--

NX装配加载方式设置结合自动判断所选装配是否存在子文件夹，或其他一些判断条件，可以实现三个加载方式智能切换，对提升客户体验及效率会有一定的帮助。