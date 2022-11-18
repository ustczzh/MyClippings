# NX二次开发-BlockUI多输入框界面的开发技巧 - 知乎
作者：薛剑腾 审校：叶齐天 适用版本：NX6以上

概述
--

在NX的BlockUI开发中我们经常会遇到用户需要在界面上设置一连串的输入控件的开发需求。这时更好的方案是通过树列表控件来满足对多个、不定数量输入的需求。但是有时我们也会遇到规定要用多个文本输入框、枚举框来进行输入的限制。我们可以通过重复大量的代码来实现这个需求，也可以参照下面的方法使代码更简洁、高效。

![](https://pic4.zhimg.com/v2-89713844757bdea27f001fc9aaef6763_b.jpg)

图 1

详细内容
----

### 布局类控件的Members属性

布局类的BlockUI控件（组、表、滚动窗口等）都包含“Members”属性。通过Members属性可以获取此控件下的所有子控件。

![](https://pic3.zhimg.com/v2-23f3e30e9cd650f8d69811a0857f254a_b.jpg)

图 2

调用UIBlock.GetProperty().GetArray("Members")可以获得一个索引型的PropertyList对象，通过这个PropertyList又可以获取到布局类控件下的子控件。以下为通过Members属性获取某BlockUI控件下一级子级或所有子级控件的代码：

![](https://pic3.zhimg.com/v2-c8a7c2db71b8fc7b412be260cd3707b2_b.jpg)

图 3

通过这种方式，我们可以在设计界面时将一系列的输入控件放在一个组控件下，通过组获取，还可以通过控件的类型进行过滤，最后通过顺序来获取和设置值。

### 示例

![](https://pic4.zhimg.com/v2-d60491d8e5972c3f0ac10435f0a4ffaf_b.jpg)

图 4

如上图所示的UI界面，我们先新建一个目标的组控件mGroup，并在里面添加足够多的输入控件，此处统一为字符串控件。我们可以在界面初始化时再设置输入控件的标题和显示状态。界面的DialogShown回调如下：

![](https://pic4.zhimg.com/v2-bbcda6179bbddc7c93c143890deb0723_b.jpg)

图 5

![](https://pic3.zhimg.com/v2-7dea9a7cddca304d4f1e8a166baa1832_b.jpg)

图 6

效果：

![](https://pic2.zhimg.com/v2-ae974a744c82f9c02e64b57461c64195_b.jpg)

图 7

按照这种模式我们可以写出通用的获取和设置输入控件值的方法：

![](https://pic3.zhimg.com/v2-16fd5d72687018fc9a0cc26de902b5fa_b.jpg)

图 8

通过在类的属性中调用这两个方法并限定输入值就可以将BlockUI控件和类属性绑定：

![](https://pic4.zhimg.com/v2-f15259439f3d7c9126a26b5d247515c3_b.jpg)

图 9

![](https://pic1.zhimg.com/v2-aba24e6647789eee17da7d580ad5bdd0_b.jpg)

图 10

![](https://pic1.zhimg.com/v2-df0aab4748ac4114acb23fd6e9af6004_b.jpg)

图 11

总结
--

对于多个输入框的BlockUI界面，使用组控件和Members属性可以更方便地获取和设置多个输入框的值，并做到根据输入值的个数来初始化界面。