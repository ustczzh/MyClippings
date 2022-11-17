# Windows下同时使用多网卡时的跃点数修改_周黎明CN的博客-CSDN博客_windows修改路由跃点数
本文介绍在Windows 10操作系统下多网卡同时使用时的跃点数修改方法，适用于每个网卡所在子网具有不同网关的情况。****修改跃点数的目的是使得应用程序能够在多网卡同时存在时正确访问外网。****

****第一步：查看windows IPv4路由表****

****打开命令行，输入命令：route print -4****

![](https://img-blog.csdnimg.cn/20210616101313905.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvemxtbmkx,size_16,color_FFFFFF,t_70)

图中网卡接口号为9的网卡为需要上外网的网卡，其余为上内网的网卡。

IPv4路由表中第二条记录

0.0.0.0 0.0.0.0 192.168.8.1 192.168.8.104 61

表示为外网网卡配置的通用路由，其中192.168.8.1为外网网卡网关地址，如果不确定可使用ipconfig命令查看（略）（如果不存在针对外网网卡的网络目标和网络掩码为0.0.0.0的记录也没关系，下文会提到解决办法），其中最后一个数字表示总跃点数，数值越小表示优先级越高。

****总跃点 = 高级TCP/IP设置中的接口跃点数 + metric跃点数****

****第二步：“高级TCP/IP设置中的接口跃点数”的设置过程如下：****

1）打开外网网卡的网络适配器的TCP/IPv4属性设置，点击“高级”

![](https://img-blog.csdnimg.cn/20210616101713678.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvemxtbmkx,size_16,color_FFFFFF,t_70)

2）将高级TCP/IP设置中的“自动跃点”取消勾选

![](https://img-blog.csdnimg.cn/20210616101734803.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvemxtbmkx,size_16,color_FFFFFF,t_70)

3）将接口跃点数设置为1（最小值），确定并关闭设置窗口...

![](https://img-blog.csdnimg.cn/20210616101802807.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvemxtbmkx,size_16,color_FFFFFF,t_70)

****第三步：“metric跃点数”的设置过程如下：****

在[命令行](https://so.csdn.net/so/search?q=%E5%91%BD%E4%BB%A4%E8%A1%8C&spm=1001.2101.3001.7020)中输入命令

****route change 0.0.0.0 mask 0.0.0.0 192.168.8.1 metric 20 if 9****

其中最后20表示metric跃点数的目标值， 9表示网卡接口号。如果在上文中通过route print -4命令查到的路由表中没有如下的记录：

0.0.0.0          0.0.0.0      192.168.8.1    192.168.8.104     XX

则可使用如下命令进行添加：

****route add 0.0.0.0 mask 0.0.0.0 192.168.8.1 metric 20 if 9****

最后再通过route print -4查看修改结果

![](https://img-blog.csdnimg.cn/20210616101919935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvemxtbmkx,size_16,color_FFFFFF,t_70)

****此时外网网卡的总跃点已经变成了21，低于其它网卡，因此应用程序均能够默认访问外网了。****

****上文中使用route change或route add命令修改的活动路由可能在电脑重启后被复原，如果需要永久修改，可在命令中增加“-p”，例如:****

****route change -p 0.0.0.0 mask 0.0.0.0 192.168.8.1 metric 20 if 9****

****完！****