# 将NX图标变为灰色不可选 - 知乎
作者：金雷 审校：叶齐天

适用版本：NX6.0以上

语言：C#

一、概述

在我们开发工作中，大的工具集项目开发中，需要先交付部分功能给客户使用，为了整体的美观和以后客户的使用交互体验的友好和连贯性，需要搭建整体的菜单框架，设置其中的按键不可选，让可使用或者测试的功能可以正常使用。以下提供代码和非代码方式实现

二、详细说明

1\. 代码方式：通过SetButtonSensitivity()函数设定

1.1 首先要用AskButtonId()获取按钮的BUTTON的ID

![](https://pic1.zhimg.com/v2-7b2c6bdc1162057b82893b979250ccb4_b.jpg)

图1

参数：

1.  button\_name是这个按钮在NX中的BUTTON名称，比如NX中“新建”的按钮值为：UG\_FILE\_NEW
2.  button\_id输出ID值，用来等会设置这个按钮的状态

1.2 用SetButtonSensitivity()设定状态

![](https://pic4.zhimg.com/v2-43828e92214e43665304b4ab0c9514bf_b.jpg)

图2

参数：

1.  button\_id就是刚才函数输出的值
2.  state为状态值，有ON和OFF

ON（按钮可选）

OFF（按钮不可选，显示为灰色不可选）

1.3 举例

以NX10为例，自己新建一个开发菜单，设置其中的第三个按钮名称为“BUTTON3”（设系统内部的按钮不可选同理），代码如下：

![](https://pic2.zhimg.com/v2-614987f39a6da1bbe78ad3343e68f079_b.jpg)

2\. 非代码方式：编辑菜单脚本文件

2.1 用记事本打开在NX二次开发的路径下“startup”文件夹下的“.men”文件。在ACTIONS前一行添加“SENSITIVITY OFF”。

![](https://pic1.zhimg.com/v2-ec12ace283b6682d64b16ddfe193b16c_b.jpg)

图4

2.2 保存并关闭菜单脚本文件，重新启动NX（菜单必须挂在NX里面）

3\. 运行结果：

![](https://pic1.zhimg.com/v2-6d592b5dcc0271725d235c489263b9e0_b.jpg)

图5（正常样式）

![](https://pic2.zhimg.com/v2-81d06d3685a313d11efdffcbf114fe7d_b.jpg)

图6（最后效果）

三、总结

通过该操作，可以方便的进行菜单按钮的灰显设置。展现菜单功能条的全貌同时，不影响程序的使用和测试环境的搭建。