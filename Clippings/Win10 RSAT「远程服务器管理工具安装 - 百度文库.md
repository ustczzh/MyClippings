# Win10 RSAT「远程服务器管理工具安装 - 百度文库
Win10 RSAT「远程服务器管理工具安装

自 Windows 2000 以来，Microsoft 管理控制台（MMC）就一直存在了，MMC 是 Windows Server 中大多数 GUI 配置工具的基础。如果您是 Windows Server 管理员，肯定已经很熟悉「Active Directory 用户和计算机（ADUC）」和「服务器管理器」等配置工具了。而 RSAT「远程服务器管理工具」就是将已经内置于 Windows Server 中的各种管理工具进行了打包，以方便管理员能够在 Windows Client 中对服务器进行远程配置和管理。

在 Windows 10 Version 1809 之前的 Windows 10 操作系统中，您需要手动[下载 对应版本的 RSAT 工具](https://www.sysgeek.cn/windows-10-hotfix/)进行安装，而 1809 已经将其内置到了操作系统中。下面系统极客就为大家介绍，如何在 Windows 10 Version 1809 中安装 RSAT 远程服务器管理工具的两种方法。

Windows 10 Version 1809安装RSAT

RSAT 已经成为 Windows 10 Version 1809 及更高版本的 [FoD（按需功能）](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)，可以通过「Windows 设置」进行安装：

1使用 Windows + I 快捷键打开「Windows 设置」

![](https://wkrtcs.bdimg.com/rtcs/image?w=553.73&md5sum=51770bf2492eef39615c5f4c0d536d19&sign=3219f61aea&rtcs_flag=2&rtcs_ver=4&l=webapp&bucketNum=208&ipr=%7B%22c%22%3A%22word%2Fmedia%2Fimage1.jpeg%22%2C%22dataType%22%3A%22jpeg%22%2C%22h%22%3A%22403.53%22%2C%22imgOriH%22%3A677%2C%22imgOriW%22%3A929%2C%22r%22%3A%22r_11%22%2C%22t%22%3A%22img%22%2C%22w%22%3A%22553.73%22%7D)

2点击「应用」——在「应用和功能」页面中点击「管理可选功能」

![](https://wkrtcs.bdimg.com/rtcs/image?w=553.73&md5sum=51770bf2492eef39615c5f4c0d536d19&sign=3219f61aea&rtcs_flag=2&rtcs_ver=4&l=webapp&bucketNum=208&ipr=%7B%22c%22%3A%22word%2Fmedia%2Fimage2.jpeg%22%2C%22dataType%22%3A%22jpeg%22%2C%22h%22%3A%22403.53%22%2C%22imgOriH%22%3A677%2C%22imgOriW%22%3A929%2C%22r%22%3A%22r_12%22%2C%22t%22%3A%22img%22%2C%22w%22%3A%22553.73%22%7D)

3点击「添加功能」按钮——在「添加功能」屏幕上，向下滚动可用功能列表，直至找到 RSAT。

现在的 RSAT 工具都是（可选）独立安装的，所以在选中所需工具后直接单击「安装」就可以添加。

方法2：使用PowerShell安装RSAT

您也可以使用 PowerShell 在 Windows 10 Version 1809 中安装和删除 RSAT 远程服务器管理工具。

1使用 Windows + X 快捷键打开快捷菜单——选择打开 Windows PowerShell（管理员）

2执行以下命令，使用 Get-WindowsCapability cmdlet 查看 RSAT 工具的名称和安装情况：

Get\-WindowsCapability  \-Name RSAT\*  \-Online  |  Select\-Object  \-Property  Name,  State

3要安装所有可用工具，可以直接将 Get-WindowsCapability 的结果传递给 Add-WindowsCapability：

Get\-WindowsCapability  \-Name RSAT\*  \-Online  |  Add\-WindowsCapability  –Online

4如果要选择安装所需工具，可以将 Get-WindowsCapability 获取到的工具名称传递给 Add-WindowsCapability 进行安装。例如，要安装 GPMC 就可以使用以下命令：

Add\-WindowsCapability  \-Name  Rsat.GroupPolicy.Management.Tools\~~~~0.0.1.0  –Online

![](https://wkrtcs.bdimg.com/rtcs/image?w=553.73&md5sum=51770bf2492eef39615c5f4c0d536d19&sign=3219f61aea&rtcs_flag=2&rtcs_ver=4&l=webapp&bucketNum=208&ipr=%7B%22c%22%3A%22word%2Fmedia%2Fimage3.png%22%2C%22dataType%22%3A%22png%22%2C%22h%22%3A%22263.73%22%2C%22imgOriH%22%3A411%2C%22imgOriW%22%3A863%2C%22r%22%3A%22r_9%22%2C%22t%22%3A%22img%22%2C%22w%22%3A%22553.73%22%7D)

5如果要移除某个 RSAT 工具，只需将 Add-WindowsCapability 替换为 Remove-WindowsCapability 即可：

注意：由于 FoD（按需功能）需要使用 Microsoft Update，如果 Windows Update 配置为使用内部服务器，如 WSUS 或 SCCM 的 SUP 时，则无法在不执行其他步骤的情况下安装 RSAT。您需要临时将设备配置为使用 Microsoft Update 并在线搜索更新。

加载win10 IOS文件后，指定源安装方式

Add-WindowsCapability -Online -LimitAccess -Source D:\\ -Name Rsat.ActiveDirectory.DS-LDS.Tools~~~~0.0.1.0