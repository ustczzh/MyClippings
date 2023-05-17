# 解决"System clock has been set back"、"FlexNet Licensing error:-88309"报错 - 陶小桃Blog
[解决"System clock has been set back"、"FlexNet Licensing error:-88309"报错 - 陶小桃Blog](https://www.52txr.cn/2022/ANSYSerror88309.html) 

 问题描述
----

> ANSYS LICENSE MANAGER ERROR  
> Failover feature 'ANSYS Mechanical Enterprise' specified in license  
> preferences is not available.  
> Request name ansys does not exist in the licensing pool.System clock has been set back.  
> Feature:ansys  
> License path: D:\\Program Files\\ANSYS Inc\\sharedFiles\\Licensing\\license_files\]ansyslmd.lic;  
> **FlexNet Licensing error:-88309**

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-17%2020-14-32/cca2ec98-ea32-466f-8947-a81906244184.webp?raw=true)
](https://my.52txr.cn/image-20220314155846289.png?imageMogr2/auto-orient/format/webp/interlace/1/blur/1x0/quality/50|watermark/1/image/aHR0cHM6Ly9zNC5heDF4LmNvbS8yMDIxLzEyLzMwL1RSRWQwMC5wbmc=/dissolve/36/gravity/SouthEast/dx/0/dy/0)

问题分析
----

大概率的原因是在系统安装完成之后，你闲着没事干瞎改了系统的时间，玩了一把穿越，反正我是这么个情况。有次闲着没事干把系统时间往后调了好几十年。

导致这个报错的的根本原因是ANSYS的一些关键位置（文件或文件夹）的时间有问题，比如现在是2022/3/14，有的地方却是2050/10/23日期。`System clock has been set back`是这个报错的关键语句。可能是电脑中了某种病毒，导致的修改；也有可能是自己作死的，比如博主就是属于这种。

问题解决
----

解决这个问题需要用到两个工具：

1、NEWFILETIME软件

[单击直接下载软件](https://my.52txr.cn/NewFileTime_x64.exe)

**NewFileTime中文版**是一款小巧易用能修改文件时间属性的工具,该软件为您提供便捷的更正和操纵时间戳记的任何文件和文件夹在您 system.several文件和/或文件夹,NewFileTime中文版可以修改文件的创建时间、访问时间、和修改时间三项数据。是修改文件时间信息的必备利器！

2、everything软件

[单击直接下载软件](https://my.52txr.cn/Everything.zip)

**Everything**是由voidtools开发的一款文件搜索工具，这款软件是基于名称实时定位文件和目录。Everything功能强大，体积小巧，第一次安装使用时会建立一个索引数据库，将所有文件和文件夹的名称导入其中，后续使用能够以极快的速度快速搜索，查找到你所需要的文件。

* * *

0、把所有的路径都要显示出来

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-17%2020-14-32/c3edc2f7-d604-4e0c-b446-d5ec6194a1f4.webp?raw=true)
](https://my.52txr.cn/image-20220314194856595.png?imageMogr2/auto-orient/format/webp/interlace/1/blur/1x0/quality/50|watermark/1/image/aHR0cHM6Ly9zNC5heDF4LmNvbS8yMDIxLzEyLzMwL1RSRWQwMC5wbmc=/dissolve/36/gravity/SouthEast/dx/0/dy/0)

1、下载并安装好这两款软件之后，都打开。

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-17%2020-14-32/9df59435-659f-46a4-9f65-e0b2c617ec1a.webp?raw=true)
](https://my.52txr.cn/image-20220314194033888.png?imageMogr2/auto-orient/format/webp/interlace/1/blur/1x0/quality/50|watermark/1/image/aHR0cHM6Ly9zNC5heDF4LmNvbS8yMDIxLzEyLzMwL1RSRWQwMC5wbmc=/dissolve/36/gravity/SouthEast/dx/0/dy/0)

2、排列顺序

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-17%2020-14-32/3af07a2d-953d-4df3-be01-e28c659cd932.webp?raw=true)
](https://my.52txr.cn/image-20220314194358950.png?imageMogr2/auto-orient/format/webp/interlace/1/blur/1x0/quality/50|watermark/1/image/aHR0cHM6Ly9zNC5heDF4LmNvbS8yMDIxLzEyLzMwL1RSRWQwMC5wbmc=/dissolve/36/gravity/SouthEast/dx/0/dy/0)

3、修改错误目录的时间。

尽可能多的减少错误的时间。虽然但是我修改了一部分之后就能打开ANSYS了，并且没有报错。

说明可能只有一部分关键的位置时间戳不能错，而不是所有的都要改。自己尽可能去尝试吧。具体修改那个目录就行了我也不是很清楚。

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-5-17%2020-14-32/28c4c6e5-dded-4a71-8191-7c2ff5baee5e.webp?raw=true)
](https://my.52txr.cn/image-20220314195111711.png?imageMogr2/auto-orient/format/webp/interlace/1/blur/1x0/quality/50|watermark/1/image/aHR0cHM6Ly9zNC5heDF4LmNvbS8yMDIxLzEyLzMwL1RSRWQwMC5wbmc=/dissolve/36/gravity/SouthEast/dx/0/dy/0)