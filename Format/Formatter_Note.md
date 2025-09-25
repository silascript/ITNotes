---
aliases: []
tags:
  - format
  - formatter
created: 2024-05-24 09:57:51
modified: 2025-09-25 21:58:41
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

> [!tip] 
> 
> 虽然 sqlfluff 功能很强，但并不适合与 [Neovim](../vim/Neovim_Note.md) 等结合使用。neovim 这种编辑器更适合使用那种选项参数更少更「傻瓜」的 LSP。

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

> [!info] 
> 
> 最常用的是 `lint`、`format` 和 `fix` 三个。

`dialects` 是显示所有的数据库方言：

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

`rules` 是列出 `lint`、`fix` 或 `format` 的规则。

语法：`sqlfluff format --dialect 数据库方言 [--rules 规则] sql文件`

> [!info] 
> 
> `--rules` 可以省略。使用这参数时，可以给多个实参，即应用多条规则，多条规则使用逗号 `,` 分隔，如：
> 
> ```shell
> sqlfluff lint --dialect mysql --rules LT10,ST05 xxx.sql
> ```

示例：

```shell
sqlfluff format --dialect mysql t01.sql
```

官方文档：[SQLFluff documentation](https://docs.sqlfluff.com/en/stable/)

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

## TOML

### Taplo

[Taplo](https://taplo.tamasfe.dev) 是 [TOML](../TOML/TOML_Note.md) 的工具集，其中包括了格式化 TOML 代码。

---

## 相关笔记

* [Prettier笔记](Prettier_Note.md)
* [格式化器 资料清单](Formatter_Material.md)
