---
aliases: []
tags:
  - chrome
  - browser
  - google
  - plugin
created: 2023-01-29 09:21:20
modified: 2025-10-05 01:41:47
---

# Chrome 浏览器笔记

---

## 相关配置

### Cookie

#### 存放位置

Linux 下 Chrome 浏览器 Cookies 文件是存放在：`.config/xxx/Default/Cookies`
> [!tip] 不同 chrome 浏览器具体位置
> 
> 如果是 chrome 浏览器 `xxx` 就是 `chrome`，而如果是微软的 edge 浏览器，则变为 `microsoft-edge`
>
> * `.config/chrome/Default/Cookies`
> * `.config/microsoft-edge/Default/Cookies`
> * `.config/chromium/Default/Cookies`

## <span id="chrome_plugins">插件</span>

### 常用插件

#### EditThisCookie

EditThisCookie 这个插件是可以编辑导入导出 Cookies。

##### 导出

导出格式也可以根据需求设置，如像 [lux](https://github.com/iawia002/lux) 下载视频时，需要指定 Cookies 文件，它就需要使用「Netscape HTTP Cookie」格式，所以就在导出前设置下相应的导出格式。

> [!tip] 
> 
> 这插件所谓导出，其实就是将 Cookie 内容复制到临时「粘贴板」上，所以需要「导出」Cookie 文件，其实是得自己新建文件，然后将「粘贴板」上的 Cookie 数据复制到新建的文件中。

### 插件相关网站

* [极简插件](https://chrome.zzzmh.cn/)

---
### <span id="chrome_plugins_issue">插件相关问题</span>

#### <span id="chrome_plugins_issue_relocation">插件迁移</span>

Chrome 的插件能用在 Chrome 系浏览器上，但如果反过来，[Edge] 中有一些好用的插件，而这些插件是 Edge 「特供」的，想要迁移到 Chrome 或 Chrome 系其他浏览器上，又如何实现？

这是件有趣的事情。因为最近发现 Edge 上有多款「梯子」软件非常好用，而 Edge 是有自己的扩展库的，所以国人是能装上插件，而如果使用原版 Chrome 或 Chrome 系其他没有本地化插件库的浏览器时，安装插件就变得非常麻烦的事情。其他常用插件还能通过国内各大 Chrome 插件网站进行安装，虽然版本有可能旧点，但毕竟还是能装上用的，但像「梯子」插件这种极度敏感的插件，那些插件网站根本不敢摆出来，而 Edge 的扩展库偏偏就有几款，而且还是能用的那种，虽然碰到「特殊节日」一样被「封印」，但大多数情况还是可能使用的，那这些好用的「梯子」只能「困」在 Edge 这池子里，显然是「屈才」了，我就想将它们弄到 Chrome 及 Chrome 系其他浏览器上，所以就研究了一下，大概总结出一个大致的迁移方法。

##### 获取插件目录路径

首先，要「迁移」插件，得先知道它们被存在在哪。这就涉及到插件存在目录。

而 Chrome 系浏览器的插件存放目录是在「用户目录下」，那进一步就得知道「用户目录」的路径。

在地址栏中输入：`chrome://version/`，显示出当前 Chrome 浏览器的版本信息。

在这堆信息中，其中 **个人资料路径** 中的路径是我们需要的。
如：`F:\scoop\locals\apps\googlechrome\current\User Data\Default` 

> [!tip]
> 这个就是我的 Chrome 浏览器的「用户目录」，用户数据都保存在这个目录中。
> 当然例子中的的路径，不是一般情况会出现的，一般用户的 Chrome 用户目录应该是这样的：
>
> `C:\User名\AppData\Local\Chrome\User Data\Default`
>
>  又如 Edge： `C:\Users\xxx\AppData\Local\Microsoft\Edge\User Data\Default`
> 
> 另外，在 Linux 系统中，用户目录是放在 `.config` 目录中

> [!tip] Linux 用户目录路径示例
> `~/.config/chromium/Default`
> 
> `~/.config/microsoft-edge/Default`
> 
> `~/.config/vivaldi/Default`

而插件或扩展数据就存放在用户数据目录下的 `Extensions` 子目录中。

> [!example] 查看 Windows 下 Chrome 插件目录
> ```
> # silascript @ PC-202107291101 in ~: [07:20:44]
>$ ls 'F:\scoop\locals\apps\googlechrome\current\User Data\Default\Extensions'
> Directory: F:\scoop\locals\apps\googlechrome\current\User Data\Default\Extensions
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>d----           2023/1/29     6:39                ncennffkjdiamlpmcbajkmaiiiddgioo
>d----           2023/1/29     6:39                ngpampappnmepgilojfohadhhmbhlaek
> 
> ```
>

上面的例子中，插件目录存放有两个插件。最后那一长串东西是这个插件的 **ID**，这是这个插件的唯一标识，同时它也是这个插件的数据目录名，而这个插件目录下，是这插件不同版本的目录，如下面这下例子：

> [! example] 查看插件目录
> ```
> ls 'F:\scoop\locals\apps\googlechrome\current\User Data\Default\Extensions\ncennffkjdiamlpmcbajkmaiiiddgioo\'
>    Directory: F:\scoop\locals\apps\googlechrome\current\User Data\Default\Extensions\ncennffkjdiamlpmcbajkmaiiiddgioo
> Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>d----           2023/1/29     6:39                3.32_0
> 
>```
> 

这个插件里只有一个 3.32 版本。

知道了插件目录结构后，要想插件迁移并安装成功，还要知道两个事情：
1. 要「迁移」某插件时，得把那个插件 ID 为目录名的目录拷到目录浏览器 `Extensions` 目录下
2. 在「目标浏览器」安装目录时，指定的插件得指定到版本号那个目录

##### 查看插件 ID

通过上面插件目录结构的查询，可以知道，要迁移插件，得知道插件具体的 ID。

1. 到扩展程序页面，点击相要迁移的插件的「详情」或「详细信息」
2. 正常情况，这个插件的详细信息是不包括该插件的 ID，要想看到该插件 ID，就得打开「开发者模式」

##### 迁移及安装插件

上面得到了插件的 ID，只要把该插件 ID 拼接到 `Extensions` 路径后，就得到了该插件的路径，这样就能将这插件复制到目标浏览器的 `Extensions` 目录下。

> [!example] 复制插件
> ```shell
> cp 'C:\Users\silascript\AppData\Local\Microsoft\Edge\User Data\Default\Extensions\lpmhmbogdfoddanepgnbngfeofdjdnge\' -Recurse 'F:\scoop\locals\apps\googlechrome\current\User Data\Default\Extensions\' -Force
>```
> 

吐槽下，PowerShell 真难用，命令恶心了！

###### 安装插件

复制成功后，就到了安装插件的环节了。要想安装 `Extensions` 目录中的插件，也得打开 `开发者模式`，这时，会看到扩展页面多出一个「加载已解压的扩展程序」的按钮，点击这按钮，进入目录插件的目录中，如果复制成功，应该看到还有版本号作目录名的子目录，再选择一个版本目录 -- 自然选最新的，最后点击「选择文件夹」按钮确定，正常情况就会加载进扩展页面，这就安装成功了！

---

## 问题

### 全局快捷键

某版本 chrome 会在启动浏览器后弹出全局快捷键设置的窗口，很烦，可以在启动中加入参数禁止这窗口弹出：`--disable-features=GlobalShortcutsPortal`

如果是使用 `desktop`，即使用桌面快捷图标的方便启动浏览器，可以在 `desktop` 中 `Exec` 启动项中添加参数，达到实现禁止全局快捷键设置窗口弹出。但 chrome 的 desktop 中可能会根本不同平台有设置不同的启动，得设置到正确的启动项，以 [Edge](Browser_Note.md#Edge) 浏览器为例：

```
# silascript @ (base) in ~ [18:27:11] 
$ cat /usr/share/applications/microsoft-edge.desktop
[Desktop Entry]
Version=1.0
Name=Microsoft Edge
# Only KDE 4 seems to use GenericName, so we reuse the KDE strings.
# From Ubuntu's language-pack-kde-XX-base packages, version 9.04-20090413.
GenericName=Web Browser
GenericName[ar]=متصفح الشبكة
GenericName[bg]=Уеб браузър
GenericName[ca]=Navegador web
GenericName[cs]=WWW prohlížeč
GenericName[da]=Browser
GenericName[de]=Web-Browser
GenericName[el]=Περιηγητής ιστού
GenericName[en_GB]=Web Browser
GenericName[es]=Navegador web
GenericName[et]=Veebibrauser
GenericName[fi]=WWW-selain
GenericName[fr]=Navigateur Web
GenericName[gu]=વેબ બ્રાઉઝર
GenericName[he]=דפדפן אינטרנט
GenericName[hi]=वेब ब्राउज़र
GenericName[hu]=Webböngésző
GenericName[it]=Browser Web
GenericName[ja]=ウェブブラウザ
GenericName[kn]=ಜಾಲ ವೀಕ್ಷಕ
GenericName[ko]=웹 브라우저
GenericName[lt]=Žiniatinklio naršyklė
GenericName[lv]=Tīmekļa pārlūks
GenericName[ml]=വെബ് ബ്രൌസര്‍
GenericName[mr]=वेब ब्राऊजर
GenericName[nb]=Nettleser
GenericName[nl]=Webbrowser
GenericName[pl]=Przeglądarka WWW
GenericName[pt]=Navegador Web
GenericName[pt_BR]=Navegador da Internet
GenericName[ro]=Navigator de Internet
GenericName[ru]=Веб-браузер
GenericName[sl]=Spletni brskalnik
GenericName[sv]=Webbläsare
GenericName[ta]=இணைய உலாவி
GenericName[th]=เว็บเบราว์เซอร์
GenericName[tr]=Web Tarayıcı
GenericName[uk]=Навігатор Тенет
GenericName[zh_CN]=网页浏览器
GenericName[zh_HK]=網頁瀏覽器
GenericName[zh_TW]=網頁瀏覽器
# Not translated in KDE, from Epiphany 2.26.1-0ubuntu1.
GenericName[bn]=ওয়েব ব্রাউজার
GenericName[fil]=Web Browser
GenericName[hr]=Web preglednik
GenericName[id]=Browser Web
GenericName[or]=ଓ୍ବେବ ବ୍ରାଉଜର
GenericName[sk]=WWW prehliadač
GenericName[sr]=Интернет прегледник
GenericName[te]=మహాతల అన్వేషి
GenericName[vi]=Bộ duyệt Web
# Gnome and KDE 3 uses Comment.
Comment=Access the Internet
Comment[ar]=الدخول إلى الإنترنت
Comment[bg]=Достъп до интернет
Comment[bn]=ইন্টারনেটটি অ্যাক্সেস করুন
Comment[ca]=Accedeix a Internet
Comment[cs]=Přístup k internetu
Comment[da]=Få adgang til internettet
Comment[de]=Internetzugriff
Comment[el]=Πρόσβαση στο Διαδίκτυο
Comment[en_GB]=Access the Internet
Comment[es]=Accede a Internet.
Comment[et]=Pääs Internetti
Comment[fi]=Käytä internetiä
Comment[fil]=I-access ang Internet
Comment[fr]=Accéder à Internet
Comment[gu]=ઇંટરનેટ ઍક્સેસ કરો
Comment[he]=גישה אל האינטרנט
Comment[hi]=इंटरनेट तक पहुंच स्थापित करें
Comment[hr]=Pristup Internetu
Comment[hu]=Internetelérés
Comment[id]=Akses Internet
Comment[it]=Accesso a Internet
Comment[ja]=インターネットにアクセス
Comment[kn]=ಇಂಟರ್ನೆಟ್ ಅನ್ನು ಪ್ರವೇಶಿಸಿ
Comment[ko]=인터넷 연결
Comment[lt]=Interneto prieiga
Comment[lv]=Piekļūt internetam
Comment[ml]=ഇന്റര്‍‌നെറ്റ് ആക്‌സസ് ചെയ്യുക
Comment[mr]=इंटरनेटमध्ये प्रवेश करा
Comment[nb]=Gå til Internett
Comment[nl]=Verbinding maken met internet
Comment[or]=ଇଣ୍ଟର୍ନେଟ୍ ପ୍ରବେଶ କରନ୍ତୁ
Comment[pl]=Skorzystaj z internetu
Comment[pt]=Aceder à Internet
Comment[pt_BR]=Acessar a internet
Comment[ro]=Accesaţi Internetul
Comment[ru]=Доступ в Интернет
Comment[sk]=Prístup do siete Internet
Comment[sl]=Dostop do interneta
Comment[sr]=Приступите Интернету
Comment[sv]=Gå ut på Internet
Comment[ta]=இணையத்தை அணுகுதல்
Comment[te]=ఇంటర్నెట్‌ను ఆక్సెస్ చెయ్యండి
Comment[th]=เข้าถึงอินเทอร์เน็ต
Comment[tr]=İnternet'e erişin
Comment[uk]=Доступ до Інтернету
Comment[vi]=Truy cập Internet
Comment[zh_CN]=访问互联网
Comment[zh_HK]=連線到網際網路
Comment[zh_TW]=連線到網際網路
Exec=/usr/bin/microsoft-edge-stable %U --disable-features=GlobalShortcutsPortal
StartupNotify=true
Terminal=false
Icon=microsoft-edge
Type=Application
Categories=Network;WebBrowser;
MimeType=application/pdf;application/rdf+xml;application/rss+xml;application/xhtml+xml;application/xhtml_xml;application/xml;image/gif;image/jpeg;image/png;image/webp;text/html;text/xml;x-scheme-handler/http;x-scheme-handler/https;
Actions=new-window;new-private-window;

[Desktop Action new-window]
Name=New Window
Name[am]=አዲስ መስኮት
Name[ar]=نافذة جديدة
Name[bg]=Нов прозорец
Name[bn]=নতুন উইন্ডো
Name[ca]=Finestra nova
Name[cs]=Nové okno
Name[da]=Nyt vindue
Name[de]=Neues Fenster
Name[el]=Νέο Παράθυρο
Name[en_GB]=New Window
Name[es]=Nueva ventana
Name[et]=Uus aken
Name[fa]=پنجره جدید
Name[fi]=Uusi ikkuna
Name[fil]=New Window
Name[fr]=Nouvelle fenêtre
Name[gu]=નવી વિંડો
Name[hi]=नई विंडो
Name[hr]=Novi prozor
Name[hu]=Új ablak
Name[id]=Jendela Baru
Name[it]=Nuova finestra
Name[iw]=חלון חדש
Name[ja]=新規ウインドウ
Name[kn]=ಹೊಸ ವಿಂಡೊ
Name[ko]=새 창
Name[lt]=Naujas langas
Name[lv]=Jauns logs
Name[ml]=പുതിയ വിന്‍ഡോ
Name[mr]=नवीन विंडो
Name[nl]=Nieuw venster
Name[no]=Nytt vindu
Name[pl]=Nowe okno
Name[pt]=Nova janela
Name[pt_BR]=Nova janela
Name[ro]=Fereastră nouă
Name[ru]=Новое окно
Name[sk]=Nové okno
Name[sl]=Novo okno
Name[sr]=Нови прозор
Name[sv]=Nytt fönster
Name[sw]=Dirisha Jipya
Name[ta]=புதிய சாளரம்
Name[te]=క్రొత్త విండో
Name[th]=หน้าต่างใหม่
Name[tr]=Yeni Pencere
Name[uk]=Нове вікно
Name[vi]=Cửa sổ Mới
Name[zh_CN]=新建窗口
Name[zh_TW]=開新視窗
Exec=/usr/bin/microsoft-edge-stable

[Desktop Action new-private-window]
Name=New InPrivate Window
Name[ar]=نافذة InPrivate جديدة
Name[bg]=Нов прозорец InPrivate
Name[bn]=নতুন InPrivate উইন্ডো
Name[ca]=Finestra InPrivate nova
Name[cs]=Nové okno InPrivate
Name[da]=Nyt InPrivate-vindue
Name[de]=Neues InPrivate-Fenster
Name[el]=Νέο παράθυρο InPrivate
Name[en_GB]=New InPrivate Window
Name[es]=Nueva ventana InPrivate
Name[et]=Uus InPrivate-aken
Name[fa]=پنجره InPrivate جدید
Name[fi]=Uusi InPrivate-ikkuna
Name[fil]=Bagong InPrivate Window
Name[fr]=Nouvelle fenêtre InPrivate
Name[gu]=નવી InPrivate વિંડો
Name[hi]=नई InPrivate विंडो
Name[hr]=Novi prozor InPrivate
Name[hu]=Új InPrivate-ablak
Name[id]=Jendela InPrivate Baru
Name[it]=Nuova finestra InPrivate
Name[iw]=חלון InPrivate חדש
Name[ja]=新しい InPrivate ウィンドウ
Name[kn]=ಹೊಸ InPrivate ವಿಂಡೋ
Name[ko]=새로운 InPrivate 창
Name[lt]=Naujas „InPrivate“ langas
Name[lv]=Jauns InPrivate logs
Name[ml]=പുതിയ InPrivate ജാലകം
Name[mr]=नवीन InPrivate विंडो
Name[nl]=Nieuw InPrivate-venster
Name[no]=Nytt InPrivate-vindu
Name[pl]=Nowe okno InPrivate
Name[pt]=Nova Janela InPrivate
Name[pt_BR]=Nova Janela InPrivate
Name[ro]=Fereastră InPrivate nouă
Name[ru]=Новое окно InPrivate
Name[sk]=Nové okno InPrivate
Name[sl]=Novo okno InPrivate
Name[sr]=Нови InPrivate прозор
Name[sv]=Nytt InPrivate-fönster
Name[ta]=புதிய InPrivate சாளரம்
Name[te]=కొత్త InPrivate విండో
Name[th]=หน้าต่าง InPrivate ใหม่
Name[tr]=Yeni InPrivate Penceresi
Name[uk]=Нове вікно InPrivate
Name[vi]=Cửa Sổ InPrivate Mới
Name[zh_CN]=新建 InPrivate 窗口
Name[zh_TW]=新 InPrivate 視窗
Exec=/usr/bin/microsoft-edge-stable --inprivate
```

> [!info] 
>
> 在上面的 `desktop` 中配有多个启动项，其中以 `[Desktop Entry]` 这块配置为主，`[Desktop Action new-window]` 与 `[Desktop Action new-private-window]` 对应的是，edge 的两个动作：`新建窗口` 及 `新建无痕窗口`。
>
> 一般设 `Exec=/usr/bin/microsoft-edge-stable %U --disable-features=GlobalShortcutsPortal` 就可以了。因为 `新建窗口` 及 `新建无痕窗口` 这两个操作都是启动了浏览器后才做的，而「全局快捷键设置」这个弹窗是在启动浏览器后才会出现的「**Bug**」。
> 
> 设置完 `desktop`，最好 `update-desktop-database` 下。
> 

---

## 相关笔记

* [浏览器笔记](Browser_Note.md)
* [浏览器资料清单](Browser_Material.md)

