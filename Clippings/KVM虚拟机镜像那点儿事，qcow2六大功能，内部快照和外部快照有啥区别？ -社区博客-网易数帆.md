# KVM虚拟机镜像那点儿事，qcow2六大功能，内部快照和外部快照有啥区别？ -社区博客-网易数帆
[KVM虚拟机镜像那点儿事，qcow2六大功能，内部快照和外部快照有啥区别？ -社区博客-网易数帆](https://sq.sf.163.com/blog/article/218146701477384192) 

 此文已由作者刘超授权网易云社区发布。

欢迎访问[网易云社区](https://sq.163yun.com/blog)，了解更多网易技术产品运营经验。

KVM虚拟机镜像对于物理机的操作系统来讲是一个文件，这个文件最主要的两种格式，一种是raw，也即原始格式，还有一种是qcow2，顾名思义cow是copy on write。

一、RAW Image

raw格式简单，性能较好

然而本身不支持稀疏格式，需要文件系统的支持才能支持稀疏文件，所以最好基于ext3文件系统。

创建image

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/71c9071c-0700-47dd-9926-10fcd1d43359.png?raw=true)
  

用dd产生image

产生非稀疏文件nonsparse file

dd if=/dev/zero of=flat1.img bs=1024k count=1000

在这里blocksize是1024k，共1000个block

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/125ea65f-6678-4413-9f40-c54ca9b13216.png?raw=true)
  

也可以产生稀疏文件sparse file

dd if=/dev/zeroof=flat2.img bs=1024k count=0 seek=2048

seek的意思是将文件的结尾设在那个地方

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/6a28e02e-2c7e-4493-8960-cd91805f2019.png?raw=true)
  

copy一个稀疏文件的时候，可能会将稀疏文件没有数据的部分压缩掉

dd if=/dev/zero of=flat3.img bs=1024k count=1 seek=2048

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/79fafea8-5a66-4f3d-9a3b-126782455daa.png?raw=true)
  

cp --sparse=never flat3.img flat3-never.img

这样拷贝会占用整个2G空间

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/d1121bbb-cc45-46cb-9584-8e1803ff81d7.png?raw=true)
  

cp --sparse=always flat3.img flat3-always.img

这样拷贝原来写入的1M的0也会被去掉

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/a3f5ed8b-601a-466e-8afa-69173d4b745e.png?raw=true)
  

cp --sparse=always flat3-never.img flat3-never-always.img

这样拷贝原来的2G都会被清理掉

**![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/6bb62ee7-be47-43de-846b-421cd3090eff.png?raw=true)**  

**二、qcow2是动态的**

即便文件系统不支持sparse file，文件大小也很小

qcow2功能一：copy on write

qcow2的格式如下

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/39ab85da-fe32-41fb-8437-31329e19b711.png?raw=true)
  

它实行的是2-Level loopup

qcow2的数据是存储在data clusters里面的，每个cluster是512 byte sector

为了能够管理这些cluster，qcow2保存了两层的Table，L1 table指向L2 Table，L2 Table管理data cluster.

在image里面的offset会被解析成三部分，L1 Table Pointer先找L1，L1 Table Pointer+ offset\[0\]是L1中的一个entry，读出来便是L2 Table Pointer, L2 Table Pointer + offset\[1\]是L2中的一个entry，读出来便是data cluster pointer, data cluster pointer +offset\[3\]便是数据所在的位置。

backing file就是基于这个原理的用处，一个qcow2的image可以保存另一个disk image的改变，而不影响另一个image

创建backing file

qemu-img create -f qcow2 -o backing_file=./ubuntutest.qcow2 ubuntutest1.qcow2

一开始新的image是空的，读取的内容都从老的image里面读取。

当一个data cluster被写入，发生改变的时候，在新的image里面创建一个新的data cluster，这就是copy on write的意义。 

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/560a2782-bf19-4f84-9b31-39f3f234bfc7.png?raw=true)
  

基于ubuntutest1.img创建一个虚拟机

qemu-system-x86_64 -enable-kvm -name ubuntutest  -m 2048 -hda ubuntutest1.qcow2 -vnc :19 -net nic,model=virtio -net tap,ifname=tap0,script=no,downscript=no -monitor stdio

在虚拟机里面创建一个1G的non sparse file

dd if=/dev/zero of=flat1.img bs=1024k count=1000

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/cb2e74df-f9fd-43e2-908d-c6662dcf262a.png?raw=true)
  

qcow2功能二：转换

 image格式之间可以转换

raw可以转换为qcow2

创建一个raw image: dd if=/dev/zero of=flat.img bs=1024k count=1000

进行转换： qemu-img convert -f raw -O qcow2 flat.img flat.qcow2

qcow2也可以转换为qcow2，转换的过程中，没用的data cluster就被去掉

qemu-img convert -f qcow2 -O qcow2 ubuntutest.img ubuntutest-convert.img

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/b65225fb-e675-4910-9d82-6c255537c6f4.png?raw=true)
  

qcow2功能三：压缩

qemu-img convert -c -f qcow2 -O qcow2 ubuntutest.img ubuntutest-compress.qcow2

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/dbd4c1f4-b3c1-4d09-9091-95170e36e48e.png?raw=true)
  

qcow2功能四：加密

qemu-img convert -o encryption -f qcow2 -O qcow2 ubuntutest.qcow2 ubuntutest-encrypt.qcow2

会要求设置密码

从这个加密image启动一个虚拟机

qemu-system-x86_64 -enable-kvm -name ubuntutest  -m 2048 -hda ubuntutest-encrypt.qcow2 -vnc :19 -net nic,model=virtio -net tap,ifname=tap0,script=no,downscript=no -monitor stdio

一开始虚拟机并不启动

在monitor中输入cont

需要输入密码后虚拟机才启动

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/bd5f6b0a-e744-4c41-98aa-32830608246e.png?raw=true)
  

qcow2功能五：扩展

cp ubuntutest.qcow2 ubuntutest-enlarge.qcow2

qemu-img resize ubuntutest-enlarge.qcow2 +10G

扩大的空间既不会被partition，也不会被format

启动虚拟机

qemu-system-x86_64 -enable-kvm -name ubuntutest  -m 2048 -hda ubuntutest-enlarge.qcow2 -vnc :19 -net nic,model=virtio -net tap,ifname=tap0,script=no,downscript=no -monitor stdio

Disk已经被扩大

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/0f4e982e-422a-489a-ac9c-3687fdd1e6ea.png?raw=true)
  

删除/dev/sda2和/dev/sda5

fdisk /dev/sda

删除分区五：d 5

删除分区二：d 2

写入修改：w

reboot

扩展/dev/sda1分区到整块硬盘

fdisk /dev/sda

删除根分区：d 1

在原来根分区的位置创建新分区：n

是一个主分区：p

其他都选默认，最后写入修改：w

partprobe

扩展文件系统

resize2fs /dev/sda1

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/9ef9ff7c-52d7-400b-9cb5-c846a46424a2.png?raw=true)
  

qcow2功能六：快照

snapshot也是copy on write的一种应用，和backing file有微妙的不同

有两种snapshot，一种是internal snapshot，一种是external snapshot

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/ca1c5001-fcfc-4248-8d3e-a43c341e785a.png?raw=true)
  

internal snapshot是qcow2中的snapshot table所维护的snapshot，所有的snapshot都是在同一个文件中维护

创建一个虚拟机

qemu-system-x86_64 -enable-kvm -name ubuntutest  -m 2048 -hda ubuntutest.qcow2 -vnc :19 -net nic,model=virtio -net tap,ifname=tap0,script=no,downscript=no -monitor stdio

在monitor中执行savevm后，ubuntutest.img会在image内部打一个snapshot

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/2d390a4d-0ff2-42e3-b3b0-3e1fda538e90.png?raw=true)
  

原理是，当打一个snapshot后，会在snapshot table中建立一项，但是起初是空的，包含L1 table的一个复制，当L2 table或者data cluster改变的时候，则会将原来的数据复制一份，由snapshot的L1 table来维护，而原来的data cluster已经改变，在原地。

external snapshot则往往采用上面的copy on write的方法

当打snapshot的时候，将当前的image不再改变，创建一个新的image，以原来的image作为backing file，然后虚拟机使用新的image

在monitor中，snapshot_blkdev ide0-hd0 ubuntutest-snapshot.img qcow2

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/b0fc13cd-679b-4f49-9d27-522ff6c6e9aa.png?raw=true)
  

在HOST机器上多了一个文件

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-12-12%2013-10-26/7b881c03-0c41-4147-8718-e8076d4f0d62.png?raw=true)
  

backing file可以是raw，也可以是qcow2，但是一旦打了snapshot，新的格式就是qcow2了。

两者很相似，稍微的不同是：

对于internal snapshot, 刚打完snapshot的时候，原image集合是不变的，snapshot的集合是空的，接下来的操作，写入在原image，将不变的加入snapshot集合

对于external snapshot，刚打完snapshot的时候，原image变成snapshot, snapshot集合是全集，新image是空的，接下来的操作，写入在新image，将改变的加入新image的集合。

和打快照有关的命令有：

qemu-img snapshot –c

if the domain is offline and –disk-only was not specified

savevm

if the domain is online and –disk-only was not specified

snapshot_blkdev

if the domain is online and –disk-only is specified

[免费体验云安全(易盾)内容安全、验证码等服务](https://www.163yun.com/free?tag=M_sq_tyg)

更多网易技术、产品、运营经验分享请[点击](https://sq.163yun.com/blog)。