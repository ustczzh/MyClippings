# NX二次开发编辑部件族信息 - 知乎
[NX二次开发-编辑部件族信息​mp.weixin.qq.com/s/zPJnJhUc7wJuVip3lg9Xtw![](https://pic4.zhimg.com/v2-504893388c4abe0ac9efe03df6d42f73_ipico.jpg)
](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/zPJnJhUc7wJuVip3lg9Xtw)

作者：吴亚 审校：王镭

适用版本：NX6.0及以上版本

概述
--

在某些设计过程中，需要提取部件族信息，同时会根据具体需要修改部件族信息。一些较大的部件族零件，打开修改时间较长。其实可以通过UFUN函数来实现部件族文件信息的读取与修改。

具体实现方法
------

1.  打印部件族信息的方法如下:

![](https://pic1.zhimg.com/v2-4af5c224ae4fe3a5f38dea6ff1992ad4_b.jpg)

1.  编辑部件族信息的方法如下：

![](https://pic2.zhimg.com/v2-bdbf5a11864ba2b510686035cf49d9d5_b.jpg)

1.  主函数如下：

![](https://pic2.zhimg.com/v2-19f3024587aa87e10f638062168df4e5_b.jpg)

1.  结果如下：

![](https://pic3.zhimg.com/v2-0b30d964d8e2c01b7840e4fc1855de86_b.jpg)

总结
--

结合上述代码，再配合Block UI界面，就可以实现在NX二次开发界面上修改部件族文件信息；因为是开发功能，可以定制只修改一部分属性信息，那么所显示信息会简洁明了；如果想做的再完美点，可以再加入一些属性信息过滤，帮助工程师快速定位到需要修改的信息行。