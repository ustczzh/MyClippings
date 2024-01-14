# 恢复windows server 2019 剪贴板历史记录功能 | WBGlIl
[恢复windows server 2019 剪贴板历史记录功能 | WBGlIl](https://wbglil.github.io/2021/12/22/%E6%81%A2%E5%A4%8Dwindows-server-2022-%E5%89%AA%E8%B4%B4%E6%9D%BF%E5%8E%86%E5%8F%B2%E8%AE%B0%E5%BD%95%E5%8A%9F%E8%83%BD/) 

 发表于 2021-12-22

最近装了个Windows Server 2022玩玩当熟练的按下Win+V的时候发现竟然没办法呼出剪贴板  
就想着能不能恢复一下首先到设置里面查看并没有发现剪贴板选项  
有点尴尬，不过想着Windows Server是基于Windows10应该差别不会太大就跑去Windows10上查看一下Win+V的快捷键注册者  
[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-14%2010-09-56/7aa1a4b2-d3a4-4f3b-9856-a48aa3899b9b.png?raw=true)
](https://s2.loli.net/2021/12/22/Vkjgai2TmHUoqxX.png)

发现是explorer.exe，不出意料，既然是explorer.exe注册的那就基本可以肯定应该是通过注册表什么选项进行控制的总不可能Server还单独一份explorer.exe吧

打开IDA拖入explorer.exe过滤导入表

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-14%2010-09-56/fc9ee4a5-e7a0-4571-8e04-728c07a15f92.png?raw=true)
](https://s2.loli.net/2021/12/22/mI6rPnfKMabUH7k.png)

转到交叉引用

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-14%2010-09-56/5c15db9f-d63d-4e15-baff-e3b6bef44ab5.png?raw=true)
](https://s2.loli.net/2021/12/22/icw2UP98FzH5ToJ.png)

选择全局快捷键注册

在这之中有一个IsCloudAndHistoryFeatureSupportedSKU函数引起了我的注意

双击进去查看IsCloudAndHistoryFeatureSupportedSKU

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-14%2010-09-56/779cc9b8-5443-4790-82c5-430650194747.png?raw=true)
](https://s2.loli.net/2021/12/22/FcNhEIMzdQrCVgW.png)

根据名字感觉就是这个转到注册表查看确实没有那我们自己新建一个

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-14%2010-09-56/46db3ca6-b43f-4336-a206-b76c54fc3e53.png?raw=true)
](https://s2.loli.net/2021/12/22/XFMwWTt2pYg8ozi.png)

关机重启

按下win+v熟悉的剪贴板又回来了

[![](https://github.com/ustczzh/MyClippings/blob/main/Images/2024-1-14%2010-09-56/0a36cc78-3079-4236-a2f7-89c09862aad8.png?raw=true)
](https://s2.loli.net/2021/12/22/ikVZC2O84P3pySI.png)