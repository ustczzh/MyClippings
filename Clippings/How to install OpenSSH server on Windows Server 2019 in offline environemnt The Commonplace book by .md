# How to install OpenSSH server on Windows Server 2019 in offline environemnt | The Commonplace book by IT-Infrastructure Engineer
[How to install OpenSSH server on Windows Server 2019 in offline environemnt | The Commonplace book by IT-Infrastructure Engineer](https://it-infra-ya.com/en/ws19-sshserver_en/) 

 ![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-28%2012-44-43/10d2f903-56eb-4a77-9a0d-fb6c85b387a3.jpeg?raw=true)
    etc.

2022-03-02

OpenSSH has been added to Windows Server and supported since Windows Server 2019. Microsoft shows the installation guide on the following website.  
[https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh\_install\_firstuse](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)  
But to follow this guide, internet access is required.

If you can not access internet, it shows the following error on **See optional feature history**  
Error code: 0x8024402C

The following guide shows how to install OpenSSH server on Windows Server 2019 that is in an offline (blocked internet access) environment.  
There are two manners:

*   [**For using Features On Demand (FOD) media of Windows 10**](#using-fod)  
    You need a appropriate volume license or Visual Studio subscription.
*   **[For using OpenSSH from GitHub](#using-github)**  
    You don’t need any licenses or subscriptions.

For using Features On Demand (FOD) media of Windows 10
------------------------------------------------------

### 1\. Download FOD ISO of Windows 10

Download the ISO file of **Windows 10 Features on Demand, version 1809** from the Microsoft Volume Licensing Service Center (VLSC) or My Visual Studio on an online (internet-accessible) environment.

You need a appropriate license or subscription.

### 2\. Copy OpenSSH server package from FOD media

Copy **OpenSSH-Server-Package~31bf3856ad364e35~amd64~~.cab** from FOD ISO to the server to be installed OpenSSH server.

### 3\. Install OpenSSH server

To install OpenSSH server, run the following cmdlet on a PowerShell as administrator.  
Note: Assume that the cab file is on C:\\temp .

```
Add-WindowsCapability -online -name OpenSSH.Server~~~~0.0.1.0 -source C:\temp
```

If it shows like the following, installed successfully.

```
Path         : 
Online       : True
ResartNeeded : False
```

The procedures for using FOD is completed.

For using OpenSSH from GitHub
-----------------------------

### 1\. Download OpenSSH from GitHub

Download **OpenSSH-Win64.zip** from the following website on an online (internet-accessible) environment. And copy OpenSSH-Win64.zip to the server to be installed OpenSSH server.  
[https://github.com/PowerShell/Win32-OpenSSH/releases](https://github.com/PowerShell/Win32-OpenSSH/releases)

### 2\. Install OpenSSH

(1) Decompress OpenSSH-Win64.zip and move to C:\\Program Files\\OpenSSH-Win64 .

(2) Run OpenSSH-Win64\\install-sshd.ps1 on PowerShell as administrator.

```
cd "C:\Program Files\OpenSSH-Win64"
```

```
.\install-sshd.ps1
```

If it shows like the following, installed successfully.

```
sshd and ssh-agent services successfully installed
```

### 3\. Edit the service startup type

On the **Computer Management**, select **Computer Management** \> **Services and Applications** \> **Services**, on the properties of the following services, change the **Startup type** to **Automatic**.

*   OpenSSH SSH Server
*   OpenSSH Authentication Agent

The procedures for using OpenSSH from GitHub is completed.