---
aliases: []
tags: []
created: 2023-12-21 10:20:42
modified: 2023-12-21 10:35:10
---
# Julia 笔记

---

## 安装

### jill

推荐使用 [jill.py](https://github.com/johnnychen94/jill.py) 安装。

jill.py 是一个安装 Jullia 的小工具，安装这个小工具，可以通过 [pip](../Python/Python_Note.md#python_pip) 来装。

当然因为 jill.py 这个工具是有可执行程序的，所以为了 [Python](../Python/Python_Note.md) 环境更「干净」，可以通过 [pipx](../Python/Python_Note.md#python_pipx) 来安装：`pipx install jill`

#### jill 简单使用

* `jill upstream`：列出所有 jullia 安装包所有可用的下载源。如下：

```shell
- Official: Julialang.org
  * julialang-s3.julialang.org (119 ms)
  * julialangnightlies-s3.julialang.org (119 ms)
- OfficialNightlies: Julialang.org
  * s3.amazonaws.com (234 ms)
  * s3.amazonaws.com (234 ms)
  * s3.amazonaws.com (234 ms)
  * s3.amazonaws.com (234 ms)
- BFSU: Beijing Foreign Studies University
  * mirrors.bfsu.edu.cn (43 ms)
- TUNA: Tsinghua University TUNA Association
  * mirrors.tuna.tsinghua.edu.cn (654 ms)
  * opentuna.cn (38 ms)
- USTC: University of Science and Technology of China
  * mirrors.ustc.edu.cn (78 ms)
- SJTUG: Shanghai Jiao Tong University Linux User Group
  * mirrors.sjtug.sjtu.edu.cn (39 ms)
- SUSTech: Southern University of Science and Technology Open Source Mirrors
  * mirrors.sustech.edu.cn (21 ms)
```

每个源都有一个「标识符」，用于安装时给 `install --upstream` 参数用的。

如使用清华的源下载安装，可以这样：`jill install --upstream TUNA`，而如果使用中科大的就可以这样：`jill install --upstream USTC`。

* `jill list`：列出已安装的各版本的链接。




---
