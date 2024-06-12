---
aliases: []
tags:
  - ladder
  - vpn
  - ssr
  - ss
  - shadowsocks
  - shadowsocksr
  - clash
created: 2024-05-25 22:58:31
modified: 2024-06-09 22:19:19
---

# 梯子笔记

---

## 相关概念

### 机场

> 机场是从 2019 年以后兴起的新的翻墙技术。它的工作原理和 VPN 不同。VPN 一般是自己有专门开发的软件，和 VPN 厂商提供的服务器进行配合，来满足用户的需求。而机场则是开源软件和自主搭建的服务器进行配合。
>
> 最早的机场服务器商用的开源软件基本上都是 Shadowsocks（SS）或者 ShadowsocksR（SSR），他们不开发软件，只搭建 SS 或 SSR 协议许可的服务器，用户只要获得订阅，就可以更新服务器。如果换一个服务商，甚至都不用重新下载 SS 或 SSR，直接换服务器订阅就好了，非常方便。
> 
>而 SS 和 SSR 的图标，就是一个纸飞机 logo，那么既然软件是纸飞机，提供给软件的服务商就叫机场了。

![ssr logo](http://vpchina.oss-ap-southeast-1.aliyuncs.com/uploads/2023/02/image-2-1024x576-1.png)

> [!info] 相关资料
> 
> * [机场的几种协议的历史](https://www.vpn-china.org/history-of-several-airport-agreements/)

---

## 常用机场

### 性价比机场

#### 疾风云

[疾风云](https://jifeng3267.xyz/) 老牌机场

入门套餐：9.9 元 ，一个月 50G 流量，60M 速度，非常适合临时用下，如登下 [Google](https://www.google.com) 和 Gmail，用来「保号」！

> [!info] 
> 
> [谷歌账号闲置期上限调整为 2 年，12 月 1 日起将删除闲置账号 - IT之家](https://www.ithome.com/0/707/827.htm)

#### 蓝帆云

[蓝帆云](https://lf.lanfan.cc/)

同样也有 9.9 的入门套餐，同样 50G 流量，不过蓝帆云 9.9 套餐不支持 V2ray 节点。

#### 贝贝云

[贝贝云](https://beibeilink.top/) 入门套餐，够便宜，9.9 元/月， 80G。

协议支持：SS 协议，基本支持所有代理软件，包括 clash，小火箭，v2rayn，quantumultx，sstap 等。不支持 ShadowsocksR。

#### SYN 机场
性器
[SYN](https://my.synn.cc/) 入门套餐：10 元/月，80G。

#### 海盗云

[海盗cloud](https://www.hdycco.xyz/) 入门套餐：9.9 元/月，1000G，量大管饱。

### 起帆云

[起帆Cloud](https://www.qf1.us/) 入门套餐：9.9 元/月，2000G，不限速、不限设备数，极具性价比。

### 超悦机场

[**超悦**](https://www.chaoyue.shop/#/register?code=2VpoP7sg) 入门套餐有两个：

1. **年付** 12 元，每月 100G 流量，限速 100M，相当于 1 元/月，适合长期订阅，流量需求不大的用户
2. 6.8 元/月，500G，限速 300M，长期订阅但单月流量较大。

### 相关资料

* [机场整理推荐](https://github.com/WallKiller-glitch/V2raySSSSRShare)
* [Clash 机场推荐（2024 最佳 Clash、Shadowrocket 节点）](https://clashios.com/)
* [机场推荐！2024年5月大量高性价比机场指南与免费机场汇总集合啦！ - 机场8指南](https://www.jichang8.com/ji-chang/stable-and-good-ji-chang.html)
* [机场评测表 | VPN中国](https://www.vpn-china.org/%e6%9c%ba%e5%9c%ba%e8%af%84%e6%b5%8b%e8%a1%a8/)
* [2024最新Clash节点订阅地址链接分享Clash机场推荐购买](https://clashnode.org/)

---

## 客户端

> [!info] 
> 
> * [【2024年】V2rayN 和 Clash 客户端哪个好用？机场小白应该如何选择？ – 胖橙博客](https://jiasupanda.com/v2rayn-clash)

### Clash

#### Clash-Meta

[Clash Meta](https://github.com/MetaCubeX/mihomo)（现更名为 *mihomo*）是一个基于广受欢迎的开源项目 Clash 的高级版本。它继承了 Clash 的核心功能，保留了原始 Clash 的灵活性和高效性，并增加了一些独特的特性，并包括部分 Clash Premium 核心功能，是目前网络代理和数据流管理最强大的软件。

它与 [Clash-Verge](#Clash-Verge) 、[Clash-Verge-Rev](#Clash-Verge-Rev)、[Clash-Nyanpasu](#Clash-Nyanpasu)、[Clash.Mini](https://github.com/MetaCubeX/Clash.Mini) 等 GUI 客户端一起使用。

#### 相关下载

* [Clash 客户端下载集合](https://mega.nz/folder/ou9jjJhb#IqFnaxXGNNcDZdxArULIeg)

#### Clash-Verge

[clash-verge](https://github.com/clash-verge-rev/clash-verge-rev) 是一个款跨 windows、Linux 和 macOS 的电脑客户端。

> [!info]
> 
> * [最新版 Clash Verge 下载 -Clash 爱好者](https://clashios.com/latest-clash-verge/)
> * [Clash Verge 教程（2024 船新版本简单上手）-Clash 爱好者](https://clashios.com/clash-verge-tutorial/)

##### 安装

以 [ArchLinux](../Linux/ArchLinux_Note.md) 为例：

```shell
pacman -S clash-verge
```

##### 配置和使用

1. 在疾风云的「**用户中心**」，点击「复制 Clash 订阅链接」，将 Clash 链接复制到剪切板中。
2. 在 Clash-Verge「*订阅*」页面中「**导入**」刚才复制的「Clash 订阅链接」。
3. 在 Clash-Verge「*代理*」页面，选择合适的「节点」。
> [!info] 
> 
> 建议使用「规则」下的节点，因为它会对国内网站进行「过滤」，让国内网站直链不走代理；如果使用「全局」，那国内网站也走代理，除非你使用 [PAC](#PAC) 模式进行过滤；
4. 在 Clash-Verge「*设置*」页面，开启「**系统代理**」，这就能「出海」了，如果不用代理，就把这项关闭就好了！

Clash-Verge 的数据目录：`~/.local/share/clash-verge`

##### 使用体验

使用各种机场，对于 [github](github.com) 虽然可以访问，但延迟还是相较于 [Google](google.com) 等普通人的常用网站来说，还是比较 高的，可能 `github` 对于普通用户来说有点非主流吧，这些机场都对其有针对性的优化，所以造成了对于 `git push` 这种操作，还不如使用 [Steam++](../Git/Git_Note.md#^steampp) 来得高效。

> [!important] 
> 
> Clash-Verge 已经停更，建议使用 [Clash-Verge-Rev](#Clash-Verge-Rev) 或 [Clash-Nyanpasu](#Clash-Nyanpasu)。这俩「继任者」的用法大同小异。

#### Clash-Verge-Rev

[clash-verge-rev](https://github.com/clash-verge-rev/clash-verge-rev) 是 [clash-verge](#clash-verge) 的一个延续版本。

操作与 [clash-verge](#clash-verge) 一样。

两者 logo 不一样，clash-verge-rev：

![clash-verge-rev logo](https://github.com/clash-verge-rev/clash-verge-rev/raw/main/src-tauri/icons/icon.png)

> [!info] 
> 
> * [Clash Verge Rev使用教程](https://clashvergerev.com/tutorial.html)

Clash-Verge-Rev 的数据目录：`~/.local/share/clash-verge` 及 `~/.local/share/io.github.clash-verge-rev.clash-verge-rev`

#### Clash-Nyanpasu

[Clash Nyanpasu](https://nyanpasu.elaina.moe/zh-CN/) [![clash nyanpasu github repo](https://img.shields.io/github/stars/LibNyanpasu/clash-nyanpasu?style=social
)](https://github.com/LibNyanpasu/clash-nyanpasu) 是 Clash 内核删库事件之后新开发的 Clash GUI 客户端，采用 Clash Meta 内核，兼容 Clash Premium 配置，支持 Windows、Mac 和 [Linux](../Linux/Linux_Note.md)。

> [!quote] 
> 
> Clash Nyanpasu 原生支持多种内核，包括 Clash Premium、Clash Meta 和 Clash Rust，用户可以根据个人喜好选择使用。

Linux 版本的相关目录：

* 配置目录位置：`~/.config/clash-nyanpasu`
> [!tip] 
> 
> 与 [clash-verge](#clash-verge) 及其衍生版都不共用，这非常好。
* 数据目录：`~/.local/share/clash-nyanpasu` 及 `~/.local/share/moe.elaina.clash.nyanpasu`

> [!info] clash nyanpasu 相关文档
> 
> * [简介 | Clash Nyanpasu](https://nyanpasu.elaina.moe/zh-CN/introduction#%E4%BB%80%E4%B9%88%E6%98%AF-clash-nyanpasu)
> * [请问该项目与clash-verge-rev的关系](https://github.com/LibNyanpasu/clash-nyanpasu/issues/17)

#### PAC

通过开启 `PAC模式` 对国内网站进行直链设置。

类似代码如下：

```js
function FindProxyForURL(url, host) {

	const directList = [
		"www.baidu.com",
		"www.bilibili.com",
		"www.163.com",
		"www.jd.com",
		"www.taobao.com",
		"www.ixigua.com",
		"www.msn.cn",
		"www.bing.com",
		"cn.bing.com",
		"tv.cctv.com",
		"www.dangdang.com",
		"www.zol.com.cn",
		"www.miguvideo.com",
		"www.smzdm.com",
		"www.zdic.net",
		"mirrors.tuna.tsinghua.edu.cn",
		"www.shuge.org"
		
		//"10.11.12.13",
	
	];
	
	if (directList.includes(host)) {
	
		return "DIRECT";
	
	}
	
	return "PROXY 127.0.0.1:%mixed-port%; SOCKS5 127.0.0.1:%mixed-port%; DIRECT;";

}

```

> [!info] 
> 
> 最重要的是 `directList` 这块代码函数。其中是要直链的网站地址列表。

### V2ray

[V2Ray](https://www.v2ray.com/) 是 ProjectV 下的一个工具。V2Ray 是一个与 Shadowsocks 类似的代理软件。

V2Ray 本身不支持 [PAC](#PAC)。

#### V2ray 客户端

> [!quote] 
> 
> 从软件上 V2Ray 不区分服务器版和客户端版，也就是说在服务器和客户端运行的 V2Ray 是同一个软件，区别只是配置文件的不同。
>
> 因此 V2Ray 的安装在服务器和客户端上是一样的，但是通常情况下 VPS 使用的是 [Linux](#Linux) 而 PC 使用的是 [Windows](#Windows)。

##### Windows

[v2rayN](https://github.com/2dust/v2rayN) 是 Windows 下的 v2ray 的客户端。

##### Linux

###### 安装及启动 v2ray

首先得先安装 `v2ray`，可以通过操作系统的包管理器进行安装，如 `pacman -S v2ray`。

安装完了，通过 `systemctl` 启动 v2ray 服务：

```shell
sudo systemctl start v2ray
```

> [!tip] 
> 
> 启动 v2ray 后 ，可以使用 `systemctl status v2ray` 来查看当前 v2ray 的状态。

###### v2rayA

Linux 下 gui 客户端项目，大部分都停更了。还活着的只有 [v2rayA](https://github.com/v2rayA/v2rayA) ，它是一个 Web 端图形界面。

同样的，通过包管理工具安装，如 `pacman -S v2raya`。

> [!tip] 
> 
> 如果在安装 `v2raya` 之前，没有安装 [V2ray](#V2ray)，包管理器会自动将其安装，`v2ray` 属于 `v2raya` 的「依赖」。

同样的，v2rayA 安装完了，它也是「服务方式运行」，所以也需要启动其服务：

```shell
sudo systemctl start v2raya
```

`v2raya` 的服务启动成功后，在浏览器中输入 `[127.0.0.1:2017](http://127.0.0.1:2017/)` 来访问 v2raya。

#### 规则

[v2ray-rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat) 是路由规则文件加强版，可代替 V2Ray 官方 `geoip.dat` 和 `geosite.dat`。

[v2ray-china-list](https://github.com/dctxmei/v2ray-china-list) 是中国域名列表，用于在规则中过滤掉中国网站，让其直链。

### Hiddify

[Hiddify](https://github.com/hiddify/hiddify-next) 是一个多平台的客户端。能同时支持 [Clash](#Clash) 和 [V2ray](#V2ray) 在内的多种订阅格式。

#### 设置

「直链 DNS」那里设置下，如使用 `114.114.115.115`。

### SurfBoard

[surfboard](https://github.com/getsurfboard/surfboard) 是安卓端的网络流量处理工具。

### 相关资料

* [如何使用疾风云机场？-2024 VPN](https://gfwoff.com/how-to-use-jifeng-cloud-vpn/)
* [Clash Verge Rev官网下载 - Clash Verge](https://clashverge.net/clash-verge-rev/)
* [toutyrater](https://toutyrater.github.io/)
* [v2rayA 入门使用教程](https://v2rayn.uuk.app/112)
* [什么是 SSR、V2ray、Trojan、Clash，什么是机场？ | Yasir Lin 的笔记](https://young1lin.me/2020/10/30/GFW/)

---

## 相关链接

* [clashios爱好者](https://clashios.com/)
* [clashverge.org](https://clashverge.org/)
