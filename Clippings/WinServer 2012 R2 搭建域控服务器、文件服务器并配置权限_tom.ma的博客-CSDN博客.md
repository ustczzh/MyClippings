# WinServer 2012 R2 搭建域控服务器、文件服务器并配置权限_tom.ma的博客-CSDN博客
### 一、安装域控服务器

1、修改主机IP，DNS，主机名

![](https://img-blog.csdnimg.cn/202106010212043.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20210601023435698.png)

2、添加角色与功能

![](https://img-blog.csdnimg.cn/20210601015930241.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

3、下一步至服务器角色，这里只勾选 Active Directory 域服务，安装时会自动安装 DNS服务

![](https://img-blog.csdnimg.cn/20210601021050409.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

4、一直下一步至安装 成功

![](https://img-blog.csdnimg.cn/20210601021350685.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

5、关闭后，点击升为域控服务器

![](https://img-blog.csdnimg.cn/20210601021516400.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

6、添加新林，本例 xielong.com

![](https://img-blog.csdnimg.cn/20210601021626886.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

7、输入还原密码

![](https://img-blog.csdnimg.cn/20210601021721933.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

8、一直下一步到安装

![](https://img-blog.csdnimg.cn/20210601021902916.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

9、重启后，可以用域用户进行登陆了

![](https://img-blog.csdnimg.cn/20210601023239346.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

10、创建 组织单位，组，用户

> 创建 一级 组织单位 xielong
> 
> 创建 二级 组织单位 Sales Deparment， Product Deparment， Finance Deparment
> 
> 在每个 二级单位下面 分别创建 对应的安全组，及用户，把用户加入对应的组里面

 ![](https://img-blog.csdnimg.cn/20210601024121333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

11、在 Sales Deparment 创建 用户，并把用户加入到 Sales Deparment 组里

![](https://img-blog.csdnimg.cn/20210601024732382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20210601024812524.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

12、其它部门如下图

![](https://img-blog.csdnimg.cn/20210601025039529.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20210601025055262.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

### 二、安装 文件服务器

1、服务器角色 选择 文件服务器与 文件服务器资源管理器

![](https://img-blog.csdnimg.cn/20210601025617312.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

2、一直下一步直至安装成功

3、点击共享，新建共享

![](https://img-blog.csdnimg.cn/20210601025910209.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

3、选择高级

![](https://img-blog.csdnimg.cn/20210601025932798.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

4、选择 自定义路径 C:\\share

![](https://img-blog.csdnimg.cn/2021060103002987.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

5、其它设置

> 启用基于存取的枚举：  
> 简单一点说就是如果A用户只能访问A目录的权限，那他就不会看到共享下面的B目录，就不会出现点击B目录没有访问权限的提示了，这样增强了用户体验，同时也加强文件服务器的安全性。
> 
>   允许共享缓存：  
> 有两种模式：分布式缓存模式、托管式缓存模式。前者主要用于办事处等没有服务器场所，后者主要用于分支机构，集中式管理所有缓存的文件信息。
> 
>   加密数据访问：  
> 在共享文件传输的时候，会对数据进行加密，以提高数据的传输安全性。（针对WIN7系统建议不要勾选）

![](https://img-blog.csdnimg.cn/20210601030126546.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

6、权限这里先选择自定义

![](https://img-blog.csdnimg.cn/20210601030454683.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

然后再点击 禁用继承，然后在添加自己要设定的[用户权限](https://so.csdn.net/so/search?q=%E7%94%A8%E6%88%B7%E6%9D%83%E9%99%90&spm=1001.2101.3001.7020)

![](https://img-blog.csdnimg.cn/20210601030520561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20210601030633492.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

添加 如下权限，给 Administrators 组 完全控制权限，Everyone 读取权限

![](https://img-blog.csdnimg.cn/20210601030745317.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

7、一直下一步完成

8、在 C盘创建 部门文件夹

![](https://img-blog.csdnimg.cn/20210601031010686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

9、为各部门设置对应的权限，下图是财务部的，其它部门也是一样设置

![](https://img-blog.csdnimg.cn/20210601031401250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20210601031234480.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

找一台机器加入域控，并以 Alice 用户登陆

财务部的人员只能查看财务部的文件夹，无法查看产品部，与销售部的

![](https://img-blog.csdnimg.cn/20210601031815344.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20210601031702528.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21zaHh1eWk=,size_16,color_FFFFFF,t_70)