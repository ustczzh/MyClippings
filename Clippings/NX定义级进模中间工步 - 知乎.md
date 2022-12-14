# NX定义级进模中间工步 - 知乎
作者：黄玮玮 审校：陈杰 版本：NX所有版本

在定义中间工步是NX级进模向导中间工步工具的第一个命令，主要作用是根据原始部件生成关联的中间展开工步（图1，表1）

![](https://pic2.zhimg.com/v2-cf2bd5150702518ca7fb0a2575135401_b.jpg)

图1

| 定义中间工步 |
| 工步序列 | 指定中间工步顺序 |
| 从部件到毛坯（创建中间工步由部件到毛坯的顺序） |
| 从毛坯到部件（创建中间由毛坯到部件的顺序） |
| 中间工步数量 | 设置中间工步数量 |
| 起始工位 | 设置中间工步在料条的位置 |
| 步距 | 设置相邻工序之间的距离 |
| 步距方向 | 设置步距矢量方向 |
| 预览 | 预览设置的步距方向 |
| 编辑中间工步 |
| 选择中间工步 | 选择一个中间工步在其后面插入一个新的工步或者选择的中间工步 |
| 设置 |
| 中间工步命名规则 | 定义中间工步命名规则 |
| 重命名组件 | 控制组件是否被重命名 |
| 为中间工步链接片体 | 除了了为中间工步链接实体，还可链接片体代替实体 |

表1

用一个例子来演示该命令如何操作：

1.  打开一个产品模型；
2.  级进模选项卡-中间工步工具-定义中间工步启动；
3.  在工步序列选择从部件到毛坯；
4.  输入中间工步数量为5
5.  设置起始工位为6
6.  设置步距为150（图2）
7.  点击应用，5个新的中间部件出现在NX窗口中产品模型的右侧（图3）；

![](https://pic2.zhimg.com/v2-966e8b645514c7eb21ec64221272a791_b.jpg)

图2

![](https://pic2.zhimg.com/v2-35c2aef57c108f712b7178a130220d45_b.jpg)

图3

如果勾选重命名组件复选框，点击应用后会出现如下对话框（图4）。可以为中间工步组件指定文件名的命名规则或者分别为每个中间工步创建一任意文件名，还可以指定中间工步组件的存放路径。

![](https://pic1.zhimg.com/v2-29980680ff622b59857826bbb702388c_b.jpg)

图4

以上就是如何使用NX级进模定义中间工序，希望能对大家有所帮助。