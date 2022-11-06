
# Java 笔记

---
## 目录

* [Java相关的配置](#java_config)
  * [Eclipse相关](#java_eclipse)

* [Lambda 相关](#java_lambda)

---

## <span id="java_config">Java相关的配置</span>



---

### Java11生成JRE

进入jdk安装目录后执行以下命令：

```shell
sudo ./bin/jlink --module-path jmods --add-modules java.desktop --output jre
```

>把jmodes目录所有模块都生成jre:
>
>```shell
>sudo ./bin/jlink --module-path jmods --add-modules ALL-MODULE-PATH --output jre
>```
>
>

---

### <span id="java_eclipse">Eclipse相关</span>

#### <span id="java_eclipse_plugins">Eclipse 常用插件</span>

* [jeeeyul_theme]() 是一个 Eclipse 界面主题插件。

* [colortheme](https://marketplace.eclipse.org/content/eclipse-color-theme) 这个与上面那个不同，这是插件是针对编辑区的配色插件。 Eclipse 市场中的 color-theme 插件地址已经失效。 请到[这个](https://eclipse-color-theme.github.io/update/) 页面，其中有个链接： [download this update site as a zip archive](https://eclipse-color-theme.github.io/update/eclipse-color-theme-update-site.zip)，下载这个压缩包。将其中的 `features` 和 `plugins` 两个目录提取出来放到一个你自定义名称的目录中，如「color-theme」，将这个包括有 `features` 和 `plugins` 的目录放到Eclipse 安装目录下的 `dropins` 目录中，重启 Eclipse ，这个插件就能生效了！

* [Bracketeer](https://marketplace.eclipse.org/content/bracketeer-java-jdt) 是一个使用注释方式标识出匹配大括号的插件。Eclipse 插件市场中的 Bracketeer 插件的地址已经失效了，应自行通过 「Install new Software」 这个方式添加插件安装地址。安装地址：https://chookapp.github.io/ChookappUpdateSite/

* [Eclipse explorer](https://marketplace.eclipse.org/content/eclipse-explorer)  [![eclipse explorer repo](https://img.shields.io/github/stars/Jamling/eclipse-explorer?style=social)](https://github.com/Jamling/eclipse-explorer/releases)是打开项目本地目录的插件。同样的，插件市场的地址也是失效了，得到 github 中下载。同 color-theme一样，下载下的包是不能直接丢到 `dropins` 目录的，得把 `features` 和 `plugins` 目录提取出来。

* [Jcolon](https://mystilleef.github.io/eclipse4-jcolon/) [![jcolon repo](https://img.shields.io/github/stars/mystilleef/eclipse4-jcolon?style=social)](https://github.com/mystilleef/eclipse4-jcolon)是一款自动补分号的插件。真是自动，不需要按快捷键。

* [EditBox](https://marketplace.eclipse.org/content/editbox) 是一款显示代码范围的插件。

![EditBox preview](https://a.fsdn.com/con/app/proj/editbox/screenshots/238862.jpg/245/183/1.5)

如果不想使用插件市场安装，可以到 [sourceforge](http://editbox.sourceforge.net/updates/) 下载 `features` 和 `plugins` 两目录下的 `jar` 文件，把这两目录放到一个目录中，如 `editbox`，然后把这个目录放在 Eclipse 安装目录下 `dropins` 目录中，重启 Eclipse，就能生效。

有时使用 `dropins` 这种方式，可以使用使用 `Install New Software` 方式来安装插件。

> editbox 安装地址：[http://editbox.sourceforge.net/updates](http://editbox.sourceforge.net/updates)


* [relative-line-number](https://marketplace.eclipse.org/content/relative-line-number-ruler) 相对行号

* [freemarker](https://marketplace.eclipse.org/content/freemarker-ide) freemarker 插件

* [SQL DAL Maker](https://github.com/panedrone/sqldalmaker) 数据链接层生成插件

* [mybatipse](https://marketplace.eclipse.org/content/mybatipse) [![mybatipse repo](https://img.shields.io/github/stars/mybatis/mybatipse?style=social)](https://github.com/mybatis/mybatipse/) MyBatis 插件。

  mybatis 中 xml、java 文件的各种功能增强，如自动完成、相关 sql 关联等，使用 mybatis 必装的插件。

* [mybatis-generator](https://marketplace.eclipse.org/content/mybatis-generator) MyBatis 生成插件。

---


#### Eclipse基础的各种软件找不到jre

>可以将在软件安装目录下建一个软链接指向jdk中的jre（如像java11+的没有预装**jre**，请用上面的命令生成**jre**）
>
>下面以**DBeaver**为例:
>
>```shell
>sudo ln -s /opt/JDK/jdk11/jre /opt/dbeaver/jre
>```

>Eclipse运行需要的模块:
>
>java.base,java.desktop,java.logging,java.xml,java.naming,java.net.http,java.sql,java.sql.rowset
>
>

#### Tomcat配置出问题

>配置tomcat时，提示“eclipse tomcat unknown version of tomcat was specified”
>
>因为配置tomcat需要访问tomcat目录下的lib库,而访问此目录需要相应的权限
>
>所以得修改lib目录的权限:
>
>```shell
>chmod -R 777 apache-tomcat-xxx/lib
>```

无独有偶，[VSCode](https://code.visualstudio.com/) 下，使用 [Tomcat to Java](https://marketplace.visualstudio.com/items?itemName=adashen.vscode-tomcat) 插件，添加 Tomcat ，可能会添加失败，报 `Please make sure you select a valid Tomcat Directory.` 错误，同样也是权限问题。

示例：
```shell
sudo chmod -R 755 tomcat-9.0.62
```



#### 启动Tomcat后404

>![](Java相关.assets/eclipse_tomcat_publish.png)
>
>要选第二项，就是将项目复制一份到tomcat安装目录下的wtpwebapps目录中进行发布


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






