# NX制图尺寸线交叉打断应用技巧 - 知乎
作者：张磊 审校：刘卫民

适用版本：NX所有版本

在使用NX制图模块出图时经常会出现尺寸线自相交或尺寸线与中心线相重叠的情况，如图1，而遇到这种情况很多企业出图标准要求将某一尺寸线打断或把中心线打断，使得尺寸标识更加清晰，如图2所示。对于在NX软件中如何打断中心线或尺寸线这是大家问的最多的问题之一，今天就把方法介绍给大家。

![](https://pic3.zhimg.com/v2-795bd2aa37b4b0af7497cfc83af7129a_b.jpg)

![](https://pic1.zhimg.com/v2-2fb2dd86e220845ef16af28fdadb6138_b.jpg)

详细操作步骤如下：

Step1：打开NX图纸，点击“菜单”→“插入”→“符号”→打开“用户定义”；

![](https://pic3.zhimg.com/v2-0631e769dd576cf3703b6c7a2ded67da_b.jpg)

Step2:弹出“用户定义符号”对话框，在“使用的符号来自于”下拉菜单用选择“使用工具目录”；

![](https://pic4.zhimg.com/v2-c9680bd2119d05ff79462fa0ac033a1b_b.jpg)

Step3：在“使用工具目录”下左侧框中选择“ug\_default.sbf”,在右侧框格中下翻选择“GAP06”符号项；在符号大小定义依据中输入长度和高度值均为“1.5”（根据实际情况调整空白处大小），最后选择“添加到制图对象中”功能；

![](https://pic4.zhimg.com/v2-a3874a88e1de95643037c1d2c0e70157_b.jpg)

Step4：完成上一步选择后鼠标在绘图空白处点击鼠标中键（**关键操作**），此时NX状态栏会提示“选择要添加符号的制图辅助对象”；

![](https://pic3.zhimg.com/v2-aa45248a609d31588e60f661ed3090c2_b.jpg)

Step5:在绘图区域鼠标左键选择要打断的尺寸线，此时NX状态栏提示“之处制图对象上的位置”；

![](https://pic2.zhimg.com/v2-9a71cabc1185908cd58774c5e6f3d079_b.jpg)

Step6紧接上一步，鼠标直接在尺寸线需要打断的地方点击左键一次；

![](https://pic3.zhimg.com/v2-520d34f958874491890ce2bc61114ad2_b.jpg)

Step7：尺寸线即被成功打断；

![](https://pic1.zhimg.com/v2-13be2a795ddc8d6fd03d7d5829914e28_b.jpg)

Step8:按照以上方法，将符大小定义依据调整到5mm，可将中心线打断；

![](https://pic2.zhimg.com/v2-3cec175147757bb43d6bfd889c3a9e31_b.jpg)

以上就是尺寸线和中心线打断的应用技巧，希望在以后的制图工作中能够给大家提供帮助。