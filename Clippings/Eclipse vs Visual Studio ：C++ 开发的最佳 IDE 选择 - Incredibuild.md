# Eclipse vs Visual Studio ：C++ 开发的最佳 IDE 选择 - Incredibuild
C++ 开发人员的 IDE 选择繁多，各有优劣。今天我们要讨论的是两个主要IDE，一是由 Eclipse 基金会开发的 [Eclipse](https://www.eclipse.org/ide/)，另一个是微软的 [Visual Studio](https://www.incredibuild.cn/integrations/visual-studio)。两者都是成熟的 IDE，历经时间与用户的考验，我们的选择很大程度上基于具体项目情况。一些开发人员始终偏好其中一个，而另一些则会根据具体项目需求做选择。

无论你是从标准代码编辑器过渡到 IDE，还是准备换个新的 IDE，这些选择都值得慎重考虑。这篇文章主要从 C++ 程序员的角度出发，对比两种 IDE 带来的不同服务与体验。

免责声明：Microsoft 是 Incredibuild [技术合作伙伴](https://www.incredibuild.com/partners)，[Visual Studio 是与我们紧密集成的开发环境之一](https://www.incredibuild.cn/solutions/accelerate-visual-studio-cc-builds)。但我们对 Eclipse 的喜爱没有丝毫受到影响，两个 IDE 都是 [业界顶级的 C++ IDE 和编辑器](https://www.incredibuild.cn/blog/best-c-ides)，我们也都偏爱有加。接下来，我们将列出一些选择 IDE 时的注意事项，帮助你做出正确抉择。

**Eclipse** **是什么?**
--------------------

![](https://www.incredibuild.cn/wp-content/uploads/2021/09/Eclipse.jpg)

Eclipse IDE 是一个名气不小的开源框架，最早于 2001 年底发布，长期以来一直受到 Java 代码开发者的青睐。虽一开始是针对 Java 设计的，但 Eclipse IDE 可高度扩展，并且支持多种语言，包括 C++、C++、C# 等。另外，Eclipse IDE 粉丝群数量庞大，超过百万，其代码突出显示和重构功能、版本控制系统集成以及高级调试功能广受用户好评。

Eclipse 正在不断开发和更新，目前发布周期为 13 周。由于其开源性，社区资源遍及全球，且异常活跃。

**什么是 Visual Studio?**
----------------------

![](https://www.incredibuild.cn/wp-content/uploads/2021/09/brandvisual-1.jpg)

作为 IDE 的首选，Visual Studio 是一个功能齐全的软件开发平台，支持多目标操作系统和多种编程语言。Visual Studio 最早发布于1997 年，捆绑了 Visual C++ 等其他产品。内置代码编辑器提供智能提示（IntelliSense）和自动补全功能实现高效编码，此外还有可自定义的代码格式选项、功能强大且易于使用的调试功能。所有这些功能的文档资源都开放给用户，这些官方文档堪称业内最高水平。

Visual Studio 支持开发控制台和 GUI 应用程序，以及 web 应用程序和 web服务。许多开发人员认为 Visual Studio 是 C++ 代码开发的终极 IDE。Visual Studio 2019 16.1 版本增加了在 Windows Linux 子系统（WSL）中使用 C++ 的功能，因此用户直接在 Windows 上运行轻量级的 Linux 环境。有关设置的说明，请参阅[本帖](https://devblogs.microsoft.com/cppblog/c-with-visual-studio-2019-and-windows-subsystem-for-linux-wsl/)。

Visual Studio 社区相当活跃，插件资源丰富，平台更新速度快。微软建议个人使用 Windows 或 Mac 的免费[社区版](https://visualstudio.microsoft.com/free-developer-offers/)，在商业和企业层面上有多种选择。

**Eclipse vs Visual Studio** **对比**
-----------------------------------

**扩展性**

如果你计划从代码编辑器升级为功能更加丰富的IDE，那么这两个选项都很不错。首要的还是扩展性能，IDE 的扩展性是借助插件或附加组件实现的，使用合适的附加组件是增强编码体验的一种快速有效方法。

[Eclipse Marketplace](https://marketplace.eclipse.org/) 上有大量的定制服务和扩展程序，辅助编码、测试和软件开发周期。其中最主要的是 [EclipseC/C++ 开发工具](https://projects.eclipse.org/projects/tools.cdt)（CDT），做出了许多重要贡献。

[Visual Studio Marketplace](https://marketplace.visualstudio.com/vs) 是 Visual Studio 插件的官方存储库，插件形式主要为宏、附加组件和包。用户可以充分利用数千个附加组件资源，增强开发体验。用户还可以自己发布插件，丰富的文档和活跃的社区资源可协助用户编写插件。Incredibuild 列举了一些高效的插件资源，可阅读博客《[Visual Studio 2019 —— C++ 使你的工作生活更轻松](https://www.incredibuild.cn/blog/cpp-visual-studio-extensions-that-will-make-your-life-a-lot-easier)》了解更多细节。

**支持多种操作系统**
------------

Visual Studio可用于Windows和MAC操作系统，尽管Mac 版的Visual Studio与微软 C++ 并不兼容。不过，它适用于 .NET 语言和跨平台开发。在讨论 Visual Studio 2022 版[博客](https://devblogs.microsoft.com/visualstudio/visual-studio-2022/)中，也提到了 Mac 版的升级与变化，其中包括 Windows 和 MacOS 版本将更相似，不过关于是否支持 C++ 的问题，依然没有提及。

在这一点上，Eclipse 与 Visual Studio 不同，因其可同时适用于 Windows、MacOS 和 Linux 的版本。

如何更好地支持 Mac 和 Linux 系统，是微软产品需要解决的问题。为了支持在这些环境，Microsoft 发布了功能强大且功能丰富的 Visual Studio 代码编辑器，可用于 Windows、MacOS 和 Linux 的开源程序。不过代码编辑器的功能还是远不如 IDE 齐全。

值得注意的是，尽管 Visual Studio 只有在 Windows 系统中才可用于 C++ 开发，但还是可以编译 Linux C++ —— 需要使用 CGWING、MINW，或[远程 Linux 机器进行编译](https://devblogs.microsoft.com/cppblog/linux-development-with-c-in-visual-studio)。 “远程”机器可以是本地的虚拟机或容器，所以实际上，你可以基于本地合作托管 Linux，编译 Linux Visual Studio C++ 代码。

上面提到的另一个选项是 [WSL（Linux 的 Windows 子系统）编译](https://devblogs.microsoft.com/cppblog/c-with-visual-studio-2019-and-windows-subsystem-for-linux-wsl/)。

**安装和配置**
---------

一般情况下，安装 Eclipse 并将其设置为独立的 C++ 开发步骤较多。其中包括安装基本框架、C/C++ 开发工具。在开始之前，需要安装 JVM。书面和视频教程随处可见，尽管如此，还是有一些用户吐槽初始设置复杂。

不过现在，我们可以使用 [Eclipse Installer](https://www.eclipse.org/downloads/packages/installer)，安装步骤大大简化。为独立的C/C++ 开发设置 Eclipse 非常简单，只需选择要安装的软件包即可。对于 Java 和 C++ 中编写代码的开发人员来说，这个过程稍微复杂一些，因为它需要多个安装包，不过总的来说，也是小菜一碟。

![](https://www.incredibuild.cn/wp-content/uploads/2021/09/Eclipse-vs-Visual-Studio-Eclipse-installer.jpg)

图片来源： [Eclipse Installer](https://www.eclipse.org/downloads/packages/installer)  ：Eclipse Installer 为 C/C++ 开发师提供的功能。

设置 Visual Studio 首先要下载 Visual Studio Installer，这也是一个可用于安装或修改 Visual Studio 环境的功能。安装程序运行后，选择工作进程可能会让人头疼，多平台开发和附加工具集等可选择的功能太多。不过，这些都需要按照[详细指导](https://docs.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2019)，一步一步进行。

![](https://www.incredibuild.cn/wp-content/uploads/2021/09/The-Desktop-and-Mobile-section-in-Visual-Studio-Installer-768x395-1.jpg)

图片注释：Visual Studio Installer 的 Desktop & Mobile 部分

安装完成后，Visual Studio Installer 依然存储于部署功能部分，用户随时进行访问。比如，如果你想安装 iOS 和 Android 的 C++ 移动开发工具，只需添加点击即可。

**用户界面**
--------

在菜单和控件层面，这两个 IDE 功能都很丰富，难分高下，且可以通过界面以及各种插件进行自定义。不过，尽管 Eclipse 比其他大部分 Java IDE 的响应更及时，但对比 Visual Studio，还是稍有逊色。当可用资源更多时，Visual Studio 界面速度会更快。总的来说，这两个 IDE 在可用性方面的评价都很高。

开发人员钟爱的 Eclipse 的其中一个功能是透视图（Perspective）。透视图其实是一种陈列展示，以视图的方式呈现任务的发展和变化。其中包括一组与 C++ 开发相关的透视图，分别用于编码和调试。另外，为了对抗菜单过载，以及避免在需要时难以找到特定命令，拥有相关控件的子集是很有效的解决方法。Eclipse 附带了几个现成的透视图，不过用户也还可以根据需求进一步定制。

在进入调试模式时，Visual Studio 也有类似的功能。某些控件是隐藏的，而其他与调试相关的控件则会出现。基本上，最相关的控制措施在最前端，触手可及。此外，我们还可以通过插件，方便用户灵活使用 Eclipse 透视图功能，但本机无法提供这些功能。

**编码效率及美观性**
------------

Eclipse 和 Visual Studio 都可进行本机定制，或者使用插件定制用户界面，不过这些插件功能与本机自带的编码辅助功能（如语法高亮显示和代码自动补全功能）还是无法比较的。

Eclipse 具有[内容辅助](https://www.eclipse.org/pdt/help/html/code_assist_concept.htm)功能，提供代码自动补全、类成员列表和相关文档选项。许多其他与编辑器相关功能包括语法着色、选项卡规范、花括号放置等等。我们甚至可以配置为每次保存文件时自动格式化代码，这是团队编码时保持统一风格的一种有用方法。

Visual Studio 的智能提示（IntelliSense）功能强大，可自动补全代码，提供参数信息、类成员列表等。其智能代码补全功能，可大幅提升整个编程速度，增强准确度，通常在编译 www 很久之前就避免了错误。除了本机硬件支持外，还可选择其他插件（如 [Visual Assist](https://wholetomato.com/)），提高生产率并使编码更加方便。另外还有语法着色和几十个代码美化功能。

**调试功能**
--------

强大的调试功能是 IDE 与简单代码编辑器的主要区别，也是开发人员所期待的。在这一点上，这两个 IDE 都非常出色。它们都可以添加条件断点，避免不加区别地停止执行，还可以通过多种方式查看变量的内容。Eclipse 和 Visual Studio 都提供了在调试时编辑代码的功能，无需重新启动即可继续执行。例如，Eclipse 的“Dynamic Printf”可以调用 print 函数，而无需重新编译或重新启动程序。

下面的屏幕截图描述了 Eclipse C++ 调试的透视图。执行在指示行暂停，所有相关信息显示在各个窗格中。

![](https://www.incredibuild.cn/wp-content/uploads/2021/09/CDT-FAQ-768x516-1.jpg)

此屏幕截图取自[CDT常见问题](https://wiki.eclipse.org/CDT/User/FAQ)，并显示了调试透视图的示例。

控制执行可以说是调试器最重要的组件，Visual Studio有一个非常强大的功能，“SETNEXT语句”。这个功能有点类似调试进程中的时间机器，如果无意中跳过了需要仔细检查的部分，我们可以倒回选择的特定点。这种能力是其他调试器无法比拟的。

下面的截图显示调试器正在工作，左侧的黄色箭头指示要执行的部分。正如工具提示消息显示，可以将箭头拖动到要执行的新行。例如，执行可以返回到前一行，在此设置“PivotPosition”。

![](https://www.incredibuild.cn/wp-content/uploads/2021/09/Full-screen-debug-mode-768x419-1.jpg)

调试模式全屏  
![](https://www.incredibuild.cn/wp-content/uploads/2021/09/A-closer-look-at-Set-next-statement.jpg)
  

“设置下一个语句”详细展示

同样，正如本次 Eclipse 与 Visual Studio 对比中的许多功能一样，整体来说，两者功能区别不大，但个人的偏好通常源自细节设计。

**用例**
------

有关如何在 Eclipse 中使用 OpenCV 的最新用法教程，请参阅 [OpenCV 项目中的“howto”](https://docs.opencv.org/4.5.2/d7/d16/tutorial_linux_eclipse.html)。

有关如何将 OpenCV 与 Visual Studio 结合的教程，请参见 [OpenCV 项目中的“howto”](https://docs.opencv.org/4.5.2/dd/d6e/tutorial_windows_visual_studio_opencv.html)。

**Eclipse** **与** **Visual Studio****对比表**

|  | Eclipse | Visual Studio |
| 硬件要求 | RAM: 4 GB （最好是 8 GB 甚至 16 GB）  
磁盘空间： ~300MB处理器： 1.8 Ghz Quad Core 以上 | RAM: 2 GB（最好是 8 GB 甚至 16 GB）

磁盘空间：至少 800 MB （正常安装需要 20-50 GB 的磁盘空间）

处理器：1.8 Ghz Quad Core 以上

 |
| 初始设置 | 首先安装 Java，然后是Eclipse，最后是 C/ C++开发工具。

Eclipse CDT 安装程序简化了这个过程。

 | 说明详细，步骤清晰易懂。C/ C++ 兼容的配置很简单，有针对不同目标平台的选项。 |
| 用户界面响应性 | 内容辅助和代码完成功能可能有点慢，特别是在资源不足的情况下 | 只能提示和代码美化功能实用且运行快速。 |
| C++ 操作系统 | Windows、MacOS、 Linux | Windows、 [Linux](https://devblogs.microsoft.com/cppblog/linux-development-with-c-in-visual-studio/) |
| 可运行的操作系统 | Windows、MacOS、Linux | 仅限于 Windows （针对C++ 开发） |
| 项目和工作区管理 | 支持单个工作间中的多个项目，或者多个工作间可用于单独项目 | 项目管理是通过解决方案资源管理器完成的，一个解决方案可以承载多个项目 |
| 社区 | 庞大、活跃、开源 | 庞大、贡献者多 |
| [顶级 IDE榜单](https://pypl.github.io/IDE.html)中的市场份额(涵盖所有软件语言) | 14.38% （2021 年 6 月数据显示排名第二） | 28.2%（2021 年 6 月数据显示排名第一） |
| [JetBrains C++ 开发调查](https://www.jetbrains.com/lp/devecosystem-2020/cpp/)显示的市场份额 | 2% | 26% |
| 定价 | 免费 | 对开源、单个开发人员，以及少于5人的团队免费。

其他则需要购买企业许可证。

 |
| 调试 | 调试功能很好用 | 具有一些强大的功能，包括“设置下一个语句” |
| 插件 | Eclipse Marketplace是一个非常活跃且值得信赖的可搜索扩展存储库 | Visual Studio Marketplace 拥有数千个宏、插件和附加组件，并且非常活跃 |

**总结**
------

Eclipse 与 Visual Studio 的对比，同时也是对这两个顶级 IDE 的简要介绍。Eclipse 的 C++ 的市场份额数据相对较低，这可能会有一些误导，让用户觉得其功能较弱。恰恰相反，使用 Eclipse CDT 的开发师都普遍称赞其功能丰富，开源且用户可以自由使用。比较 Eclipse 和 Visual Studio 时，经常难论高下，尤其 IDE 的选择本身就有很强的主观性。因此，用户的最终选择很可能由某一个功能触发，或者源自总体的外观和感觉。考虑到这两个 IDE 都可以免费试用，最好的建议是先试试！

[![](https://www.incredibuild.cn/wp-content/uploads/2021/09/Speed_up_cpp_1200x360-1-1.png)
](https://www.incredibuild.cn/resources/speed-up-your-cpp-builds)