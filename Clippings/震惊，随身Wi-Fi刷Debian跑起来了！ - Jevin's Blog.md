# 震惊，随身Wi-Fi刷Debian跑起来了！ - Jevin's Blog
前几天在逛loc时，无意间看到有MJJ发了个随身Wi-Fi的车，短短几天里面大家就把它从14块买涨价到了19块。当时我就很好奇，一个随身Wi-Fi棒子为啥会有这样的吸引力？结果仔细看回帖才知道十来块钱的东西，给了骁龙410+512MB的配置，跑着Android系统，还可以root玩，或者刷OpenWRT、Debian玩。可以说是性价比炸裂了，忍不住也买了一根，周末到货就按搜来的教程刷了一遍，记录一下经过（根据别人的教程，我再复写一遍☺）。

#### 感谢

首先，在刷机过程中，我的所有操作基本上是根据两个教程来操作的，在此对两位大佬表示感谢！

> 参考教程：《高通骁龙芯片的随身wifi入门刷机教程》，作者：伏莱兮浜  
> 链接地址：[](https://www.coolapk.com/feed/37834896?shareKey=OTQ3NWNhOTZkY2UwNjJkZjRhNDI)[https://www.coolapk.com/feed/37834896?shareKey=OTQ3NWNhOTZkY2UwNjJkZjRhNDI](https://www.coolapk.com/feed/37834896?shareKey=OTQ3NWNhOTZkY2UwNjJkZjRhNDI)  
> 参考教程2：《UFI 系列 4G WiFi 棒研究记录》，作者：lxnchan  
> 链接地址：[](https://lxnchan.cn/ufi4gstick.html)[https://lxnchan.cn/ufi4gstick.html](https://lxnchan.cn/ufi4gstick.html)

#### 工具包

另外，在刷机过程中用到了各种软件工具和[苏苏小亮亮大佬制作的Debian固件包](https://www.coolapk.com/feed/36547490?shareKey=MDk1MjQ1Zjg1OTk1NjJkZjYzMDg)，在此也感谢各位大佬的辛勤付出！  
我基于别人分享的内容进行了增删，打了一个新的包，里面都是刷棒子必须的东西。  
工具包地址：[](https://pan.baidu.com/s/1WK_dDOQjy7nyFeCAQrTBZQ?pwd=2i3m)[https://pan.baidu.com/s/1WK\_dDOQjy7nyFeCAQrTBZQ?pwd=2i3m](https://pan.baidu.com/s/1WK_dDOQjy7nyFeCAQrTBZQ?pwd=2i3m)  
提取码: 2i3m  
在本文描述中所用到的软件，该工具包均已囊括，且在Windows 10 专业版系统中经历了成功的实践。

#### 识别硬件

根据网上的说法，目前随身Wi-Fi版本众多，卖家不会标注也不会告知你硬件的具体配置，所以购买的过程也是碰运气。大家可以去酷安搜一下相关内容，有大佬会分享自己购买的链接，直接上车即可！

不过，不管是自己碰运气下单还是上的别人车，到手之后最好都拆开自己看一下，版号是否满足刷机条件，怎么判断呢，基本上就是看电路板丝印信息，是否有匹配对应的固件包，有就可以刷，没有就不行。目前常见的丝印信息有UFI001B、UFI001C、UFI003、UZ801、SP970等。

![](https://www.jevin.org/usr/uploads/2022/08/3890584713.png)

拆的时候，仅需拧下3颗小螺丝，没有难度！

如图可见，我买的这个随身Wi-Fi的丝印信息为UFI003\_MB\_V02，是属于可以刷机的设备。

根据后面的确认，我这个棒子的具体配置为：

| 项目 | 参数 |
| --- | --- |
| CPU | Qualcomm Snapdragon MSM8916 Cortex-A53 × 4 @ 1.2GHz |
| RAM | 512MB |
| ROM | 8G |
| 备注 | CPU即高通骁龙410，是采用28nm工艺制程的64位4核处理器 |

#### 刷前备份

不管你有多么熟练，在刷机之前都建议进行一次完整的备份，这是我们在进行各种折腾前都需要做好的一个必要操作。

##### 安装驱动

1、在工具包中找到驱动文件夹，打开并安装文件夹里的vivo9008drivers.exe

2、插入Wi-Fi棒，打开ADB驱动安装工具(通用).exe，点击右下方Install安装ADB

![](https://www.jevin.org/usr/uploads/2022/08/2248183068.png)

##### 开启9008模式

1、插入Wi-Fi棒

2、在工具包中调试文件夹里，找到并运行搞机工具箱V9.10文件夹里的搞机工具箱V9.exe

3、在左侧切换到ADB终端，右边上面的命令栏里输入：

```null
adb reboot edl

```

![](https://www.jevin.org/usr/uploads/2022/08/3852516442.png)

此时，我们在电脑的设备管理器中，应该可以看到有一个高通的9008结尾的设备出现了。

![](https://www.jevin.org/usr/uploads/2022/08/402967395.png)

###### Miko备份

前面的准备工作做完了，现在就应该正儿八经开始备份了，安装工具包里救砖备份的对应软件，并根据对应的说明文件完成激活工作。

通过loader打开miko\_service\_tool\_pro\_v5.3\_fix，就可以看到这样一个程序界面

![](https://www.jevin.org/usr/uploads/2022/08/1483865898.png)

然后，选择到Read标签页，并按照下图数字逐一点击对应按钮

![](https://www.jevin.org/usr/uploads/2022/08/231850648.png)

操作过程中会让你选择保存备份文件的地址，以及你自己命名的一个bin文件。

大约5分钟左右，下方进度条走完，右侧也表示Success，则备份完成了！

![](https://www.jevin.org/usr/uploads/2022/08/2303561319.png)

###### Qualcomm Premium Tool备份

安装并激活Qualcomm Premium Tool（用注册机的时候强烈建议关掉声音），然后让棒子进入9008模式。

然后打开Qualcomm Premium Tool，右侧选择到Qualcomm标签，并按图标注的数字操作。

![](https://www.jevin.org/usr/uploads/2022/08/3050688984.png)

需要注意的是，这个操作要点两次下方的Do Job按钮，第一次是扫描识别设备，第二次是备份，第二次点击后，等待它备份完成即可。

![](https://www.jevin.org/usr/uploads/2022/08/3022901562.png)

###### root系统

在执行后面的备份工作前，我们需要先对系统进行root操作，也很简单，首先安装工具包里，图形化桌面文件夹中的ARDC软件，这是一个投屏软件，帮助我们在可视化的情况下对棒子进行操作。

在本项工作中，我们让棒子正常启动进入Android系统就好，ARDC上会看到一个简陋的界面，此时我们需要安装一个桌面软件来进入系统。

在文件夹内找到这个launcher.apk，将其拖入ARDC软件界面上，就会自动把launcher安装到棒子中了。

![](https://www.jevin.org/usr/uploads/2022/08/3355770995.png)

拖进去等个几秒钟就可以操作，通过鼠标左键实现点击/长按等操作，右键实现Android返回键的操作，此时我们点一下鼠标右键，就会出现一个弹窗，让选择桌面程序，我们选择刚拖进去的launcher就行了！

![](https://www.jevin.org/usr/uploads/2022/08/3456501231.png)

接着继续把工具包里同一文件夹内的es文件管理器和magisk拖入安装，安装好后桌面就会有对应的图标啦！

![](https://www.jevin.org/usr/uploads/2022/08/2896570521.png)

打开es文件管理器，把我们前面用Qualcomm Premium Tool备份的boot文件拖入下载文件夹，然后打开magisk开始安装，点击下一步后会让你选择一个文件修复，这个时候我们就要选择刚才拖进来的boot文件，以文件的方式。

![](https://www.jevin.org/usr/uploads/2022/08/444422155.png)

修补好了之后在es文件管理器打开下载，可以看到除了boot，又有了另一个文件，就是修补好的boot，这里建议重命名为自己能记住的名字（鼠标左键长按）。

然后点击ARDC上面菜单栏最后面的箭头，打开右侧ADB操作区，在CMD栏输入：

```null
adb pull /sdcard/Download/aaa.img b:/ccc

```

这个命令的意思是把刚才修补的文件传到电脑上，其中aaa是你修补的文件名字，b是电脑的盘符，ccc是对应存放的文件夹路径。输入完成后回车或者点右边那个叉叉就行了。

接下来进入bootloader去刷boot了，继续输入命令：

```null
adb reboot bootloader

```

进入bootloader之后，再输入：

```null
fastboot flash boot

```

按个空格，把刚才导出到电脑的修补过的boot文件拖到命令栏处，会出现它的路径，直接执行就行了！

等右边那个区域显示刷写完成之后，就可以重启了，还是打命令：

```null
fastboot reboot

```

重启之后再去打开magisk，会一直卡在启动页面，这是正常的，耐心等就完事了！

终于进入主界面，我们可以看到下面多了4个图标，这就说明，我们root成功了。

![](https://www.jevin.org/usr/uploads/2022/08/775824159.png)

接下来继续打命令获取一下超级用户的root权限：

```null
adb shell su

```

在弹窗里点允许之后就可以关掉ARDC了。

###### 备份QCN

任务管理器看一下，关掉adb进程，然后打开工具包救砖备份里面的星海SVIP，如果报错请先百度微软常用运行库来装一下。

然后就是按图，上面选择高通，下面高通强开走起一键执行。

![](https://www.jevin.org/usr/uploads/2022/08/263620265.png)

此时，我们可以在电脑的设备管理器看到有一个结尾是901D的高通设备。

![](https://www.jevin.org/usr/uploads/2022/08/3571573488.png)

看到设备出现了，星海这边还是高通里面，选择备份QCN，点击一键执行。

![](https://www.jevin.org/usr/uploads/2022/08/4071171961.png)

这次会需要等比较久一点的时间（其实也就两三分钟的样子），主要是要看你保存的QCN文件大小，应该是有500k以上才是正常的。

![](https://www.jevin.org/usr/uploads/2022/08/2300484257.png)

至此，我们虽然还没开始刷机，但是我们已经备份完毕了！以上流程基本遵照酷安的伏莱兮浜大佬教程操作，如果有看不明白的地方，可以看大佬的原版教程参考印证，本文开头就有链接！

#### 开始刷机

经过前面的一系列操作的热身之后，其实刷机就很简单了，基本上就是前面的Qualcomm Premium Tool备份简化版操作，加上会在电脑上双击文件就行了。

##### 专门再次备份基带

按照前面的方法进入9008模式，打开Qualcomm Premium Tool，还是选择高通，partition，这次Scan之后，不要点Backup All了，依次选择列表里的modemst1、modemst2、fsc、fsg进行备份。

![](https://www.jevin.org/usr/uploads/2022/08/3469673640.png)

是依次备份，就是选中一个，然后下面Backup点一下Do Job，把这4个备份文件单独放在一个文件夹里，并加上.bin的后缀。

![](https://www.jevin.org/usr/uploads/2022/08/2430714919.png)

##### 刷机

然后找到工具包里固件包文件夹里的固件，我这里是刷的苏苏小亮亮大佬的Debian，所以找到了对应版号的固件压缩包，解压出来，把刚才备份的4个以.bin结尾的文件拖进去。

![](https://www.jevin.org/usr/uploads/2022/08/3761586001.png)

棒子正常插上，打开工具包里调试文件夹下的高技工具箱，让棒子一键进入线刷模式。

![](https://www.jevin.org/usr/uploads/2022/08/2965719835.png)

好了之后，再回来固件包这边，找到刚才那个解压出来的文件里的flash.bat，双击就开始刷了。

![](https://www.jevin.org/usr/uploads/2022/08/1015761003.png)

直到最后All Done，表示刷机完成了，与前面的备份比起来，刷机真的是简单得不行。

#### 其他

刷完之后，用手机和电脑看无线网络，应该可以看到一个名称：4G\_UFI\_123456的无线热点，通过密码：12345678进行连接，可以看到设备所在的IP地址（似乎都是192.168.68.1）

![](https://www.jevin.org/usr/uploads/2022/08/2031711401.png)

后面就是正常的SSH连接了，我因为不知道账号和密码还搞错了两次，后来才注意到教程里都有说，账户就是root账户，密码：1，于是最终终于正常连接上了！

![](https://www.jevin.org/usr/uploads/2022/08/1059458547.png)

后面把棒子拿到另一个电脑上插的时候，

发现系统没有识别，设备管理器显示是一个感叹号的RNDIS，这个时候我们右键更新驱动，浏览电脑里的驱动程序，设备类型选网络适配器，厂商这边选Microsoft，右边型号选基于远程NDIS的Internet设备，然后下一步安装就行了，电脑就正确识别了。

![](https://www.jevin.org/usr/uploads/2022/08/1909760332.png)

#### 最后

其实这个玩具的乐趣就是上面刷机的过程，在这个设备和它的使用场景限制下，其实很难有合适的业务去跑的，所以刷完就可以把棒子收起来吃灰了。

其实我在记录自己刷机过程的时候，觉得我基本就是把前面说到的两位大佬的教程复写了一遍，感觉没有带来实质意义。

备份阶段，我操作的时候只觉得在用magisk修补boot的那里比较模糊，我其实可以只写那一点点就行了，避免别人跟我一样在那里迟疑一下（我觉得大概率是因为我没用Android手机的原因，如果是经常root的人，应该一看就明白了）。

刷机阶段，备份基带的地方，有两个modemst文件，原始教程都写的modemst1，我还找了一下是不是有个modemstl之类的，最终确定应该是大佬笔误了，其实我只写这一点应该就够了。

所以，水了这么长，其实就是复写了一遍大佬的内容，意义没有那么大，且算是有个自己随时方便查看的备份吧（毕竟博客时长在这里摆着，自己存一份才是最稳的，哈哈哈）。