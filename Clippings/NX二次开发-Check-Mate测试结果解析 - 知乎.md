# NX二次开发-Check-Mate测试结果解析 - 知乎
作者：王键鑫 审校：叶齐天 适用版本：NX7.5及以上

一、概述

在进行NX二次开发时，某些业务场景需要进行Check-Mate相关的开发，那么如何解析Check-Mate测试结果以及如何获取某些失败的检查项关联的NX对象呢，下面进行详细说明。

二、功能说明

参考前一篇技巧《NX二次开发-Check-Mate测试执行》可知，利用NXOpen.Validate.Validator类与NXOpen.Validate.Parser类中的方法，可从代码层面执行Check-Mate测试，那么现在讲解一下如何解析Check-Mate测试结果。

在执行Check-Mate测试时，可以指定生成Check-Mate外部日志文件，该文件包含测试结果信息，文件格式为PLMXML，可用解析XML的方式对文件进行解析。

![](https://pic1.zhimg.com/v2-2c6767b866e73ac01a0c1060978fdcb8_b.jpg)

图1 Check-Mate 代码中生成外部日志文件配置选项

![](https://pic4.zhimg.com/v2-5d7a82cae36ad7cedd4f5fc0b7eaf61b_b.jpg)

图2 Check-Mate功能中生成外部日志文件配置选项

执行Check-Mate测试后，可以在指定路径下找到日志文件，日文件名称以创建时间为规范，例如：2021Mar30151101.xml，即2021年3月30日15分11秒01毫秒。

![](https://pic1.zhimg.com/v2-6b244f261c0b9fbd0c05bd7713261d3c_b.jpg)

图3 Check-Mate 外部日志文件

![](https://pic1.zhimg.com/v2-071d3dbeea0ee00eef093fb4e2ec91e8_b.jpg)

图4 Check-Mate 外部日志文件内容

文档内容中包含Check-Mate执行内容与结果，包括检测部件、检测包信息、检查器信息、错误信息、错误对象等，基于该文档进行解析，即可获取到Check-Mate测试结果的具体信息。

三、总结

综上所述，可以通过上诉的函数执行Check-Mate测试，并基于外部日志信息可获取到Check-Mate测试结果的具体内容。