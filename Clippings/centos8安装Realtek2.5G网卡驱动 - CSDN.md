# centos8安装Realtek2.5G网卡驱动 - CSDN
### 文章目录

*   [前言](#_7)
*   [一、获取驱动](#_15)
*   *   [1.装载centos系统](#1centos_16)
    *   [2.查询网卡驱动](#2_18)
    *   [3.获取网卡驱动](#3_24)
*   [二、安装驱动](#_29)
*   *   [1\. 解压驱动](#1__30)
    *   [2.可能遇到的问题](#2_39)
    *   *   [a. 错误提示 ：/src/r8125.h:68:20 error: redefinition of 'ether\_addr\_copy' static inline void ether\_addr\_copy](#a__srcr8125h6820_error_redefinition_of_ether_addr_copy_static_inline_void_ether_addr_copy_40)
        *   [b.错误提示 ：r8125\_n.c:12245:9 error :unknown field 'ndo\_change\_mtu' specified in initializer](#b_r8125_nc122459_error_unknown_field_ndo_change_mtu_specified_in_initializer_49)
        *   [c.错误提示 ：r8125\_n.c:13519:28 error 'struct net\_device' has no member named 'last\_rx'](#c_r8125_nc1351928_error_struct_net_device_has_no_member_named_last_rx_57)
*   [三. 添加网卡](#__64)
*   *   [1.进入网卡所在文件夹](#1_66)
    *   [2.配置网卡文件](#2_73)
    *   [3.启动网卡](#3_99)

* * *

前言
==

主板为B560M爆破弹，在安装完Centos7双系统后发现没有网卡驱动无法上网，网上查了点资料，发现还挺麻烦的

* * *

一、获取驱动
======

1.装载centos系统
------------

特别提示：在进行安装最开始选择`server with gui`时，右边会有拓展安装项，要选择`Development Tools`这个选项，不然gcc等一系列命令都无法使用。

2.查询网卡驱动
--------

使用`lspci`命令  
输出结果

```
Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8125 2.5GbE Controller (rev 04)

```

3.获取网卡驱动
--------

另外一台电脑访问  
https://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software  
将下载的对应驱动文件复制到U盘中，并上传到centos7系统中

二、安装驱动
======

1\. 解压驱动
--------

这里我将驱动文件放在 Downloads文件夹下面  
r8125-9.008.00.tar.bz2

```
tar -zxvf r8125-9.008.00.tar.bz2
cd r8125-9.008.00
sudo ./autorun.sh

```

2.可能遇到的问题
---------

### a. 错误提示 ：/src/r8125.h:68:20 error: redefinition of ‘ether\_addr\_copy’ static inline void ether\_addr\_copy

```
// 解决方案 在r8125.h 这个文件的第68行
#if LINUX_VERSION_CODE < KERNEL_VERSION(3,14,0)
// 改为
#if LINUX_VERSION_CODE < KERNEL_VERSION(3,10,0)

```

### b.错误提示 ：r8125\_n.c:12245:9 error :unknown field ‘ndo\_change\_mtu’ specified in initializer

```
// 解决方案
cat /usr/src/kernels/3.10.0-327.el7.x86_64/include/linux/netdevice.h | grep ndo_change_mtu
// 这个文件里面 ndo_change_mtu 定义的是什么名称  我这里是ndo_change_mtu_rh74
// 在r8125_n.c的12245行对应改掉就行了 将ndo_change_mtu改为ndo_change_mtu_rh74

```

### c.错误提示 ：r8125\_n.c:13519:28 error ‘struct net\_device’ has no member named ‘last\_rx’

```
//在提示行将之直接用//进行注释即可

```

完成之后继续进行操作 sudo ./autorun.sh

三. 添加网卡
=======

此时网络已经有了显卡，但是重启后要重新运行 ./autorun.sh，不如添加一块显卡来的稳定

1.进入网卡所在文件夹
-----------

```
cd /etc/sysconfig/network-scipts/
mv ifconfig-enp0s2 ifconfig-con	//创建一个ifconfig-con新显卡
vim ifconfig=con

```

2.配置网卡文件
--------

```
HWADDR=2C:F0:5D:EF:D7:7A   //这个是电脑自动生成 不用改和我一样
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static			//这里改为静态地质
DEFROUTE=yes
IPADDR=192.168.1.31		//给自己的电脑分配的地址
NETMASK=255.255.255.0	//掩码
GATEWAY=192.168.1.1		//要改
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME="con"		//次数要更改为ifconfig-xxxx后面这个xxxx
UUID=127e6fc2-d0db-3227-be65-6b3536b3e000	//这个是电脑自动生成 不用改和我一样
ONBOOT=yes
AUTOCONNECT_PRIORITY=-999
CONNECTION_METERED=no
DNS1=114.114.114.114					//一般要添加
DNS2=8.8.8.8				//一般要添加

```

3.启动网卡
------

重启后有上角有了Wire-Connected 然后进入Network删除多余的即可