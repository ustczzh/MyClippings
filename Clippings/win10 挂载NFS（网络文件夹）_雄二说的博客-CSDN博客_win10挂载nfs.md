# win10 挂载NFS（网络文件夹）_雄二说的博客-CSDN博客_win10挂载nfs
[win10 挂载NFS（网络文件夹）_雄二说的博客-CSDN博客_win10挂载nfs](https://blog.csdn.net/qq_34158598/article/details/81976063) 

 * * *

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-4%2013-48-10/ead6afce-ea2d-42a4-bf07-c7c452c60557.png?raw=true)
  
打开控制面板 > 程序 \> 启用或关闭 Windows 功能，找到NFS服务打开子目录勾选NFS客户端与管理工具。  
NFS客户端：通过界面操作挂在NFS  
管理工具：通过命令行挂在NFS

* * *

```
showmount -e 远程电脑的IP

```

完整用法

```
showmount -e [server]    显示 NFS 服务器导出的所有共享。
showmount -a [server]    列出客户端主机名或 IP 地址，以及使用“主机:目录”格式显示的安装目录。
showmount -d [server]    显示 NFS 服务器上当前由某些 NFS 客户端安装的目录。

```

* * *

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-4%2013-48-10/823929cd-5e60-4d8f-88ac-53b5ffaa7f59.png?raw=true)
  
打开我的电脑点击此电脑 \> 映射网络驱动器  
![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-4%2013-48-10/83ee02c2-c85f-4c69-b7f6-2e4dd692d296.png?raw=true)
  
如果连接成功你会发现在此电脑多了一个网络盘符  
![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-4%2013-48-10/4d6f1133-e6ec-49c9-a930-1f90766d1cd5.png?raw=true)
  
下来就可以打开查看文件了

需要**读写权限**的需要修改注册表  
通过修改注册表将windows访问NFS时的UID和GID改成0即可，步骤如下  
1、在运行中输入regedit，打开注册表编辑器；  
2、进入HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\ClientForNFS\\CurrentVersion\\Default条目；  
3、选择新建----QWORD值，新建AnonymousUid，AnonymousGid两个值，值为0；  
4、`重启电脑` 注册表才会生效；  
![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-4%2013-48-10/329e8736-03a4-4e85-8e7c-fa1dd13e932a.png?raw=true)
  
右键查看属性发现读写权限跟隐藏文件都打开了  
![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-2-4%2013-48-10/e3ec6153-8323-4527-93af-571ac417bcd4.png?raw=true)

**不要使用资源管理器的“断开网络驱动器”**

```
umount 盘符

```

例如：umount V:  
如果要卸载全部的NFS挂载：

```
umount -f -a

```