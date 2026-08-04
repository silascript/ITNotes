---
aliases: []
tags:
  - linux
  - shell
  - json
  - tools
  - jq
created: 2026-08-04 20:29:43
modified: 2026-08-04 20:33:41
---

# Jq 笔记

---

[jq](https://jqlang.org) 是一个 Shell 下操作 json 的小工具。

#### 安装

```shell
yay -S jq
```

#### 语法

```shell
jq [options] <jq filter> [file...]
jq [options] --args <jq filter> [strings...]
jq [options] --jsonargs <jq filter> [JSON_TEXTS...]
```

##### 选项

* `-c`：紧凑而不是漂亮的输出
* `-n`：使用 `null` 作为单个输入值
* `-e`：根据输出设置退出状态代码
* `-s`：将所有输入读取（吸取）到数组中；应用过滤器;
* `-r`：输出原始字符串，而不是 JSON 文本
* `-R`：读取原始字符串，而不是 JSON 文本
* `-C`：为 JSON 着色
* `-M`：单色（不要为 JSON 着色）
* `-S`：在输出上排序对象的键
* `--tab`：使用制表符进行缩进;
* `--arg a v`：将变量 `$a` 设置为 `v`
* `--argjson a v`： 将变量 `$a` 设置为 JSON `v`
* `--slurpfile a f`：将变量 `$a` 设置为从 `f` 读取的 JSON 文本数组
* `--rawfile a f`：将变量 `$a` 设置为包含 `f` 内容的字符串
* `--args`：其余参数是字符串参数，而不是文件
* `--jsonargs`：其余的参数是 JSON 参数，而不是文件
* `--`：终止参数处理

#### 内置函数

`jq` 支持一些内置函数，如 `length`, `keys`, `values`, `tostring` 等，用于操作和处理 JSON 数据。

* `del`：直接删除目标字段，生成新对象。

##### 数组

* `map(f)`：对数组中的每个元素应用过滤器 `f`
* `sort`：对数组中的元素进行排序
* `sort_by(f)`：根据过滤器 `f` 的结果对数组排序
* `min`，`max`：找出数组最小值和最大值。
* `reverse`：反转数组元素的顺序

##### 对象操作

* `keys` ：函数是获取对象所有的键，并以数组形式返回
* `values` ：函数是获取对象的值。
* `map_values(f)`：对对象中每个值应用过滤器 `f`
* `has(key)`：判断对象是否有某个键

##### 字符串操作

* `contains(x)`：判断输入是否完全包含参数 `x`
* `tostring`：将输入转换成字符串

#### 示例

##### 示例 1

```shell

# 从assets 数组中获取browser_download_url元素的值
# 过滤除了main.js manifest.json styles.css三个文件外所有文件
jq -r '.assets[] | .browser_download_url | select ( contains("main.js") or contains("manifest.json") or contains("styles.css") )' $json_path

```

> [!info] 
> 
> * `contains()` 方法是用来判断是否包含某字符串，包含返回 `true`，否则返回 `false`
> * `select()` 选择过滤数据

##### 示例 2

```shell
local tagstr="$2"
curl http://hub-mirror.c.163.com/v2/library/${image}/tags/list | jq --arg tstr $tagstr -r '.tags[]| select(contains($tstr))'
```

> [!info] 
> 
> * `jq --arg` 是定义变量的选项
> * `jq --arg tstr $tagstr`： `tstr` 为形参变量，是 jq 内部使用；而 `$tagstr` 是实参，外部传进来的。要使用形参时，使用 `$` 打头，跟普通 shell 变量使用一致。

传多个参数：

```shell
dl_url=$(curl $channel_json_v3 | jq -r --arg pkg_name $package_name --arg pkg_version "$package_version" '.packages_cache.[].[]| select(.name==$pkg_name)|.releases[]| select(.version==$pkg_version).url')
```

> [!info] 
> 

> `pkg_name` 和 `pkg_version` 这两个是形参，用于 jq 内部引用的。
> 
> `$package_name` 和 `$package_version` 是实参，是 jq 外部实际传进来的值。
>  
> 注意，`select(.name==$pkg_name)` 或 `select(.version==$pkg_version)`，引用「形参」时，不要加双引号，而且 `==` 不要加空格。
> 
> 「实参」`"$package_version"` 这个可以加双引号，防止传进来的字符串带有空格，被「自动切割」。

##### 示例 3

下面是 [Obsidian](../../NoteSoft/Obsidian/Obsidian_Note.md) 的 [vault](../../NoteSoft/Obsidian/Obsidian_Note.md#vault) 列表配置文件：

```json
{
  "vaults": {
    "88a790a7d8b3e712": {
      "path": "/home/silascript/MyNotes/ITNotes",
      "ts": 1763336737061,
      "open": true
    },
    "50006d35f784463b": {
      "path": "/home/silascript/MyNotes/WritingNotes",
      "ts": 1763353935904
    },
    "38ba7ce6d75f3dc4": {
      "path": "/home/silascript/MyNotes/WritingExericse",
      "ts": 1763304814607
    },
    "8e5254dd564849f2": {
      "path": "/home/silascript/MyNotes/LHP_Note",
      "ts": 1763324507963
    }
  },
  "frame": "custom",
  "disableGpu": true,
  "updateDisabled": true
}
```

如果想要根据 vault 的目录路径找到相应的 vault 的 ID，可以使用以下代码：

```shell
cat .config/obsidian/obsidian.json | jq -r '.vaults | map_values(select(.path=="/home/silascript/MyNotes/WritingExericse")) | keys'
```

##### 示例 4

检测某节点是否存在，如果存在返回 `true`，反之返回`false`：

```shell
jq 'has("userDataProfiles")' ~/.config/Code/User/globalStorage/storage.json
```

---

## 相关笔记

* [Shell 笔记](Shell_Note.md)
* [Shell 示例笔记](Shell_Example.md)
* [Shell 资料清单](Shell_Material.md)