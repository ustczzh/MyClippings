# centos8安装RTL2.5G网卡驱动 - CSDN
有时候我们装完Centos或Ubuntu系统会发现没有网卡信息，说明网卡驱动不匹配，所以我们要重新安装。

首先去服务器主板官网下载网卡驱动，注意32位和64位。

先查看网卡型号

```
lspci | grep -i ethernet
```

 我的网卡型号是  00:1f.6 Ethernet controller: Intel Corporation Device 0d55。

![](https://img-blog.csdnimg.cn/20210817150818611.png)

 网上一找，压根没找到这型号的网卡，别说驱动了。怀着试一试的心态，就安装i219-v系的网卡驱动，还别说，成功了。

针对i219-v网卡的linux版本的驱动下载地址：

[适用于 PCIe\* 英特尔® 千兆位以太网网络连接的英特尔® 网络适配器驱动程序 Linux\* 下](https://downloadcenter.intel.com/zh-cn/download/15817?_ga=1.159975677.114505945.1484457019 "适用于 PCIe* 英特尔® 千兆位以太网网络连接的英特尔® 网络适配器驱动程序 Linux* 下")

![](https://img-blog.csdnimg.cn/20210817151234141.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dzaGMxOA==,size_16,color_FFFFFF,t_70)

 下载完之后，通过u盘拷到安装centos的电脑里面，也只能通过u盘，移动硬盘这些工具了，因为没有联网啊。不过我的是最小化安装，折腾半天也识别不了U盘。只能通过手机USB连接互联网。把下载下来的安装包放到公司的演示环境项目上，直接下载到服务器上。用的这个命令：

wget http://域名/e1000e-3.8.4.tar.gz

 检查依赖环境

```
rpm -qa | grep kernel
```

![](https://img-blog.csdnimg.cn/20210817152053245.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dzaGMxOA==,size_16,color_FFFFFF,t_70)

 没有的话把这些依赖装上，命令是 yum install 依赖名

先查看安装kernel依赖后生成的目录名，cd到kernel目录下

```
[root@localhost e1000e-3.8.4]# cd /usr/src/kernels/
```

![](https://img-blog.csdnimg.cn/20210817153115176.png)

 把安装包解压

```
tar -zxf  e1000e-3.8.4.tar.gz
```

切换到root用户进入解压缩后的驱动文件夹，进入里面包含一个src目录。

![](https://img-blog.csdnimg.cn/20210817152348693.png)

 编辑 common.mk, 在63行后面回车，加入一行“/usr/src/kernels/3.10.0-1160.36.2.el7.x86\_64 \\”

修改前：

KSP := /lib/modules/${BUILD\_KERNEL}/source \\  
/lib/modules/${BUILD\_KERNEL}/build \\  
/usr/src/linux-${BUILD\_KERNEL} \\  
/usr/src/linux-$(${BUILD\_KERNEL} | sed ‘s/-.\*//’) \\  
/usr/src/kernel-headers-${BUILD\_KERNEL} \\  
/usr/src/kernel-source-${BUILD\_KERNEL} \\  
/usr/src/linux-$(${BUILD\_KERNEL} | sed ‘s/\\(\[0-9\]\*\\.\[0-9\]\*\\)\\..\*/\\1/’) \\  
/usr/src/linux \\  
/usr/src/kernels/${BUILD\_KERNEL} \\  
/usr/src/kernels

安装kernel-devel以后kernel开发目录为 /usr/src/kernels/3.10.0-1160.36.2.el7.x86\_64，如何查看开发目录，上面有介绍。  
修改后，添加一行,变成这样

KSP := /lib/modules/${BUILD\_KERNEL}/source \\  
/lib/modules/${BUILD\_KERNEL}/build \\  
/usr/src/kernels/3.10.0-1160.36.2.el7.x86\_64 \\  
/usr/src/linux-${BUILD\_KERNEL} \\  
/usr/src/linux-$(${BUILD\_KERNEL} | sed ‘s/-.\*//’) \\  
/usr/src/kernel-headers-${BUILD\_KERNEL} \\  
/usr/src/kernel-source-${BUILD\_KERNEL} \\  
/usr/src/linux-$(${BUILD\_KERNEL} | sed ‘s/\\(\[0-9\]\*\\.\[0-9\]\*\\)\\..\*/\\1/’) \\  
/usr/src/linux \\  
/usr/src/kernels/${BUILD\_KERNEL} \\  
/usr/src/kernels

 修改完后在src目录下，依次执行：

```
make             ## 编译驱动器源码
make install     ## 安装相应的驱动器程序
```

我这里有报错：

\[root@ megaraid\_sas-07.712.02.00\]# make

compile.sh: line 39: ./clean.sh: No such file or directory

compile.sh: line 40: ctags: command not found

make: Entering directory \`/usr/src/kernels/3.10.0-862.el7.centos.x86\_64'

arch/x86/Makefile:166: \*\*\* CONFIG\_RETPOLINE=y, but not supported by the compiler. Compiler update recommended.. Stop.

make: Leaving directory \`/usr/src/kernels/3.10.0-862.el7.centos.x86\_64'

解决：

vim /usr/src/kernels/3.10.0-1160.36.2.el7.x86\_64/arch/x86/Makefile

修改Makefile,  第166行，注释掉KBUILD\_CFLAGS += $(RETPOLINE\_CFLAGS) -DRETPOLINE  和  $(error CONFIG\_RETPOLINE=y, but not supported by the compiler. Compiler update recommended.)

![](https://img-blog.csdnimg.cn/20210817154011700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dzaGMxOA==,size_16,color_FFFFFF,t_70)

继续尝试编译成功.

没有报错，进入目录/lib/modules/3.10.0-1160.36.2.el7.x86\_64/updates/drivers/net/ethernet/intel/e1000e下，把e1000e.ko文件拷贝到目录/lib/modules/3.10.0-1160.36.2.el7.x86\_64/updates/drivers/net下

```
cp e1000e.ko /lib/modules/3.10.0-1160.36.2.el7.x86_64/updates/drivers/net
```

加载驱动程序

```
depmod -a
```

测试驱动程序，没报错说明正确。

```
modprobe e1000e
```

查看是否已经加载：

```
lsmod
```

![](https://img-blog.csdnimg.cn/20210817154509175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dzaGMxOA==,size_16,color_FFFFFF,t_70)

重启网络服务

```
service network restart
```

![](https://img-blog.csdnimg.cn/20210817154555147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dzaGMxOA==,size_16,color_FFFFFF,t_70)

 网络通了。

参考网络：[Linux系统安装网卡驱动\_luhuaxiang的博客-CSDN博客\_linux网卡驱动](https://blog.csdn.net/luhuaxiang/article/details/88396882?utm_term=linux%E5%AE%89%E8%A3%85e1000e&utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduweb~default-4-88396882&spm=3001.4430 "Linux系统安装网卡驱动_luhuaxiang的博客-CSDN博客_linux网卡驱动")

[在centos7上安装intel e1000e 网卡驱动 | bloomsource.org](https://www.bloomsource.org/blog/?p=164 "在centos7上安装intel e1000e 网卡驱动 | bloomsource.org")

[RAID卡更新驱动\_owlcity123的博客-CSDN博客\_config\_retpoline=y](https://blog.csdn.net/owlcity123/article/details/111478790 "RAID卡更新驱动_owlcity123的博客-CSDN博客_config_retpoline=y")