# 解决Windows11远程桌面提示Windows Defender Credential Guard 不允许使用已保存的凭据的问题-CSDN博客
最新推荐文章于 2023-10-31 14:03:10 发布

![](https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)

[simon-hacker](https://blog.csdn.net/Lynn_Lee_Java "simon-hacker") ![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCurrentTime2.png)
 于 2022-08-01 22:47:46 发布

版权声明：本文为博主原创文章，遵循 [CC 4.0 BY-SA](http://creativecommons.org/licenses/by-sa/4.0/) 版权协议，转载请附上原文出处链接和本声明。

![](https://img-blog.csdnimg.cn/6ae748f1660b490a838086c87cc765e5.png)

[Windows11](https://so.csdn.net/so/search?q=Windows11&spm=1001.2101.3001.7020) 22H2开始Windows开始更新内核保护了。这玩意让我不能使用已经保存的密码来连接远程桌面。我也很绝望啊，我TMD都忘记我的远程桌面的密码了啊！太复杂了。所以我决定关闭他了。翻遍了百度和谷歌都没有讲到这玩意，终于我翻看到微软文档的时候里面写到一份如何开启的。所以也许是目前的设备驱动对新版本系统还不支持，也许是 windows 11 22H2预览版的bug，所以并不是很确定问题原因。

如何关闭 Windows Defender Credential Guard
--------------------------------------

使用组策略禁用 Windows Defender Credential Guard  
在组策略管理控制台上，转到计算机配置 -> 管理模板 -> 系统 -\> Device Guard。

![](https://img-blog.csdnimg.cn/8e73fdc7e600458e9a90883a91e893e1.png)

 双击打开基于[虚拟化](https://so.csdn.net/so/search?q=%E8%99%9A%E6%8B%9F%E5%8C%96&spm=1001.2101.3001.7020)的安全，然后单击禁用选项。

![](https://img-blog.csdnimg.cn/64f0052f799c4383a3373907665b3dc3.png)

确定配置并重启电脑即可解决。