---
aliases: []
tags:
  - format
  - formatter
created: 2024-05-24 09:57:51
modified: 2025-10-09 10:54:52
---

# 格式化工具笔记

---

## Prettier

* [Prettier 笔记](Prettier_Note.md)

---

## java

### google-java-format

[google-java-format](https://github.com/google/google-java-format) 是一个 Google 开发的 [Java](../Java/Java_Note.md) 代码格式化器。

[Linux](../Linux/Linux_Note.md) 下，可以使用包管理器直接装：`yay -S aur/google-java-format`。

[VSCode](../Editors/VSCode_Note.md)、[Eclipse](../Java/IDE/Java_IDE_Eclipse.md) 或 IDEA 可以通过安装插件方式使用，详情参考：[google-java-format#using-the-formatter](https://github.com/google/google-java-format#using-the-formatter)

### ktlint

[ktlint](https://github.com/pinterest/ktlint) 是一款 [Kotlin](../Java/Kotlin/Kotlin_Note.md) 的带有格式功能的 Linter。

安装：`yay -S extra/ktlint`

格式化：`ktinlt --format` 或者 `ktlint -F`

ktlint 命令：

```shell
Options:
  -v, --version                   Show the version and exit
  --code-style=(android_studio|intellij_idea|ktlint_official)
                                  (deprecated)
  --color                         Make output colorful
  --color-name=<text>             Customize the output color
  -F, --format                    Fix deviations from the code style when possible
  --limit=<int>                   Maximum number of errors to show (default: show all)
  --relative                      Print files relative to the working directory (e.g. dir/file.kt instead of
                                  /home/user/project/dir/file.kt)
  --reporter=<text>               A reporter to use (built-in: plain (default), plain?group_by_file, plain-summary, json, sarif,
                                  checkstyle, html). To usea third-party reporter specify a path to a JAR file on the filesystem
                                  via ',artifact=' option. To override reporter output, use ',output=' option.
  -R, --ruleset=<text>            A path to a JAR file containing additional ruleset(s)
  --stdin                         Read file from stdin
  --stdin-path=<text>             Virtual file location for stdin. When combined with option '--format' the actual file will not
                                  be overwritten
  --patterns-from-stdin[=<text>]  Read additional patterns to check/format from stdin. Patterns are delimited by the given
                                  argument. (default is newline). If the argument is an empty string, the NUL byte is used.
  --editorconfig=<text>           Path to the default '.editorconfig'. A property value from this file is used only when no
                                  '.editorconfig' file on the path to the source file specifies that property. Note: up until
                                  ktlint 0.46 the property value in this file used to override values found in '.editorconfig'
                                  files on the path to the source file.
  --baseline=<text>               Defines a baseline file to check against
  -l, --log-level=<value>         Defines the minimum log level (trace, debug, info, warn, error) or none to suppress all
                                  logging
  -h, --help                      Show this message and exit

Commands:
  generateEditorConfig     Generate kotlin style section for '.editorconfig' file. Output should be copied manually to the
                           '.editorconfig' file.
  installGitPreCommitHook  Install git hook to automatically check files for style violations on commit
  installGitPrePushHook    Install git hook to automatically check files for style violations before push
```

---

## lua

### lua-fmt

[lua-fmt](https://github.com/trixnz/lua-fmt)

### Stylua

[StyLua](https://github.com/JohnnyMorganz/StyLua) 这是 [Lua](../Lua/Lua_Note.md) 的格式化器。

### 安装

很简单，可以通过系统的包管理器安装，如：`yay -S extra/stylua`

---

## Markdown

> [!info] 
> 
> 其实 [Prettier](Prettier_Note.md) 也能格式化 [Markdown](../Markdown/Markdown_Note.md) 文件，只不过 Prettier 是基于 [NodeJS](../Node/NodeJS_Note.md) 的，得安装 Node 相关环境。

### mdformat

[mdformat](https://github.com/hukkin/mdformat) 是一个用 [Python](../Python/Python_Note.md) 写的 [Markdown](../Markdown/Markdown_Note.md) 的格式化器。

安装，可以通过 [pipx](../Python/Python_Note.md#python_pipx) 来安装：

```shell
pipx install mdformat
```

如果要支持 GFM，可以这样安装：

```shell
pipx install mdformat
pipx inject mdformat mdformat-gfm
```

如果要支持 Github 所有的扩展语法，可以这样安装：

```shell
pipx install mdformat
pipx inject mdformat mdformat-gfm mdformat-frontmatter mdformat-footnote mdformat-gfm-alerts
```

---

## SQL

### sqlfluff

[sqlfluff](https://github.com/sqlfluff/sqlfluff) 是一个使用 [Python](../Python/Python_Note.md) 写的 [SQL](../DataBase/SQL_Note.md) 的格式化及 Lint 工具。

命令列表：

```shell
dialects  Show the current dialects available.
fix       Fix SQL files.
format    Autoformat SQL files.
lint      Lint SQL files via passing a list of files or using stdin.
parse     Parse SQL files and just spit out the result.
render    Render SQL files and just spit out the result.
rules     Show the current rules in use.
version   Show the version of sqlfluff.
```

#### 子命令解析

* `dialects` 是显示所有的数据库方言列表。

示例：

```shell
$ sqlfluff dialects
==== sqlfluff - dialects ====
ansi:                 ANSI dialect [inherits from 'nothing']
athena:            AWS Athena dialect [inherits from 'ansi']
bigquery:     Google BigQuery dialect [inherits from 'ansi']
clickhouse:        ClickHouse dialect [inherits from 'ansi']
databricks:    Databricks dialect [inherits from 'sparksql']
db2:                  IBM Db2 dialect [inherits from 'ansi']
doris:          Apache Doris dialect [inherits from 'mysql']
duckdb:            DuckDB dialect [inherits from 'postgres']
exasol:                Exasol dialect [inherits from 'ansi']
flink:       Apache Flink SQL dialect [inherits from 'ansi']
greenplum:      Greenplum dialect [inherits from 'postgres']
hive:             Apache Hive dialect [inherits from 'ansi']
impala:         Apache Impala dialect [inherits from 'hive']
mariadb:             MariaDB dialect [inherits from 'mysql']
materializ:   Materialize dialect [inherits from 'postgres']
e                                                           
mysql:                  MySQL dialect [inherits from 'ansi']
oracle:                Oracle dialect [inherits from 'ansi']
postgres:          PostgreSQL dialect [inherits from 'ansi']
redshift:    AWS Redshift dialect [inherits from 'postgres']
snowflake:          Snowflake dialect [inherits from 'ansi']
soql:        Salesforce Object Query Language (SOQL) dialect
                                      [inherits from 'ansi']
sparksql:    Apache Spark SQL dialect [inherits from 'ansi']
sqlite:                SQLite dialect [inherits from 'ansi']
starrocks:         StarRocks dialect [inherits from 'mysql']
teradata:            Teradata dialect [inherits from 'ansi']
trino:                  Trino dialect [inherits from 'ansi']
tsql:         Microsoft T-SQL dialect [inherits from 'ansi']
vertica:              Vertica dialect [inherits from 'ansi']
```

> [!tip] 
> 
> 使用 [awk](../Linux/Linux_Note.md#awk) 工具从 `dialects` 子命令得到所有子方言信息中获取其方言名称：
> 
> `sqlfluff dialects | awk '/:/{print $1}{FS=":"}'`
> 
> 结果：
> 
> ```shell
> ansi
>athena
>bigquery
>clickhouse
>databricks
>db2
>doris
>duckdb
>exasol
>flink
> greenplum
> hive
> impala
> mariadb
> materializ
> mysql
> oracle
> postgres
>redshift
> snowflake
> soql
> sparksql
> sqlite
> starrocks
> teradata
> trino
> tsql
> vertica
> ```
> 

* `rules` 是列出 `lint`、`fix` 或 `format` 的规则。

语法：`sqlfluff format --dialect 数据库方言 [--rules 规则] sql文件`

> [!info] 
> 
> 
> * `--dialect`：这个选项必须得指定，sqlfluff 没有像 [sql-formatter](#sql-formatter) 那样给方言设置一个默认值，所以必须得用户指定
> 	* 方言的值可以使用 `dialects` 命令查询得到
> * `--rules` 可以省略。使用这参数时，可以给多个实参，即应用多条规则，多条规则使用逗号 `,` 分隔，如：
> 
> ```shell
> sqlfluff lint --dialect mysql --rules LT10,ST05 xxx.sql
> ```

示例：

```shell
sqlfluff format --dialect mysql t01.sql
```

* `format`：格式化 SQL 文件，返回格式化的统计结果
* `render`：格式化 SQL 文件，返回格式化的 SQL 内容

> [!important] 
> 
> 必须带上参数 `--dialect`，指定 SQL 的方言类型。sqlfluff 支持的方言类型，可以通过 `sqlfluff dialects` 子命令查询。

`format` 这个子命令不能在 [Vim](../vim/Vim_Note.md) 或 [Neovim](../vim/Neovim_Note.md) 中使用如 [formatter.nvim](../vim/Neovim_Note.md#formatter.nvim) 这种格式化插件调用，因为 `format` 这个子命令最后返回的结果是格式了几条语句，而非格式化后的 sql 最终结果。故要在 `formatter.nvim` 这种插件调用，得使用 `render` 子命令。

对比 `format` 与 `render` 子命令：

```shell
$ sqlfluff format --dialect mysql t01.sql
==== finding fixable violations ====
==== no fixable linting violations found ====                                                                                   
All Finished 📜 🎉!
```

```shell
$ sqlfluff render --dialect mysql t01.sql
use exercise01;

select emp.ename
from
    emp;

select dept.deptno
from
    dept;
```

#### 相关文档

* [SQLFluff documentation](https://docs.sqlfluff.com/en/stable/)
	* [CLI Reference](https://docs.sqlfluff.com/en/stable/reference/cli.html)
	* [Rules Reference](https://docs.sqlfluff.com/en/stable/reference/rules.html)

### sql-formatter

[sql-formatter](https://github.com/sql-formatter-org/sql-formatter) 是一个用 [TypeScript](../JS/TypeScript/TypeScript_Note.md) 写的 SQL 格式化器。

#### 安装

```shell
npm install sql-formatter -g
```

#### 配置

`.sql-formatter.json` 是配置文件。

示例：

```json
{
  "language": "spark",
  "tabWidth": 2,
  "keywordCase": "upper",
  "linesBetweenQueries": 2
}
```

常用配置项：

`language`：默认为 `sql`，详情参考：[sql-formatter-docs#language](https://github.com/sql-formatter-org/sql-formatter/blob/master/docs/language.md)

---

## XML

### xmlstarlet

[XMLStarlet](https://xmlstar.sourceforge.net) 是一个 [XML](../XML/XML_Note.md) 专门的格式化器。

#### 安装

使用系统的包管理器直接安装就好了：`yay -S extra/xmlstarlet`

### Tidy

tidy 同样也是在 [Linux](../Linux/Linux_Note.md) 上常用的 [XML](../XML/XML_Note.md) 工具。

#### 安装

使用系统的包管理工具就能安装：`yay -S tidy `

#### tidy 使用

使用 tidy 格式化 xml 时 ，得加上 `-xml` 选项使用。

常用参数：

* `-xml`：指定输入为 XML（重要！）
* `-i`：缩进元素
* `-m`：修改原文件
* `-wrap 80`：在 80 字符处换行
* `-indent`：缩进内容
* `-quiet`：不显示警告
* `-omit` ：省略 XML 声明
* `--indent-attributes yes`：属性缩进

### XMLLint

[XmlLint](https://xmllint.com) 这是一个 [XML](../XML/XML_Note.md) 的 Linter，它包括了格式化功能。

#### 安装

要安装 xmllint，只要安装 `libxml2` 就可以使用该命令了。

[Linux](../Linux/Linux_Note.md) 很多核心库都带了这东西，不要在包管理器搜 xmllint，那是找不到的：

```shell
core/libxml2 2.14.6-1 (795.1 KiB 3.1 MiB) (已安装)
    XML C parser and toolkit
```

#### XMLLint 使用

可用的选项及参数如下：

```shell
Parse the XML files and output the result of the parsing
--version : display the version of the XML library used
--shell : run a navigating shell
--debug : show additional debug information
--copy : used to test the internal copy implementation
--recover : output what was parsable on broken XML documents
--huge : remove any internal arbitrary parser limits
--noent : substitute entity references by their value
--noenc : ignore any encoding specified inside the document
--noout : don't output the result tree
--path 'paths': provide a set of paths for resources
--load-trace : print trace of all external entities loaded
--nonet : refuse to fetch DTDs or entities over network
--nocompact : do not generate compact text nodes
--valid : validate the document in addition to std well-formed check
--postvalid : do a posteriori validation, i.e after parsing
--dtdvalid URL : do a posteriori validation against a given DTD
--dtdvalidfpi FPI : same but name the DTD with a Public Identifier
--insert : ad-hoc test for valid insertions
--strict-namespace : Return application failure if document has any namespace errors
--quiet : be quiet when succeeded
--timing : print some timings
--repeat : repeat 100 times, for timing or profiling
--dropdtd : remove the DOCTYPE of the input docs
--html : use the HTML parser
--nodefdtd : do not default HTML doctype
--xmlout : force to use the XML serializer when using --html
--push : use the push mode of the parser
--memory : parse from memory
--maxmem nbbytes : limits memory allocation to nbbytes bytes
--nowarning : do not emit warnings from parser/validator
--noblanks : drop (ignorable?) blanks spaces
--nocdata : replace cdata section with text nodes
--nodict : create document without dictionary
--pedantic : enable additional warnings
--output file or -o file: save to a given file
--format : reformat/reindent the output
--encode encoding : output in the given encoding
--pretty STYLE : pretty-print in a particular style
				 0 Do not pretty print
				 1 Format the XML content, as --format
				 2 Add whitespace inside tags, preserving content
--compress : turn on gzip compression of output
--c14n : save in W3C canonical format v1.0 (with comments)
--c14n11 : save in W3C canonical format v1.1 (with comments)
--exc-c14n : save in W3C exclusive canonical format (with comments)
--nsclean : remove redundant namespace declarations
--catalogs : use SGML catalogs from $SGML_CATALOG_FILES
			 otherwise XML Catalogs starting from 
		 file:///home/silascript/miniforge3/etc/xml/catalog are activated by default
--nocatalogs: deactivate all catalogs
--auto : generate a small doc on the fly
--xinclude : do XInclude processing
--noxincludenode : same but do not generate XInclude nodes
--nofixup-base-uris : do not fixup xml:base uris
--loaddtd : fetch external DTD
--dtdattr : loaddtd + populate the tree with inherited attributes 
--stream : use the streaming interface to process very large files
--walker : create a reader and walk though the resulting doc
--pattern pattern_value : test the pattern support
--relaxng schema : do RelaxNG validation against the schema
--schema schema : do validation against the WXS schema
--schematron schema : do validation against a schematron
--sax1: use the old SAX1 interfaces for processing
--sax: do not build a tree but work just at the SAX level
--oldxml10: use XML-1.0 parsing rules before the 5th edition
--xpath expr: evaluate the XPath expression, imply --noout
--max-ampl value: set maximum amplification factor
```

##### 常用用法

格式化语法：

* `xmllint --format xxx.xml`
* `xmllint --format --pretty 2 xx.xml`
> [!info] 
> 
> 	`--pretty` 参数是指定部分格式化风格，参数 `2`，就是给一些标签加空格。主要是对 [XML](../XML/XML_Note.md) 命名空间那块的美化，如：
> 
> 未添加 `--pretty 2` 选项的： 
> ```shell
> <settings xmlns="http://maven.apache.org/SETTINGS/1.2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 https://maven.apache.org/xsd/settings-1.2.0.xsd">
> ```
> 
> 添加了 `--pretty 2` 选项的：
> ```shell
> <settings
>   xmlns="http://maven.apache.org/SETTINGS/1.2.0"
>   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 https://maven.apache.org/xsd/settings-1.2.0.xsd"
> ```

* `--format --recover`：属性缩进

---

## TOML

### Taplo

[Taplo](https://taplo.tamasfe.dev) 是 [TOML](../TOML/TOML_Note.md) 的工具集，其中包括了格式化 TOML 代码。

---

## 相关笔记

* [Prettier笔记](Prettier_Note.md)
* [格式化器 资料清单](Formatter_Material.md)
