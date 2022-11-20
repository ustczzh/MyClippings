# WindowsServer2019AD域，禁止域用户使用本地账户登录_51CTO博客_域控禁止本地用户登录
精选 原创

[老君眉](https://blog.51cto.com/aurogon) 2021-08-25 15:57:12 ©著作权

**_文章标签_ [AD域](https://blog.51cto.com/topic/adyu.html) [活动目录](https://blog.51cto.com/topic/active-directory.html) [Windowsserver2019](https://blog.51cto.com/topic/windowsserver2019.html) [域用户](https://blog.51cto.com/topic/yuyonghu.html) [禁止域用户本地登录](https://blog.51cto.com/topic/jinzhiyuyonghubendidenglu.html)** **_文章分类_ [Windows](https://blog.51cto.com/nav/windows) [系统/运维](https://blog.51cto.com/nav/ops)** **_阅读数_**1万****

©著作权归作者所有：来自51CTO博客作者老君眉的原创作品，如需转载，请与作者联系，否则将追究法律责任

1、禁止使用用户使用本地账户登录禁止的是设置计算机权限，而不是账号权限。

*   打开Active Directory用户和计算机
*   ![](https://s2.51cto.com/images/blog/202108/25/d948b6bae590561f0937da21a650b424.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)
    
*   找到computers，computers下面都是已经加入到域的计算机
*   ![](https://s2.51cto.com/images/blog/202108/25/6f421ff8225030c1a430711205604023.jpg?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)
    
*   将需要禁止本地账户登录的计算机加入到已创建好的组织单位(鼠标拖拽计算机到已创建好的组织单位)
*   ![](https://s2.51cto.com/images/blog/202108/25/4dab192cff913247ba1734c8ff9446fb.jpg?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)
    

2、打开组策略管理

*   ![](https://s2.51cto.com/images/blog/202108/25/83b524ac2bc9f051575e391e887312b2.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)
    
*   选择林--域--组织单位
*   ![](https://s2.51cto.com/images/blog/202108/25/3afeb7a30bf6c211f14b4f37c4da6291.jpg?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)
    
*   在组织单位上右键选择在这个域中创建GPO并在此处链接
*   ![](https://s2.51cto.com/images/blog/202108/25/bc2282920b4ded32b6718a4f65f5b68d.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)
    
*   取个名称
*   ![](https://s2.51cto.com/images/blog/202108/25/f76b7a66a6fa40beb6bbeee30a44845d.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)
    
*   右键点击刚才创建的GPO，编辑
*   ![](https://s2.51cto.com/images/blog/202108/25/71f82adb8dc8c05aecb04219677694a1.jpg?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)
    
*   在打开的组策略管理编辑器里依次打开计算机配置--策略--Windows设置--安全设置--本地策略--用户权限分配--拒绝本地登录
*   ![](https://s2.51cto.com/images/blog/202108/25/311f03e2a80c7440f74072e435c894b3.jpg?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)
    
*   在拒绝本地登录中依次添加本地管理员账户，本地账户组，本地账户和管理组成员组
*   ![](https://s2.51cto.com/images/blog/202108/25/43d7b777751cb3aa6b5df7602d698ac7.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)
    

3、禁止域用户使用本地账户登录设置完成，在域用户计算机使用本地账户登录会提示不允许使用你正在尝试的登录方式

![](https://s2.51cto.com/images/blog/202108/25/e179c1cf70107b52803e2611949506a7.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)

*   **打赏**
*   ****1**赞**
*   ****2**收藏**
*   ****3**评论**
*   **举报**

上一篇：[wps2019专业版序列号](https://blog.51cto.com/aurogon/2483139)

 [![](https://ucenter.51cto.com/images/noavatar_middle.gif)](https://blog.51cto.com/) 

提问和评论都可以，用心的回复会被更多人看到 **评论**

**发布评论**

**全部评论** (**3**) 最热 最新

[![](https://s2.51cto.com/images/100/ucenter/noavatar_small.gif?x-oss-process=image/format,webp/ignore-error,1)
](https://blog.51cto.com/u_14916609)

### [qq5f4506321c06c](https://blog.51cto.com/u_14916609) 6 月前

添加本地账户的时候，提示”“找不到名为”本地账户“的对象

回复 点赞

![](https://s2.51cto.com/images/100/ucenter/noavatar_small.gif?x-oss-process=image/format,webp/ignore-error,1)

### wg\_cFGjEFBB _回复了_ **qq5f4506321c06c** 5 月前

对象类型勾选“计算机”，再按下检查名称即可

回复 点赞

![](https://s2.51cto.com/oss/202109/17/b424866757c182f294a46207b94baa47.jpeg?x-oss-process=image/format,webp/ignore-error,1)

### 一剑断天涯 _回复了_ **qq5f4506321c06c** 6 月前

对啊，你是怎么解决的，我也找不到

回复 点赞

**相关文章**

*   [
    
    域账户登录本机无法创建本地配置文件
    
    今天自己遇到一个问题，使用我的域账户登录本机无法创建本地配置文件，每次用域账户桌面都还原1、环境域名it.com       域 账户sj        计算机名s-pc2、解决方法 在注册表中找到域账户相应的值并删除 3、原因，
    
    ](https://blog.51cto.com/u_136840/971714)
    
    注册表 用户配置文件
    
*   [
    
    设置ssh证书登录，禁止root登录，禁止su到root，sudo权限设置
    
    一、设置ssh证书登录，禁止root登录useraddnewuserpasswdnewuservim/etc/ssh/sshd\_configPermitRootLoginnoRSAAuthenticationyesPubkeyAuthenticationyesPasswordAuthenticationnoClientAliveInterval30ClientAliveCountMax5二、禁止s
    
    ](https://blog.51cto.com/87453343/2457825)
    
    ssh 权限 sudo root
    
*   [
    
    禁止别人PING本机IP
    
       我的电脑-控制面板-管理工具-本地安全策略-ip安全策略这是给我们的配置ip管理的工具，我这里只说一下如何禁止别人ping我的主机。共有四个步骤：1。建立禁ping 规则2。建立禁止/允许规则3。把这两个规则联系在一起4。指派详细：1。右击ip安全策略-管理ip筛选器表和筛选器操作-ip筛选器列表-添加：名称：ping;描述：pi
    
    ](https://blog.51cto.com/zengyixiang/580934)
    
    职场 休闲 PING IP 禁止PING IP 禁止别人PING自己的IP
    
*   [
    
    安全策略禁止本机管理员登录解决办法
    
    在Windows 2003环境下，被组策略拒绝本地登录一直是件比较令人头疼的事情。本文将介绍所有用户都被拒绝本地登录后的解决方法。在Windows2003中，如果某个用户被取消了本地登录权限，当这个用户本地登录计算机时，系统就会提示"此系统的本地策略不允许您采用交互式登录"，导致登录失败。遇到这种情况，通常请管理员在组策略中重新设置一下，将该用户从"拒绝本地登
    
    ](https://blog.51cto.com/u_18266/530136)
    
    职场 组策略 Windows Server 休闲
    
*   [
    
    账户和权限
    
    用户账号和组账号概述 Linux基于用户身份对资源访问进行控制用户账号   超级用户    root（为所欲为）拥有系统的最高权限ID=0   普通用户    普通用户账号需要由root用户或其他管理员用户创建，拥有的权限受到一定限制，一般只在用户自己的宿主目录中拥有完整权限   系统用户   &nbs
    
    ](https://blog.51cto.com/u_15437535/4837924)
    
    用户账号 字段 用户名
    
*   [
    
    mysql全局权限账户%登录不上ERROR 1045 (28000)
    
    mysql全局权限账户%登录不上ERROR 1045 (28000):
    
    ](https://blog.51cto.com/xiaocao13140/2120137)
    
    mysql全局权限账户%登录不上ERRO
    
*   [
    
    31Linux - 用户/权限管理（退出登录账户： exit）
    
    如果是图形界面，退出当前终端；如果是使用ssh远程登录，退出登陆账户；如果是切换后的登陆用户，退出则返回上一个登陆账号。
    
    ](https://blog.51cto.com/u_15294985/2993277)
    
    \# Linux操作系统
    
*   [
    
    WindowsServer2019AD域，禁止域用户使用本地账户登录
    
    windowsserver2019AD域禁止域用户使用本地账户登录
    
    ](https://blog.51cto.com/aurogon/3638074)
    
    AD域 活动目录 Windowsserver2019 域用户 禁止域用户本地登录
    
*   [
    
    禁止U盘复制本机文件
    
    　对于单位的电脑，通过U盘非法复制重要文件，导致泄密事件屡见不鲜。借助注册表控制，就能让插入的电脑的U盘无法复制本机文件（适用于XP SP2及以上的用户） 　　　　操作步骤 　　第一步：在开始菜单打开“运行”输入“regedit”打开注册表编辑器。展开\[HKEY\_LOCAL\_MacHINE\\SYSTEM\\CurrentControLSet\\Control\],在该分支下新建一个名为“Storag
    
    ](https://blog.51cto.com/freemanluo/204375)
    
    职场 休闲
    
*   [
    
    电脑账户无“改用Microsoft账户登录”
    
    开始-运行 -netplwiz
    
    ](https://blog.51cto.com/u_15082397/3836874)
    
    随笔
    
*   [
    
    账户和权限管理
    
    管理用户和组账号用户和组账号概述Linux基于用户身份对资源访问进行控制用户帐号：超级用户 root  超级用户，即root用户，类似于Windows系统中的Administrator用户，非执行管理任务时不建议使用root用户登录系统普通用户   普通用户帐号一般只在用户自己的宿主目录中有完全权限程序用户   程序用户：用于维持系统或某个程序的正
    
    ](https://blog.51cto.com/u_13684970/2135965)
    
    账户管理 权限管理
    
*   [
    
    账户管理与权限
    
    用户账户超级用户：root用户默认的超级用户账号普通用户：由root用户或其它管理员用户创建，只在用户自己的宿主目录中拥有完整权限程序用户：用户不允许登录到系统，而仅用于维护系统或某个程序的正常运行  UID号每个用户账号都有一个数字形式的身份标识，称为UID（用户标识号）Root用户账号的uid为固定值0程序用户账号的uid默认为1~999普通用户默认1000~60000&n
    
    ](https://blog.51cto.com/u_15569117/5210213)
    
    用户账号 主目录 用户账户
    
*   [
    
    MySQL账户权限管理
    
    一、授予用户lili在数据库studentinfo的表student上拥有对列sno和sname的select权限grant select(sno,sname) on student to 'lili'@'localhost';二、授予用户lili可以在数据库studentinfo中执行所有数据库操作的权限grant all on studentinfo.\* to 'lili'@'...
    
    ](https://blog.51cto.com/u_15346415/5171571)
    
    账户权限管理 数据库 数据库操作 mysql 其他
    
*   [
    
    MongoDB 账户权限配置
    
    1\. 账户权限配置 创建超级管理用户 修改数据库配置文件 重启 mongodb 服务 windows + R 用超级管理员
    
    ](https://blog.51cto.com/u_15302032/4912502)
    
    前端 MongoDB mongodb 数据库 数据
    
*   [
    
    tomcat登录账户配置
    
    tomcat7和tomcat6的用户信息配置有些不一样,tomcat7中添加了manager=gui和admin-gui角色,配置参考如下:再 tomcat 文件夹的conf文件夹中的 tomcat-users.xml代码：<role rolename="manager"/><role rolename="manager-gui"/><role role
    
    ](https://blog.51cto.com/u_9058648/3563536)
    
    tomcat 用户信息 xml 服务器端
    
*   [
    
    禁用访客账户登录
    
    echo allow-guest=false | sudo tee -a /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
    
    ](https://blog.51cto.com/kmt1994/1830182)
    
    false
    
*   [
    
    linux账户权限和特殊权限管理
    
    linux账户与权限管理,特殊权限管理
    
    ](https://blog.51cto.com/liyuanjie/2140422)
    
    linux账户与权限管理
    
*   [
    
    登录系统的本机ip
    
    @RequestMapping("/") public String index2(ServletRequest request, Model model){// System.out.println("userService="+adminuserService); HttpServletRequest req = (HttpServletR...
    
    ](https://blog.51cto.com/lirixing/4978218)
    
    java
    
*   [
    
    SQL安全管理--建立管理登录账户与相应权限的设定
    
    实验名称：建立管理登录账户与相应权限的设定实验需求描述 ：在电信公司服务器的默认实例中已经建立了一个数据库Tariffsmall用来存储通话计费信息，现在需要加强数据库的安全，以保障系统的正常运行。通过适当的权限分配，授予或撤销用户的访问数据库及其对象的权限。试验步骤：1．      设置SQL server身份验证模式。
    
    ](https://blog.51cto.com/yueyuanyuan/311462)
    
    SQL 账户 权限 管理 登录
    

举报文章

请选择举报类型

内容侵权 涉嫌营销 内容抄袭 违法信息 其他

具体原因

包含不真实信息 涉及个人隐私

原文链接（必填）

补充说明

0/200

上传截图

格式支持JPEG/PNG/JPG，图片不超过1.9M

已经收到您得举报信息，我们会尽快审核