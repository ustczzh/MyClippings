# 折腾高通410方案的随身WiFi - 寒冬日志
![](https://www.cwlog.net/usr/uploads/2022/07/1617460692.jpg)

![](https://www.cwlog.net/usr/uploads/2022/07/2042299673.jpg)

![](https://www.cwlog.net/usr/uploads/2022/07/1840610927.jpg)

简述
--

最近不少人在折腾这个，跟风买了几个来玩

总共买了5个，根据容量和基带来分，分别是：2个4G的UFI001C、1个4G的UFI003、2个8G的UFI003

出厂的安卓4.4.4系统，商家做了限制，如果插卡了会自动打开飞行模式禁止使用，根据我目前找到的资料，可以刷改过删除云控的安卓、OpenWRT、Debian，我目前给UFI001C分别刷了安卓和OpenWRT测试，用的联通卡，安卓系统下无线可以跑到30Mbps - 50Mbps，OpenWRT的下行只能跑到10Mbps - 15Mbps，上行却能跑到 20Mbps - 30Mbps，暂时不清楚是什么原因

安卓：

![](https://www.cwlog.net/usr/uploads/2022/07/654671281.png)

OpenWRT：

![](https://www.cwlog.net/usr/uploads/2022/07/320278603.jpg)

* * *

刷机
--

一、刷机前准备：  
1、ARDC、可以把设备屏幕内容显示到电脑上，集成了ADB和Fastboot；  
2、Miko Service Tool，备份还原工具，到手先备份整个EMMC，刷坏了可以还原回去，提取基带的时候也要用；  
3、纸盒UFI系列全能刷机：酷安大佬 酷铵水遍 制作的，可以一键刷安卓Root和移除云控的system分区；  
4、星海QCN工具：安卓系统下备份还原基带用的，需要Root权限；

下载链接：  
[](https://pan.baidu.com/s/17STlX_Iw0PSUO11pJrldAQ?pwd=adwz)[https://pan.baidu.com/s/17STlX\_Iw0PSUO11pJrldAQ?pwd=adwz](https://pan.baidu.com/s/17STlX_Iw0PSUO11pJrldAQ?pwd=adwz) ，提取码: adwz  
[](https://share.qust.me/%E8%B7%AF%E7%94%B1%E5%99%A8/%E9%9A%8F%E8%BA%AB%20wifi)[https://share.qust.me/%E8%B7%AF%E7%94%B1%E5%99%A8/%E9%9A%8F%E8%BA%AB%20wifi](https://share.qust.me/%E8%B7%AF%E7%94%B1%E5%99%A8/%E9%9A%8F%E8%BA%AB%20wifi)

二、刷安卓：

不想折腾推荐刷安卓，设置简单些，可以直接用酷安大佬 酷铵水遍 制作的一键刷机包，  
1、棒子买回来是默认开启adb的，插上电脑等待开机，先用ARDC连接设备，进入设置查看基带版本；

2、使用ARDC右侧执行adb命令 `adb reboot edl` 将设备重启到9008模式；

3、打开Miko Service Tool，选择 Read、Partition Backup/Erase，进入备份，分别备份所有分区（用于提取基带分区）和整个EMMC的镜像（用于救砖）；

![](https://www.cwlog.net/usr/uploads/2022/07/1213566890.png)

4、拔掉设备，打开纸盒UFI系列全能刷机，根据提示操作即可，刷完会有提示，拔掉重启，等待创建WiFi后连接；

![](https://www.cwlog.net/usr/uploads/2022/07/2253856670.png)

访问192.168.100.1打开管理后台，在里面设置WiFi信息和选择SIM卡；后台密码默认admin，如果无法登录需要用ARDC安装桌面（将APK拖进去即可安装），然后右击返回桌面，进入设置将设备恢复出厂设置一次；  
桌面下载连接：[迷你安卓桌面](https://pan.baidu.com/s/1zSeIttaR5WTQxVkHhi-4Hg?pwd=q5tn)

SIM 1是自己插的卡，SIM 2是机器自带的eSIM；如果插自己的卡刷完没网需要用星海QCN工具刷入全网通基带，在这里下载：[全网通基带](https://share.qust.me/%E8%B7%AF%E7%94%B1%E5%99%A8/%E9%9A%8F%E8%BA%AB%20wifi/%E9%9A%8F%E8%BA%ABWiFi%20%E5%88%B7%20openwrt/%E9%9A%8F%E8%BA%AB%20wifi%20%E5%9F%BA%E5%B8%A6)

三、刷OpenWRT：

OpenWRT功能比较多，可玩性高一些，但目前的版本下载速度不行

1、前3步参考安卓进行备份，然后提取分区备份那步备份出来的基带文件：`fsc、fsg、modemst1、modemst2`；

2、下载并解压对应基带版本的OpenWRT包：[下载地址](https://www.kancloud.cn/a813630449/ufi_car/2792820)

3、将先前提取的`fsc、fsg、modemst1、modemst2` 分别重命名，加上`.bin`后缀，移动到OpenWRT包内，覆盖包内原文件；

4、用ARDC执行ADB命令 `adb reboot bootloader`，将设备重启到Fastboot模式，运行OpenWRT包内的flash.bat，按几次回车执行完全部步骤，拔电重启；

默认基带一般只支持电信卡，如果要用移动卡或者联通卡需要先刷安卓，插卡正常上网后拔电关机进入9008备份上面的四个基带分区，替换到OpenWRT包内刷入；

总的来说：  
用安卓需要通过刷boot分区来获取root权限，刷改过的system分区来移除云控（也可以自己用root权限改），然后通过恢复出厂格式化用户分区来来清除商家的配置；刷OpenWRT需要先在安卓下提取设置好的基带分区（换其他运营商的卡要重新刷对应的基带），和对应版本的OpenWRT系统一起刷入；

参考：  
[](https://lxnchan.cn/ufi4gstick.html)[https://lxnchan.cn/ufi4gstick.html](https://lxnchan.cn/ufi4gstick.html)  
[](https://qust.me/post/msm8916/)[https://qust.me/post/msm8916/](https://qust.me/post/msm8916/)