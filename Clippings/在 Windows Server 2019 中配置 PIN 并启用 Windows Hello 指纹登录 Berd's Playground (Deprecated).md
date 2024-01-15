# 在 Windows Server 2019 中配置 PIN 并启用 Windows Hello 指纹登录 | Berd's Playground (Deprecated)
[在 Windows Server 2019 中配置 PIN 并启用 Windows Hello 指纹登录 | Berd's Playground (Deprecated)](https://blog.berd.moe/archives/windows-server-2019-setup-pin-and-biometric-login/) 

 0x00 前言
-------

放在宿舍的计算机经常需要锁定和解锁，但这就带来了一些问题：一是反复输入长密码非常累人，二是可能产生潜在的安全隐患。

如果我们能通过指纹登录，上述问题就迎刃而解了。但是当我尝试添加指纹的时候却发现 Windows 不让我这么做：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/cb3df4e4-d3d4-4be9-b149-704a728223f7.png?raw=true)

我相信即使是猴子也可以在 Google 上找到一堆解决方案，但是你基本不可能找到一个真正有用的方案（这篇文章不包含在内 ![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/5f5d90f3-949a-44f3-a98a-763247d334fc.png?raw=true)
）。因为那些方案针对的问题根源和我们现在碰到的不同。Microsoft 把 Windows Server 的 Windows Hello 功能砍了一部分，并且要求我们 [配置 Windows Hello for Business](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-overview) 来实现 PIN 和生物特征登录。

如果你已经配好了一套完整的 AD，也许上面的链接对你帮助更大。但是我并不想配置或加入域，因此这篇博客将记录一个另辟蹊径配置 Windows Hello 指纹登录的方法。

0x01 问题溯源
---------

首先，我们现在看到最直接的现象就是两个灰色按钮点不了，让我们先打开 [The Inspect](https://docs.microsoft.com/en-us/windows/win32/winauto/inspect-objects) 看看这两个按钮的相关属性：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/659e3fba-fa46-48bf-944f-34767e638435.png?raw=true)

很明显，`IsEnabled: false` 表示这个按钮被禁用了（这不是重点），我们关注的是下面的 `AutomationId: "CAddPinSetting_AddPinButton"`，这个 ID 有助于在后面逆向时快速定位到逻辑。

你问为什么我不看添加指纹按钮？因为添加指纹实际上是可以通过调用 [Windows Biometric Framework API](https://docs.microsoft.com/en-us/windows/win32/api/_secbiomet/) 往 System Pool 里注册一个指纹完成的，这个框架没有被 MS 砍掉（因为 WH Business 还要用嘛）。顺便一提你可能需要在服务器管理器里装上这个框架才能用指纹功能：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/4c78d20c-1d7f-4dc4-9fee-7e15afb39997.png?raw=true)

让我们继续搞逆向，找到 `%windir%\System32\SettingsHandlers_User.dll`（其实看起来更像包含具体逻辑的是 `SettingsHandlers_SignInOptions.dll`，不过里面并没有我们要找的东西），拖进 IDA，搜索我们刚才找到的按钮 ID，查看 Invoke 函数：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/786ae41e-73c6-43eb-bd02-2507f2592bc5.png?raw=true)

很明显，这按钮的实际功能就是调用一个外部的 `EnrollPin` 函数，至于上面下面全是遥测代码，MS 是真的很喜欢遥测 ![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/26ffd587-e08c-4849-82e6-3d07f5056ae9.png?raw=true)
：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/dd335ea5-6631-49a2-bb05-36ac83c305f9.png?raw=true)

IDA 告诉我们这个函数在 `ext-ms-win-pinenrollment-enrollment-l1-1-0.dll` 里，但这个 DLL 实际上只是个 Stub，我们要找的 DLL 是 `PinEnrollmentHelper.dll`

然后你很快就会发现——系统里没有这个文件 ![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/59f21699-d0d1-44fa-898e-cdf46f7d1c96.png?raw=true)
，原因很简单，MS 把这个功能砍掉了。

0x02 恢复 PinEnrollment 组件
------------------------

我们需要到一个正常的 Windows 镜像中找这两个（还有一个 `PinEnrollmentBroker.exe`，别问我怎么知道的）文件进行安装，就在 `%windir%\System32` 里：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/b1d3f886-6206-4033-a68f-4dbd261ba1d2.png?raw=true)

我在这里放一个压缩包，是从 Windows 10 Enterprise LTSC 17763.316 中提取的。文件没有 MS 的数字签名，**USE AT YOUR OWN RISK**：

_\* 你可能已经发现了，这两个文件其实被包含在 MS 下发给我们的更新包中，但并没有被安装到系统上。如果你有更好的方法（比如用 DISM?）将它们直接从 LCU 里拉出来装到系统里，欢迎在评论区留言。_

把这两个文件直接扔进 System32 文件夹就行，当然设置界面的按钮并不会因此就被启用，让我们写一个简单的小程序来调用 EnrollPin：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/9551c717-5ab4-4923-a57c-c5597dc5ad35.png?raw=true)

你可以直接用我写好的，反正丢进 dnSpy 就可以审计代码：

然后你会发现点了按钮并没有任何反应 ![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/97c87398-989a-4095-bc48-4ca98ffe6058.png?raw=true)
，不要慌，让我们把 `PinEnrollmentHelper.dll` 丢进 IDA，随便在 EnrollPin(Helper) 里打一个断点（当然最开始是打在函数头的，一步步往下追）：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/7172aa77-4750-4a96-a853-f1913f1d35c2.png?raw=true)

然后 Attach 到我们的 PinEnroller.exe，摁一下按钮往下追，很容易就能发现 `InitializePinEnrollmentBroker` 里 `RoActivateInstance` 返回了 `0x80040154` 导致调用失败，查文档可知错误是 `REGDB_E_CLASSNOTREG`，也就是说 `PinEnrollment.PinEnrollmentBroker` 这个类并没有在我们的系统里注册：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/0077b574-cc74-48d8-9405-e2913cb5609d.png?raw=true)

0x03 注册 PinEnrollment 组件
------------------------

_\* 这里涉及到一些 Windows Runtime 的东西，我自己也没完全搞清楚，就不展开解释了。这里给出的注册表修改方法也是纯经验方法，如果你知道更为标准的操作方法欢迎在评论区留言。在我的测试中，regsrv32 是用不了的。_

总之要 “注册” 这个类，我们可以往 `HKLM\SOFTWARE\Microsoft\WindowsRuntime` 里直接加上对应的值：

```
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsRuntime\ActivatableClassId\PinEnrollment.PinEnrollmentBroker]
"ActivationType"=dword:00000001
"Server"="PinEnrollment"
"TrustLevel"=dword:00000000

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsRuntime\Server\PinEnrollment]
"ExePath"="C:\\Windows\\System32\\PinEnrollmentBroker.exe"
"IdentityType"=dword:00000000
"Permissions"=hex:01,00,14,80,64,00,00,00,70,00,00,00,14,00,00,00,30,00,00,00,\
  02,00,1c,00,01,00,00,00,11,00,14,00,04,00,00,00,01,01,00,00,00,00,00,10,00,\
  10,00,00,02,00,34,00,02,00,00,00,00,00,14,00,0b,00,00,00,01,01,00,00,00,00,\
  00,05,0b,00,00,00,00,00,18,00,0b,00,00,00,01,02,00,00,00,00,00,0f,02,00,00,\
  00,01,00,00,00,01,01,00,00,00,00,00,05,0a,00,00,00,01,02,00,00,00,00,00,05,\
  20,00,00,00,21,02,00,00
"ServerType"=dword:00000000
```

这样就注册好了，可以被 `RoActivateInstance` 正常初始化。但是当我们再次调用的时候会发现 DLL 在这个位置给我们扔异常：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/81cd0c13-d720-48b3-b851-ba09a3dfd9ce.png?raw=true)

追溯原因，还是一些 GUID 没有被正确注册。总之我们还需要加上下面的这一堆 ID：

```
[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{C459DF76-7545-4671-A85D-A42903991174}]
@="PSFactoryBuffer"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{C459DF76-7545-4671-A85D-A42903991174}\InProcServer32]
@="C:\\Windows\\System32\\PinEnrollmentHelper.dll"
"ThreadingModel"="Both"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{FF5265E8-6D8B-4BD4-A52D-8D99008E04C8}]
@="PSFactoryBuffer"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{FF5265E8-6D8B-4BD4-A52D-8D99008E04C8}\InProcServer32]
@="C:\\Windows\\System32\\Windows.Internal.UI.BioEnrollment.ProxyStub.dll"
"ThreadingModel"="Both"

[HKEY_CLASSES_ROOT\Interface\{A1814F32-5250-4EA6-A5D7-2531FA859B6B}]
@="__x_Windows_CInternal_CUI_CBioEnrollment_CDataModel_CPin_CIPinEnrollmentManager"

[HKEY_CLASSES_ROOT\Interface\{A1814F32-5250-4EA6-A5D7-2531FA859B6B}\ProxyStubClsid32]
@="{FF5265E8-6D8B-4BD4-A52D-8D99008E04C8}"

[HKEY_CLASSES_ROOT\Interface\{CB756C0A-14EF-4D61-9889-86B1E132090E}]
@="__x_PinEnrollment_CIPinEnrollmentBroker"

[HKEY_CLASSES_ROOT\Interface\{CB756C0A-14EF-4D61-9889-86B1E132090E}\ProxyStubClsid32]
@="{C459DF76-7545-4671-A85D-A42903991174}"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Interface\{A1814F32-5250-4EA6-A5D7-2531FA859B6B}]
@="__x_Windows_CInternal_CUI_CBioEnrollment_CDataModel_CPin_CIPinEnrollmentManager"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Interface\{A1814F32-5250-4EA6-A5D7-2531FA859B6B}\ProxyStubClsid32]
@="{FF5265E8-6D8B-4BD4-A52D-8D99008E04C8}"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Interface\{CB756C0A-14EF-4D61-9889-86B1E132090E}]
@="__x_PinEnrollment_CIPinEnrollmentBroker"

[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Interface\{CB756C0A-14EF-4D61-9889-86B1E132090E}\ProxyStubClsid32]
@="{C459DF76-7545-4671-A85D-A42903991174}" 
```

_\* 由于操作中涉及到的注册项都是 `NT Service\TrustedInstaller` 管理的，在修改上可能会有一些麻烦，推荐使用 [NSudo](https://github.com/M2Team/NSudo) 或类似软件以 TrustedInstaller 权限一次性导入 reg 文件：_

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/4efdc108-dd9c-431f-8ac4-aefdb917aed3.png?raw=true)

导入完成后，这个组件就可以正常使用了。

0x04 设置 PIN 和生物特征
-----------------

系统设置中的 PIN 按钮暂时不会被激活，让我们再摁一下之前的小工具，现在它能正常唤起 Pin Enrollment 流程了：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/730cdafe-52ec-42b1-8424-bce87a05d249.png?raw=true)

按流程走一遍就可以正常设置 PIN，设置完 PIN 后可能会弹出一个这样的错误，点 **暂时跳过** 忽略即可：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/6bd1c905-c018-4b72-9b59-ede2910ca841.png?raw=true)

现在转到设置 APP，我们就可以正常添加生物特征或者修改 PIN 啦 ![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/adaca47b-3cdb-430a-b14b-5c87943ffdcd.png?raw=true)
：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-15%2010-57-18/3f128d1d-448b-40a9-ac5c-cf77fb66132e.png?raw=true)

需要注意的是，如果我们删除了 PIN，这两个按钮又会被禁用。如果还需要使用它们，请用之前提供的小工具重新调用 EnrollPin。

在 Windows Server 2019 中配置 PIN 并启用 Windows Hello 指纹登录
----------------------------------------------------