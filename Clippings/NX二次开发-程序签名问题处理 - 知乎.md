# NX二次开发-程序签名问题处理 - 知乎
作者：王键鑫 审校：叶齐天

适用版本：NX5.0及以上

一、概述

从NX5.0开始，在进行NX二次开发时，完成程序开发的最后一个步骤，便是给开发好的程序签名。从NX9.0及以上的版本便不再支持32位，但目前仍有部分客户使用的是NX8.5及以下的版本，此时进行程序签名时便要注意，开发程序针对32位、64位还是AnyCPU，还需要注意是使用什么编程语言开发程序。

二、功能说明

NX二次开发可以使用多种编程语言，例如：C++、.Net （C#/VB）、JAVA、Python等。而不同编译语言对应的程序签名过程也有所不同。

当使用C++开发时，需要在项目中加入“NXSigningResource.cpp”，编译成DLL文件以后，使用“signcpp.exe”（8.5之前使用nxsign.exe）对DLL文件进行签名。

![](https://pic4.zhimg.com/v2-7ccefc17abecc103a102de1a1aacf747_b.jpg)

图1 C++程序签名

当使用.Net开发时，需要在项目中加入“NXSigningResource.res”，编译成DLL文件以后，使用“signDotNet.exe”（8.5之前使用signLibrary.exe）对DLL文件进行签名。

![](https://pic2.zhimg.com/v2-739e37c48aab64d60ebf901193feed35_b.jpg)

图2 .Net程序签名

当使用JAVA开发时，则无需在项目中添加文件，直接编译好的JAR文件，使用“SignJar.jar”即可。

![](https://pic3.zhimg.com/v2-921a49e4c9d940fe8ed788da96686f0e_b.jpg)

图3 JAVA程序签名

其次，在签名时要注意开发程序针对32位CPU还是64位CPU还是AnyCPU。32位开发程序使用32位的NX签名程序进行签名；AnyCPU与64位开发程序使用64位的NX签名程序进行签名

还需要注意的是，若在64位的系统中，安装了32位NX时，签名时要先运行"run\_managed.exe"，再运行签名程序。

![](https://pic4.zhimg.com/v2-e5e7168357678c2cc614ed97d0bf2447_b.jpg)

图4 特殊环境签名

三、总结

程序签名是NX二次开发不可缺少的工作，只有正确签名过的程序，才能在NX环境下运行正常。