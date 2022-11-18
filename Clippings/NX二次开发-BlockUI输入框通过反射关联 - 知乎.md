# NX二次开发-BlockUI输入框通过反射关联 - 知乎
作者：薛剑腾 审校：叶齐天

适用版本：NX6以上

概述
--

在NX的BlockUI开发中我们经常会遇到用户需要在界面上设置一连串的输入控件的开发需求。正如之前提及对于这种情况我们可以通过布局类控件的“Members”属性去获取其包含的输入控件并按照顺序关联到类的属性。除此之外我们还可以使用反射来定义类属性和控件的关联，通过统一的代码自动设置控件对应的类属性的值。

![](https://pic4.zhimg.com/v2-89713844757bdea27f001fc9aaef6763_b.jpg)

图 1

详细内容
----

### 自定义BlockUI关联特性（Attribute）

为了给类的属性标记其所关联的BlockUI控件，我们可以自定义一个.NET中的特性：

![](https://pic3.zhimg.com/v2-12dbced8966fd4446802d73300aff332_b.jpg)

图 2

此特性中记录了属性目标的Block控件的BlockID，特性的目标类型定义为属性，因此只能用于标记类的属性。

### 参数列表基类

虽然对于不同的BlockUI界面，我们要定义的和控件关联的类属性都不一样，但是“通过属性标记的BlockID找到控件匹配的属性并进行更新和同步”的方法是一致的，因此我们可以创建一个参数列表基类，将上述方法封装进去，并允许继承基类。

在本例中我们假设关联的控件和属性都是字符串类型的，基类的定义如下：

![](https://pic3.zhimg.com/v2-ac74dc7a21fc84dd1cf75c86be05fa46_b.jpg)

图 3

![](https://pic4.zhimg.com/v2-4bbc3a5eadbbfaca71e15efc8452ae27_b.jpg)

图 4

![](https://pic2.zhimg.com/v2-70b0a3f5b72b43fce4d24a77d012d935_b.jpg)

图 5

![](https://pic2.zhimg.com/v2-d8de64e6708b5bf48e9ae1f5e01f56e9_b.jpg)

图 6

![](https://pic2.zhimg.com/v2-5006fd8ade36737e20167cfb3f0fbfb1_b.jpg)

图 7

### 创建程序参数列表并关联属性

在BlockUI程序中创建一个新的参数列表类，继承于ParamListBase类；根据需要定义属性（建议将set访问器设置为private），并使用BlockUIAttribute标记关联的BlockUI控件的BlockID：

![](https://pic1.zhimg.com/v2-344347a5fd7710bcc425265dd49d1a14_b.jpg)

图 8

### 在BlockUI程序中使用参数列表

定义和初始化：

在BlockUI类中添加一个参数列表作为成员变量，并在界面初始化时示例化对象和初始化参数：

![](https://pic3.zhimg.com/v2-e307e24ff984afc567d0739bc120eece_b.jpg)

图 9

需要注意，应在UI界面调用Show()方法以后才能调用ParamList.Initialize()方法对参数列表的值进行初始化，否则会因为无法找到BlockUI控件而抛出异常。

更新：

为了在用户更改界面的值后对参数列表进行同步更新，需要在界面的更新回调中调用ParamList.Update()方法：

![](https://pic2.zhimg.com/v2-8809146cc3bb7019d85efae0a3010b65_b.jpg)

图 10

获取和设置值：

通过ParamList定义的属性即可获取用户输入的参数值。

若需要在代码中设置参数的值，为了保持和界面中控件的值保持同步，应该调用ParamList.SetValue()方法。如以下代码，设置界面中string0字符串控件和属性A的值为"AAA"：

pList.SetValue(string0, "AAA");

![](https://pic2.zhimg.com/v2-2a2dc4fc2ee203b47949bb48feac9b35_b.jpg)

图 11

![](https://pic2.zhimg.com/v2-8f759eb194670cc85f7c834bfb399a11_b.jpg)

图 12

总结
--

对于多个输入框的BlockUI界面，使用组控件和Members属性可以更方便地获取和设置多个输入框的值，并做到根据输入值的个数来初始化界面。