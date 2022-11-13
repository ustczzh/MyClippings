# Windows下快捷方式 (*.lnk) 的使用技巧整理 - SHARP-EYE - 博客园
日常应用中，许多软件都会在安装过程最后一步添加多个命令，针对其应用创建快捷方式发送到桌面以及快速启动栏和开始菜单，供人们快速找到并打开。在我的使用习惯中也会将诸多常用的应用`右键-发送到-桌面快捷方式`来实现快速启动。但是偶然一次的使用中，应用和其对应的快捷方式都移植到了另一台机子上，发现打开快捷方式会有问题。这就引起了我的注意。对地址、路径敏感一点的同行都能想到肯定是传统的快捷方式使用了绝对的地址，而想要移植，那么相对路径是一个好对策。

就像这样对一个.exe右键创建快捷方式：

![](https://img2020.cnblogs.com/blog/944545/202107/944545-20210707132056658-1237473141.png)

我们右键属性，可以看到用到的是**绝对路径**。倘若移植到了不同环境，那么该快捷方式就不能正常运行。如何来修改呢？

正文
--

如果想要直接在该文件属性上修改是不好实现的，反正我没成功。我们尝试采用正常的流程创建快捷方式--- `右键资源管理器中得文件夹区域-新建-快捷方式`,会弹出下方对话框：

![](https://img2020.cnblogs.com/blog/944545/202107/944545-20210707132719883-278872037.png)

如果选择“浏览”，可以选择文件或文件夹，但还是绝对地址。在此要手动输入。

### 使用Explorer方式

键入

```null
%SystemRoot%\explorer.exe

```

然后后面加上打开的文件或文件名，使用相对路径，如：

```null
%SystemRoot%\explorer.exe abc

```

意思是打开该目录下的名为‘abc’的文件夹。`%SystemRoot%`是系统环境变量，详情请见`计算机-高级系统设置-环境变量`。注意创建完快捷方式要`右键-属性`把\[起始位置\]删除，如下：

![](https://img2020.cnblogs.com/blog/944545/202107/944545-20210707133616600-2044610677.png)

这样就可以实现相对路径下打开文件夹，如果想打开文件，如下键入：

```null
%SystemRoot%\explorer.exe .\main\_conf\config.ini

```

```null
%SystemRoot%\explorer.exe .\HxD.exe

```

甚至还可以打开根目录下的上级目录中的一个文件夹，如：

```null
%SystemRoot%/explorer.exe ..\steam

```

此命令代表打开当前目录的上级目录中名为‘steam’的文件夹。  
这样的调用方式，就不怕移植、换环境了。

### 使用cmd.exe方式

同样的道理，创建快捷方式，键入：

```null
%windir%\system32\cmd.exe /c

```

然后后面加标准的dos批处理命令，如运行一个应用\[注意这里的 `/c`一定得加，不然dos界面会跳出等待用户输入，这不是我们要的，让它开一个dos命令后立即关闭dos界面\]：

```null
%windir%\system32\cmd.exe /c .\HxD.exe

```

还是要看一下属性中的\[起始位置\]，把它**清空**。当然这里可以使用`start`启动应用，如：

```null
%windir%\system32\cmd.exe /c start .\HxD.exe

```

如果要打开文件夹：

```null
%windir%\system32\cmd.exe /c start .\abc

```

当然，你还可以以正常的批处理代码键入，如：

```null
%windir%\system32\cmd.exe /c echo hello world>.\hello.txt

```

还可以多指令执行，如：

```null
%windir%\system32\cmd.exe /c echo hello world>.\hello11.txt & start .\hello11.txt

```

这样也可以极大满足环境变化。

总结
--

【注意】所有键入的**斜杠、反斜杠**都是敏感的，不能随意更改，否则无效！！

这两方式殊途同归，给不同环境下快捷方式正常运行做好基础。有了此解决方案后，开发后期也能得到应用，做一些快捷通道，包括执行语句、调用文件等操作、逻辑判断、文件读写、数据通讯等功能，个人认为挺酷的😎 我们下回再见！~ 😘

参考文献：  
\[1\]      [https://www.cnblogs.com/vibratea/archive/2010/09/16/1827761.html](https://www.cnblogs.com/vibratea/archive/2010/09/16/1827761.html) DOS中Start命令详解  
\[2\]      [https://www.zhihu.com/question/20061568](https://www.zhihu.com/question/20061568) Windows 中如何创建一个指向某相对路径的快捷方式 。。。  
\[3\]      [https://blog.csdn.net/ju\_pan/article/details/79454394](https://blog.csdn.net/ju_pan/article/details/79454394) 创建使用相对路径的快捷方式