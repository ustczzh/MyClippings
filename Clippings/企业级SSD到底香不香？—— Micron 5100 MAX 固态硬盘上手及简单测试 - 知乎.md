# 企业级SSD到底香不香？—— Micron 5100 MAX 固态硬盘上手及简单测试 - 知乎
[企业级SSD到底香不香？—— Micron 5100 MAX 固态硬盘上手及简单测试 - 知乎](https://zhuanlan.zhihu.com/p/446512705) 

 经过最近一年的折腾，本人逐步完善了家庭网络系统的搭建。随着实践深入和经验积累，不少从一开始就秉承的理念慢慢发生了变化，比如对虚拟化和All-In-One的看法：所有的设备放在一起运行，既不方便调试维护，也不利于最佳性能的发挥。至于NAS存储采用机械盘阵列，更是觉得毫无必要：使用了一块10T的HGST氦气盘做仓库，已经是我能够接受的磁盘体积和噪音上限了。

正好最近在小黄鱼上入手了一台NUC5i3RYH，可以用作独立NAS。这个准系统的价格比较给力（400块），但我手头却没有合适的存储介质。时逢双十一，各种电脑配件的促销活动让人应接不暇，但我最终也没剁手。垃圾佬的江湖，自然还是在海鲜市场~~

NUC没法接3.5寸硬盘，不光是尺寸的缘故，也因为主板上并没有12V供电，而且无法在不破坏机箱的情况下引出SATA接口；如果通过USB3.0口外接，只能使用独立供电，又使硬盘的电源管理变得复杂。最终我决定还是用上NUC的2.5寸硬盘位，淘一块可靠耐用的大容量SATA SSD。

### 准备工作

固态硬盘自然首先考虑大厂，比如英特尔，三星，镁光等。我家中已经有一些英特尔和三星的船货，所以这次出于尝鲜的目的，想试一试镁光的产品。

目前镁光的消费级SATA SSD自然是指英睿达（Crucial）的这两个型号了，各平台上一直有不少晒单和晒价。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/2df7a517-3054-4683-ab85-e28b4d3ac947.jpeg?raw=true)

但是我扒了扒口碑，觉得不太行。镁光和英特尔这两家做消费级产品一向很水，能缩就缩，能简就简，无他利薄耳。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/b7e4a404-0c20-4345-978a-a961e64221a0.jpeg?raw=true)

4T MX500的PCB板只有全盘一半不到的面积，用料之省可见一斑。而BX500甚至连个铝壳都不舍得配

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/87314c27-4733-43fd-a594-0ec19f9b7085.jpeg?raw=true)

MX500和BX500 1TB的寿命都只有360TBW，虽然不挖矿也够用了，但起码表明产品的设计规格不高

相比较而言，企业级产品的用料却并不含糊，在读写稳定性，使用寿命，掉电保护和命令延时等方面完胜，只是发热及功耗表现上可能稍逊于消费级产品。目前镁光的数据中心级固态存储分为5000，7000和9000三个系列，其中后两者作为中高端产品均采用了NVMe协议的u.2和m.2规格，只有5000系列采用了2.5寸/m.2 SATA规格。

2017-2020年镁光陆续推出了5100，5200和5300系列的3D TLC固态硬盘，容量从240GB到7680GB不等；每个系列又大致分为ECO，PRO和MAX三个型号——分别对应读取密集型，读写均衡型和写入密集型三种应用场景。各型号的顺序读写性能和随机读写性能基本一致，但在写入寿命和响应延迟等指标上有极大差别。对标其他家的产品大概是英特尔S4500/S4600系列和三星的PM863。

虽然存世量大，但该系列产品在网上的晒单和拆解很少，最专业和详细的只有贴吧上的一篇：

[镁光5100系列PRO MAX系列评测](https://link.zhihu.com/?target=https%3A//tieba.baidu.com/p/6147902239)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/9b842ce4-4fd3-48b0-a34c-78b3ac66a600.jpeg?raw=true)

图示为5100 MAX 1920G 2.5寸 SSD拆解，PCB板上满满当当的颗粒和电容~~

引用贴吧大佬开盖后的解说：

> 全盘几乎无螺丝，外壳为金属卡扣。有易损贴。单PCB设计，双面共16颗NW858颗粒，2颗D9STQ DDR3 DRAM，同样的88SS1074主控。经查询该颗粒为eTLC单颗粒1536Gb（192GB），总容量3072GB，可用1920GB，OP约1152GB（60%）。单颗粒4Die4CE，完全满足1074主控通道。总寿命17.6PB，颗粒PE数为5729次。另外值得注意的是，5100MAX的一面颗粒有导热胶贴，另一面没有。

ECO和PRO系列就寒碜多了，虽然依旧吊打消费级硬盘：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/173803bf-2f0d-4e5c-aa50-07e5bb12591a.jpeg?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/f81ab7a7-4845-4011-99d3-4a42733b33db.jpeg?raw=true)

图示为5200 ECO 480G 2.5寸SSD拆解，可以看清主控的型号88SS1074

目前5000系列的三种盘在海鲜市场存量很大，大部分型号和容量都有，使用程度和价格千差万别。

关于选购的标准，我有以下考量：

1）作为仓库盘，首先要保证容量。用于NAS存放常用的媒体文件，容量大概不能少于960G；

2）预算控制在1000以内，以海鲜市场目前的价格水平，容量大概不会超过1920G；

3）市面的货源以矿场锻炼盘居多。对于动辄写入几百上千TB的矿渣，即便是稳定的企业级固态，也保不准哪天会突然挂掉，谨慎起见不考虑入手；

4）全新盘不太看好。一方面价格偏高，另一方面虽然该系列使用的马牌主控88SS1074据说尚未破解，但对于非官方渠道的“全新零通电读写”固态硬盘，有必要适度警惕；

5）耐用度尽可能高。以5100为例，参考镁光官方的datasheet，除了ECO相对拉胯之外，PRO和MAX的写入寿命都很可观，MAX的耐用度甚至是同容量下MX500的25倍，相当逆天了！

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/a9349853-9323-4917-b1e9-febf8b4c0f09.jpeg?raw=true)

### 产品上手

最后经过多方比较和挑选，入手了一块价位七百多，写入量1T左右的5100 MAX 960G次新盘。至少从2019年开始，5100型号就在二手市场广泛存在了。当年也许有过大船货，不过现在流通的绝大部分是矿场精选和本地拆机，非全新盘的价格从5毛-8毛/G不等。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/4d58d3b2-a23c-4534-8f21-67e7db3e14f3.jpeg?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/b222e8a6-3c0a-47ff-b1bf-d646df77a60b.jpeg?raw=true)

我对这个外观一开始有些疑惑。因为相同或相关型号的硬盘，外观各不相同，以小黄鱼上的实物为例，有零售版，思科版，惠普版，联想版等等，即便是零售版的外观也不尽相同：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/2e0880a0-390e-49f8-aeb3-bee4229b27cd.jpeg?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/2f6e07ed-ea65-4406-bf3a-51b0c6da6084.jpeg?raw=true)

不过有易碎贴护体，我也无法拆开去验证主控和颗粒信息，反正贴吧大佬已经拆解过一次了。好奇心驱使下，给这块盘过了下秤，想侧面验证有没有造假或偷工减料：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/40ef016d-c7c4-4d81-988c-cf51aef358c8.jpeg?raw=true)

先用一块已知质量82g的S3710 400G SSD对厨房电子秤做了校正，计算出这块5100 MAX的实际质量为69.9g，和官方标称的不超过70g相吻合。从拆解图和预期使用寿命来看，960G及以上容量的5100 MAX应该是全长PCB和颗粒贴满的状态，但小容量的盘会轻得多。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/e6f41c54-d650-413a-b242-a0ff75a15bbf.jpeg?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/1a074e82-71d3-4ab1-85d4-a777eeb2cf41.jpeg?raw=true)

过秤的真实原因是看多了这样的图比较上头

### 性能测试

接下来是上机环节。首先看下CDI显示的S.M.A.R.T.信息：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/f476c52c-21a5-4412-abb5-cfcf98b8f7d0.jpeg?raw=true)

“重新分配扇区计数”，“保留块计数”这些信息都没有显示什么异常。从固件信息来看，应该是零售版无疑了。既然如此先更新一下固件，以满足我强迫症的心理需求。

先下载官方的固态硬盘管理工具MSE（Micron Storage Executive）

安装运行后可以看到各种硬盘信息，包括当前固件版本：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/edac5faa-6afd-49c1-9bfb-2084a68ab8ca.jpeg?raw=true)

不过这里显示当前已安装固件是最新版本（显然并不是），并无更新提示，那只好手动进行更新了。因为镁光的官方固件需要在网站注册并登录后才能下载，我是从其他网站下载的固件：

其中5100 MAX 960G对应的最新固件版本号是D0MU075（2021年2月），但不必提取固件压缩包中的bin文件，只要在MSE的固件更新页面下选择5100\_D0MU075\_D0MU445\_D0MU845\_Storage\_Executive\_fwbin.zip 即可手动更新，工具软件会自动选择正确的版本。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/65c45947-7cfd-47be-aa7c-5b09883ca368.jpeg?raw=true)

然后再看看读写速度是否符合标称。已知官方datasheet的性能列表如下：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/9f01bf73-50d9-4218-9784-44cd5775ad20.jpeg?raw=true)

先来两个娱乐跑分AS SSD Benchmark和CDM:

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/67bd6d1d-000f-48ca-bfa1-df7a68ec3db8.jpeg?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/8fd4b2dd-f498-4e56-ac70-768173dd1444.jpeg?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/b8fbddda-7045-4855-bba3-020dc7ac9212.jpeg?raw=true)

AS SSD的跑分结果比CDM和官标要差一些。本人的电脑配置是B360M + i5-9600KF + 32G内存，应该不至于影响测试结果。不过我对此类企业盘的需求定位是稳定与耐用，速度低些倒也无妨。引用贴吧大佬的话：

> 由于企业盘没有任何SLC CACHE。所有操作都是3D eTLC直写直读，然而eTLC为了寿命又降低了电压，也就是降低了直写性能。所以快餐跑分成绩必然没有ULTRA3D这种同1074主控的消费级硬盘好看。实际上ASSSD跑分能跑过Ultra3D、860EVO 1T的企业SATA不存在，即使是最强的S3710 1.2T，也要被860EVO 1T按在地上摩擦。毕竟这种盘的跑分都是当代主控+3D SLC的颗粒来跑分。

接下来再进入稍微专业点的测试。因为颗粒直读写和无缓存，那么测一小时和测五分钟的结果，除了温度之外应该不会有本质区别，所以我这里只进行一些轻度测试，毕竟是自用盘不舍得写入太多：

1) HDtune基准文件测试（100G）：稳如老狗~~

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/ad341014-4940-42da-b00d-7203967e9f85.jpeg?raw=true)

2) IOMETER读写稳定性测试：严格按照官方datasheet中的条件来执行，即顺序读写单元128KB，队列深度32；随机读写单元4KB，队列深度32；延迟读写单元4KB，队列深度1。每种模式测试5分钟。最终得到的平均读写速度（Total MBs per Second Decimal）竟然与官方结果完全一致！！

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/261c8abb-418b-427f-94d2-3cef3153c98b.jpeg?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/5263b359-30fd-44c7-b070-1b555189a39e.jpeg?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/c3f5b4eb-1065-4c95-81e5-462adebb8812.jpeg?raw=true)

瞬时顺序读取速度折线图（5分钟），依然稳稳当当，估计再跑一小时也是一样

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/41a08332-ba2b-45d7-af2b-0540b8692a96.jpeg?raw=true)

相比读取速度，瞬时顺序写入速度的波动稍微大一些

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/0105d034-3b0d-4b7c-9f8b-17b6f88f2e68.jpeg?raw=true)

随机4K的读取性能在90000IOPS左右，接近官标93000IOPS

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/eb1fd775-c304-492a-a937-f6b0b9bed7df.jpeg?raw=true)

随机4K的写入性能在79000IOPS左右，反而高于官标74000IOPS

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/8f7e5fd5-2985-4fca-8bd4-a4b8cf16975b.jpeg?raw=true)

七三开的随机4K混合读写，总性能在82000IOPS左右，比官标数据高14%左右

不过IOMETER虽然能采集实时的I/O延迟（Latency），却不能直观地显示延迟分布情况。如果要完全验证官方datasheet的结果，只能照葫芦画瓢，顺便学习一下非生产环境不太常见，但功能强大的I/O压力测试工具—— FIO

把硬盘拆下来装到另一台Xubuntu系统的电脑上，安装FIO：

```bash
~$ sudo apt update
~$ sudo apt install fio
```

然后分别执行命令：

```bash
128K顺序读	~$ sudo fio -name=5100max_sequential_read -filename=/dev/sda -iodepth=32 -direct=1 -ioengine=libaio -rw=read -bs=128k -runtime=300
128K顺序写	~$ sudo fio -name=5100max_sequential_write -filename=/dev/sda -iodepth=32 -direct=1 -ioengine=libaio -rw=write -bs=128k -runtime=300
4K随机读取	~$ sudo fio -name=5100max_random_read -filename=/dev/sda -iodepth=32 -direct=1 -ioengine=libaio -rw=randread -bs=4k -runtime=300
4K随机写入	~$ sudo fio -name=5100max_random_write -filename=/dev/sda -iodepth=32 -direct=1 -ioengine=libaio -rw=randwrite -bs=4k -runtime=300
4K混合读写	~$ sudo fio -name=5100max_70/30_R/W -filename=/dev/sda -iodepth=32 -direct=1 -ioengine=libaio -rw=randrw -rwmixread=70 -bs=4k -runtime=300
4K读取延迟	~$ sudo fio -name=5100max_read_latency -filename=/dev/sda -iodepth=1 -direct=1 -ioengine=libaio -rw=randread -bs=4k -percentile_list=99.9:99.999 -runtime=300
4K写入延迟	~$ sudo fio -name=5100max_write_latency -filename=/dev/sda -iodepth=1 -direct=1 -ioengine=libaio -rw=randwrite -bs=4k -percentile_list=99.9:99.999 -runtime=300
```

解释下常用参数的含义：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-1-31%2018-16-19/bb7aa7e1-7d83-491e-9fa4-042145bc118d.jpeg?raw=true)

测试内容除了用命令行执行以外，还能通过job文件的形式来组织，然后执行命令~$ fio job_file即可进行测试，此处不赘述。

因为FIO测试顺序读写性能和随机读写性能的结果与IOMETER差别不大，这里就不晒了。看一下关于I/O延迟的测试情况。

4K读取延迟：

```bash
~$ sudo fio -name=5100max_read_latency -filename=/dev/sda -iodepth=1 -direct=1 -ioengine=libaio -rw=randread -bs=4k -percentile_list=99.9:99.999 -runtime=300
5100max_read_latency: (g=0): rw=randread, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=1
fio-3.12
Starting 1 process
Jobs: 1 (f=1): [r(1)][100.0%][r=21.3MiB/s][r=5455 IOPS][eta 00m:00s]
5100max_read_latency: (groupid=0, jobs=1): err= 0: pid=27054: Thu Dec 16 19:05:41 2021
  read: IOPS=5842, BW=22.8MiB/s (23.9MB/s)(6847MiB/300001msec)
    slat (usec): min=6, max=3612, avg=18.83, stdev= 6.45
    clat (usec): min=3, max=13801, avg=148.77, stdev=47.66
     lat (usec): min=40, max=13818, avg=168.05, stdev=49.33
    clat percentiles (usec):
     | 99.900th=[  709], 99.999th=[ 2442]
   bw (  KiB/s): min=17520, max=26200, per=100.00%, avg=23372.89, stdev=1629.96, samples=599
   iops        : min= 4380, max= 6550, avg=5843.23, stdev=407.50, samples=599
  lat (usec)   : 4=0.01%, 10=0.01%, 20=0.01%, 50=2.29%, 100=1.15%
  lat (usec)   : 250=96.16%, 500=0.21%, 750=0.14%, 1000=0.02%
  lat (msec)   : 2=0.02%, 4=0.01%, 10=0.01%, 20=0.01%
  cpu          : usr=6.58%, sys=17.84%, ctx=1757619, majf=0, minf=11
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=1752765,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=22.8MiB/s (23.9MB/s), 22.8MiB/s-22.8MiB/s (23.9MB/s-23.9MB/s), io=6847MiB (7179MB), run=300001-300001msec

Disk stats (read/write):
  sda: ios=1752084/0, merge=0/0, ticks=255341/0, in_queue=255341, util=100.00%
```

4K写入延迟：

```bash
~$ sudo fio -name=5100max_write_latency -filename=/dev/sda -iodepth=1 -direct=1 -ioengine=libaio -rw=randwrite -bs=4k -percentile_list=99.9:99.999 -runtime=300
5100max_write_latency: (g=0): rw=randwrite, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=1
fio-3.12
Starting 1 process
Jobs: 1 (f=1): [w(1)][100.0%][w=84.8MiB/s][w=21.7k IOPS][eta 00m:00s]
5100max_write_latency: (groupid=0, jobs=1): err= 0: pid=31815: Thu Dec 16 19:12:12 2021
  write: IOPS=19.5k, BW=76.2MiB/s (79.9MB/s)(22.3GiB/300001msec); 0 zone resets
    slat (usec): min=5, max=2213, avg=13.38, stdev= 3.31
    clat (usec): min=2, max=10101, avg=35.48, stdev=12.67
     lat (usec): min=29, max=10121, avg=49.22, stdev=13.32
    clat percentiles (usec):
     | 99.900th=[   82], 99.999th=[ 1565]
   bw (  KiB/s): min=68952, max=93600, per=99.99%, avg=78061.93, stdev=3899.66, samples=599
   iops        : min=17238, max=23400, avg=19515.48, stdev=974.92, samples=599
  lat (usec)   : 4=0.02%, 10=0.01%, 20=0.02%, 50=99.00%, 100=0.90%
  lat (usec)   : 250=0.03%, 500=0.01%, 750=0.01%, 1000=0.01%
  lat (msec)   : 2=0.01%, 4=0.01%, 10=0.01%, 20=0.01%
  cpu          : usr=15.58%, sys=35.44%, ctx=5861668, majf=0, minf=11
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,5855471,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
  WRITE: bw=76.2MiB/s (79.9MB/s), 76.2MiB/s-76.2MiB/s (79.9MB/s-79.9MB/s), io=22.3GiB (23.0GB), run=300001-300001msec

Disk stats (read/write):
  sda: ios=0/5852933, merge=0/0, ticks=0/215879, in_queue=215879, util=100.00%
```

从输出结果中的“clat percentiles (usec)”这一项可以看到：

1）在99.9%的百分位数区间，随机读取延迟709μs与官方测试结果500μs相当；而在99.999%的百分位数区间，随机读取延迟2.442ms甚至远好于官方测试结果9.0ms。

2）在99.9%的百分位数区间，随机写入延迟82μs远好于官方测试结果500μs；而在99.999%的百分位数区间，随机写入延迟1.565ms同样远好于官方测试结果9.0ms。

总之没毛病。猜测结果有出入的原因：

1）官方肯定使用了大量的样品进行测试，而我这里只进行了单盘测试；

2）设定测试的时间很短（5分钟），长时间读写产生的偶发高延迟没有机会被采集；

3）其他硬件因素如CPU和环境因素如温度，可能也会对延迟测试结果产生少量影响。

所以也别说我测了个寂寞，大家当消遣看看就好~~

### 总结评价

本次测试的内容到此为止。入手的这块硬盘，除了在容量上稍显欠缺之外（仓库盘当然是越大越好了，买之前务必要想清楚），其他各方面的表现都完全符合心理预期。随着最近一些矿场的崩盘，流入二手市场的企业级固态硬盘价格非常低廉。虽说鱼龙混杂，上船的门槛比以往高了不少，但瞅准时机用心挑选，还是不难找到中意的商品。