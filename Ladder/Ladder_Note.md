---
aliases: []
tags:
  - ladder
  - vpn
  - ssr
  - ss
  - shadowsocks
  - shadowsocksr
  - vmess
  - hysteria
  - v2ray
  - clash
  - xray
  - 机场
created: 2024-05-25 22:58:31
modified: 2025-07-23 17:05:05
---

# 梯子笔记

---

## 相关概念

### 机场

>[!quote] 
>
> 机场是从 2019 年以后兴起的新的翻墙技术。它的工作原理和 VPN 不同。VPN 一般是自己有专门开发的软件，和 VPN 厂商提供的服务器进行配合，来满足用户的需求。而机场则是开源软件和自主搭建的服务器进行配合。
>
> 最早的机场服务器商用的开源软件基本上都是 Shadowsocks（SS）或者 ShadowsocksR（SSR），他们不开发软件，只搭建 SS 或 SSR 协议许可的服务器，用户只要获得订阅，就可以更新服务器。如果换一个服务商，甚至都不用重新下载 SS 或 SSR，直接换服务器订阅就好了，非常方便。
> 
> 而 SS 和 SSR 的图标，就是一个纸飞机 logo，那么既然软件是纸飞机，提供给软件的服务商就叫机场了。

![ssr logo | 200x200](http://vpchina.oss-ap-southeast-1.aliyuncs.com/uploads/2023/02/image-2-1024x576-1.png)

> [!info] 相关资料
> 
> * [机场的几种协议的历史](https://www.vpn-china.org/history-of-several-airport-agreements/)

### 类型

#### 直连

直接过墙连接墙外中转服务器。

优点：价低量大管饱
缺点：延迟高，被封机率大

#### 中转

墙内中转服务器套层壳再出墙。

优点：延迟低，不容易被封
缺点：价格销高

#### 专线

通过专线绕过墙

优点：延迟低，速度快
缺点：价格高，主要用于游戏等有低延迟的出墙需求

##### BGP

Border Gateway Protocol，边界网关协议。

##### IPLC

International Private Leased Circuit 即国际专线，专用跨国通信线路。

延迟低、速度快。价格较高，带宽小。

##### IEPL

International Ethernet Private Line 国际以太网专线。此线路主要为跨国性企业提供高品质的心态网专线服务。

优点：低延时和高可靠性。
缺点：价格高，带宽小

##### CN2

China telecom Next Carrier Network 中国电信下一代承载网。

延迟低、速度快。

### 协议

#### Shadowsocks

#### ShadowsocksR

SSR 是 ShadowsocksR 的缩写，它是在 [Shadowsocks](#Shadowsocks) 的基础上增加一些数据混淆方式，修复部分安全问题。

目前由于中国运营商已有能检测 SS 及 SSR 这两种流量的技术，所以这两种协议容易被封，所以使用越来越少。

#### Vmess

> Vmess 是一种基于 WebSocket 的代理协议,由 [V2ray](#V2ray) 项目开发，算是 [V2ray](#V2ray) 专用的加密传输协议。它采用 AES-128-GCM 加密算法,并支持多种传输方式,如普通 TCP、[Http/2](../Network/Http_Note.md)、WebSocket 等,具有较强的隐蔽性。vmess 协议广泛应用于科学上网领域,是 [V2ray](#V2ray)、[Clash](#Clash) 等众多代理工具的默认协议。
> 
> 它分为**入站**和**出站**两部分，其作用是帮助客户端跟服务器之间建立通信。在 V2Ray 上客户端与服务器的通信主要是通过 VMess 协议通信。（[v2ray doc - vmess](https://www.v2ray.com/chapter_02/protocols/vmess.html#vmess)）

现在机场支持 V2ray，一般指的是支持 VMess 协议。

> [!important] 
> 
> VMess 依赖于系统时间，请确保使用 [V2ray](#V2ray) 的系统 UTC 时间误差在 90 秒之内，与 V2Ray 服务器所在时区无关。在 Linux 系统中可以安装 ntp 服务来自动同步系统时间。

#### VLess

VLess 是 [Vmess](#Vmess) 轻量版，VLESS 是一个无状态的轻量传输协议，它分为入站和出站两部分，可以作为 [V2ray](#V2ray) 客户端和服务器之间的桥梁。

与 [Vmess](#Vmess) 不同，VLess 不依赖于系统时间，认证方式同样为 UUID，但不需要 alterId。

#### Hysteria

[Hysteria](https://github.com/apernet/hysteria)

> [!info] 文档
> 
> * [Hysteria 中文文档](https://v2.hysteria.network/zh/)

#### Trojan

Trojan，特洛伊，在病毒界那是木马病毒，但在机场领域，指的是一种新的科学上网技术，全称「Trojan-GFW」，是目前最成功的科学上网伪装技术之一。Trojan 的设计理念，是直接**模仿 HTTPS** 协议。这个协议速度比 v2ray 更快，伪装比 v2ray 更逼真，「抗封力」更强。

> [!info] 
> 
> HTTPS 本身就含有加密，而且 TLS 1.3 更是强加密，现今无法通过简单方式破解。

Trojan 最大缺点是用**443**端口输出。

#### Trojan-Go

Trojan-Go 相较 [Trojan](#Trojan) 多了些功能：多路复用降低延迟，提升并发性能。

#### Reality

[REALITY](https://github.com/XTLS/REALITY) 目前最安全的 [XRay](#XRay) 代理协议。

#### 协议相关资料

* [SS、SSR、V2ray、Trojan这几种翻墙协议与VPN对比有何不同？ - overwall](https://overwallvpn.com/vpn-vs-proxy/)
* [科学上网的主流协议大对比！这里面有你在使用的吗？ | TechFen's Blog](https://www.techfens.com/posts/kexueshangwang.html)

### 路由规则

#### GFWList

「GFWList」产生于 Shadowsocks 诞生之时，Shadowsocks 根据 GFWList 判断哪些网站需要被代理，如果网站被收录在 GFWList 里，那这个网站将通过 Shadowsocks 代理，否则不会被代理。即 GFWList 收集了已知的被墙的网站。GFWList 中的网站是被墙的，即黑名单，所以这种分流方式被称为「黑名单模式」。 #黑名单模式

#### ChinaList

ChinaList 收录了中国大陆境内的网站，如果访问的网站在此列表中，该网站直连，反之不在此列表中，则会通过代理。这种方式被称为「白名单模式」。 #白名单模式 

> [!tip] 
> 
> 白名单模式有一些不被墙的网站都走了代理，比黑名单更耗流量。
>
> 但实际上那些不被墙的网站走直连速度也非常「可人」，所以花点流量有更好的体验，也是非常不错的。
> 
> [GitHub - dctxmei/v2ray-china-list](https://github.com/dctxmei/v2ray-china-list)

#### geoip

geoip 是使用 MaxMind 的 [GeoLite2](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data) IP 库，非常全面。

> 一个中国大陆网站的域名通过代理 DNS 查询到一个国外的 IP，根据上面的规则，这个网站则会通过代理访问，一样会出现慢的问题。

> [!info] 相关链接
> 
> * [GitHub - Loyalsoldier/geoip: GeoIP 规则文件加强版，同时支持定制 V2Ray dat 格式路由规则文件 geoip.dat 和 MaxMind mmdb 格式文件 Country.mmdb](https://github.com/Loyalsoldier/geoip)
> * [GitHub - Loyalsoldier/v2ray-rules-dat: V2Ray 路由规则文件加强版，可代替 V2Ray 官方 geoip.dat 和 geosite.dat](https://github.com/Loyalsoldier/v2ray-rules-dat)

#### geosite

#### geodata

#### DNS 分流

国内网站使用国内 DNS 解析，国外的使用国外 DNS 解析。

DNS 分流 +[白名单模式](#ChinaList)，是现比较好的解决方案。

#### 参考资料

* [路由规则设定方法 · V2Ray 配置指南](https://waloyn.github.io/rater/routing/configurate_rules.html)

---

## 常用机场

机场选择一般有几个因素考虑：

* 流量
> [!info] 
> 
> 如果只是日常使用，使用 Google 搜索、收发 Gmail 邮件、看下 youtube、更新下 [Vim](../vim/Vim_Note.md) 插件什么的，每月流量至少 80G 才用得比较舒服，如果像 [疾风云](#疾风云)10 元套餐 50G，就有点紧张，得捏着指头算着用）
* 价格
* 延迟，特别是晚高峰时段延迟（晚上 8 点到 11 点这段时间，节点延迟会很高，甚至会出现所有节点「全红」的现象）
* 带宽速率
> [!tip] 
> 
> 速率不如延迟重要，大部分机场速率都够日常使用，如果延迟太差，不是黄就是红，那再高的速率也没用。
* 客户端数量
* [协议](#协议)
> [!info] 
> 
> [Shadowsocks](#Shadowsocks) 及 [ShadowsocksR](#ShadowsocksR)「抗封力」太低，慎选。主流的 [Vmess](#Vmess) 可选，如果是 [Hysteria](#Hysteria) 或 [Trojan](#Trojan) 那就非常不错了！

### 稳定机场

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

#### Ouonet

[OuONetwork](https://board.ouonet.work) 入门套餐：10 元/月，100G，无速率限制，平时 1 倍率，闲时 0.8 倍率（9:00 - 11:00）。

只能使用 gmail 邮箱注册。

#### CoffeeCloud

[CoffeeCloud](https://portal.love-coffee.club) 入门套餐：10 元/月，130G，不限客户端数。

只能使用 gmail 邮箱注册。

#### FastFly

[FastFly](https://fastestcloud.xyz) 入门套餐：10 元/月，150G，1000M 带宽速率，不限客户端数。

#### 海盗云

[海盗cloud](https://www.hdycco.xyz/) 入门套餐：9.9 元/月，300G，100M 带宽速度，限 2 台设备。

> [!info] 
> 
> 有限时活动：60 元每年 3000GB 的限量（一次性流量）2000M 带宽速度（[IEPL专线中转](#IEPL)），限 6 台设备，平均算，相当于每月 5.5 元 250G 流量，0.022/G -- 2 分钱 1G，性价比也非常高啊。

#### 起帆云

[起帆Cloud](https://www.qf1.us/) 入门套餐：9.9 元/月，2000G，不限速、不限设备数，极具性价比。

> [!info] 
> 
> 起帆云节点主要在亚太，有香港、台湾、俄罗斯和越南等地的节点。

### 高性价比机场

价低量大，适合长期订阅。缺点：延迟高，特别是晚高峰可能会节点「全红」。

#### 超悦机场

[**超悦**](https://www.chaoyue.shop/#/register?code=2VpoP7sg) 入门套餐有两个：

1. **年付** 12 元（实付 12.96 元，0.96 元是手续费），每月 100G 流量，限速 100M 带宽速度，相当于 1 元/月，相当于 0.01/G -- 1 分钱 1G，性价比应该是最高的了，非常适合长期订阅，流量需求不大的用户。
2. 6.8 元/月，500G，限速 300M，长期订阅但单月流量较大。

超悦使用 [Hysteria1](#Hysteria) 或 [Hysteria2](#Hysteria) 协议。

超悦更适合作为「保底」机场使用，各节点稳定性不如那些 9.9 一个月的机场，但毕竟 1 块钱一个月，还能要求什么呢！

防失联：[超悦官网](https://chaoyue695.github.io)

> [!info] 
> 
> 超悦节点主要在欧洲，最近的只有新加坡和印度的节点。不如 [疾风云](#疾风云) 那样有台湾和香港的节点。不过延迟还行，非高峰时段，都保持大部分节点是绿的，只有偶尔晚高峰，节点会全红！
> 
> 还有，超悦只支持 [Clash](#Clash) 订阅。

#### 鳄鱼机场

> [!info] 域名
> * 永久跳转域名： [https://www.eyujichang.com](https://www.eyujichang.com)
> * 不需要翻墙（适合墙内用户 (临时)）： [https://eyujichang.biz](https://eyujichang.biz)
> * 永久防失联地址 (建议保存,使用墙内用户访问)：[https://eyucdn.github.io](https://eyucdn.github.io)

[鳄鱼](https://www.eyujichang.com) 入门套餐：

1. 年付 12 元/年，每月 100G，100M 速率，不限客户端数
2. 月付 6.8/月，每月 1000G，300M 速率

这跟 [超悦](#超悦机场) 几乎一样。同样属于保底机场特性：价低量大，估计跟 [超悦](#超悦机场) 差不多。不过根据网上测评，这机场有香港、日本和韩国节点，日本、韩国节点走的是 [Hysteria](#Hysteria) 协议，香港节点走的是 [VLess](#VLess) 协议，还行吧。

#### 缥缈墟/凌云阁

[缥缈墟](https://www.pmxu.xyz)

[缥缈墟](https://www.piaomiaoxu.net) 或 [凌云阁](https://dawosima.com) 入门套餐：3 元/月，500G，150M 带宽速度，不限客户端。

> [!tip] 
> 
> **非 IPLC 专线**节点。

> [!important] 
> 
> 已成为中转站！

#### 鸡场

[鸡场](https://鸡场.net) 入门套餐：1.88 元/月，每月 500G，有环大陆节点。

> [!info] 网址
> 
> * 官网入口： [鸡场.net](https://鸡场.net)
> * 国内入口：[鸡场1st.top](https://鸡场1st.top)
> * 国内入口：[鸡场2nd.top](https://鸡场2nd.top)
> * 国内入口：[鸡场3rd.top](https://鸡场3rd.top)
> * 国内入口：[鸡场4th.top](https://鸡场4th.top)

这个机场性价比最合理，**可以月付并且很便宜**，**500G**流量也够大，而且有环大陆节点，不像 [超悦](#超悦机场) 得年付，虽然 12 元一年，平均起来每月更便宜，但要承担更大的机场跑路的风险，并且 [超悦](#超悦机场) 的节点最近都是新加坡，**高峰时期全红机率更大**。当然虽然它的节点都在大陆周边，但延迟相较于 [疾风云](#疾风云) 这些 10 元/月档位的机场而言，会高一些，没办法，一分钱一分货。但这个机场还是**非常适合长期订阅的**。

> [!tip] 
> 
> 这机场的节点使用 [Shadowsocks](#Shadowsocks) 协议，「抗封」能力较低。

#### 一分机场

[一分机场](https://一分机场.com) 入门 套餐：2 元/月，100G，20000M 带宽速度，不限客户端。

不限时套餐：

1. 11.88，100G，20000Mbps 速率，不限时。
2. 19.88，1000G，20000Mbps 速率，不限时。

> [!info] 
> 
> 有香港日本韩国节点，而且延迟也非常不错 -- 闲时，绿的节点很多，晚高峰还有是不少节点是绿的，比 [飞狗](#飞狗)、[Lemon](#Lemon) 要好很多。有 [VLess](#VLess) 和 [Hysteria2](#Hysteria) 两种协议。使用 [whoer.com](https://whoer.com) 来检测，这个机场隐蔽性还是很高的。
>
> 从延迟和协议角度看，2 元月付及 11.88 的 100G 不限时套餐，性价比较好 --2 元月付可以临时用下，而 11.88 的 100G 可以当作长期的补充机场使用。甚至因为它只要 11.88 元，完全可以将它当成小流量的**年付套餐**来用，即每月 8G 流量（比 [养鸡场](#养鸡场) 的 1 元 5G 月付套餐还划算）。
>
> 此机场节点，会用一段时间就会断，得开关下。稳定性不如 [超悦](#超悦机场)。
>
>

#### 三分机场

~~[三分机场](https://shop.sanfen.co)~~

~~>~~
~~> 域名发布页（备用官网域名）： [https://sanfennetwork.github.io/](https://sanfennetwork.github.io/)~~
~~>~~
~~> [三分机场通知频道](https://t.me/s/sanfenjichang)~~

~~> [!tip] ~~
~~>~~
> ~~此机场将逐步将 [直连](#直连) 节点「更换为 [Hysteria2](#Hysteria) 协议」~~

~~1. 年付 9.5 元/年，每月 200G，[直连](#直连) 套餐，不限速不限客户端数， [VLess](#VLess) +[Reality](#Reality) 协议，部分地区可能无法使用（福建福州 泉州 湖南及河南大部分地区此套餐可能无法使用）。~~
~~2. 月付 5 元/月，每月 5000G，[直连](#直连) 套餐，不限速不限客户端数，[VLess](#VLess) +[Reality](#Reality) 协议，部分地区可能无法使用（福建福州 泉州 湖南及河南大部分地区此套餐可能无法使用）。~~
~~> [!quote] ~~
~~> ~~
~~> 三分机场套餐已调整，  ~~
~~> 以下套餐将于 **2024 年 12 月 31 日 23:59:59（UTC+8）** 关闭续费（不可再续费）：  ~~
~~> * VIP1 - 900G / 每年（直连年付套餐）  ~~
~~> * VIP1 - 5000G / 每月（月付套餐）  ~~
~~> * VIP1 - 200G / 每月（年付套餐）  ~~
~~> * VIP1 - 300G / 每月（年付套餐）~~  
~~> * VIP1 - 1T（不限制使用时间）~~

~~因为机场套餐调整，性价比较高的只剩一个了：~~

~~* 季付 8 元/季 200g/月，[Hysteria2](#Hysteria) 协议~~
~~> [!tip] ~~
~~> 
~~> 相当于 2.67 元一个月，不过一买就得买一季，即 3 个月，如果像 [一分机场](#一分机场) 那样没事就掉，那就得「吃屎」3 个月了 -- 当然如果当体验可以试下。~~
~~> ~~
~~> 这个套餐的节点很少，而且只有美国和英国的节点，真心**不推荐**。~~

~~这个机场比 [超悦](#超悦机场) 一样有超值的年付套餐，但更具性价比，年付才**9.5**，每月 200G 流量也比 [超悦](#超悦机场) 大，最重要是它有香港台湾日本这些大陆周边节点。可以是 [超悦](#超悦机场) 的平替。~~

~~5 元月付，流量高达 5000G 不限速套餐，比 [超悦](#超悦机场)6.8 元 500G 还限 300M 速套餐「纸面性价比」更高。~~

~~> [!tip] ~~
~~> ~~
~~> 有网友反映三分卡，估计跟 [超悦](#超悦机场) 一样，主打就一个「保底」，真是一分钱一分货，没办法。~~
~~> ~~
~~> 这机场连购买套餐结算都会断链接，不推荐。~~

> [!tip] 
> 
> 这个机场已经没有性价比套餐了，最便宜是月付 9.9 80G 的。

#### 三毛机场

~~[三毛机场](https://三毛机场.com)~~

> [!info] 
> 
> * 最新国外地区访问域名：[三毛导航.com](https://三毛机场.com) 
> * 最新大陆地区访问域名： [三毛导航.com](https://xn--ehqx35aimmzwv.com/)

[三毛导航](https://三毛导航.com) （[smjcdh](https://www.smjcdh.com)）性价比套餐：

1. 年付，3 元/年，每月 5G，不限客户端数
2. 年付，9.99 元/年，每月 100G，不限客户端数
3. 月付，5 元/月，每月 1200G，不限客户端数

不限时套餐：

25.99/一次性，1000G，送 GPT 账号一个。

> [!tip] 
> 
> 不看节点如何，单看 3 元一年，这价格就很适合当「补充机场」，对于临时性的「补充机场」，每月 5G 完全够用了。
> 
> 主要以 [Hysteria2](#Hysteria) 协议为主，还有部分 [Vmess](#Vmess) 及少数 [Trojan](#Trojan) 协议。 [Hysteria2](#Hysteria) 协议延迟都挺好。
> 
> 节点有香港、日本、韩国和新加坡这些亚洲节点，延迟还不错。
> 
> 其中包括 10 个香港节点（[Hysteria2](https://github.com/apernet/hysteria) 协议的香港节点就有 7 个，另外还有 2 个 Trojan 协议及 1 个 Vmess 协议的香港节点）），3 个日本节点及 2 个韩国节点。
> 
> 至于 9.99 元一年那个年付套餐，就看节点情况如何了，如果能达到 [超悦](#超悦机场) 的延迟，是可以平替 [超悦](#超悦机场) 作为基础机场。
> 
> 
> 此机场禁止使用【美国、加拿大】节点下载 bt。发现直接封号处理。

#### 五毛机场

域名列表：

* **永久官网**：[https://五毛.top](https://五毛.top)
* [https://www.freebb.me](https://www.freebb.me)
* [https://五毛.top](https://五毛.top)
* [https://200900.xyz](https://200900.xyz)

性价比套餐：

* 3 元/月付，500G，10Gbps，不限制设备
* 5 元/月付，1000G，10Gbps，不限制设备

> [!info] 
> 
> 3 元 500G，跟 [鸡场](#鸡场)2 元 500G 相当。
> 
> 节点有香港（有 1 倍和 3 倍的）、台湾、日本、韩国和新加坡，[VLess](#VLess) 协议，速度还行。
> 
> *但这机场竟然不能用自己的代理更新订阅！*
> 

不限时长套餐：

* 24 元，1000G，不限制设备

#### 山水云

永久官网：

* [山水云](山水云.com)
* [山水云机场](山水云机场.com)

性价比套餐：

1. 3 元/月付，50G，全节点 [ss](#Shadowsocks) 中转高速线路
2. 4 元/月付，100G，全节点 [ss](#Shadowsocks) 中转高速线路

不限时套餐：

16 元，150G，全节点 [ss](#Shadowsocks) 中转高速线路

> [!important] 
> 
> 此机场新疆地区切勿购买。

#### FSCloud

[FSCloud](https://dash.996cloud.top) 

网站永久防失联域名：[https://fscloud.vip](https://fscloud.vip)

入门套餐：

1. 年付 13 元/年，每月 100G
2. ~~月付 3 元/月，每月 350G，有流量重置包。~~

有香港台湾节点，[Hysteria2](#Hysteria) 协议。新注册有 3 天 10G 流量试用，还挺好。

这个机场比 [鸡场](#鸡场) 贵点，但协议比 [鸡场](#鸡场) 好，节点延迟也比 [鸡场](#鸡场) 要低不少。

年付套餐跟 [超悦](#超悦机场) 差不多（超悦实际是 12.96 元/年，0.96 是手续费 -- 不知道是什么东西），但节点有香港、台湾等，延迟好像也比 [超悦](#超悦机场) 低点。

> [!info] 
> 
> 从试用的流量节点看，无论是闲时还是晚高峰，各节点延迟都很不错，应该优于 [超悦](#超悦机场)。
> 
> 但节点有时会出现 IP 异常，明明是香港节点，但 youtube 上显示日本的，而且有时延迟非常低，但某些网站会卡。

~~月付 3 元 350G 套餐也比 [超悦](#超悦机场) 的 6.8 套餐 500G 对于单月流量使用没到 500G 的用户来说更合适。~~

> [!info] 
> 
> 总体而言 FSCloud 同样也可以成为 [超悦](#超悦机场) 的平替。

#### 飞狗

[飞狗 ](https://1.mmcks.top) （[新域名](https://na.fgmcks.top)）套餐调整，[直连](#直连) 套餐减少 -- 其实就是变相涨价：

1. 年付 6 元/年，每月 100G，[直连](#直连) 节点，含 [Hysteria2](#Hysteria)，不限客户端数（节点 0.5x 消耗，相当于能使用 200g）
~~2. 季付 6 元/季，每月 500G，港、台 [直连](#直连) 节点，不限客户端数~~
2. 季付 9 元/季，每月 200G，港、台 [直连](#直连)+[中转](#中转) 节点，不限客户端数
3. 月付 5 元/月，每月 500G，港、台 [直连](#直连)+[中转](#中转) 节点，不限客户端数
~~5. 月付 6 元/月，每月 1T，港、台 [直连](#直连)+[中转](#中转) 节点，不限客户端数~~

> [!info] 
> 
> 感觉 9 元/季这个套餐比较合适，相当于 3 元/月，200G，有 [中转](#中转) 节点，「抗封力」及速度都有保证。

还有不限时套餐：

~~1. 5 元，500G[直连](#直连)~~
~~2. 9.9 元，500G[直连](#直连)+[中转](#中转)~~
~~3. 10 元，1T[直连](#直连)~~

1. 6 元，200G [中转](#中转)
2. 12.99 元，500G [中转](#中转)

> [!info] 
> 
> 飞狗节点主要是 [Vmess](#Vmess) 和部分 [Hysteria](#Hysteria) 协议，「抗封力」还不错，但 [直连](#直连) 节点延迟比较高，比 [超悦](#超悦机场) 还高。虽然在晚高峰时段，没像 [超悦机场](#超悦机场) 出现全红现象，但非高峰时段，节点也「保持」半数黄那种，这明显比 [超悦](#超悦机场) 还差。

~~飞狗应该是现在为止性价比最高的机场了 --6 元年付，5 毛钱一个月，真 TMD 牛逼，而且可选套餐也非常多。~~

~~飞狗的不限时套餐非常适合做为其他机场补充。~~

> [!info] 域名
>
> 最新网址：[https://feigou.idsduf.com](https://feigou.idsduf.com)
>  
>  旧网址：
>  
> * [https://2.feigou.xyz](https://2.feigou.xyz)
> * [1.mmcks.top](https://1.mmcks.top) **（需要梯子访问）**

~~#### Lemon~~

~~[Lemon NetWork](https://hi.lmnw1.top) 入门套餐与 [超悦机场](#超悦机场) 一样有两个：~~

~~* 月付 5 元/月，1000G，不限制客户端数量。~~
~~* 年付 12/年，每月 200G，不限制客户端数量。~~

~~不限时套餐：~~

~~6. 7.9/一次性，200G，不限客户端数~~
~~1. 28/一次性，2000G，不限客户端数~~

~~> [!important] ~~
~~> ~~
~~> Lemon 的年付套餐「纸面」上可以作为 [超悦](#超悦机场) 的年付套餐平替。但节点延迟很高，非高峰时段一大半节点不是黄就是红，没几个能用的，比 [飞狗](#飞狗) 还垃圾。建议要入就入不限时套餐。~~
~~> ~~

#### 一元机场

[一元机场](https://一元机场.club) 是一个性价比非常高的机场。

官网入口：

* [一元机场入口1](https://一元机场.club)
* [一元机场入口2](https://一元机场.xyz)
* [一元机场入口3](https://一元机场.art)

> [!tip] 
> 
> 只能用 gamil 注册

性价比套餐：

1. 年付 12 元/年（相当于 1 元/月），每月 50G，不限速不限连接数，节点：香港日本等
2. 季付 15/季（相当于 5 元/月），每月 4000G，不限速不限连接数，节点：香港日本等
3. 月付 7 元/月，每月 8000G，不限速不限连接数，节点：香港日本等

一元机场的 12 元的年付不如 [超悦](#超悦机场)，流量太小了，才 50G。
比较合适的是 12 元的季付套餐，相当于 5 元一个月，4000G 的流量，流量够大。

#### 低价机场

[低价机场](https://低价机场.com) 性价比套餐：

1. 年付 11 元/年，每月 100G，香港日本等节点，[Trojan协议](#Trojan)
2. 月付 7 元/月，每月 1000G，香港日本等节点，[Trojan协议](#Trojan)

年付套餐跟 [超悦](#超悦机场) 的年付差不多，不过这机场用的是 Trojan 协议，而且有香港节点。

月付 7 元套餐跟 [超悦](#超悦机场) 的 6.8 元套餐也半斤八两。

#### 极速机场

[极速机杨](https://极速机场.com) 性价比套餐：

1. 年付，12/年，每月 200G，20000Mbps 速率，不限客户端数
2. 月付，5 元/月，每月 1000G，20000Mbps 速率，不限客户端数

不限时套餐：

* 8 元，不限时 200G，20000Mbps 速率，不限客户端数
* 30 元，不限时 2000G，20000Mbps 速率，不限客户端数

这个机场，年付跟 [超悦机场](#超悦机场)、[低价机场](#低价机场)、[Lemon](#Lemon)、[FSCloud](#FSCloud) 等机场差不多。

这机场有些节点是流量**0.1**倍率，不过跟 [一分机场](#一分机场) 的 0.1 倍率节点一样，延迟太高。另外，这机场还有**1.5**倍率的节点，这些节点速率会比正常的 1 倍率的速度快。主要是 [Hysteria2](#Hysteria) 及 [VLess](#VLess) 协议为主。
> [!tip] 
> 
> 香港节点，有 0.1 倍、1 倍、1.5 倍三种倍率。韩国节点比较多，而且都是 [Hysteria2](#Hysteria) 协议，延迟也非常不错，不过延迟最低的两个韩国节点是 3 倍率的。

但这机场有不限时套餐，其中 8 元 200G 不限时套餐跟 [Lemon](#Lemon)7.9 的 200G 套餐一个档。

#### 顶级机场

[顶级机场](https://顶级机场.com) 性价比套餐：

~~1. 年付，**12** 元/年，每月 **200**G，不限客户端数~~

~~2. **5** 元/月，**1000**G，不限客户端数~~

1. 年付，**15.6** 元/年，每月 **200**G，不限客户端数
2. **6.5** 元/月，**1000**G，不限客户端数

不限时套餐：

* 12 元/一次性，200G，不限客户端数

~~看套餐，跟 [极速机场](#极速机场) 一样。~~

**涨价**了，性价比不如 [极速机场](#极速机场)。

> [!info] 
> 
> 看网上某 up 主推荐视频中，晚高峰，大部分节点都「红」了，果然便宜没好货！

#### NanoCloud

域名：[https://edu.360buyimg.men](https://edu.360buyimg.men)

* 最新官网❶：[https://cloud.52iplc.com](https://cloud.52iplc.com)
* 永久官网❷：[https://nanoapi.github.io](https://nanoapi.github.io)

性价比套餐：

1. 1.00/月付 **100**G， **100**Mbps，支持**2**台设备同时在线

> [!info] 
> 
> 月付 1 元套餐，节点有香港和美国，以 [Vmess](#Vmess) 协议为主。价格应该跟 [超悦](#超悦机场) 等 12 元的年付套餐相近，不过这是可以月付的，抗跑路风险更强了！
> 
> 速度非常不错，不过流量估计是**3x**，虽然没标识。
> 
> 也就是说这个 1 元 100G 套餐，实际流量可能只有 50G 或 30G，顶多也就是相当于 2 元 100G，这与 [鸡场](#鸡场) 的 2 元 100G 一样，但速度和节点数量却是非常不错，可以做临时机场用下，反正 1 块钱也不贵。

#### 杜卡迪

* 杜卡迪官网：[https://www.dukadi.one ](https://www.dukadi.one)
* 国内可访问官网：
	* [https://dukadi.biz](https://dukadi.biz)  (不定时更新)
	* [https://dukadi.info](https://dukadi.info) (2024/12/18)  
	* [https://dukadi.cv](https://dukadi.cv) (2024/12/18)  
	* [https://dukadi.eu](https://dukadi.eu) (2024/12/18)  
	* [https://dukadi.sbs](https://dukadi.sbs) (2024/12/18) 
	* [https://dukadi.shop](https://dukadi.shop) (2024/12/18)

性价比套餐：

* 年付，12 元/年（两年 20 元），每月 200G，100M 速率

又是一个有年付 12 元的机场，同样也是 [Hysteria2](#Hysteria) 协议。

> [!tip] 
> 
> 看 [Youtube](Youtube_Note.md) 上的 up 主测试，节点延迟比 [超悦机场](#超悦机场) 高，唯一优点，有日本、韩国节点。

#### 光速机杨

[✨光速机场](https://gsulink.com)

光速官网：[https://gsgs.nxxbbf.com](https://gsgs.nxxbbf.com)

性价比套餐：

* 6 元/季付，200G/月，1000Mbps 速率，无限制设备
* 12 元/年，120G/月，1000Mbps 速率，无限制设备

不限时套餐：

* 5 元，100G
* 15 元，500G

> [!info] 
> 
> 看起来非常不错。无论是季付、年付，还是不限时套餐。
> 
> 以 5 元不限时 100G 的套餐看，节点有香港、韩国、印度、马来西亚、美国、泰国、巴西、以色列等，[Shadowsocks](#Shadowsocks) 协议。速度还行。
> 

#### 养鸡场

[养鸡场](https://yangjichang.com) 性价比套餐：

1. 1 元/月，每月 5G，1G 速率
~~2. 10 元/季，每月 50G，1G 速率~~
2. 4.99 元/月，每月 50G，1G 速率

不限时套餐：

~~* 20 元/一次性，300G 永久，1G 速率~~
* 34.99/一次性，300G 永久，1G 速率

> [!info] 
> 
> ~~作为「补充机场」，月付 1 元 5G 及 10 元一季，即 3 元一个月，50G，这两套餐还行。季付这个性价比更高，至于 1 元 5G 这个，不如加钱入 [飞狗](#飞狗) 的 1.88 元 500G 套餐，当然节点数及延迟上会差点！~~
> 
> 除了 1 元 5G 套餐外，其他套餐均涨价了！
> 
> 这机场节点主要是东南亚，有日本（有一半是 2 倍率）、香港、新加坡（大部分是 2 倍率）、秦国、印尼（节点都是 2 倍率）、大马。延迟还不错，就是有大量节点是 2 倍率的。跟 [鸡场](#鸡场) 一样，都是使用 [Shadowsocks](#Shadowsocks) 协议。
> 
> 这机场有时会无法访问，有跑路风险，慎入！~~

其他域名：[养鸡场加速器](https://hd.yangjichang.com)

#### 米兰云

[米兰云](https://mirocloud.shop) 入门套餐：

1. 6 元/月，每月 300G，1000M 速率，不限客户端数
2. 10/月，每月 990G，1000M 速率，不限客户端数

米兰云使用 [Hysteria2](#Hysteria) 协议，延迟非常不错，新注册有 2G 试用流量。对延迟有要求的可以用下。6 元/月，小贵，不过延迟低，也就这样了！

不限时套餐：

* 69.9 元，3000G

~~#### 超实惠机场~~

~~[超实惠机场](https://cshjc.top) 入门套餐：3.99 元/月付，150G，不限速，不限客户端数，每人限购一次，就用一个月，这个机场就可以丢了，其他套餐都没有什么性价比！~~

#### 魔戒.net

[魔戒.net](https://www.mojie.cyou) 这个机场所有套餐都是不限时按量付费的。

新域名：[https://mojie.app](https://mojie.app)

性价比套餐：

1. 1 元 2G，不能续费，不限时，不限速度，不限客户端数
> [!tip] 
> 
> 这个很适合，其他机场所有节点都「红」，临时「撑」下
2. 14.9 元 130G，不限时，不限宽速度，不限客户端数
3. 42 元 420G，不限时，不限宽速度，不限客户端数

这机场的节点协议有 [Vmess](#Vmess)、[Trojan](#Trojan)。不过有时会有不正常情况，延迟都挺高，八成节点都「红」了，没几个节点能用。

但日本、香港和台湾的节点非常多，大部分正常情况下，延迟也非常不错。就是价格上不太「美丽」，算是一个优质的不限时的「补充机场」。

#### 雷霆

[雷霆](https://www.leiting.app) ，这个机场非常特殊，它是按流量计费。

> [!info] 
> 
> 网址：
> 
> * [GitHub - 雷霆](https://github.com/winston779/leitingjichang)
> * [www.leiting.io](https://www.leiting.io)
> * [leiting.lol](https://leiting.lol)
> * [lt.uniss.me](https://lt.uniss.me)

首次注册送 3G 流量。

账号余额至少 1 元，最低充值 2 元。

这机场不同用户等级，计费不同，速率也不同：

* 白银：0.3 元 512M，即 0.6 元/G，100M 速率
* 黄金：0.22 元/G，200M 速率
* 白金：0.18 元/G，300M 速率
* 钻石：0.16 元/G，500M 速率
* 黑钻：0.13 元/G，1000M 速率

按白银等极，2 元相当于 3G 流量。

> [!info] 
> 
> 中国大陆周边的节点有香港（4 个）、台湾（2 个）、日本（4 个）、新加坡（4 个），很不错。雷霆使用 [IEPL](#IEPL) 内网以及中国电信 [CN2](#CN2) ，所以延迟非常不错，晚高峰时段，大部分都还是绿的，并且延迟还低，这机场在其他机场都挂了，它还能用。不过节点是 [Shadowsocks](#Shadowsocks) 协议，有点老。

综合言之，这机场，真是非常适合做**临时机场**。

> [!important] 
> 
> 雷霆这个机场的订阅链接中的 `token` 值是随机产生的，有**3 分钟**的时效性，所以得及时导入到 [客户端](#客户端) 中。

### 专线机场

#### CloudFisher

[CloudFisher](https://cloudfisher.net) 是一个 [IEPL](#IEPL) 专线的机场。

* 9 元/月，每月 120G，300M 速率。

不限时套餐：

* 40 元/一次性，300G，600M 速率

> [!info] 
> 
> ![cloudfisher screenshot | 800x600](https://t.tutu.to/img/mOcjQ)
> 
> 看上述几个套餐，如果是专线的话，5 元 150G，性价比还不错。如果要与其他机场搭配使用，建议还是入手不限时 35 元 300G，就当是其他直连机场在高峰时段「全红」后的保底。

### 恶臭机场

#### 十元一年

[十元一年](https://go.10520.click) 性价比套餐：

* 年付，9.9/年，每月 100G，限 3 个客户端，50m 速率

不限时套餐：

* 69.9 元/一次性，1,000,000G，不限速，限 3 个客户端

> [!info] 
>
> 年付勉强还行，但速率太慢，才 50m，太扣门。不限时 69.9 元，一百万 G，挺唬人，估计流量没用完就跑路了，而且还限客户端，也是很扣门。得加群才有 3 元永久 20G 流量，太麻烦。
> 
> 最奇葩的还有一个厦套餐：
> 
> ```txt
> 账号保留
> ¥3.00 /一次性
>
> 站内会不定时清理无套餐用户
> 请各位推广人使用此套餐进行账号保留
>
> 注意：没有任何套餐的账号被清除后将没有任何渠道进行找回！ 
> ```
>
> 帮你推广还得自己掏钱保号，看门狗自己花钱买骨头，简直就是机场界的恶臭！
> 
> 
> 而且从注册送的 1G 流量看节点情况，有日本、韩国、香港和新加坡节点，但包括这些节点在内，所有的节点半数都是黄的，延迟太高。

#### Needay

[Needay](https://needay.xyz) 

官网域名：

* [直连域名](https://needay04.729388.xyz)
* [需代理域名](https://needay03.729388.xyz)

订阅域名 2024 年 12 月 6 日更新： 

* https://service03.needay.xyz
* https://service02.needay.xyz
* https://service01.needay.xyz

性价比套餐有蛮多款：

~~1. 月付 3.99/月，每月 1000G，直连，不限客户端数~~
~~2. 年付 49/年，每月 1TB，直连，不限客户端数~~

* 月付 4.99/月，每月 1000G
* 年付 15/年，每月 300G

不限时套餐：

~~* 9 元，1000G，直连~~
~~* 50 元，9TB，直连~~

* 13 元，1000G，[Hysteria2](#Hysteria) 节点，部分 [CN2](#CN2) 线路、部分 AS0020 线路、部分移动 CMI 线路。

> [!info] 
> 
> ~~9 元，1000G，相当 1 元 111G，这比 [飞狗](#飞狗)5 元 500G 的不限时套还划算。~~
> 
> ~~但节点，香港节点，最低倍率是 3 个倍。1 倍率的美国节点，延迟非常高，这是个坑。偶尔会出现 0.5~1 倍节点，也不知道是不是长期存在的这节点。反正这机场坑很多。~~
> 

> [!important] 
> 
> 没事就停用账号，随时有跑路的危险，千万不要使用此机场！

---

## 客户端

> [!info] 
> 
> * [【2024年】V2rayN 和 Clash 客户端哪个好用？机场小白应该如何选择？ – 胖橙博客](https://jiasupanda.com/v2rayn-clash)

### Clash

#### Clash-Meta

#Clash/Mihomo 
 
[Clash Meta](https://github.com/MetaCubeX/mihomo)（现更名为 *Mihomo* ）是一个基于广受欢迎的开源项目 Clash 的高级版本。它继承了 Clash 的核心功能，保留了原始 Clash 的灵活性和高效性，并增加了一些独特的特性，并包括部分 Clash Premium 核心功能，是目前网络代理和数据流管理最强大的软件。 ^mihomo

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

* 在疾风云的「**用户中心**」，点击「复制 Clash 订阅链接」，将 Clash 链接复制到剪切板中。
* 在 Clash-Verge「*订阅*」页面中「**导入**」刚才复制的「Clash 订阅链接」。
* 在 Clash-Verge「*代理*」页面，选择合适的「节点」。
> [!info] 
> 
> 建议使用「规则」下的节点，因为它会对国内网站进行「过滤」，让国内网站直链不走代理；如果使用「全局」，那国内网站也走代理，除非你使用 [PAC](#PAC) 模式进行过滤；
* 在 Clash-Verge「*设置*」页面，开启「**系统代理**」，这就能「出海」了，如果不用代理，就把这项关闭就好了！

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

![clash-verge-rev logo |80x80](https://github.com/clash-verge-rev/clash-verge-rev/raw/main/src-tauri/icons/icon.png)

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
		"www2.bing.com",
		"cn.bing.com",
		"tv.cctv.com",
		"www.dangdang.com",
		"www.zol.com.cn",
		"www.miguvideo.com",
		"www.smzdm.com",
		"www.zdic.net",
		"mirrors.tuna.tsinghua.edu.cn",
		"www.shuge.org",
		"www.tmp.link",
		"www.lingshulian.com",
		"www.deepseek.com",
		"linux.do",
		"www.e-daixia.com"

		// 影视
		// "update.code.visualstudio.com"
		// "yingshi.tv",
		// "oss.yingshi.tv",
		// "api.yingshi.tv",
		// "api.3qi.live",
		// "heimuer.tv",
		// "m3u8.heimuertv.com",
		
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
> `update.code.visualstudio.com` 这是 [VSCode](../Editors/VSCode_Note.md) 插件安装更新，按需添加或注释。
>  
>  相关文档
> 
> * [代理自动配置文件（PAC）文件 - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Proxy_servers_and_tunneling/Proxy_Auto-Configuration_PAC_file)

### V2ray

[V2Ray](https://www.v2ray.com/) 是 ProjectV 下的一个工具。V2Ray 是一个与 Shadowsocks 类似的代理软件。

V2Ray 本身不支持 [PAC](#PAC)。

### FLClash

[FLClash](https://github.com/chen08209/FlClash) 也是一个基于 [Clash-Meta](#Clash-Meta) 的多平台客户端。

#### 设置

下载好了按照这个步骤操作点开 「工具」>「覆写」>「基础」> 把第二个「UA」改成 `clash-verge/v1.6.6`

FLClash 的端口默认为 `7890`，这软件连复制环境变量的功能都没有，只能手动敲以下代码在 [terminal](../Linux/Linux_Note.md#terminal%20相关) 中执行：

```shell
export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890
```

> [!info] 
> 
> 因为 FLClash 是基于 [Clash-Meta](#Clash-Meta)，所以机场的订阅时得注意是否支持。
> 
> [Clash-Meta](#Clash-Meta) 与通用订阅链接相比，只是后面多了个参数：`&flag=clashmeta`

### Mihomo-Party

[Mihomo Party](https://mihomo.party)[Mihomo](#^mihomo) 的客户端。

[Mihomo Party github](https://github.com/mihomo-party-org/mihomo-party) 

#### 安装

```shell
# 源码包
paru/yay -S mihomo-party
# 二进制包
paru/yay -S mihomo-party-bin
# Git 版本源码包
paru/yay -S mihomo-party-git
# 使用系统electron的源码包
paru/yay -S mihomo-party-electron
# 使用系统electron的二进制包
paru/yay -S mihomo-party-electron-bin
```

#### 设置

##### 代理模式

Mihomo-Party 同样也能开启「PAC 模式」并设置，与 [Clash-Verge-Rev](#Clash-Verge-Rev) 的 [PAC](#PAC) 设置完全一致。

#### 问题

##### 规则数据库权限

`.config/mihomo-party/work/rules` 这个目录下是存放着外部资料，规则数据库文件，就是 [geoip](#geoip)、[geosite](#geosite) 这些。有时不知道为什么，会出现权限问题，使用 `ls` 命令可以看到有的库文件所有者变成 `root`，这就造成更新这库时，现出权限问题。

解决这个问题方法很简单，直接删除 `rules` 这个目录。重启 Mihomo-Party，它就会重新生成一个新的 `rules` 目录，这时再使用 `ls` 命令查看，就会发现库文件的所有者就*变回*到当前用户，这就没有权限问题了！

### V2Fly

因为 [V2ray](#V2ray) 的创始人失联，V2Ray 的更新已经由新的社区 [V2Fly](https://www.v2fly.org) 负责。

#### XRay

XRay 是 Project X 项目下的一个核心工具也就是现在的 [Xray-core](https://github.com/XTLS/Xray-core)，和 V2Ray-Core 类似主要负责网络协议、路由等网络通信功能的实现，但在功能上是 V2Ray-Core 的超集，[VLess](#VLess) 两者都有，但是 XTLS 只有 XRay 支持。

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

> [!info] 
> 
> WARN: v2raya@.service was deprecated; please use user service v2raya-lite.service instead.
> 
> This does NOT impact v2raya.service users.

#### v2ray 规则设置

[v2ray-rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat) 是路由规则文件加强版，可代替 V2Ray 官方 `geoip.dat` 和 `geosite.dat`。

[v2ray-china-list](https://github.com/dctxmei/v2ray-china-list) 是中国域名列表，用于在规则中过滤掉中国网站，让其直链。

### Hiddify

[Hiddify](https://github.com/hiddify/hiddify-next) 是一个多平台的客户端。能同时支持 [Clash](#Clash) 和 [V2ray](#V2ray) 在内的多种订阅格式。

#### 设置

「直链 DNS」那里设置下，如使用 `114.114.115.115`。

### SurfBoard

[surfboard](https://github.com/getsurfboard/surfboard) 是安卓端的网络流量处理工具。

---

## 系统相关设置

### 设置临时代理

Linux 参考：[临时代理](../Linux/Linux_Note.md#临时代理)

当然不想手动设置，如果使用了 [Clash](#Clash) 的客户端，可以右键图标，下拉菜单中有「复制环境变量」，就能复制要设置的环境变量，把复制的值粘到终端中执行下就好了。

而环境变量的类型，在「设置」面板中「复制环境变量类型」中可选。

以 Bash 类型认例，复制出的环境变量是这样的：

```shell
export https_proxy=http://127.0.0.1:7897 http_proxy=http://127.0.0.1:7897 all_proxy=socks5://127.0.0.1:7897
```

> [!info] 相关资料
> 
> * [终端使用代理加速的正确方式（Clash） | Ln's Blog](https://weilining.github.io/294.html)
> * [终端使用代理 - faf4r - 博客园](https://www.cnblogs.com/faf4r/p/17765134.html)
> * [在 Linux 中使用 Clash | AISYUN's Blog](https://blog.cyida.com/2023/32KRQRV.html#%E4%BD%BF%E7%94%A8%E4%BB%A3%E7%90%86)

---

## 相关链接

* [clashios爱好者](https://clashios.com/)
* [clashverge.org](https://clashverge.org/)
* [whoer.net](https://whoer.net)
* [fanqiangdang](https://fanqiangdang.org)
* [ACL4SSR 在线订阅转换](https://acl4ssr-sub.github.io)
* [Subscription Converter](https://api.nameless13.com)

### 机场汇集

* [2025年稳定好用的机场推荐（2025-2更新） - Kerry的学习笔记](https://kerrynotes.com/best-ssr-v2ray-proxy/)
* [2025年性价比机场推荐（持续更新）](https://kerrynotes.com/cost-effective-ss-proxy/)
* [2025年最新性价比机场推荐](https://github.com/KaWaIDeSuNe/xingjiabijichang)
* [GitHub - KaWaIDeSuNe/dijiajichang: 2025最新低价机场推荐](https://github.com/KaWaIDeSuNe/dijiajichang)
* [2025最新低价机场推荐](https://github.com/KaWaIDeSuNe/dijiajichang)
* [性价比机场测速 - by Duang](https://duangks.com)

#### 跑路机场

* [科学上网🕸️之跑路机场名单收集（2020-2025）](https://github.com/limbopro/Paolujichang)

---

## 相关笔记

* [梯子资料](Ladder_Material.md)
* [Youtube 笔记](Youtube_Note.md)
* [搭配方案](搭配方案.md)
* [网络笔记](../Network/Network_Note.md)
