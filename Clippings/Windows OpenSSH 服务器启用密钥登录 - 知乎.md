# Windows OpenSSH 服务器启用密钥登录 - 知乎
[Windows OpenSSH 服务器启用密钥登录 - 知乎](https://zhuanlan.zhihu.com/p/404179039) 

 由于奇怪的需求需要部署一台windows服务器，记录一下部署的过程

1安装Windows OpenSSH
------------------

首先通过powershell安装OpenSSH的服务端

在开始图标上点击右键，选择_Windows PowerShell(管理员)(A)_

在powershell中输入：

```text
# 安装OpenSSH客户端
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

#安装OpenSSH服务端
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

在powershell中输入：

```text
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'
```

若返回为：

```text
Name  : OpenSSH.Client~~~~0.0.1.0
State : Installed

Name  : OpenSSH.Server~~~~0.0.1.0
State : Installed
```

则表示安装成功，若返回为：

```text
Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

表示安装失败，再来一遍吧，注意全程需要**管理员权限。** 

2启动SSH服务器
---------

依然是以管理员身份打开PowerShell，然后运行以下命令来启动 _sshd service_

```text
# 启动sshd服务
Start-Service sshd

# 将sshd服务设置为自动启动，若不设置需要在每次重启后重新开启sshd
Set-Service -Name sshd -StartupType 'Automatic'

# 确认防火墙规则，一般在安装时会配置好
Get-NetFirewallRule -Name *ssh*

# 若安装时未添加防火墙规则"OpenSSH-Server-In-TCP"，则通过以下命令添加
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

3开启密钥登录
-------

这一步是重中之重，加强安全，减少麻烦！！！

生成密钥的方法都是一样的，可以自行搜索，与linux不同的地方在于权限管理和默认authorized_keys存放位置。

公钥 (~\\.ssh\\id_rsa.pub) 的内容需放置在服务器上的一个名为`authorized_keys`的文本文件中，该文件位于 C:\\Users\\username\\.ssh\\。 OpenSSH 客户端包括了 scp 来帮助实现此目的，这是一个安全的文件传输实用工具。

将本地的公钥部署到服务器上

```powershell
# 确保服务器上存在.ssh 文件夹，若不存在则使用下面命令创建
ssh username@ip mkdir C:\Users\username\.ssh\

#通过scp将本地的公钥上传到服务器上并重命名为authorized_keys，注意此方法会覆盖原有authorized_keys
scp C:\Users\username\.ssh\id_rsa.pub user1@ip:C:\Users\username\.ssh\authorized_keys
```

通过上述方法会覆盖原有authorized\_keys文件，若要添加多个公钥，则通过记事本（更推荐使用vscode等文本编辑器）打开authorized\_keys，把另起一行并把新公钥粘贴到authorized_keys文件中。

**以下是windows中特有的操作**

更改authorized_keys文件权限，不更改则无法通过密钥登录

```text
# 远程通过ACL更改文件权限
ssh --% user1@ip icacls.exe "C:\Users\username\.ssh\authorized_keys" /inheritance:r /grant "Administrators:F" /grant "SYSTEM:F"
```

在服务器端则可以通过以下命令修改，注意需要管理员权限。

```text
#在服务器端修改authorized_keys文件权限
icacls.exe "C:\Users\username\.ssh\authorized_keys" /inheritance:r /grant "Administrators:F" /grant "SYSTEM:F"
```

在Windows OpenSSH中，默认的授权密钥存放位置为ProgramData\\ssh\\administrators\_authorized\_keys，此位置对应为管理用户权限。因此需要修改默认授权文件位置。通过文本编辑器（推荐vscode）打开ProgramData\\ssh\\sshd_config，修改以下条目

```text
#允许公钥授权访问，确保条目不被注释
PubkeyAuthentication yes

#授权文件存放位置，确保条目不被注释
AuthorizedKeysFile	.ssh/authorized_keys

#可选，关闭密码登录，提高安全性
PasswordAuthentication no

#注释掉默认授权文件位置，确保以下条目被注释
#Match Group administrators
#       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
```

注意修改sshd_config需要管理员权限，修改完成后保存并推出。

在PowerShell(管理员)中重启sshd服务

```text
#重启sshd，需要管理员权限
Restart-Service sshd
```

至此可以尽情享受windows服务器带来的各种不便了！enjoy！！！

最后附赠如何使用PowerShell卸载Windows OpenSSH

```text
# 卸载 OpenSSH 客户端
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# 卸载 OpenSSH 服务端
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

参考内容：

1.  [适用于 Windows 的 OpenSSH 密钥管理 | Microsoft Docs](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_keymanagement)
2.  [多台WIN10之间的SSH免密登录 (zhihu.com)](https://www.zhihu.com/tardis/sogou/art/111812831)
3.  [sshd_config · PowerShell/Win32-OpenSSH Wiki (github.com)](https://link.zhihu.com/?target=https%3A//github.com/PowerShell/Win32-OpenSSH/wiki/sshd_config)