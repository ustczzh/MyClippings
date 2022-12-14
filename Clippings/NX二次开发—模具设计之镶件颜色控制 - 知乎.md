# NX二次开发—模具设计之镶件颜色控制 - 知乎
作者：谢买粮 审校：陈建红

适用版本：NX6以上

1.  **简介**

在塑胶模具设计过程中，为了使在设计过程中和加工过程中更容易理解工件上的特征的作用，通常把在3D模型中，把实体上的进行分类并上不同的颜色，常用的分类有胶位面、分型面、插穿面、镶件孔、顶针孔等等，这样做有利于减少工作中的失误，如下图镶件（图1），胶位面和镶件其它面用了不同颜色；拆镶件通常有三种方法，一种是用画镶件的截面，然后用分割功能把体分成两个体，这种方法拆镶件后，镶件上的胶位面颜色能够保持，但镶件体其它面的颜色同模仁体，后续要写大量的代码修改面的颜色，第二种方法是先把镶件包围盒块做出来，指定颜色，再用求交和求差的功能把镶件做出来，这种方法如果使用NX默认的参数，达不到想要的效果，但对NX的默认建模参数进行修改后，能达到要求，本技巧将讲解修改建模参数对实体颜色面的影响。

![](https://pic4.zhimg.com/v2-e0c99ab8e2a0f244ec95a37c0934ea27_b.jpg)

图1

**二、举例说明**

修改“建模首选项”里的“布尔运算面属性”值能改变布尔运算后面的颜色，NX默认是选中目标体（图2），选中“目标体”做出来镶件的效果如图3，镶件面颜色与镶件孔颜色不符合要求。

![](https://pic1.zhimg.com/v2-077a1c905771eaf6c2a2967944d79260_b.jpg)

图2

![](https://pic4.zhimg.com/v2-3eed2ad69fc2436bbcd57dae30eb6e87_b.jpg)

图3

选中“工具体”做出来镶件的效果如图4，镶件面颜色与镶件孔颜色符合要求。

![](https://pic3.zhimg.com/v2-a7d357e762af464c063bcf1986b8a66e_b.jpg)

图4

修改此设置的方法C#代码： TheSession.Preferences.Modeling.BooleanFaceProperties = NXOpen.Preferences.SessionModeling.BooleanFacePropertiesInheritance.ToolBody，在使用前先记录原先的值，使用完成后再把改回去。

**三、总结**

本技巧实际讲解了布尔运算在一些特殊场景下的使用方法，这部分对开发来说非常简单，关键是应用知识，熟练应用对二次开发来说事半功倍；通过本技巧，希望对NX二次开发初学人员有所帮助。