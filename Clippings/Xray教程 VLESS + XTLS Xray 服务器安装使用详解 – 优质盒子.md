# Xray教程 VLESS + XTLS Xray 服务器安装使用详解 – 优质盒子
Xray 性能至上、可扩展性空前，目标是全场景终极协议
---------------------------

划时代的革命性概念与技术：XTLS！当我们使用基于 TLS 的代理浏览 HTTPS 网站、刷手机 APP 时，其实是两层 TLS：外层是代理的 TLS，内层是网站、APP 的 TLS。

1.  从第二个内层 TLS data record 开始，数据不二次加解密，直接转发，且从外面看还是一条连贯的普通 TLS。
2.  服务端无法被主动探测出差异：VLESS 在验证 UUID、该用户请求且可以用 XTLS 后才会开启它的特殊功能。
3.  Write 和 Read 妥善处理非预期数据和中间人攻击等，对任何干扰的反应与普通 TLS 表现一致。

### Xray是什么

**[Xray](https://uzbox.com/tag/xray "Xray")** 顾名思义，犹如镭射眼X光一样可以穿透一切，最好的 V2ray-Core 服务端。支持 XTLS，支持VLESS协议，代理速度更快，延迟更低，是 V2ray 的升级版。青出于蓝而胜于蓝，Xray-core 是 v2ray-core 的超集，含更好的整体性能和 XTLS 等一系列增强，且完全兼容 v2ray-core 现有的功能及配置。

*   只有一个可执行文件，含 ctl 的功能，run 为默认指令
*   配置上完全兼容，环境变量和 API 对应要改为以 XRAY\_ 开头
*   全平台开放了裸协议的 ReadV
*   提供完整的 VLESS & Trojan XTLS 支持，均有 ReadV
*   提供了 XTLS 多种流控模式, 性能一骑绝尘!

[xray官网](https://uzbox.com/tag/xrayguanwang "xray官网") ：[https://xtls.github.io](https://xtls.github.io/)

### Xray下载

[Xray下载](https://uzbox.com/tag/xrayxiazai "xray下载")地址：[https://github.com/XTLS/Xray-core/releases](https://github.com/XTLS/Xray-core/releases)![](https://uzbox.com/wp-content/uploads/2022/11/c3d8424067dd4eac15d4cbdf1d1a02fc.png)

### Xray 和 V2ray 的区别

*   V2ray：Project V 是用于构建基础通信网络的工具合集，其核心工具称为V2Ray。V2ray主要负责网络协议和功能的实现，既可以单独运行，也可以和其它工具配合。V2ray官网：[https://v2ray.com/](https://v2ray.com/)，Github项目主页：[https://github.com/v2ray](https://github.com/v2ray)（目前已经停止更新维护）V2ray原开发者长期不上线，其他维护者没有完整权限，导致V2ray项目维护困难。因此社区在2019年组建了 **V2fly** 组织，继续维护V2ray，也是目前 V2ray 发展的主力。V2fly官网：[https://www.v2fly.org](https://www.v2fly.org/)，Github项目主页：[https://github.com/v2fly](https://github.com/v2fly)
*   Xray：因为许可理念之争，VLESS和XTLS的作者单独创建了Xray项目，目前是V2ray的超集。Xray文档官网：[https://xtls.github.io](https://xtls.github.io/)， Github项目主页：[https://github.com/XTLS](https://github.com/XTLS)

Xray 是 V2ray 的一个分支(Fork)。Xray项目基于V2ray而来，其支持并且兼容V2ray的配置。Xray是V2ray的超集。虽然最新版V2ray删除了XTLS，但仍保留VLESS协议。Xray提供完整的VLESS和XTLS支持，目前是V2ray的超集，但后续Xray可能会有会有自己的发展方向！

[![](https://uzbox.com/wp-content/uploads/2022/11/20071443d699f00350a32da7eee2c67c-700x447.png)
](https://uzbox.com/wp-content/uploads/2022/11/20071443d699f00350a32da7eee2c67c.png)

### Xray 安装和使用教程

xray的安装方法和v2ray类似，xray-core是xray的核心程序，可以使用官方提供的一键安装脚本。

你也可以使用 [ProxySU](https://github.com/proxysu/ProxySU) 进行一键安装！ [ProxySU](https://github.com/proxysu/ProxySU) 可一键安装 V2ray/Xray, Shadowsocks, Trojan, Trojan-Go, NaiveProxy, MTProto Go, Brook 后续还会再添加其他。

如果是新安装的系统，需要先安装wget、ca-certificates和bind-utils软件包。

```
dnf -y install wget ca-certificates bind-utils
```

#### xray 官方安装脚本

**安装和升级 Xray-core 和地理数据User=nobody，但不会覆盖User现有服务文件**

```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```

**仅更新 geoip.dat 和 geosite.dat**

```
\# bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install-geodata
```

**删除 Xray，json 和日志除外**

```
\# bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ remove
```

**安装并将 Xray-core 升级到预发布版本**

```
\# bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install --version 1.5.8

```

**安装和升级 Xray-core 和 geodata with User=root，这将覆盖User现有的服务文件**

```
\# bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install -u root

```

**在没有地理数据的情况下安装和升级 Xray-core**

```
\# bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install --without-geodata
```

**删除 Xray，包括 json 和日志**

```
\# bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ remove --purge
```

**帮助**

```
\# bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ help
```

官方脚本安装的文件符合FHS规范，可执行文件xray在 /usr/local/bin 目录下，配置文件位于 /usr/local/etc/xray目录内。

**Xray 在系统中的存放位置：** 

```
installed: /etc/systemd/system/xray.service
installed: /etc/systemd/system/xray@.service
installed: /usr/local/bin/xray
installed: /usr/local/etc/xray/\*.json
installed: /usr/local/share/xray/geoip.dat
installed: /usr/local/share/xray/geosite.dat
installed: /var/log/xray/access.log
installed: /var/log/xray/error.log
```

#### kirin10000的xray安装脚本

Xray：（VLESS/VMess）-（TCP/gRPC/WebSocket）-（XTLS/TLS）+Web搭建/管理脚本。

脚本运行前，先解析一个域名到所在服务器的IP上。需要域名进行证书申请和配置nginx。

```
#获取更新脚本
wget -O Xray-TLS+Web-setup.sh https://github.com/kirin10000/Xray-script/raw/main/Xray-TLS+Web-setup.sh
#执行脚本
bash Xray-TLS+Web-setup.sh
```

Xray-TLS+Web搭建/管理脚本项目地址：[https://github.com/kirin10000/Xray-script](https://github.com/kirin10000/Xray-script)

1.  支持 (VLESS/VMess)-(TCP/gRPC/WebSocket)-(XTLS/TLS) + Web 的搭建/管理，支持多种协议并存
2.  集成 多版本bbr/锐速 安装选项
3.  支持多种系统 (Ubuntu CentOS Debian deepin fedora ...)
4.  支持多种指令集 (x86 x86\_64 arm64 ...)
5.  支持ipv6only服务器 (需自行设置dns64)
6.  集成删除阿里云盾和腾讯云盾功能 (仅对阿里云和腾讯云服务器有效)
7.  使用Nginx作为网站服务
8.  使用Xray作为前置分流器
9.  使用acme.sh自动申请/更新域名证书
10.  支持选择搭建个人网盘作为伪装网页

**安装流程：** 

\[升级系统组件\]->\[安装bbr\]->\[安装php\]->安装Nginx->安装Xray->申请证书->配置文件->\[配置伪装网站\]

执行安装脚本后，进入到脚本安装界面：

[![](https://uzbox.com/wp-content/uploads/2022/11/91c176231d1470696b229ce26bc23d1b-700x598.png)
](https://uzbox.com/wp-content/uploads/2022/11/91c176231d1470696b229ce26bc23d1b.png)按1，进行安装Xray-TLS+Web，之后按y重新设置一下ssh的超时参数，这里脚本会自动设置，重新启动服务器后，再次运行安装脚本。

```
bash Xray-TLS+Web-setup.sh
```

系统组件更新选择2，仅更新已安装软件之后，回车进行组件更新。[  
](https://uzbox.com/wp-content/uploads/2022/11/d92072d54e10c825ff167f0a60c3f2d4.png)[![](https://uzbox.com/wp-content/uploads/2022/11/d92072d54e10c825ff167f0a60c3f2d4.png)
](https://uzbox.com/wp-content/uploads/2022/11/d92072d54e10c825ff167f0a60c3f2d4.png)

之后安装BBR，选择1，安装、升级最新稳定版内核并启用bbr。

[![](https://uzbox.com/wp-content/uploads/2022/11/91f84430f0e88c8eeee9246e314be588-700x399.png)
](https://uzbox.com/wp-content/uploads/2022/11/91f84430f0e88c8eeee9246e314be588.png)

> 提示：  
> 更换内核后服务器将重启，卸载闲置的内核，重启后，请再次运行脚本完成 Xray-TLS+Web 剩余部分的安装/升级  
> 再次运行脚本时，重复之前选过的选项即可。

重新启动后，运行安装脚本，依次选择：1.安装Xray-TLS+Web>3.不更新组件>0.退出bbr安装。

传输层协议选择1.TCP，只有TCP能使用XTLS，且XTLS完全兼容TLS。

[![](https://uzbox.com/wp-content/uploads/2022/11/d4ad131e7d722577e77b862306ff2dca-700x256.png)
](https://uzbox.com/wp-content/uploads/2022/11/d4ad131e7d722577e77b862306ff2dca.png)

输入你的域名，确认已经解析到服务器IP上。

[![](https://uzbox.com/wp-content/uploads/2022/11/9f0672d874d00d8fab84277260fd1c72-700x150.png)
](https://uzbox.com/wp-content/uploads/2022/11/9f0672d874d00d8fab84277260fd1c72.png)

选择伪装网站页面，选择2.安装个人网盘Nextcloud

[![](https://uzbox.com/wp-content/uploads/2022/11/42033d70ccdc7d6c5236afd61d5c6c5c-700x244.png)
](https://uzbox.com/wp-content/uploads/2022/11/42033d70ccdc7d6c5236afd61d5c6c5c.png)

脚本安装Nextcloud时需要安装php，编译安装php可能需要额外消耗 **15-60** 分钟。  
选择之后，耐心等待就可以了。

当脚本全部安装完毕后，会提示访问你之前设置的域名，进行Nextcloud初始化设置：  
自定义管理员的用户名和密码后，关闭web页面，[Xray安装](https://uzbox.com/tag/xrayanzhuang "Xray安装")完毕。

下面是xray的客户端配置说明：

```
\--------------------- VLESS-TCP-XTLS/TLS (不走CDN) ---------------------
protocol(传输协议)：vless(V2RayN选择"添加\[VLESS\]服务器";V2RayNG选择"手动输入\[VLESS\]")
address(地址)：服务器ip(Qv2ray:主机)
port(端口)：443 #默认端口
id(用户ID/UUID)：#自动生成的UUID
flow(流控)：#使用XTLS：Linux/安卓/路由器：xtls-rprx-splice(推荐)或xtls-rprx-direct，其它：xtls-rprx-direct，使用TLS：空
encryption(加密)：none
--------------Transport/StreamSettings(底层传输方式/流设置)--------------
network(传输方式)：tcp(Shadowrocket传输方式选none)
type(伪装类型)：none(Qv2ray:协议设置-类型)
security(传输层加密)：xtls或tls (此选项将决定是使用XTLS还是TLS)(V2RayN(G):底层传输安全;Qv2ray:TLS设置-安全类型)
serverName：#你的域名(V2RayN(G):SNI;Qv2ray:TLS设置-服务器地址;Shadowrocket:Peer 名称)
allowInsecure：false(Qv2ray:TLS设置-允许不安全的证书(不打勾);Shadowrocket:允许不安全(关闭))
fingerprint：使用XTLS ：空。使用TLS  ：空/chrome(推荐)/firefox/safari(此选项决定是否伪造浏览器指纹，空代表不伪造)
alpn：伪造浏览器指纹  ：此参数不生效，可随意设置。不伪造浏览器指纹：若serverName填的域名对应的伪装网站为网盘，建议设置为http/1.1；否则建议设置为h2,http/1.1 (此选项为空/未配置时，默认值为"h2,http/1.1")(Qv2ray:TLS设置-ALPN) (注意Qv2ray如果要设置alpn为h2,http/1.1，请填写"h2|http/1.1")
------------------------其他-----------------------
Mux(多路复用)：使用XTLS必须关闭;不使用XTLS也建议关闭(V2RayN:设置页面-开启Mux多路复用)
```

### 注意事项

*   为了防止上层应用使用 QUIC，启用 XTLS 时客户端 VLESS 会自动拦截 UDP/443 的请求。若不需拦截，请在客户端填写流控： xtls-rprx-direct-udp443，服务端不变。Linux系统推荐设置为：xtls-rprx-splice-udp443。
*   可设置环境变量 V2RAY\_VLESS\_XTLS\_SHOW = true 以显示 XTLS 的输出，适用于服务端与客户端（仅用于确信 XTLS 生效了，千万别设成永久性的，不然会很卡）。
*   不能开启 Mux。XTLS 需要获得原始的数据流，所以原理上也不会支持 WebSocket、不适用于 VMess。此外，UDP over TCP 时，VLESS 不会开启 XTLS 的特殊功能。

### xray客户端

服务端配置好后，接下来是配置客户端。目前有如下客户端支持Xray：

**Xray Windows客户端：** 

**V2rayN**：最新版本[V2rayN5.38](https://github.com/2dust/v2rayN/releases)。之后下载最新Windows版本的[Xray-core](https://github.com/XTLS/Xray-core/releases)，将解压的文件放到V2rayN-Core文件夹下即可。[v2rayn 下载使用教程](https://uzbox.com/tech/v2rayn.html)

打开V2rayN客户端，添加VLESS服务器。core类型选择Xray，之后将系统代理设置为自动设置系统代理即可。

[![](https://uzbox.com/wp-content/uploads/2022/11/ce0c05eb02e3eb866ee60d8fcb0c79c9-700x569.png)
](https://uzbox.com/wp-content/uploads/2022/11/ce0c05eb02e3eb866ee60d8fcb0c79c9.png)**winXray**：WinXray是一个支持 [Xray/V2Ray(vmess/vless/xtls)、Shadowsocks、Trojan、Trojan-go](https://github.com/XTLS/Xray-core)、SSR、NaïveProxy、 等等 网络代理的通用客户端（Windows系统），可自动检测并连接访问速度最快的代理服务器。服务器连接异常时可以自动更换代理服务器 - 再也不用担心服务器抽风了。winXray 也提供一键安装 XRay(V2Ray、Shadowsocks、Trojan) 服务器工具。

winXray官网地址下载：[https://www.winxray.com](https://www.winxray.com/)

**Qv2ray：** 使用 Qt 框架的跨平台 V2Ray 客户端。支持 Windows, Linux, macOS。插件系统支持 SSR / Trojan / Trojan-Go / NaiveProxy

截止2021-08-17，由于开发者内部出现矛盾，Qv2ray项目已经不再维护。最后版本[Qv2ray v2.70](https://github.com/Qv2ray/Qv2ray/releases)

**Xray安卓客户端：** 

V2rayNG：V2rayNG可以说是最跟随Xray步伐的V2ray客户端了，Xray发布新版本后会在第一时间更新，推荐使用。

[V2RayNG 使用教程，V2RayNG免费订阅地址](https://uzbox.com/tech/v2rayng.html)

**Xray Mac客户端：** 

Qv2ray：Qv2ray是一个基于Qt框架开发的跨平台v2ray客户端，因此支持MacOS系统。Qv2ray算得上Mac系统上支持VLESS协议的独苗。

**Xray苹果客户端：** 

Shadowrocket/小火箭：小火箭目前是ios系统上更新最频繁的[Xray客户端](https://uzbox.com/tag/xraykehuduan "xray客户端")，价格也不贵，支持多种协议，推荐使用。

### 参考资料：

[Xray教程](https://itlanyan.com/xray-tutorial/)

[Project X源于XTLS协议，提供了一套网络工具，如Xray-core](https://github.com/XTLS/Xray-core)