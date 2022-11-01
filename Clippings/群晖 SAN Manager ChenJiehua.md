# 群晖 SAN Manager | ChenJiehua
去年群晖 DSM7.0 发布正式版至今已经快一年了，期间官方也在不停的修复各种bug，而最近刚发布了 DSM7.1。目前 DSM7 应该是比较稳定了，因此决定将NAS系统升级一下，于是便发现了 SAN Manager 这个有趣的套件。

前世今生
----

早在 DSM6.2 的时候，群晖系统里就已经提供了 iSCSI Manager，当时看其简介说可以帮助轻松管理和监控 iSCSI 服务，可惜对 iSCSI 不甚了解就一直没有管。

转眼来到 DSM7.0，曾经的 iSCSI Manager 已经升级成 SAN Manager。

官方对其介绍：

> SAN Manager 可帮助您轻松管理和监控 iSCSI 和光纤通道服务。iSCSI 和光纤通道是存储区域网络 (SAN) 服务，可提供对整合的块级数据存储的访问。客户端可以在存储网络上像访问本地硬盘那样访问存储空间。

看其功能描述：

*   提供用于虚拟化环境的高性能且可靠的存储解决方案
*   提供直观的 iSCSI 服务管理和监控
*   虚拟化认证，包括 VMware® vSphere™、Microsoft® Hyper-V®、Citrix® XenServer™ 和 OpenStack Cinder

再看使用界面：

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-11-1%2008-53-47/918911f9-ef4c-4bad-93fb-5d400a06207b.png?raw=true)
](https://chenjiehua.me/wp-content/uploads/2022/06/image-31.png)

第一次看到上面的描述介绍是不是有点无从下手，SAN、LUN、iSCSI这些都是啥呀，相互之间有什么关联，都要怎么使用呢？

基本概念
----

### SAN

Storage Area Network（存储区域网络），是一种将计算机系统连接到高性能存储系统的专用高速网络。

### LUN

Logic Unit Number（逻辑单元号），是一个存储设备的编号，可以简单理解LUN指的就是存储设备，通过块存储系统向主机呈现并可用于进行格式化的存储卷。

此外，还有一个词需要了解：**Target**（目标），它用来标识可以由主机访问的单个存储单元。

一般而言，有两种方式向主机呈现存储系统：单个Target上包含多个LUN、单个Target只包含单个LUN：

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-11-1%2008-53-47/3cfe7f30-f87b-408f-ae99-51cc545db28a.png?raw=true)
](https://chenjiehua.me/wp-content/uploads/2022/06/image-32.png)

在群晖NAS中，[官方建议](https://kb.synology.cn/zh-cn/DSM/tutorial/How_to_configure_permission_for_hosts_in_SAN_Manager)将所有的LUN映射到同一目标，然后再为每个LUN分配权限来提高安全性。

不过此时务必开启“**允许从一个或多个iSCSI启动器的多重链接**”，否则将会出现只有第一个主机能正常连接，其他主机都访问不了的情况。

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-11-1%2008-53-47/bcf9a75c-64b5-4328-86ec-15cdb584af8e.png?raw=true)
](https://chenjiehua.me/wp-content/uploads/2022/06/image-34.png)

### SCSI

Small Computer System Interface（小型计算机系统接口），是一种用于计算机及其周边设备之间（硬盘、软驱、光驱、打印机、扫描仪等）系统级接口的独立处理器标准。SCSI标准定义命令、通信协议以及实体的电气特性（换成OSI的说法，就是占据物理层、链接层、套接层、应用层），最大部分的应用是在存储设备上。

对于硬盘，常用的有SATA和SAS，其中SAS即 Serial Attached SCSI。

### iSCSI

Internet SCSI，顾名思义，它是通过以太网来传输SCSI命令的技术，它将SCSI命令封装在TCP/IP包里，并使用一个iSCSI帧头。

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-11-1%2008-53-47/157b30e9-ed35-4bd5-84a8-b3d979ad8646.png?raw=true)
](https://chenjiehua.me/wp-content/uploads/2022/06/image-33.png)

说得更加通俗一点，iSCSI可以将一个远程存储设备映射到本地，并作为块设备（也即磁盘）来使用。对于用户而言，这个磁盘在使用上与本地硬盘基本一样（除了读写速度）。

在操作过程中，可能还会碰到一个词：iSCSI Initiator（启动器），启动器有软件类型、也有专用硬件，在非专业环境下，我们接触到的一般是主机上安装的软件程序。

### NFS

Network File System（网络文件系统），我们可以使用 NFS 客户端通过网络访问位于 NAS 服务器上的 NFS 卷并挂载卷，并将其用作 NFS 数据存储。

其实 NFS 和 iSCSI 都是为了解决存储资源的共享问题，不过 **iSCSI 在本地呈现的是一个块设备（磁盘），而 NFS 则是一个目录树**。

挂载iSCSI
-------

首先，我们要创建一个LUN并分配到某个Target，在 SAN Manager 中进行操作：

创建完成：

接着我们就可以在本地挂载该 iSCSI设备，以Windows10为例，先打开 “iSCSI 发起程序”：

然后在磁盘管理器中就可以看到新增了一块磁盘：

挂载NFS
-----

先在 NAS 中开启 NFS服务，并为某个共享文件夹设置NFS权限：

在NFS权限里面可以设置允许访问的IP、读写权限，特别需要注意**在没有使用Kerberos验证的情况下（默认AUTH\_SYS），必须设置Squash映射用户**，否则极有可能因客户端用户与NAS用户不一致导致挂载失败：

接着我们就可以通过NFS相关命令来进行挂载了，以Windows10为例，首先需要开启 NFS服务：

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-11-1%2008-53-47/9d6d9b3b-724e-4339-b657-ef95edac3a19.png?raw=true)
](https://chenjiehua.me/wp-content/uploads/2022/06/image-45.png)

然后以管理员身份运行命令提示符：

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-11-1%2008-53-47/11f11e1a-ff7b-4d3e-85ad-3226cc2c93d4.png?raw=true)
](https://chenjiehua.me/wp-content/uploads/2022/06/image-46.png)

查看服务器的共享目录：

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-11-1%2008-53-47/92e19707-9c56-4cc0-b86a-d5b30f032399.png?raw=true)
](https://chenjiehua.me/wp-content/uploads/2022/06/image-51.png)

使用 mout 命令进行挂载：

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-11-1%2008-53-47/9546d87b-ad95-4d4b-a53f-3e0c81b8582e.png?raw=true)
](https://chenjiehua.me/wp-content/uploads/2022/06/image-53.png)

卸载 NFS卷：

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2022-11-1%2008-53-47/124d4073-814c-4b6b-ac6f-6085a3d54693.png?raw=true)
](https://chenjiehua.me/wp-content/uploads/2022/06/image-54.png)

参考：

*   [Synology SAN Manager Desc](https://kb.synology.cn/zh-cn/DSMUC/help/DSMUC/SANManager/SANManager_desc?version=)
*   [Synology SAN Manager Spec](https://www.synology.cn/zh-cn/dsm/7.1/software_spec/san_manager)