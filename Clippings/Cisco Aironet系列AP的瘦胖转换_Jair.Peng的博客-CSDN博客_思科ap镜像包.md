# Cisco Aironet系列AP的瘦胖转换_Jair.Peng的博客-CSDN博客_思科ap镜像包
### Cisco Aironet系列AP的瘦胖转换

*   [两个方法做Cisco AP的瘦胖转换](#Cisco_AP_2)
*   *   [适用范围](#_7)
    *   [准备基础环境](#_10)
    *   [升级/转换AP IOS的方法一](#AP_IOS_45)
    *   [升级/转换AP IOS的方法二](#AP_IOS_99)
*   [已知的问题](#_116)
*   [AP命令](#AP_133)
*   [胖AP设备配置命令](#AP_135)
*   *   *   [案例一](#_258)
        *   [案例二](#_392)

一种是使用AP默认配置信息完成自动升级/转换，无需要手动配置AP的IP地址和[网关](https://so.csdn.net/so/search?q=%E7%BD%91%E5%85%B3&spm=1001.2101.3001.7020)，无需单独地运行TFTP和解压命令。  
一种是进阶的手动配置信息来完成升级/转换，需要情况AP的基本命令和使用。

适用范围
----

在没有WLC控制的情况下的Cisco Aironet 系列AP，已测试CAP1602、CAP2602、CAP3602

准备基础环境
------

| 工具 | 形式 | 下载链接/信息 |
| --- | --- | --- |
| 账号密码 | 文件 | 默认情况下均是 Cisco |
| Console线+USB-RS232转接线 | 硬件 | 有时候会因为线或硬件问题导致AP与TFTP Server之间无法建立连接，必要时换线或换PC重新测试 |
| PL2303驱动 | 软件 | USB-RS232-PL2303驱动下载 |
| RJ45网络跳线 | 硬件 |  |
| TFTP Server(例如TFTPD32/3CDaemon) | 软件 | TFTP Server-TFTPD32&64下载 |
| 需要转换/升级的AP的IOS | 镜像 | Cisco Aironet CAP系列胖IOS下载 |
| SecuCRT或PuTTY等CLI管理工具 | 软件 | [SecuCRT8.0下载](https://download.csdn.net/download/pzhier/9704897)、[PuTTY下载](https://www.putty.org/) |

1.  Console线+USB-RS232转接线#  
    USB一端与PC连接，Console线与AP的Console口连接，然后进入Windows设备管理器查看PL2303转接线的驱动。  
    如果PL2303未安装，请先正确下载安装PL2303驱动。安装驱动后，即可在电脑上正常识别到该串口设备。  
    此处记下对应的COM port number，我这里是COM3。  
    ![](https://img-blog.csdnimg.cn/20190831135521596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B6aGllcg==,size_16,color_FFFFFF,t_70)
    
2.  RJ45网络跳线  
    RJ45网络跳线两端分别连接PC与AP的Ethernet口连接。  
    进入控制面板\\网络中心里修改用于连接AP的网卡属性，IP设置为10.0.0.2/8。  
    ![](https://img-blog.csdnimg.cn/2019083114130121.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B6aGllcg==,size_16,color_FFFFFF,t_70)
    
3.  TFTP Server(例如TFTPD32/3CDaemon/SolarWinds)  
    下载、安装TFTP Server，常见有TFTPD32/3CDaemon/SolarWinds，我这里使用TFTP32。  
    配置TFTP Server，浏览并选择一个目录，用于存放下载好的IOS镜像。  
    选择服务器对应的端口，即用于与AP连接的网卡IP地址，我这里是10.0.0.2。  
    下载AP IOS并放到TFTP的配置目录下，并将IOS文件名改为以xxx.default。我这里是将ap3g2-k9w7-tar.153-3.JF10.tar改为ap3g2-k9w7-tar.default。至于为什么要这样改，请仔细往后看。  
    ![](https://img-blog.csdnimg.cn/20190831160452964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B6aGllcg==,size_16,color_FFFFFF,t_70)
    
4.  PuTTY或SecuCRT等CLI管理工具  
    下载、安装CLI管理工具，常见有PuTTY、SecuCRT和Xshell，我这里使用SecuCRT。  
    配置SecuCRT，参数如下图所示(端口是我们刚刚在设备管理器中查看到的COM3，这个端口根据实际情况修改)。  
    ![](https://img-blog.csdnimg.cn/20190831161717398.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B6aGllcg==,size_16,color_FFFFFF,t_70)
    
5.  准备好之后我们就可以用CLI管理工具打开控制界面，如下图所示：  
    以AP+MAC#的形式显示，说明这是一个瘦AP。  
    ![](https://img-blog.csdnimg.cn/20190831162101593.png)
    

升级/转换AP IOS的方法一
---------------

以下是升级/转换AP IOS的步骤

1.  断电开AP电源，按住AP上的MODE键，然后给AP加电开机。
    
2.  进入恢复模式。  
    方法是看到显示：button is pressed, wait for button to be released…这句话后释放AP的MODE键（一般需要按20-30秒，少于20秒不生效）。
    
    > AP7c69.f68d.3562#  
    > IOS Bootloader - Starting system.  
    > flash is writable  
    > FLASH CHIP: Numonyx Mirrorbit (0089)  
    > Xmodem file system is available.  
    > flashfs\[0\]: 23 files, 3 directories  
    > flashfs\[0\]: 0 orphaned files, 0 orphaned directories  
    > flashfs\[0\]: Total bytes: 31997952  
    > flashfs\[0\]: Bytes used: 7407104  
    > flashfs\[0\]: Bytes available: 24590848  
    > flashfs\[0\]: flashfs fsck took 17 seconds.  
    > Reading cookie from SEEPROM  
    > Base Ethernet MAC address: 7c:69:f6:8d:35:62  
    > Ethernet speed is 1000 Mb - FULL Duplex  
    > **button is pressed, wait for button to be released…**  
    > ![](https://img-blog.csdnimg.cn/20190831142216824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B6aGllcg==,size_16,color_FFFFFF,t_70)
    
3.  如下图所示，AP将会读取网络中TFTP服务器的文件。
    
    > button pressed for 25 seconds ##说明我按了25秒  
    > process\_config\_recovery: set IP address and config to default 10.0.0.1 ##Cisco AP默认会自动将IP地址设置为默认的10.0.0.1，然后将信息发往广播地址来寻找TFTP服务器，所以这也是为什么我们要将AP与TFTP服务器的IP配置在同一网段中的原因。  
    > process\_config\_recovery: image recovery  
    > image\_recovery: Download default IOS tar image tftp://255.255.255.255/ap3g2-k9w7-tar.default  
    > **##从网络中读取文件，这也是为什么我们要将IOS文件名设置为ap3g2-k9w7-tar.default的原因。这个文件名可能视不同系统版本而不同，但要以这个名为准，否则会提示无法读取到文件或提示没有找到相应的文件。  
    > 如果你之前没有设置好这个文件名，请将AP IOS文件名改成对应的.default名字，不用保留镜像包之前的后缀名。**   
    > examining image…  
    > DPAA Set for Independent Mode  
    > DPAA\_INIT = 0x0  
    > extracting info (285 bytes)  
    > Image info:  
    > Version Suffix: k9w7-.153-3.JF10  
    > Image Name: ap3g2-k9w7-mx.153-3.JF10 #这是我们下载的镜像  
    > Version Directory: ap3g2-k9w7-mx.153-3.JF10  
    > Ios Image Size: 10670592  
    > Total Image Size: 13814272  
    > Image Feature: WIRELESS LAN  
    > Image Family: AP3G2  
    > Wireless Switch Management Version: 8.5.151.0  
    > Extracting files… #解压文件到AP的Flash中  
    > ap3g2-k9w7-mx.153-3.JF10/ (directory) 0 (bytes)  
    > extracting ap3g2-k9w7-mx.153-3.JF10/ap3g2-k9w7-mx.153-3.JF10 (230262 bytes)…  
    > extracting ap3g2-k9w7-mx.153-3.JF10/ap3g2-k9w7-tx.153-3.JF10 (73 bytes)  
    > extracting ap3g2-k9w7-mx.153-3.JF10/ap3g2-bl-2600 (190140 bytes)…  
    > extracting ap3g2-k9w7-mx.153-3.JF10/ap3g2-bl-3600 (189183 bytes)…  
    > ![](https://img-blog.csdnimg.cn/20190831142333774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B6aGllcg==,size_16,color_FFFFFF,t_70)
    
4.  如下图所示，AP将会自动执行解压、删除旧镜像、安装新镜像等一系列工作，相应地TFTP服务器也会显示一些工作日志。  
    ![](https://img-blog.csdnimg.cn/20190831143250627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B6aGllcg==,size_16,color_FFFFFF,t_70)
    5\. 等待一段时间后，完成升级/转换后的版本显示如下图：  
    ![](https://img-blog.csdnimg.cn/2019083116514049.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B6aGllcg==,size_16,color_FFFFFF,t_70)
    

升级/转换AP IOS的方法二
---------------

在准备好基础环境，并进入AP恢复模式，按以下命令一步步操作即可。

> ap: delete flash:private-config #如果有足够空间，可以不使用该命令  
> ap: delete flash:private-multiple-fs #如果有足够空间，可以不使用该命令  
> ap: set IP\_ADDR 10.0.0.1 **#手动设置AP的IP地址，你也可以为172.16.4.100或其它IP地址，但也要相应地更改你TFTP Server的IP为其它同网段IP(例如172.16.4.3)**  
> ap: set NETMASK 255.255.255.0 #手动设置AP的子网掩码  
> ap: set DEFAULT\_ROUTER 10.0.0.2 **#手动设置AP的默认网关(TFTP Server IP)，与IP地址一样，也需要与TFTP的IP对应。**   
> ap: tftp\_init  
> ap: ether\_init  
> ap: flash\_init  
> ap: tar -xtract tftp://10.0.0.2/ap3g2-k9w7-tar.153-3.JF10.tar flash: #手动解压文件到AP的Flash中  
> ap: dir flash:/ #可以查看是否成功将文件解压到AP的Flash中  
> ap: set BOOT flash:/ap3g2-k9w7-tar.153-3.JF10/ap3g2-k9w7-tar.153-3.JF10  
> ap: set #查看以上的配置是否正确，确认启动的文件为我们刚刚导入的文件 BOOT=flash:/ap3g2-k9w7-tar.153-3.JF10/ap3g2-k9w7-tar.153-3.JF10  
> ap: boot #最后重启，AP将会自动完成升级/转换。  
> 如下图，来自Youtube截图所以文件名与IP地址与我们本文章的会不一样。  
> ![](https://img-blog.csdnimg.cn/20190831175014748.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B6aGllcg==,size_16,color_FFFFFF,t_70)

1.  设备空间已满
    
    > **A. 只删除掉当前目录下的版本文件即可：**   
    > ap: delete flash:c1140-k9w7-tar.124-25d.JA1.tar  
    > Are you sure you want to delete “flash:c1140-k9w7-tar.124-25d.JA1.tar” (y/n)?y  
    > File “flash:c1140-k9w7-tar.124-25d.JA1.tar” deleted  
    > ![](https://img-blog.csdnimg.cn/20190831173105923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3B6aGllcg==,size_16,color_FFFFFF,t_70)
    >   
    > **B. 如果要直接将整个flash格式化，可以使用以下命令：**   
    > ap: Fsck flash:
    
2.  初始化tftp模块确保可以正常使用tftp命令，否则输入tar –xtract命令会提示permission denied，如下图所示：  
    ![](https://img-blog.csdnimg.cn/20190831180447815.png)
    
    > 则使用命令tftp\_init
    

```
en
Cisco
conf t
line con 0
logging synchronous
exit
no enable sec
no ip domain-lookup
ip domain-name xxx.com
hostname XXXX-AP01

/配置管理IP和网关/
/在默认情况下，AP 把有线接口、2.4G Wi-Fi 接口和 5G Wi-Fi 接口绑定到一个桥接组（BVI1）上/
int bvi 1
no ipv6 enable
ip add 192.168.0.100 255.255.255.0
exit
ip default-gateway 192.168.0.1
 
/配置ssh用rsa密钥，中间的no是为防止已生成rsa密钥，如未生成会报错，忽略即可*/
crypto key gen rsa
no
1024
/配置ssh版本以及web登陆时所用认证，并关闭http启用https安全登陆/
ip ssh ver 2
no ip http ser
ip http auth local
ip http secure-ser
 
/配置用户名密码，删除默认账户/
username cisco pri 15 se Cisco123
 
/配置ssh登陆认证，日志同步，并限定只接受ssh安全登陆，此处配置与web登陆无关/
lin vty 0 15
 logging synchronous
 login local
 transport preferred ssh
 transport input ssh
exi
 
/配置时区，注意UTC为标准时区，如果输入CN等，由于未知时区，会报错，UTC为时区，非国家/
clock timezone UTC 8 0
sntp server 202.120.2.101
 
/防止密码穷举，配置限定300秒内输入错误5次密码，禁止登陆120秒，记录登陆失败事件并发送至syslog/
login block 300 att 5 with 120
login on-f log
 
/配置日志审计，记录输入的配置命令（查看命令不在里面），隐藏输入密码，并发送syslog/
arch
log config
hidekey
logg enable
notify syslog
 
/配置arp-cache 使AP缓存ARP并提供客户端解析记录，全局开启band-select（5G优先于2.4G）允许ssid使用/
dot11 arp-cache
dot11 band-select parameters
 
/配置SSID/认证/密码/广播SSID，VLAN ID，启用band-select（5G优先于2.4G）/
/也可以只起一个SSID，启用了band-select后会自动优选5G，因需要在绑定SSID时绑定到同一个Wi-Fi接口(例如本案例是bridge-group 1)即可/
dot11 ssid cisco
   vlan 1
   band-select
   authentication open
   authentication key-management wpa version 2
   guest-mode
   wpa ascii Cisco123
   
dot11 ssid cisco-5G
   vlan 1
   band-select
   authentication open
   authentication key-management wpa version 2
   guest-mode
   wpa ascii Cisco123
 
 
/配置有线连接接口vlanID对应子接口/
/注意，此为g 0口的子接口，对于型号为1131、1242这类802.11G的AP由于不存在千兆口，请改为int f 0.110这样的百兆子接口/
int g 0.1
enc dot 1
bridge-group 1
 
/2.4G对应VlanID的子接口，绑定到bridge-group 1/
int dot 0.1
enc dot 1
bridge-group 1
 
/5G对应VlanID的子接口，绑定到bridge-group 1/
int dot 1.1
enc dot 1
bridge-group 1

/启用2.4G接口。并配置ssid，以及ssid对应Vlan封装，将 SSID 绑定到 Wi-Fi 接口上/
int dot 0
encryption mode ciphers aes
encryption vlan 1 mode ciphers aes
ssid cisco
channel least-congested 2412 2437 2462
antenna gain 128
power local maximum
power client maximum
stbc
beamform ofdm
/启用接口/
no shutdown
 
/启用5G接口。并配置ssid，以及ssid对应Vlan封装，将 SSID 绑定到 Wi-Fi 接口上/
int dot 1
encryption mode ciphers aes
encryption vlan 1 mode ciphers aes
ssid cisco-5G
channel least-congested
antenna gain 128
power local maximum
power client maximum
stbc
beamform ofdm
no shutdown

```

### 案例一

```
`主要配置命令
hostname ap      /设置AP名
 
dot11 ssid AP-N2    /设置SSID（AP-N2为SSID号）
authentication open    /认证开放
authentication key-management wpa version 2     /认证密钥版本为WPA版本2
guest-mode    /显示SSID，no guest-mode表示隐藏SSID
wpa-psk ascii 123456789      /SSID登陆密钥设置（123456789为密钥）
exit

interface dot11radio0      /进入2.4G接口
encryption mode ciphers aes-ccm     /加密密码AES CCM模式
ssid AP-N2       /加入SSID AP-N2
power local maximum   /设置AP信号发射功率，默认可不配置
no shutdown           /开启端口
exit

interface dot11radio1      /进入5.8G接口
encryption mode ciphers aes-ccm      /加密密码AES CCM模式
ssid AP-N2       /加入SSID AP-N2
power local maximum   /设置AP信号发射功率，默认可不配置
no shutdown           /开启端口
exit

interface BVI1    进入管理接口
ip address 192.168.0.191 255.255.255.0          /配置接口IP地址
no shutdown           /开启端口

ip default-gateway 192.168.0.1     /配置网关

配置范例
version 15.2
no service pad
service timestamps debug datetime localtime
service timestamps log datetime localtime
service password-encryption
!
hostname test-ap
!
logging rate-limit console 9
enable secret 5 $5$4fXI$i1Zl2wBy24uDQd2j2Tuf41
!
no aaa new-model
clock timezone BeiJing 8 0
no ip source-route
no ip cef
!
dot11 syslog
!
dot11 ssid testssid
   authentication open 
   authentication key-management wpa version 2
   guest-mode
    wpa-psk ascii 7 1443130D010939102541
!
dot11 guest
!
username myname secret 5 $4$7p8Z$yjeef33iW422J3bDrdN8Q/
!
bridge irb
!
interface Dot11Radio0
 no ip address
 no shutdown
 !
 encryption mode ciphers aes-ccm 
 !
 ssid testssid
 !
 antenna gain 0
 power local maximum
 packet retries 64 drop-packet
 bridge-group 1
 bridge-group 1 subscriber-loop-control
 bridge-group 1 spanning-disabled
!
interface Dot11Radio1
 no ip address
 !
 encryption mode ciphers aes-ccm 
 !
 ssid testssid
 !
 no shutdown
 !
 antenna gain 0
 power local maximum
 peakdetect
 dfs band 3 block
 packet retries 64 drop-packet
 bridge-group 1
 bridge-group 1 spanning-disabled
!
interface GigabitEthernet0
 no ip address
 duplex auto
 speed auto
 bridge-group 1
 bridge-group 1 spanning-disabled
!
interface GigabitEthernet1
 no ip address
 duplex auto
 speed auto
 bridge-group 1
 bridge-group 1 spanning-disabled
!
interface BVI1
 ip address 192.168.0.16 255.255.255.0
 ipv6 address dhcp
 ipv6 address autoconfig
 ipv6 enable
!
ip default-gateway 192.168.0.1
ip forward-protocol nd
ip http server
no ip http secure-server
ip http help-path http://www.cisco.com/warp/public/779/smbiz/prodconfig/help/eag
!
snmp-server community public RO
bridge 1 route ip
!
line con 0
line vty 0 4
 login local
 transport input all
!
sntp server 192.168.0.1
end` 

![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)

*   1
*   2
*   3
*   4
*   5
*   6
*   7
*   8
*   9
*   10
*   11
*   12
*   13
*   14
*   15
*   16
*   17
*   18
*   19
*   20
*   21
*   22
*   23
*   24
*   25
*   26
*   27
*   28
*   29
*   30
*   31
*   32
*   33
*   34
*   35
*   36
*   37
*   38
*   39
*   40
*   41
*   42
*   43
*   44
*   45
*   46
*   47
*   48
*   49
*   50
*   51
*   52
*   53
*   54
*   55
*   56
*   57
*   58
*   59
*   60
*   61
*   62
*   63
*   64
*   65
*   66
*   67
*   68
*   69
*   70
*   71
*   72
*   73
*   74
*   75
*   76
*   77
*   78
*   79
*   80
*   81
*   82
*   83
*   84
*   85
*   86
*   87
*   88
*   89
*   90
*   91
*   92
*   93
*   94
*   95
*   96
*   97
*   98
*   99
*   100
*   101
*   102
*   103
*   104
*   105
*   106
*   107
*   108
*   109
*   110
*   111
*   112
*   113
*   114
*   115
*   116
*   117
*   118
*   119
*   120
*   121
*   122
*   123
*   124
*   125
*   126
*   127
*   128
*   129


```

### 案例二

建立两个不同的SSID，然后分别这调用在Dot11 Radio0和Dot11 Radio1

```
`dot11 ssid Cisco-5G
	authentication open
	authentication key-management wpa version 2
	guest-mode
	wpa-psk ascii 7 PASSWORD
!
dot11 ssid Cisco-2.4G
	authentication open
	authentication key-management wpa version 2
	guest-mode
	wpa-psk ascii 7 PASSWORD
!
username Cisco privilege 15 password Cisco
!
interface Dot11Radio0
no ip address
encryption mode ciphers aes-ccm
!
ssid Cisco-2.4G
!
antenna gain 0
station-role root
bridge-group 1
bridge-group 1 subscriber-loop-control
bridge-group 1 spanning-disabled
bridge-group 1 block-unknown-source
no bridge-group 1 source-learning
no bridge-group 1 unicast-flooding
!
!
interface Dot11Radio1
no ip address
encryption mode ciphers aes-ccm
!
ssid Cisco-5G
!
antenna gain 0
peakdetect
dfs band 3 block
channel dfs
station-role root
bridge-group 1
bridge-group 1 subscriber-loop-control
bridge-group 1 spanning-disabled
bridge-group 1 block-unknown-source
no bridge-group 1 source-learning
no bridge-group 1 unicast-flooding
!
!
!
interface GigabitEthernet0
no ip address
duplex auto
speed auto
bridge-group 1
bridge-group 1 spanning-disabled
no bridge-group 1 source-learning
!
!
interface BVI1
ip address 192.168.0.100 255.255.255.0
no shutdown` 

![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)

*   1
*   2
*   3
*   4
*   5
*   6
*   7
*   8
*   9
*   10
*   11
*   12
*   13
*   14
*   15
*   16
*   17
*   18
*   19
*   20
*   21
*   22
*   23
*   24
*   25
*   26
*   27
*   28
*   29
*   30
*   31
*   32
*   33
*   34
*   35
*   36
*   37
*   38
*   39
*   40
*   41
*   42
*   43
*   44
*   45
*   46
*   47
*   48
*   49
*   50
*   51
*   52
*   53
*   54
*   55
*   56
*   57
*   58
*   59
*   60
*   61
*   62


```

参考文章：  
https://www.youtube.com/watch?v=c70mOOpqMsg  
https://blog.csdn.net/xiaoyaoleer/article/details/90450212