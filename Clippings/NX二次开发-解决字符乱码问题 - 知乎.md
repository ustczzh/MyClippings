# NX二次开发-解决字符乱码问题 - 知乎
作者：叶齐天 审校：金雷

适用版本：NX6以上

1.  概述

NX二次开发的程序，虽然内部执行的操作和手动是一样的，但是输入的内容可能会出现乱码的问题。尤其当程序部署到其他环境的时候最容易出现。本章讲解如何解决该问题。

1.  详细说明

以下是用程序保存并关闭部件后，重新打开NX跳出的提示。该报错出现的原因是历史部件的预览图文件路径中文出现乱码，导致NX无法定位文件。

![](https://pic2.zhimg.com/v2-a04fbd6ceb327b5c6e6004647fcefb0d_b.jpg)

图1

当然出现乱码的情况还有很多，比如读取的txt文件代码找不到想要的信息，有时候就是因为读取到的是乱码。

解决问题的办法很简单，将编译用的代码文件用其他格式另存即可。

用记事本的方式打开cs文件，选择文件->另存为

![](https://pic1.zhimg.com/v2-ee988683a3b00f1e8f9c7faa7e0ba230_b.jpg)

图2

当前的编码为“带有BOM的UTF-8”，我们将其改为“UTF-8”即可。

![](https://pic4.zhimg.com/v2-5602b53179dc84823a912bbb55de5e3b_b.jpg)

图3

重新编译并重复操作，预览图正常显示。

![](https://pic1.zhimg.com/v2-016aa46253d76de71369d0cd05719624_b.jpg)

图4

如果还不能正常显示，请检查开发用的NX版本和部署的NX版本是否一致（包含小版本和补丁），此外还需要检查自己的电脑的系统，是32位还是64位，如果是32位，编译的时候平台选项可以尝试设置为AnyCPU。

1.  总结

开发过程中所有乱码的问题基本都是和编码有关，尝试用不同的编码进行编译多数时候都能解决问题。此外，尽可能地还原问题的环境也很重要。对于NX的二次开发程序来说，过于冗长的带中文、日期的目录也会导致程序问题。