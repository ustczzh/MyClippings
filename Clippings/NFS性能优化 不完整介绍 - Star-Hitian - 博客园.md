# NFS性能优化 不完整介绍 - Star-Hitian - 博客园
[NFS性能优化 不完整介绍 - Star-Hitian - 博客园](https://www.cnblogs.com/Star-Haitian/articles/8995628.html) 

 https://blog.csdn.net/freedom8531/article/details/43793517

~~~~~~~~~~~~~

Table of Contents
-----------------

*   1 NFS概述
*   2 设置NFS读写块大小，优化传输速度
*   3 网络包大小和网卡驱动
*   4 网络包分片导致的溢出
*   5 使用NFS over TCP
*   6 超时和重传值
*   7 守护进程NFSD的个数
*   8 输入队列的内存限制
*   9 关闭网卡和集线器的自动协商协议
*   10 NFS的同步和非同步选项
*   11 和NFS无关的提高服务器性能的方法
*   12 文件属性与目录属性的更新时间

### 1 NFS概述

NFS：Network file system，网络文件系统.由sun公司1984年推出，用来在网络中的多台计算机间实现资源共享(包括象文件或cd-rom).设计的目的是：实现在不同系统间交互使用，所以它的通信协议采用与主机和操作系统无关的技术

NFS Server可以看作是File Server，它可以让你的PC通过网络将远端得NFS SERVER共享出来的档案MOUNT到自己的系统中，在CLIENT看来使用NFS的远端文件就象是在使用本地文件一样.

NFS协议从诞生到现在有多个版本： NFS V2（rfc1094）, NFS V3（rfc1813）(最新的版本是V4(rfc3010))

如何查看nfs当前的版本：

rpm -qi portmap

rpm -qi nfs-utils

### 2 设置NFS读写块大小，优化传输速度

在客户端中有两个挂载选项——rsize和wsize——来设定客户端和服务器间来回的数据包大小。如果没有设定默认的rsize和wsize一般是4K(4096 byte)。理论上，NFS v2的最大块大小是8K，但是NFS v3最大的是32K(32*1024 byte)（over TCP）或者64K（over UDP），在linux服务端上，最大的块大小定义在内核的源文件./include/linux/nfsd/const.h中的NFSSVC_MAXBLKSIZE。根据不同的需要，默认值可能太大或者太小。这就需要通过不断的实验调整rsize和wsize。

首先是在客户端写文件，设定文件块为32K，共写入16384个文件块，文件大小为32K*16384

```
\# time dd if=/dev/zero of=/mnt/home/testfile bs=32k count=16384
```

然后是在客户端读文件，文件大小一般要是内存大小的两倍,以保证不被内存缓冲机制所缓冲

```
\# time dd if=/mnt/home/testfile of=/dev/null bs=16k
```

这样重复几次取平均值。在每次测试前记得在客户端重新挂载文件系统，这样能清除缓冲。  
然后，换更大或者更小的块大小。要保证块大小是1024的整数倍，并且不能超过NFS最大的系统块大小限制。不论\*NFSSVC_MAXBLKSIZE\*定义的是多少，NFS Version 2最大的块大小都是8K；Version 3可以允许最大支持64K（over UDP），文件的块大小必须是2的幂。尽管如此，一些用户也报告说可以不是2的幂，但也是文件系统和网络包大小的整数倍。

挂载更大的块以后，cd进挂载的文件目录，执行ls命令，确保一切正常。如果rsize/wsize太大，系统就会表现得很奇怪并且不能100%的显示命令输出。一种典型的现象就是，执行ls命令后，不能显示完整的文件列表，摈弃没有任何的错误嘻嘻，或者没有任何错误提示的情况下读文件失败。在指定好rsize/wsize以后，可以再次测试速度。

通常的做法都是在/etc/fstab中指定rsize/wsize的大小：

```
\# echo "foo:/tmp /mnt/foo nfs rw,hard,intr,rsize=8192,wsize=8192 0 0"
    >\> /etc/fstab
\# mkdir /mnt/foo
\# mount /mnt/foo
```

### 3 网络包大小和网卡驱动

大部分的linux网卡驱动是正常的，但是总有少数有问题。有问题的时候可以尝试升级驱动程序，来看看是否能解决问题。先使用ping back的命令

```
#ping -f
#ping -s
```

如果有很多丢包或者长时间的响应，就很有可能是网卡的问题了

除此以外，还用nfsstat分析NFS的流量，客户端和服务器统计，网络统计。选项“-o net”可以显示丢包的数目。在UDP传输中最重要的统计项是重传数——可能是由于丢包，socket缓冲移出，超时等情况引起。这些都会严重影响NFS性能，必须仔细检查。

PS：nfsstat不再支持参数“-z”（把nfsstat的统计清零），所以得留意之前的nfsstat统计值

为了纠正网络问题，有时候需要重新配置网卡的默认IP包大小。经常会发生路由的最大IP包大小比网卡的小。TCP协议可以自适应，但是UDP协议不行（会导致分片丢包，然后重传）。所以NFS over UDP特别要注意设置MTU的大小。可以用命令\*tracepath\*来看网络上的MTU值，用ifconfig命令来看网卡的MTU值，要使两者匹配。

### 4 网络包分片导致的溢出

当rsize/wsize大于网络的MTU（大部分网络都是1500，除非设置了支持大包）时，IP包在用UDP协议传输时会分片。大量IP包分片会消耗网络两端大量的CPU资源，而且还会导致网络通信更不稳定（因为完整的RPC在UDP分片的任何一个包丢失时都得整个RPC重传）。任何RPC的重传增加都会导致时延的增加。这是NFS over UDP性能的最大瓶颈。

如果你的网络拓扑很复杂，UDP的分片包的路由很可能不同，可能不会都及时到达服务器。内核对分片包的缓存有限制，最大值由ipfrag/\_high\_thresh指定。可以查看文件/proc/sys/net/ipv4/ipfrag\_high\_thresh和/proc/sys/net/ipv4/ipfrag\_low\_thresh。一旦未处理的包数目超过ipfrag\_high\_thresh，内核就会丢弃分片包，直到数量达到ipfrag\_low\_thresh

另一种监视的方法是文件/proc/net/snmp中的IP：ReasmFails。这是分片组合失败的数量，如果这个值在大量文件传输过程中上升太快，就有可能是有上述问题。

### 5 使用NFS over TCP

NFS over TCP 是 2.4和2.5内核的新特性。和UDP相比有不同的优缺点。

*   优点是在低可靠性的网络中的性能优于UDP。一旦丢包只要重传一个包而不是整个RPC。另外，TCP在不同网速的网络中可以自协调速度，性能也会比UDP好。
*   缺点是TCP是可靠连接协议，如果在包的传输过程中服务端奔溃了，在客户端需要重新挂载

### 6 超时和重传值

NFS over UDP 中有两个选项 timeo 和 retrans ，分别控制超时和重传数。其中timeo的单位是0.1s，默认值是0.7s。retrans是超时后重传的次数，默认值是3。重传时会显示消息

```
Server not responding
```

一旦客户端显示这个消息，客户端就会重新发起请求。如果这时正好另一个超时发生了，客户端就会先处理更近的超时错误，处理结束后再接着处理这个超时。如果处理成功则会显示消息

```
Server OK
```

### 7 守护进程NFSD的个数

大部分的默认情况下，linux或者其它操作系统都会启动8个NFSD。这个数字是在NFS早期由Sun公司的一个经验法则。后来其它人就沿袭下来了。没有什么好方法来确定究竟多少个NFSD才是最优的，但是更大的负载可能需要更多个NFSD。虽然你应该一个处理器至少有一个NFSD,但是每个处理器8个可能是比较好的经验法则。如果你的内核是2.4以上，并且你想看看每个NFSD 线程的负载情况，你可以看文件/proc/net/rpc/NFSD。文件中th行的最后的10个数说明线程的负载的百分比对于时间的直方图（第一个数代表运行的NFSD 数，第二个数代表所有线程需要的时间，后面的就是那一时刻的所需时间/最大允许时间）。如果后面的几个数加速增加了，那么你的服务端就得要更多的NFSD 线程。

修改NFSD 线程数是在 /etc/rc.d/init.d/nfs 中的RPCNFSDCOUNT值。记得重启服务。

### 8 输入队列的内存限制

在2.2和2.4内核中，默认的socket读缓存rmem\_default是64k，写缓冲wmem\_default是8k。这两个值对有大量读写负载的情况很重要。

许多公开的对NFS的测评多使用大得多的读写缓存\[rw\]mem\_default和\[rw\]mem\_max。可以考虑增加这些值到256k。按下面的方法做比较安全(以读缓冲为例)

*   在文件中增加值

```
\# echo 262144 > /proc/sys/net/core/rmem_default
\# echo 262144 > /proc/sys/net/core/rmem_max
```

*   重启NFS(Redhat环境)

```
\# /etc/rc.d/init.d/nfs restart
```

*   测试结束后恢复正常值

```
\# echo 65536 > /proc/sys/net/core/rmem_default
\# echo 65536 > /proc/sys/net/core/rmem_max
```

### 9 关闭网卡和集线器的自动协商协议

如果网卡和hub与交换机自协商异常并且运行在不同的速度上，或者处在不同配置的双工模式下（全双工、半双工），那么性能会因为过多冲突或丢包等受到严重的影响。如果可能，尽量在100M全双工内网传输，这样可以为NFS over UDP消除大部分的网络。当关闭网卡自适应时，要注意项链的hub或者交换机将重置到其它双工设置，并且一些网卡为了支持老hub默认是半双工的。最好的解决方法是强制网卡为100M全双工。

### 10 NFS的同步和非同步选项

NFSD v2 和 v3协议默认的是非同步的。非同步默认让服务端不等把数据写入本地磁盘就响应客户端请求。在服务端“exportfs -av”出的列表中可以看到async选项。虽然有可能在服务端仍然有未写入的数据时服务器重启会导致数据丢失的代价，但是非同步的性能更好。当发生上述情况时不能被检测到，因为“async”选项指示服务器欺骗客户端，让它以为所有的数据确实都写入磁盘了，而不考虑协议是怎样做的。

当使用同步选项时，NFS v2将等到所有的数据都写入磁盘以后才响应请求；NFS v3则不同，它会返回状态来高数客户端什么数据仍然在缓冲中和什么数据可以安全的丢弃。状态有三种值，定义在include/linux/nfs.h的nfs3\_stable\_how字段：

*   NFS_UNSTABLE 在服务端数据还没写入磁盘，必须在客户端缓冲数据，除非随后客户端提交请求确认服务端已经把数据写入磁盘了
*   NFS\_DATA\_SYNC 数据还没完全写入，必须在客户端缓冲。这时必须返回上面说的请求
*   NFS\_FILE\_SYNC 没有数据需要被缓冲这时不需要返回

除此以外，当服务端使用同步选项，客户端如果打开文件的时候使用O_SYNC选项,客户端都会坚持等把数据写入磁盘以后再响应其它请求。这种情况下，从客户端来看NFS v2 和v3 的性能看起来都是一样的。

如果在服务端设定非同步，那么O_SYNC选项将不起作用，因为服务端不等数据完全写入磁盘就马上响应。v2和v3版本间无差异。

最后，当NFS v3 使用同步选项并且当文件关闭或者fsync()的时候，将强制服务器把所有缓冲中的数据写入磁盘才响应客户端请求。如果使用非同步选项，这个请求没有任何作用。

### 11 和NFS无关的提高服务器性能的方法

通常来说，服务器的性能和磁盘服务的性能也会严重影响NFS的性能。一下几点有必要考虑：

*   如果使用RAID阵列，用RAID 1/0可以保证写速度和冗余；RAID 5有比较好的读速度但是会降低写速度
*   journalling 文件系统会减少重启时间。目前ext3已经可以正确的和NFS v3。另外，Reiserfs v3.6可以在2.4.7或者更高的内核上和NFS v3 一起工作。Reiserfs更早版本会有问题
*   Additionally, journalled file systems can be configured to maximize performance by taking advantage of the fact that journal updates are all that is necessary for data protection. One example is using ext3 with data=journal so that all updates go first to the journal, and later to the main file system. Once the journal has been updated, the NFS server can safely issue the reply to the clients, and the main file system update can occur at the server’s leisure.

The journal in a journalling file system may also reside on a separate device such as a flash memory card so that journal updates normally require no seeking. With only rotational delay imposing a cost, this gives reasonably good synchronous IO performance. Note that ext3 currently supports journal relocation, and ReiserFS will (officially) support it soon. The Reiserfs tool package found at ftp://ftp.namesys.com/pub/reiserfsprogs/reiserfsprogs-3.x.0k.tar.gz contains the reiserfstune tool, which will allow journal relocation. It does, however, require a kernel patch which has not yet been officially released as of January, 2002.

*   用自动挂载（比如autofs或者amd）可以在机器down以后自动挂载。
*   一些厂商（Network Appliance, Hewlett Packard或者其它）用NVRAM提供NFS加速。NVRAM提高访问速度到async的访问速度

### 12 文件属性与目录属性的更新时间

在NFS中如果需要提高实时性，还有一组非常容易忽略的参数

*   acregmin=n：设定最小的在文件更新之前cache时间，默认是3
*   acregmax=n：设定最大的在文件更新之前cache时间，默认是60
*   acdirmin=n：设定最小的在目录更新之前cache时间，默认是30
*   acdirmax=n：设定最大的在目录更新之前cache时间，默认是60
*   actimeo=n：将acregmin、acregmax、acdirmin、acdirmax设定为同一个数值，默认是没有启用。
*   noac: 关闭cache机制。

新建和删除文件属于修改目录属性；增加和修改文件属于文件属性。

在抓包测试中发现，当客户端读文件属性的时候，NFS的工作方式是nfs客户端直接给出cache的文件属性。而服务端最快每隔acregmin更新一次文件属性，每隔acdirmin更新一次目录属性。因此就算在局域网的理想条件下，也常常会有其实文件或目录已经更新,在NFS客户端却无法及时看到更新的假象。