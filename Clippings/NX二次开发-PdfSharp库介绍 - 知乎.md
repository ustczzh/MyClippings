# NX二次开发-PdfSharp库介绍 - 知乎
作者：叶齐天 审校：金雷

适用版本：NX6以上

1.  概述

在开发过程中，经常会遇到导出PDF的需求，而在图纸模块，因为国内打印图纸习惯的关系，会有智能拼接的需求。（将多张不同图幅的图纸按照最大化利用纸张的要求自动排布），除了利用CGM的技术以外，也可以通过操作多个pdf文件进行拼接。本篇将给大家简单介绍pdfSharp，它是一个第三方的开源库，里面包含了许多对pdf文件的基础操作。

1.  详细说明

**安装方法**

首先打开Visual Studio的NuGet程序包管理界面。（此处为2019）工具->NuGet包管理器->管理解决方案的NuGet程序包。

![](https://pic4.zhimg.com/v2-80e33819a5c499f01384bb1203558ee7_b.jpg)

1.  NuGet包管理器菜单

选择“浏览”选项卡，输入PdfSharp并搜索，在右侧选中要安装的项目勾选并点击“安装”。

![](https://pic1.zhimg.com/v2-3d114b1091d2de1c7304a485ae3fd004_b.jpg)

1.  查找PdfSharp

安装成功以后，在“已安装”选项卡能找到安装的PdfSharp以及对应的项目。同时在右侧的解决方案导航器中，项目的引用下面可以找到对应的dll文件。

![](https://pic3.zhimg.com/v2-a2cbebb4eb2a13e27eb640d35683f91e_b.jpg)

1.  完成安装PdfSharp

**内容介绍**

关于内容的介绍，大家可以参考PdfSharp的官方网站：[http://www.pdfsharp.com/PDFsharp/](https://link.zhihu.com/?target=http%3A//www.pdfsharp.com/PDFsharp/)

在左侧的导航栏中，有详细的Introduction，还有包含例子的Samples等等，能帮助你快速上手。

![](https://pic2.zhimg.com/v2-e20ef911529884f4f2436912c8abd805_b.jpg)

1.  官方页面

以下的代码是最基础也是PdfSharp入门的代码示例，功能是新建一个pdf文件，并在页面上写下“Hello, World!”几个字符。

![](https://pic3.zhimg.com/v2-651bc557e38ec15f1e190b320b864066_b.jpg)

1.  代码示例
2.  总结

PdfSharp是一款功能强大的第三方开源Pdf编辑功能库，但是自身也存在局限，那就是无法直接将Pdf导出为图片文件，最多只能获取页面中的图片元素进行导出。如果要解决该问题，可以使用另一第三方开源库PdfiumViewer来辅助实现，或者使用官方的Acrobat.dll获取最完整的功能。