# 【转】关于NFS讨论 参数说明_cuanwuqian6695的博客-CSDN博客
[【转】关于NFS讨论 参数说明_cuanwuqian6695的博客-CSDN博客](https://blog.csdn.net/cuanwuqian6695/article/details/100478716) 

 一． **NFS****介绍**  
    NFS（Network Filesystem）。即网络文件系统，由Sun Microsystem在1985年推出，最初是用作无盘客户机的替代文件系统而出现的，发展至今已成为最流行的一种远程共享文件系统方式。它最大的优势就在于，通过网络，让不同的机器、不同的操作系统，可以彼此分享对方的数据文件信息（sharefile）。从NFS Server共享出来文件“mount”到客户端的文件系统，从客户端自己看来就好像是在使用本地的文件系统一样。

二． **NFS****版本**NFS 开发至今已有多个版本，V2（rfc1094）、V3（rfc1813），最新版本是V4（rfc3010）。最早，Sun公司曾将NFS V2设计为只使用UDP协议，主要原因是当时极其的内存，网络速度和CPU的影响，不得不选择对极其负担较轻的方式。而到V3，Sun公司选择了TCP协议作为缺省传输方式。

三． **V3****和****V2****的主要区别** 虽然现在V4开始逐渐替代V3，但鉴于主要用于生产使用的NFS方式，通过升级操作系统等方式仍是少数（这毕竟需要升级相应的生产软件，而且需要大量的测试时间），因此，V3仍是主要的NFS版本。V3和V2的主要区别：1\. 文件传输尺寸：V2版本最大值支持32Bit文件大小（4G），而NFS V3则支持64Bit文件大小。具体可查看/usr/include/linux 中nfs2.h、nfs3.h文件的NFS2_FHSIZE(32)，NFS3_FHSIZE(64) ；2\. 文件传输方式：V2使用的是可靠性较小的UDP协议，这样在一些高要求的网络环境中有很大限制，而V3是基于TCP协议；  
 3\. V3版本增加和完善了许多错误和成功信息返回，优化了文件传输，增强了灾难恢复功能；  
 4\. NFS V2定义的buffersize大小的限制为8192，V3的限制为32768。可以查看/usr/include/linux 下的nfs2.h和nfs3.h文件中NFS2_MAXDATA和NFS3\_MAX\_FILE\_IO\_BUFFER_SIZE；5\. 异步读写特性；6\. 改进了SERVER的mount性能；7\. 有更好的I/O WRITES 性能；8\. 更好的网络性能；9\. 加强了灾难恢复功能。

四． **V4****的讨论** NFSv3 必须要依赖于几个辅助协议实现对远程计算机上目录的无缝加载，而不会太过依赖于底层的文件系统机制。然而，这会出现一些不必要的复杂，举例来说， Mount 协议调用初始文件句柄，而 Network Lock Manager 协议解决文件锁定，这两个操作都需要状态，而 NFSv3 却并没有提供状态。因此，必须使用的几个扶助协议在没有提供类似的数据流机制的协议层中需要进行复杂的交互。 可以说版本3的NFS（NFSv3）是无状态的，而 NFSv4 是有状态的。我们前面提到过必须调用辅助协议，因为用户层的进程已经被舍弃了。相反，每个文件打开操作以及相当多的 RPC 调用都被转换成了内核层的文件系统操作。V4通过引入一个所谓的 **复合操作** 来简化了这个过程，它包含了很多文件系统对象操作。当然，这样做的直接影响是在网络上传输的 RPC 调用和数据会少很多，不过每个 RPC 调用所携带的数据都要比实际需要的数据要多。根据评估说明，NFSv3 RPC 调用所需要的次数是 NFSv4 复合 RPC 过程所需要的客户机－服务器交互次数的 5 倍。 另外，V4一个进步就是加强了安全认证，对于NFSv3 的安全性存在大量的批评。尽管 NFSv3 服务器是在 TCP 上运行的，但是也完全可以在 Internet 上运行 NFSv3 网络。不幸的是，这必须要打开多个端口，因而会导致几个众所周知的安全性问题。通过让 2049 端口成为 NFS 的强制端口，就可以跨过防火墙来使用 NFSv4，而不用太过注意其他协议（例如 Mount 协议）正在侦听哪些端口。因此，丢弃 Mount 协议具有几方面的正面影响：  
**强制的强认证机制：**  NFSv4 强制强认证机制。Kerberos 非常常见，低层基础设施公钥机制（Lower Infrastructure Public Key Mechanism，LIPKEY）也必须要支持。NFSv3 只支持 UNIX 型的标准加密来对访问进行认证 —— 这在大型网络中会导致一些主要的安全问题。  
**强制的 Microsoft Windows NT 型的访问控制列表（ACL）方案**：尽管 NFSv3 允许对认证进行强加密，但是它并没有推动 Windows NT 型的 ACL 访问方案。可移植操作系统接口（POSIX）风格的 ACL 一段时间也曾实现过，但是一直都没有得到广泛采用。NFSv4 强制采用了 Windows NT 型的 ACL 方案。  
**协商的认证类型和机制**： NFSv4 使对认证类型和机制进行协商成为可能。在 NSFv3 中，只能手工确定采用哪种加密类型。系统管理员然后必须对加密和安全协议进行协调。

五． **RPC****（Remote Procedure Call远程过程调用）**  
  NFS服务本身并没有远程访问的功能，需要其他的协议来辅助它完成这些工作。这就需要调用RPC的几个daemon：  
  rpc.nfsd：接受从远程系统发来的NFS请求并将这些请求转化为本地文件系统请求；  
  rpc.mountd：执行被请求的文件系统的挂在和卸载操作；  
  rpc.portmapper：将远程请求映射到正确的NFS守护进程，当RPC服务启动后，需要在portmap daemon 中注册，它会告诉daemon正在监听的端口号以及他提供的RPC程序号。这样portmap daemon就会知道每一个主机上已经登记的端口位置以及上边运行的程序。当客户端想根据一个给定的程序号进行RPC调用的时候，首先它会联系服务器的portmap daemon，来决定RPC包需要从那个端口上发送。因此在任何RPC服务启动之前一定首先要激活portmap daemon。

六． **配置****NFS****服务器** 首先要在NFS服务器中将需要mount的文件系统export出去，客户端然后才能mount该文件系统。对于RH linux、IBM Aix、Sun Solaris，虽然配置文件略有不同，但原理都是一样。1  a) 对于solaris操作系统，要配置/etc/dfs 下的三个文件：  
     dfstab：存放了共享NFS文件系统的命令，用来在系统启动时读取执行； 格式：share \[-F fstype\] \[ -o options\] \[-d ""\] name> \[resource\]  
         例如：share  -F nfs  -o rw=engineering  -d "home dirs"  /export/home2  
               share -F nfs /home  
               share -F nfs /soft  
     fstypes：记录文件系统类型，缺省为NFS；  
     sharetab：所共享的文件系统的系统记录； 例如：/home   -       nfs     rw  
              /soft   -       nfs     rw  
    b)  AIX操作系统的nfs共享管理文件是/etc/exports 例如：/home  -rw  
    c)  RH linux 的nfs文件管理 /etc/exports 格式：/nfs\_file  cli\_host(参数，参数) 例如：/home1  host1(rw,no\_root\_squash)    host2(rw)  
              /home2  *(rw)          
       \[注意\]：  cli_host指客户端hostname，共有四种表示方式：

| 

类型

 | 

语法

 | 

含义

 |
| 

主机名

 | 

hostname

 | 

单个主机

 |
| 

网络组

 | 

@groupname

 | 

NIS网络组

 |
| 

通配符

 | 

*和?

 | 

通过通配符来表示cli_host，*不匹配点号.

 |
| 

IP网络

 | 

ipaddr/mask

 | 

通过IP来表示主机

 |

  2  NFS文件系统参数

| 

选 项

 | 

说 明

 |
| 

ro

 | 

以只读方式导出

 |
| 

rw

 | 

以读写方式导出（默认方式）

 |
| 

rw=list

 | 

除list中为读写外，其他库户端均为只读

 |
| 

root_squash

 | 

将root的UID和GID映射（“压制”）成anonuid和anongid所指定的值

 |
| 

no\_root\_squash

 | 

运行root正常访问（这很危险）

 |
| 

all_squash

 | 

将所有的UID和GID映射到他们各自的匿名版本上，用于支持不可信的单用户

 |
| 

anonuid=xxx

 | 

指定远程root帐号应被映射的UID号

 |
| 

anongid=xxx

 | 

指定远程root帐号应被映射的GID号

 |
| 

sSecure

 | 

远程访问必须从授权端口发起

 |
| 

insecure

 | 

运行从任何端口远程访问

 |
| 

noaccess

 | 

防止访问这个目录及其子目录（用于嵌套导出）

 |
| 

sync

 | 

将数据同步写入内存和硬盘中

 |
| 

async

 | 

将数据线暂存在内存中，而非直接写入硬盘

 |

3 共享命令 需要通过命令引导配置文件中文件系统及参数来管理向外输出的nfs文件系统，对于solaris来说，在文件 /etc/dfs/dfstab 中记录的share \[-F fstype\] \[ -o options\] \[-d ""\] name> \[resource\] 就是输出的命令；而对于aix和linux，命令为exportfs：\* 不加参数：直接使用exportfs时，可以显示已输出的文件系统  
    *  -a：根据/etc/exports文件输出所有目录（文件系统）  
    *  -v：详细列出所输出的目录及输出参数  
    *  -u：停止输出所指出的目录，当加上a后，会根据/etc/exports文件停止输出所有目录  
    *  -o：输出目录时（文件系统），命令加入的参数，这里的参数和/etc/exports文件中是一样的  
    *  -i：忽略/etc/exports文件中的参数，只以系统缺省的和命令行中指定的参数为基准（并没有输出动作）  
    *  -r：重新输入所有目录4 补充  
    \* showmount：显示所有mount本服务器的客户端名称  
    \* AIX和LINUX中的/etc/xtab记录了当前输出的nfs目录

七． **客户端设置**1 mount  \[-t vfstype\] \[-o options\] device dir  
  -t 为文件系统类型-o 为nfs挂在参数，这些参数也可以配置/etc/fstab文件，然后只需用 mount dir，会自动查找fstab中参数。下表为主要的option：

| 

标  志

 | 

说 明

 |
| 

rw

 | 

以读写方式安装文件系统（注意server输出也必须放开）

 |
| 

ro

 | 

以只读方式安装文件系统

 |
| 

hard

 | 

如果服务器down机，让试图访问它的操作被阻塞，直到服务器恢复为止

 |
| 

soft

 | 

如果服务器down机，让试图访问它的操作失败，返回一条出错消息。  
（这项功能对于避免进程“挂”在无关紧要的操作来说非常有用）

 |
| 

intr

 | 

允许用户中断被阻塞的操作（并让他们返回一条出错消息）

 |
| 

nointr

 | 

不允许用户中断

 |
| 

retrans=n

 | 

指定在以软方式安装的文件系统上，在返回一条出错消息之前重复发出请求的次数

 |
| 

timeo=n

 | 

设置请求的超时时间（以千分之一秒为单位）

 |
| 

rsize=n

 | 

设置读缓冲的大小为n字节

 |
| 

wsize=n

 | 

设置写缓冲的大小为n字节

 |
| 

nfsvers=

 | 

设置NFS协议的版本2或3

 |
| 

tcp

 | 

选择通过TCP来传输，默认UDP

 |
| 

bg

 | 

如果mount失败（服务器没有响应），在后台一直尝试（缺省为fg）

 |
| 

fg

 | 

如果mount失败，则在前台一直尝试

 |

2 在启动时挂载远程文件系统  
配置/etc/fstab，方法如下  
/etc/fstab的格式如下：  
fs_specfs_filefs_typefs_optionsfs_dumpfs_pass  
fs_spec:该字段定义希望加载的文件系统所在的设备或远程文件系统,对于nfs这个参数一般设置为这样：192.168.0.1:/NFS  
fs_file:本地的挂载点  
fs_type：对于NFS来说这个字段只要设置成nfs就可以了  
fs_options:挂载的参数，可以使用的参数可以参考上面的mount参数。  
fs_dump-　该选项被"dump"命令使用来检查一个文件系统应该以多快频率进行转储，若不需要转储就设置该字段为0  
fs_pass-　该字段被fsck命令用来决定在启动时需要被扫描的文件系统的顺序，根文件系统"/"对应该字段的值应该为1，其他文件系统应该为2。若该文件系统无需在启动时扫描则设置该字段为0 。

3 mount 其它常用命令：mount –a 挂载fstab中所有文件系统（包括本地）mount –a –t nfs 只挂载fstab中远程文件系统

八． **NFS****性能优化**  
    WSIZE、RSIZE对于NFS的效能有很大的影响。通过WSIZE,RSIZE参数来优化NFS的执行效能。  
    wsize和rsize设定了SERVER和CLIENT之间往来数据块的大小，这两个参数的合理设定与很多方面有关，不仅是软件方面也有硬件方面的因素会影响这两个参数的设定（例如LINUX KERNEL、网卡，交换机等等）。 下面这个命令可以测试NFS的执行效能，读和写的效能可以分别测试，分别找到合适的参数。对于要测试分散的大量的数据的读写可以通过编写脚本来进行测试。在每次测试的时候最好能重复的执行一次MOUNT和unmount。  
time dd if=/dev/zero f=/mnt/home/testfile bs=16k count=16384  
用于测试的WSIZE,RSIZE最好是1024的倍数，对于NFS V2来说8192是RSIZE和WSIZE的最大数值，如果使用的是NFS V3则可以尝试的最大数值是32768。  
如果设置的值比较大的时候，应该最好在CLIENT上进入mount上的目录中，进行一些常规操作（LS,VI等等），看看有没有错误信息出现。有可能出现的典型问题有LS的时候文件不能完整的列出或者是出现错误信息，不同的操作系统有不同的最佳数值，所以对于不同的操作系统都要进行测试。

**九．** **NFS****安全**

NFS的不安全性主要体现于以下4个方面:

1、新手对NFS的访问控制机制难于做到得心应手,控制目标的精确性难以实现

2、NFS没有真正的用户验证机制,而只有对RPC/Mount请求的过程验证机制

3、较早的NFS可以使未授权用户获得有效的文件句柄

4、在RPC远程调用中,一个SUID的程序就具有超级用户权限.

加强NFS安全的方法：

1、合理的设定/etc/exports中共享出去的目录，最好能使用anonuid，anongid以使MOUNT到NFS SERVER的CLIENT仅仅有最小的权限，最好不要使用root_squash。

2、使用IPTABLE防火墙限制能够连接到NFS SERVER的机器范围

iptables -A INPUT -i eth0 -p TCP -s 192.168.0.0/24 --dport 111 -j ACCEPT

iptables -A INPUT -i eth0 -p UDP -s 192.168.0.0/24 --dport 111 -j ACCEPT

iptables -A INPUT -i eth0 -p TCP -s 140.0.0.0/8 --dport 111 -j ACCEPT

iptables -A INPUT -i eth0 -p UDP -s 140.0.0.0/8 --dport 111 -j ACCEPT

3、为了防止可能的Dos攻击，需要合理设定NFSD 的COPY数目。

4、修改/etc/hosts.allow和/etc/hosts.deny达到限制CLIENT的目的

/etc/hosts.allow

portmap: 192.168.0.0/255.255.255.0 : allow

portmap: 140.116.44.125 : allow

/etc/hosts.deny

portmap: ALL : deny

5、改变默认的NFS 端口

NFS默认使用的是111端口，但同时你也可以使用port参数来改变这个端口，这样就可以在一定程度上增强安全性。

6、使用Kerberos V5作为登陆验证系统

转自：[http://blog.163.com/yinlong_bgp/blog/static/2173327720077923515336/](http://blog.163.com/yinlong_bgp/blog/static/2173327720077923515336/)

来自 “ ITPUB博客 ” ，链接：http://blog.itpub.net/14184018/viewspace-713457/，如需转载，请注明出处，否则将追究法律责任。