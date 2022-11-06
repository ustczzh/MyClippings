# 在 Linux 中使用 WSL 调试 .NET 应用 - Visual Studio (Windows) | Microsoft Learn
[跳转至主内容](#main)

此浏览器不再受支持。

请升级到 Microsoft Edge 以使用最新的功能、安全更新和技术支持。

### 本文内容

1.  [必备条件](#prerequisites)
2.  [通过 WSL 开始调试](#start-debugging-with-wsl)
3.  [选择特定的分发版](#choose-a-specific-distribution)
4.  [以多个分发版为目标](#target-multiple-distributions)
5.  [附加到正在运行的 WSL 进程](#attach-to-a-running-wsl-process)
6.  [启动配置文件中的 WSL 设置](#wsl-settings-in-the-launch-profile)

适用范围：![](https://learn.microsoft.com/zh-cn/visualstudio/media/yes-icon.png?view=vs-2022)
Visual Studio ![](https://learn.microsoft.com/zh-cn/visualstudio/media/no-icon.png?view=vs-2022)
Visual Studio for Mac ![](https://learn.microsoft.com/zh-cn/visualstudio/media/no-icon.png?view=vs-2022)
Visual Studio Code

在不离开 Visual Studio 的情况下，可以使用 WSL 轻松地在 Linux 中运行和调试 .NET 应用。 如果你是跨平台开发人员，可将此方法用作一种用来测试更多目标环境的简单方法。

对于面向 Linux 的 Windows .NET 用户，WSL 在生产现实性和工作效率之间处于最有效的位置。 在 Visual Studio 中，你已经可以使用[远程调试器](https://learn.microsoft.com/zh-cn/visualstudio/debugger/remote-debugging-dotnet-core-linux-with-ssh?view=vs-2022)在远程 Linux 环境中进行调试，或者使用[容器工具](https://learn.microsoft.com/zh-cn/visualstudio/containers/overview?view=vs-2022)在容器中进行调试。 当生产现实性是你主要关注的问题时，应使用这些选项之一。 当一个简单而快速的内部循环更重要时，WSL 是一个不错的选择。

不需要只选择一种方法！ 你可在同一个项目中为 Docker 和 WSL 创建启动配置文件，然后针对特定的运行操作，选择其中一个适合的配置文件即可。 部署应用后，如果出现问题，你始终可使用远程调试器附加到该应用。

备注

从 Visual Studio 2019 版本 16.11 预览版 3 开始，WSL 2 调试目标已重命名为 WSL。

[](#prerequisites)必备条件
----------------------

*   Visual Studio 2019 v16.9 预览版 1 或更高版本（通过 WSL 可选组件进行 .NET 调试）。
    
    若要检查 WSL 组件，请选择“工具”>“获取工具和功能” 。 在 Visual Studio 安装程序中，请确保通过选择“单个组件”选项卡，然后键入“WSL”作为搜索词来安装组件 。
    
    在某些版本的 Visual Studio 中，默认情况下会在某些 .NET 工作负载中包含可选组件。
    
*   安装 [WSL](https://learn.microsoft.com/zh-cn/windows/wsl/about)。
    
*   安装所选的[分发版](https://aka.ms/wslstore)。
    

[](#start-debugging-with-wsl)通过 WSL 开始调试
----------------------------------------

1.  安装所需的组件后，在 Visual Studio 中打开一个 ASP.NET Core Web 应用或 .NET Core 控制台应用。你将看到一个名为 WSL 的新启动配置文件：
    
    ![](https://learn.microsoft.com/zh-cn/visualstudio/debugger/media/linux-wsl2-debugging-select-launch-profile.png?view=vs-2022)
    
2.  选择此配置文件，将其添加到 launchSettings.json。
    
    以下示例显示了该文件中的一些主要属性。
    
    备注
    
    从 Visual Studio 2022 预览版 3 开始，启动配置文件中的命令名称从 WSL2 更改为 WSL。
    
    ```
    "WSL": {
        "commandName": "WSL",
        "launchBrowser": true,
        "launchUrl": "https://localhost:5001",
        "environmentVariables": {
            "ASPNETCORE_URLS": "https://localhost:5001;http://localhost:5000",
            "ASPNETCORE_ENVIRONMENT": "Development"
        },
        "distributionName": ""
    } 
    ```
    
    ```
    "WSL": {
        "commandName": "WSL2",
        "launchBrowser": true,
        "launchUrl": "https://localhost:5001",
        "environmentVariables": {
            "ASPNETCORE_URLS": "https://localhost:5001;http://localhost:5000",
            "ASPNETCORE_ENVIRONMENT": "Development"
        },
        "distributionName": ""
    } 
    ```
    
    选择新的配置文件后，扩展会检查 WSL 分发版是否已为运行 .NET 应用进行了配置，并帮助你安装任何缺少的依赖项。 安装这些依赖项后，即可在 WSL 中进行调试。
    
3.  正常开始调试，你的应用将在默认的 WSL 分发版中运行。
    
    要验证你是否在 Linux 中运行，一种简单的方法是检查 `Environment.OSVersion` 的值。
    

备注

只有 Ubuntu 和 Debian 经过了测试且受支持。 .NET 支持的其他分发版应可正常工作，但要求手动安装 [.NET 运行时](https://aka.ms/wsldotnet)和 [Curl](https://curl.haxx.se/)。

[](#choose-a-specific-distribution)选择特定的分发版
-------------------------------------------

默认情况下，WSL 2 启动配置文件使用 wsl.exe 中设置的默认分发版。 如果希望启动配置文件以特定的分发版为目标，而不考虑默认设置，则可修改启动配置文件。 例如，如果你要调试一个 Web 应用，并希望在 Ubuntu 20.04 上对其进行测试，则启动配置文件应如下所示：

```
"WSL": {
    "commandName": "WSL",
    "launchBrowser": true,
    "launchUrl": "https://localhost:5001",
    "environmentVariables": {
        "ASPNETCORE_URLS": "https://localhost:5001;http://localhost:5000",
        "ASPNETCORE_ENVIRONMENT": "Development"
    },
    "distributionName": "Ubuntu-20.04"
} 
```

```
"WSL": {
    "commandName": "WSL2",
    "launchBrowser": true,
    "launchUrl": "https://localhost:5001",
    "environmentVariables": {
        "ASPNETCORE_URLS": "https://localhost:5001;http://localhost:5000",
        "ASPNETCORE_ENVIRONMENT": "Development"
    },
    "distributionName": "Ubuntu-20.04"
} 
```

[](#target-multiple-distributions)以多个分发版为目标
-------------------------------------------

再深入一步，如果你要处理一个需要在多个分发版中运行的应用程序，并且希望快速地在每个分发版上测试该应用程序，可创建多个启动配置文件。 例如，如果需要在 Debian、Ubuntu 18.04 和 Ubuntu 20.04 上测试控制台应用，可使用以下启动配置文件：

```
"WSL : Debian": {
    "commandName": "WSL",
    "distributionName": "Debian"
},
"WSL : Ubuntu 18.04": {
    "commandName": "WSL",
    "distributionName": "Ubuntu-18.04"
},
"WSL : Ubuntu 20.04": {
    "commandName": "WSL",
    "distributionName": "Ubuntu-20.04"
} 
```

```
"WSL : Debian": {
    "commandName": "WSL2",
    "distributionName": "Debian"
},
"WSL : Ubuntu 18.04": {
    "commandName": "WSL2",
    "distributionName": "Ubuntu-18.04"
},
"WSL : Ubuntu 20.04": {
    "commandName": "WSL2",
    "distributionName": "Ubuntu-20.04"
} 
```

通过这些启动配置文件，可轻松地在各个目标分发版之间来回切换，且均无需离开 Visual Studio。

![](https://learn.microsoft.com/zh-cn/visualstudio/debugger/media/linux-wsl2-debugging-switch-target-distribution.png?view=vs-2022)

[](#attach-to-a-running-wsl-process)附加到正在运行的 WSL 进程
---------------------------------------------------

除了使用 F5 从应用启动进行调试外，还可以使用附加到进程功能附加到正在运行的 WSL 进程进行调试。

1.  在应用正在运行的情况下，选择“调试”>“附加到进程”。
    
2.  对于“连接类型”，请选择“适用于 Linux 的 Windows 子系统 (WSL)”，然后选择“连接目标”的 Linux 分发。
    
3.  选择 **“附加”** 。
    
    ![](https://learn.microsoft.com/zh-cn/visualstudio/debugger/media/linux-wsl2-debugging-attach-to-process.png?view=vs-2022)
    

[](#wsl-settings-in-the-launch-profile)启动配置文件中的 WSL 设置
------------------------------------------------------

下表显示了启动配置文件中支持的设置。

| 名称 | 默认 | 目的 | 支持令牌？ |
| --- | --- | --- | --- |
| executablePath | dotnet | 要运行的可执行文件 | 是 |
| commandLineArgs | 映射到 WSL 环境的 MSBuild 属性 TargetPath 的值 | 传递到 executablePath 的命令行参数 | 是 |
| workingDirectory | 对于控制台应用：{OutDir}  
对于 Web 应用：{ProjectDir} | 在其中开始调试的工作目录 | 是 |
| environmentVariables |  | 要为已调试进程设置的环境变量的键值对。 | 是 |
| setupScriptPath |  | 要在调试前运行的脚本。 适用于运行 ~/.bash\_profile 等脚本。 | 是 |
| distributionName |  | 要使用的 WSL 分发版的名称。 | 否 |
| launchBrowser | false | 是否启动浏览器 | 否 |
| launchUrl |  | launchBrowser 为 true 时要启动的 URL | 否 |

支持的令牌：

{ProjectDir} - 项目目录的完整路径

{OutDir} - MSBuild 属性 `OutDir` 的值

备注

所有路径均适用于 WSL 而非 Windows。

* * *

建议的内容
-----

*   [
    
    ### 调试 Linux 上的 .NET Core - Visual Studio (Windows)
    
    ](https://learn.microsoft.com/zh-cn/visualstudio/debugger/remote-debugging-dotnet-core-linux-with-ssh?source=recommendations)
    
    在 Linux 上使用安全外壳 (SSH) 通过附加到进程来调试 .NET Core。 准备应用进行调试。 构建并部署应用。 附加调试程序。
    
*   [
    
    ### 在本地 Docker 容器中调试应用 - Visual Studio (Windows)
    
    ](https://learn.microsoft.com/zh-cn/visualstudio/containers/edit-and-refresh?source=recommendations)
    
    了解如何通过编辑和刷新以及设置调试断点功能来修改本地 Docker 容器中运行的应用以及刷新容器。
    
*   [
    
    ### 在 Ubuntu 上安装 .NET - .NET
    
    ](https://learn.microsoft.com/zh-cn/dotnet/core/install/linux-ubuntu?source=recommendations)
    
    演示在 Ubuntu 上安装 .NET SDK 和 .NET 运行时的各种方式。
    
*   [
    
    ### Docker Compose 生成设置 - Visual Studio (Windows)
    
    ](https://learn.microsoft.com/zh-cn/visualstudio/containers/docker-compose-properties?source=recommendations)
    
    了解如何编辑 Docker Compose 生成属性以自定义 Visual Studio 生成和运行 Docker Compose 应用程序的方式。
    

*   [
    
    ### 在 Linux 发行版上安装 .NET - .NET
    
    ](https://learn.microsoft.com/zh-cn/dotnet/core/install/linux?source=recommendations)
    
    了解如何在 Linux 上安装 .NET。 .NET 不仅可以在 package.microsoft.com 上获得，还可以在各种 Linux 发行版的官方软件包存档中找到。
    
*   [
    
    ### 启动 Docker Compose 服务的子集 - Visual Studio (Windows)
    
    ](https://learn.microsoft.com/zh-cn/visualstudio/containers/launch-profiles?source=recommendations)
    
    了解如何使用 Docker Compose 启动配置文件，以及如何控制在 Visual Studio 中使用 Docker Compose 时要启动的服务。