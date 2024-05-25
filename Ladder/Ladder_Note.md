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
modified: 2024-05-26 02:37:53
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

[jifeng3267](https://jifeng3267.xyz/)

9.9 一个月的入门级套餐，一个月 50G 流量，非常适合临时用下，如登下 [Google](googl.com) 和 Gmail，用来「保号」！

> [!info] 
> 
> [谷歌账号闲置期上限调整为 2 年，12 月 1 日起将删除闲置账号 - IT之家](https://www.ithome.com/0/707/827.htm)

#### 蓝帆云

[蓝帆云](https://lf.lanfan.cc/)

同样也有 9.9 的入门套餐，同样 50G 流量，不过蓝帆云 9.9 套餐不支持 V2ray 节点。

#### 贝贝云

[贝贝云](https://beibeilink.top/) 入门套餐，够便宜，9.9 元 80G。

协议支持：SS 协议，基本支持所有代理软件，包括 clash，小火箭，v2rayn，quantumultx，sstap 等。不支持 ShadowsocksR。

#### SYN 机场

[SYN](https://my.synn.cc/) 入门套餐：10 元，80G。

### 其他机场

* [RioLU.443 地址发布页](https://clashios.com/riolu)

### 相关资料

* [机场整理推荐](https://github.com/WallKiller-glitch/V2raySSSSRShare)
* [Clash 机场推荐（2024 最佳 Clash、Shadowrocket 节点）](https://clashios.com/)
* [机场评测表 | VPN中国](https://www.vpn-china.org/%e6%9c%ba%e5%9c%ba%e8%af%84%e6%b5%8b%e8%a1%a8/)

---

## Clash

### clash-verge

[clash-verge](https://github.com/clash-verge-rev/clash-verge-rev) 是一个款跨 windows、Linux 和 macOS 的电脑客户端。

> [!info]
> 
> * [最新版 Clash Verge 下载 -Clash 爱好者](https://clashios.com/latest-clash-verge/)
> * [Clash Verge 教程（2024 船新版本简单上手）-Clash 爱好者](https://clashios.com/clash-verge-tutorial/)

### 安装

以 [ArchLinux](../Linux/ArchLinux_Note.md) 为例：

```shell
pacman -S clash-verge
```

### 配置和使用

1. 在疾风云的「**用户中心**」，点击「复制 Clash 订阅链接」，将 Clash 链接复制到剪切板中。

2. 在 Clash-Verge「*订阅*」页面中「**导入**」刚才复制的「Clash 订阅链接」。

3. 在 Clash-Verge「*代理*」页面，选择合适的「节点」。

4. 在 Clash-Verge「*设置*」页面，开启「**系统代理**」。

### 相关资料

* [如何使用疾风云机场？-2024 VPN](https://gfwoff.com/how-to-use-jifeng-cloud-vpn/)

---

## 相关链接

* [clashios爱好者](https://clashios.com/)
* [clashverge.org](https://clashverge.org/)
