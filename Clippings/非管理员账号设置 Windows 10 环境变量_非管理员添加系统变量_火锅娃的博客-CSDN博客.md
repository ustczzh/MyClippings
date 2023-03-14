# 非管理员账号设置 Windows 10 环境变量_非管理员添加系统变量_火锅娃的博客-CSDN博客
[(2条消息) 非管理员账号设置 Windows 10 环境变量_非管理员添加系统变量_火锅娃的博客-CSDN博客](https://blog.csdn.net/weixin_42005898/article/details/115531523) 

 *   [1\. 运行命令](https://www.dazhuanlan.com/2019/11/02/5dbc8e7dbd70d/#1-%E8%BF%90%E8%A1%8C%E5%91%BD%E4%BB%A4)
*   [2\. 在用户账号界面配置](https://www.dazhuanlan.com/2019/11/02/5dbc8e7dbd70d/#2-%E5%9C%A8%E7%94%A8%E6%88%B7%E8%B4%A6%E5%8F%B7%E7%95%8C%E9%9D%A2%E9%85%8D%E7%BD%AE)
*   [3\. 使用 SETX 命令](https://www.dazhuanlan.com/2019/11/02/5dbc8e7dbd70d/#3-%E4%BD%BF%E7%94%A8-setx-%E5%91%BD%E4%BB%A4)

这里主要说的是**非管理员账号**，如何设置自己的环境变量。

在公司使用的电脑是没有管理员权限的，而作为一个开发人员难免有时候需要手动配置环境变量。经过实践大概有以下几种方式可行：

### 1\. 运行命令

这种方式忘记以前是在哪看到的。即按 Win + R，打开运行，输入 `rundll32.exe sysdm.cpl,EditEnvironmentVariables`，回车。便会弹出配置用户环境变量的界面。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-14%2023-16-25/a24e658b-3957-4df1-a36f-5a8683e2b6d4.jpeg?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-14%2023-16-25/7c088eb7-fc94-4d80-9673-1c8d13865e77.jpeg?raw=true)

### 2\. 在用户账号界面配置

按 Win + R 打开运行，输入 `control`，回车。打开控制面板，进入“用户帐户”。

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-14%2023-16-25/1c36c7b1-8879-4142-9ec2-c769ce3125c4.jpeg?raw=true)

可以看到，这里修改环境变量是不需要管理员权限的

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-14%2023-16-25/73205e7f-717e-40a3-a717-690089204004.jpeg?raw=true)

### 3\. 使用 SETX 命令

SETX 不同于 SET 命令，创建或修改的变量将在以后的命令窗口可用，是永久有效的。详细用法可以使用命令 `setx /?` 查看帮助信息。默认情况下写入的是用户变量，要想写系统环境变量，加参数 /M。而我们只能设置用户变量，就使用最简单的格式 `SETX [变量名] [变量值]` 即可，变量值最好用英文双引号引住。例如：

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-14%2023-16-25/78e89751-18e7-436d-8768-c418ec759e7b.jpeg?raw=true)

![](https://github.com/ustczzh/MyClippings/blob/main/Images/2023-3-14%2023-16-25/8fab53aa-1aba-416f-a090-cd284e323fc9.jpeg?raw=true)