---
aliases:
  - 
tags:
  - PL
  - java
  - jdk
  - Eclipse
created: 2023-01-13, 12:27:45
modified: 2023-01-30, 9:15:30
---

# Java 笔记

---
## 目录

* [Java相关的配置](#java_config)
  * [Eclipse相关](#java_eclipse)
* [SDKMan](#java_sdkman)
* [Lambda 相关](#java_lambda)

---

## <span id="java_jdk">JDK</span>

不同的版本有不同的 JDK。而且不同厂商也有不同的 JDK。而非 [Oracle](https://www.oracle.com/) 的 JDK 均源于「Open JDK」。

### JDK8 

新的版本 JDK8 的部分命令更改。

`java` 及 `javac` 查看版本信息的 ` --version` 选项已经没有了，只剩下 `-version` 方式可查看 jdk8 的版本信息。

所以在新版本 jdk8 中查看版本信息只能使用：`java -version` 和 `javac -version`。

但离奇的是，在 [JDK11](#JDK11) 及 [JDK17](#JDK17) 仍保持 `--version` 这种用法。猜测是想要把 java 8 跟后继的版本「区隔」开，而且给那些仍使用 java8 的「钉子户」程序员一个好像 java8 用不了的「错觉」。

> [!info] 关于 version 选项
> 通过 java 的帮助可以知道两种 `version` 是有一点区别的。
> 
> `-version` 将产品版本输出到错误流并退出
> 
>  `--version` 将产品版本输出到输出流并退出

### JDK11

### JDK17

## <span id="java_config">Java 相关的配置</span>

---

### Java11 生成 JRE

进入 jdk 安装目录后执行以下命令：

```shell
sudo ./bin/jlink --module-path jmods --add-modules java.desktop --output jre
```

>把 jmodes 目录所有模块都生成 jre:
>
>```shell
>sudo ./bin/jlink --module-path jmods --add-modules ALL-MODULE-PATH --output jre
>```
>
>

---

## <span id="java_sdkman">SDKMan</span>

[SDKMan](https://sdkman.io/) 是一个 Linux 系统中对 java 相关的软件管理器。它能够对软件安装、卸载、版本切换。

### SDKMan 安装

执行以下命令：
```shell
curl -s "https://get.sdkman.io" | bash
```

### SDKman 使用

使用 `sdk help` 能查看 SDKMan 帮助，这个帮助已经涵盖了 SDKMan 几乎所有常用操作。

SDKMane 语法 `sdk <command> [candidate] [version]`。

* `command`：命令
	* 最常用的命令：`list`、`install`、`uninstall`、`use`、`default`
* `candidate`：目标软件
* `version`：目标软件的版本号

#### list 命令

`list` 命令是用来浏览软件所有列表

`sdk list`：什么软件名都不带的，就是列出 SDKMan 可以装哪些软件，使用 `J`、`K` 上下滚动，`q` 离开列表浏览模式。

`sdk list 软件名`：列出此软件所有版本。

以 `sdk list java` 为例：
![sdkman list 1](./Java_Note.assets/sdkman_list_1.png)

可以从截图看出列出了 SDKMan 包含的 JDK 版本信息。

`Status` 栏的信息一般是表明这个版本是已安装（「installed」），亦或是本地包（「local only」）。

`Use` 栏是表示此软件使用情况。`>` 表示当前正在使用，`*` 表示已经安装，`+` 表示本地安装包。

`Version` 栏是版本号。 

`Dist` 栏是版本发行商。

`Identifier` 栏是标识符，用于安装时指定的。就是 `sdk <command> [candidate] [version]` 命令语法中最后那个 `version` 的值。

#### install 命令

---

### <span id="java_eclipse">Eclipse 相关</span>

#### <span id="java_eclipse_hotkeys">Eclipse 快捷键</span>

`Ctrl+M`：最大化当前视图，看焦点在哪里哪里就最大化。

`Ctrl+W`：关闭当前窗口

`Ctrl+Shift+W`：关闭所有窗口

`Ctrl+Shift+F`：格式化代码

`Ctrl+/`：单行注释或取消注释

`Ctrl+Shift+/`：多行注释

`Ctrl+Shift+\`：取消行注释

`Ctrl+O`：显示当前类的结构，支持搜索指定的方法、属性等

`Ctrl+T`：显示光标所在类的继承树结构

`Ctrl+K`：搜索选中的单词，并向下跳转，如果光标跳到了结果最尾项，再按 `Ctrl+K` 会往回跳到结果首项。

`Shift+Enter`：新建下一行，不用把光标移到行末再回车了。如果 Eclipse 装了 [Vrapper](#Vrapper) 插件，就可以使用 `o`（小写）来新建下一行。

`Ctrl+Shift+Enter`：新建上一行，不用把光标移到行首回车。如果 Eclipse 装了 [Vrapper](#Vrapper) 插件，就可以使用 vim 的 `O`（大写）来新建上一行。

`Ctrl+D`：删除当前行

`Alt+UP` 或 `Alt+Down`：将当前行的代码往上移或往下移

`Ctrl+Alt+UP` 或 `Ctrl+Alt+Down`：复制当前行到上一行或下一行

`Alt+Enter`：显示选中的当前项目或文件的属性

##### getter 和 setter 生成

1. `Shift+Alt+S`，呼出「Source」菜单 如果装了 [Vrapper](#Vrapper) 插件并装了 [Java extensions](#^491eb0) 子扩展，那就可以使用 `gm` 快捷键呼出「Source」菜单。
2. 按 `r`，呼出 getter 和 setter 配置菜单
3. 选择要生成 getter 和 setter 的属性，全选： `Alt+A`；取消所有： `Alt+D`；选择所有的 getter：`Alt+G`；选择所有 setter：`Alt+L`。
4. 回车生成 getter 和 setter 如果 `Generate` 按钮失去焦点，就按 `Alt+G`。

##### 生成构造方法

1. `Shift+Alt+S`，呼出「Source」菜单，跟 [getter 和 setter 生成](#getter%20和%20setter%20生成) 完全一样
2. 按 `o`，呼出构造方法配置菜单
3. 选择构造方法所需的属性，如果全选就 `Alt+A`，取消所有选择就按 `Alt+D`
4. 回车确认生成构造方法

---

#### <span id="java_eclipse_plugins">Eclipse 常用插件</span>

##### jeeeyuls-eclipse-themes

[jeeeyul_theme](https://marketplace.eclipse.org/content/jeeeyuls-eclipse-themes) 是一个 Eclipse 界面主题插件。

##### colortheme

[colortheme](https://marketplace.eclipse.org/content/eclipse-color-theme) 这个与上面那个不同，这是插件是针对编辑区的配色插件。 Eclipse 市场中的 color-theme 插件地址已经失效。 请到 [这个](https://eclipse-color-theme.github.io/update/) 页面，其中有个链接： [download this update site as a zip archive](https://eclipse-color-theme.github.io/update/eclipse-color-theme-update-site.zip)，下载这个压缩包。将其中的 `features` 和 `plugins` 两个目录提取出来放到一个你自定义名称的目录中，如「color-theme」，将这个包括有 `features` 和 `plugins` 的目录放到 Eclipse 安装目录下的 `dropins` 目录中，重启 Eclipse ，这个插件就能生效了！

##### Bracketeer

[Bracketeer](https://marketplace.eclipse.org/content/bracketeer-java-jdt) 是一个使用注释方式标识出匹配大括号的插件。Eclipse 插件市场中的 Bracketeer 插件的地址已经失效了，应自行通过 「Install new Software」 这个方式添加插件安装地址。安装地址：[https://chookapp.github.io/ChookappUpdateSite/](https://chookapp.github.io/ChookappUpdateSite/)

##### Eclipse explorer

[Eclipse explorer](https://marketplace.eclipse.org/content/eclipse-explorer)  [![eclipse explorer repo](https://img.shields.io/github/stars/Jamling/eclipse-explorer?style=social)](https://github.com/Jamling/eclipse-explorer/releases) 是打开项目本地目录的插件。同样的，插件市场的地址也是失效了，得到 github 中下载。同 color-theme 一样，下载下的包是不能直接丢到 `dropins` 目录的，得把 `features` 和 `plugins` 目录提取出来。

> [!tip] explorer
> [Eclipse Explorer](https://marketplace.eclipse.org/content/eclipse-explorer) 这插件又在插件市场上架了。并且手动丢包到 `dropins` 目录方式，好像失效了 -- 至少在 Linux 上失效。
> 
> 安装地址为：[https://www.ieclipse.cn/PDESite/updates/](https://www.ieclipse.cn/PDESite/updates/)

##### Jcolon

[Jcolon](https://mystilleef.github.io/eclipse4-jcolon/) [![jcolon repo](https://img.shields.io/github/stars/mystilleef/eclipse4-jcolon?style=social)](https://github.com/mystilleef/eclipse4-jcolon) 是一款自动补分号的插件。真是自动，不需要按快捷键。

##### EditBox

[EditBox](https://marketplace.eclipse.org/content/editbox) 是一款显示代码范围的插件。

![EditBox preview](https://a.fsdn.com/con/app/proj/editbox/screenshots/238862.jpg/245/183/1.5)

如果不想使用插件市场安装，可以到 [sourceforge](http://editbox.sourceforge.net/updates/) 下载 `features` 和 `plugins` 两目录下的 `jar` 文件，把这两目录放到一个目录中，如 `editbox`，然后把这个目录放在 Eclipse 安装目录下 `dropins` 目录中，重启 Eclipse，就能生效。

有时使用 `dropins` 这种方式，可以使用使用 `Install New Software` 方式来安装插件。

> editbox 安装地址：[http://editbox.sourceforge.net/updates](http://editbox.sourceforge.net/updates)

##### relative-line-number

[relative-line-number](https://marketplace.eclipse.org/content/relative-line-number-ruler) 相对行号。

##### freemarker

[freemarker](https://marketplace.eclipse.org/content/freemarker-ide) freemarker 插件。这插件 github 地址：[https://github.com/ddekany/jbosstools-freemarker](https://github.com/ddekany/jbosstools-freemarker) 。

这个插件是从 [JBossTools Freemarker](https://github.com/jbosstools/jbosstools-freemarker) 插件中分支出来的，因为原版的插件在 JBoss Tools 4.5.3 时就已经被移除了，估计是 Freemarker 用得人太少，已经算是过时的技术了，所以 JBoss 就把这货从 JBoss Tools 中移除掉。

事实上这插件也已经有 2 年多没更新了，估计停止维护也不远了，现在还是能用的。不过估计随着 Eclipse 继续版本迭代，不兼容性迟早会出现，到时候是真的就用不了了！

##### SQL DAL Maker

[SQL DAL Maker](https://github.com/panedrone/sqldalmaker) 数据链接层生成插件

##### mybatipse

[mybatipse](https://marketplace.eclipse.org/content/mybatipse) [![mybatipse repo](https://img.shields.io/github/stars/mybatis/mybatipse?style=social)](https://github.com/mybatis/mybatipse/) MyBatis 插件。

  mybatis 中 xml、java 文件的各种功能增强，如自动完成、相关 sql 关联等，使用 mybatis 必装的插件。

##### MyBatis-Generator

[mybatis-generator](https://marketplace.eclipse.org/content/mybatis-generator) MyBatis 生成插件。

##### Vrapper

[Vrapper](https://marketplace.eclipse.org/content/vrapper/) 是一个在 Eclipse 上模拟 [vim](../vim/Vim_Note.md) 的插件。

这插件有几个子扩展，是 vim 下常用的插件模拟。

推荐安装大名鼎鼎的 [Surround](../vim/vim_plugin.md#Surround)。

另外，作为 Java 开发，也建议把 「Java extensions」这个扩展也勾选安装上。其实这个折展就是为 Eclipse 两个右键菜单添加 vim 式的快捷键： ^491eb0
* `gr` Eclipse 「refactor」（重构）菜单
* `gm`：Eclipse 「source」菜单

Vrapper 更详细使用请参考 [Vrapper Documentation](https://vrapper.sourceforge.net/documentation/?topic=introduction)。

---

#### Eclipse 基础的各种软件找不到 jre

>可以将在软件安装目录下建一个软链接指向 jdk 中的 jre（如像 java11+ 的没有预装 **jre**，请用上面的命令生成 **jre**）
>
>下面以 **DBeaver** 为例:
>
>```shell
>sudo ln -s /opt/JDK/jdk11/jre /opt/dbeaver/jre
>```

>Eclipse 运行需要的模块:
>
>java.base,java.desktop,java.logging,java.xml,java.naming,java.net.http,java.sql,java.sql.rowset
>
>

#### Tomcat 配置出问题

>配置 tomcat 时，提示“eclipse tomcat unknown version of tomcat was specified”
>
>因为配置 tomcat 需要访问 tomcat 目录下的 lib 库,而访问此目录需要相应的权限
>
>所以得修改 lib 目录的权限:
>
>```shell
>chmod -R 777 apache-tomcat-xxx/lib
>```

无独有偶，[VSCode](https://code.visualstudio.com/) 下，使用 [Tomcat to Java](https://marketplace.visualstudio.com/items?itemName=adashen.vscode-tomcat) 插件，添加 Tomcat ，可能会添加失败，报 `Please make sure you select a valid Tomcat Directory.` 错误，同样也是权限问题。

示例：
```shell
sudo chmod -R 755 tomcat-9.0.62
```

#### 启动 Tomcat 后 404

>![](Java相关.assets/eclipse_tomcat_publish.png)
>
>要选第二项，就是将项目复制一份到 tomcat 安装目录下的 wtpwebapps 目录中进行发布

---

## <span id="java_lambda">Lambda 相关</span>

---

## <span id="java_commands">java 命令使用</span>

### <span id="java_commands_javac">javac</span>

命令一般格式：`javac xxx.java` ，即可编译 java 代码。

常用选项：

`javac -g xxx.java` 在编译 java 代码时加上 `-g` 选项，可以在使用 `javap` 命令时显示「局部变量表」的信息。 
```shell
LocalVariableTable:
	Start  Length  Slot  Name   Signature
	  0       7     0  args   [Ljava/lang/String;
	  2       5     1     i   I
	  4       3     2     j   I
```

### <span id="java_commands_javap">javap</span>

`javap` 是 jdk 自带的反编译工具。

一般常用的选项有三个：
* `-v` 不仅会输出行号、局部变量表信息、反编译汇编代码，还会输出当前类用到的常量池等信息
* `-l` 会输出行号和局部变量表信息
* `-c` 会对当前 class 字节码进行反编译生成汇编代码

示例：

源码：

```java
public class Test05{
    public static void main(String[] args){
    

        int i=2;
        int j=0;

        i=j;


    }
}

```

使用的大致步骤：
1. 先使用 `javac -g Test05.java` 来编译，如上述所讲的，如果不加 `-g` 来编译，使用 `javap` 时，将不会显示「局部变量表」的信息。

2. 使用 `javap -v Test05` 后显示的内容：

```shell

javap -v Test05   
Classfile /home/silascript/DevWorkSpace/JavaExercise/Test05.class
  Last modified 2022年7月10日; size 411 bytes
  MD5 checksum 26a137d802c1bb7d139bee1102f4eeed
  Compiled from "Test05.java"
public class Test05
  minor version: 0
  major version: 55
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #2                          // Test05
  super_class: #3                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 2, attributes: 1
Constant pool:
   #1 = Methodref          #3.#20         // java/lang/Object."<init>":()V
   #2 = Class              #21            // Test05
   #3 = Class              #22            // java/lang/Object
   #4 = Utf8               <init>
   #5 = Utf8               ()V
   #6 = Utf8               Code
   #7 = Utf8               LineNumberTable
   #8 = Utf8               LocalVariableTable
   #9 = Utf8               this
  #10 = Utf8               LTest05;
  #11 = Utf8               main
  #12 = Utf8               ([Ljava/lang/String;)V
  #13 = Utf8               args
  #14 = Utf8               [Ljava/lang/String;
  #15 = Utf8               i
  #16 = Utf8               I
  #17 = Utf8               j
  #18 = Utf8               SourceFile
  #19 = Utf8               Test05.java
  #20 = NameAndType        #4:#5          // "<init>":()V
  #21 = Utf8               Test05
  #22 = Utf8               java/lang/Object
{
  public Test05();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 1: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   LTest05;

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=1, locals=3, args_size=1
         0: iconst_2
         1: istore_1
         2: iconst_0
         3: istore_2
         4: iload_2
         5: istore_1
         6: return
      LineNumberTable:
        line 5: 0
        line 6: 2
        line 8: 4
        line 11: 6
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       7     0  args   [Ljava/lang/String;
            2       5     1     i   I
            4       3     2     j   I
}
SourceFile: "Test05.java"

```

