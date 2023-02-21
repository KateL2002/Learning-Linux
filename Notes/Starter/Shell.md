# Shell

>🕮 **引言**
>
>BASH SHELL 是基于文本的命令行界面，它可用于向系统输入命令。Linux 命令行由名为 shell 的程序提供。多年来已经为 shell 程序开发了各种选项，而且可以配置不同的用户来使用不同的 shell。但是，大多数用户都坚持使用当前默认设置。

### 1、什么是 Shell？

- 基于文本的**命令行界面**
- 只需要键盘
- 一条命令完成操作，高效且有用

### 2、Shell 的前置结构

```java
[user@localhost ~]$
```

```java
[root@localhost ~]#
```

- `user`：当前登录的用户名
- `localhost`：主机名
- `~`：当前所在的工作目录【`~` 为用户的主目录（家目录）】
- `$`：提示符，用于标识（除 root 用户外的）用户，`#` 为 `root` 用户

### 3、Shell 命令组成

- 由`命令 [ + 选项 + 参数]` 组成
  - **命令**表示程序名称。如：`ls` / `cd` / `pwd`
  - **选项**通常由 `-[大小写字母]` 、`--[单词]`组成。例如：`-h` / `–help`
  - 可**同时带多个选项**。例如：`-lh` / `--optionA --optionB`
  - **参数**表示使用的对象，紧跟**选项**的后面
- 举例：`ls -l /home/alice`
  - `ls` 命令
  - `-l` 使用的选项
  - `/home/alice` 选项对应参数
  - 此命令表示以长列表的形式查看 `/home/alice` 下的目录内容

### 4、使用 Shell

#### 1）简单示例

在一行内输入命令，按下<kbd>Enter</kbd>键，系统会显示输出结果，下面是实例：

```java
[root@localhost ~]# whoami 
root
[root@localhost ~]# hostname
localhost.localdomain
[root@localhost ~]# 
```

> 💡 **提示**
>
> 关于 Shell 的更多命令，请直接跳转至 [🔖 Shell 命令速查表](../Extra/Shell-Command.md)。

#### 2）组合命令

在一行内可以使用`;`、`&&`、`||`符号连接多条命令并组合，如：`date ; whoami ; hostname`

下表是它们的使用区别：

| 符号 | 用例                   | 作用                                   |
| ---- | ---------------------- | -------------------------------------- |
| `;`  | `cmd1; cmd2; cmd3`     | 按先后顺序依次执行，**不论遇错都执行** |
| `&&` | `cmd1 && cmd2 && cmd3` | 按先后顺序依次执行，**遇错停止执行**   |
| `\|\|` | `cmd1 \|\| cmd2 \|\| cmd3` | 按先后顺序依次执行，**遇错继续执行**   |

#### 3）👍 `TAB` 补全命令

通过按下 <kbd>Tab</kbd> 键，可快速补全命令以及文件名。再次按下<kbd>Tab</kbd>键将会列出匹配的所有命令或文件名

示例 1：快速补全命令

```java
[root@localhost ~]# host<Tab><Tab>
hostid       hostname     hostnamectl  
[root@localhost ~]# hostn<Tab>
[root@localhost ~]# hostname
localhost.localdomain
```

示例 2：快速补全选项

```java
[root@localhost ~]# uname --<Tab><Tab>
--all                --kernel-name        --machine            --processor
--hardware-platform  --kernel-release     --nodename           --version
--help               --kernel-version     --operating-system   
```

示例 3：快速补全文件名

```java
[root@localhost ~]# cat /etc/host<Tab><Tab>
host.conf  hostname   hosts      
[root@localhost ~]# cat /etc/hosts
```

#### 4）长命令

当遇到较长命令时，输入 `\` 并按下<kbd>Enter</kbd>键。当看到下一行出现的 `> ` 时继续输入，它可以连续使用很多行。

示例：同时查看两个文件内容

```java
[root@localhost ~]# head -n 5 \
> /usr/share/dict/linux.words \
> /usr/share/dict/<Tab>
linux.words  words        
> /usr/share/dict/words 
==> /usr/share/dict/linux.words <==
1080
10-point
10th
11-point
12-point

==> /usr/share/dict/words <==
1080
10-point
10th
11-point
12-point

```

#### 5）👍 命令历史记录

使用 `history` 可以显示所有之前执行过的命令。如下例：

```shell
[root@localhost ~]# history 
    1  uname -a
    2  cat /etc/*rele*
    3  lspci
    4  lscpu
    5  lsusb
    6  df -h
    7  uptime 
    8  who
    9  history 
[root@localhost ~]# 
```

关于其具体用法如下：

| 命令 / 参数         | 作用                                     |
| ------------------- | ---------------------------------------- |
| `history`           | 查看所有记录                             |
| `history n`         | 查看最近 n 条记录                        |
| `!<num>`            | 执行第 `<num>` 条记录                    |
| `!<String>`         | 执行与最近一条以 `<String>` 为开头的命令 |
| `!!`                | 执行上一条记录                           |
| `history -c`        | 清除所有记录                             |
| `history -d <num> ` | 删除第 `<num>` 条记录                    |

👍 当然，你还可以直接使用快捷键 <kbd>Ctrl</kbd>+<kbd>R</kbd> 来回溯最近一次与之匹配的命令

#### 6）Shell 快捷键

以下列举了部分较为有用的快捷方式。**请务必熟练使用这些快捷键，这将会大大提升输入效率**。

- 命令行

  | 快捷键                                       | 作用                                   |
  | -------------------------------------------- | -------------------------------------- |
  | <kbd>Ctrl</kbd>+<kbd>C</kbd>                 | 👍 终止执行 / 直接跳入下一行            |
  | <kbd>Ctrl</kbd>+<kbd>L</kbd>                 | 👍 清空屏幕                             |
  | <kbd>Ctrl</kbd>+<kbd>R</kbd>                 | 👍 搜索最近输入过的命令（**命令回溯**） |
  | <kbd>↑</kbd> 或 <kbd>Ctrl</kbd>+<kbd>P</kbd> | 上一条输入过的命令                     |
  | <kbd>↓</kbd> 或 <kbd>Ctrl</kbd>+<kbd>N</kbd> | 下一条输入过的命令                     |

- 光标

  | 快捷键                       | 作用                   |
  | ---------------------------- | ---------------------- |
  | <kbd>←</kbd>                 | 向左移动一个字符       |
  | <kbd>→</kbd>                 | 向右移动一个字符       |
  | <kbd>Tab</kbd>               | 👍 快速补全命令或路径   |
  | <kbd>Ctrl</kbd>+<kbd>←</kbd> | 将光标向前移动一个字词 |
  | <kbd>Ctrl</kbd>+<kbd>→</kbd> | 将光标向后移动一个字词 |
  | <kbd>Ctrl</kbd>+<kbd>A</kbd> | 👍 将光标移到行首       |
  | <kbd>Ctrl</kbd>+<kbd>E</kbd> | 👍 将光标移到行尾       |

- 系统与终端

  | 快捷键                                           | 作用                                                         |
  | ------------------------------------------------ | ------------------------------------------------------------ |
  | <kbd>Ctrl</kbd>+<kbd>D</kbd>                     | 👍 注销当前用户                                               |
  | <kbd>Ctrl</kbd>+<kbd>Z</kbd>                     | 👍 将当前正在执行的程序转入后台（执行 `fg` 命令转回前台）     |
  | <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Fn</kbd>     | 切换第 n 个终端（如 <kbd>F1</kbd> 为 1 号终端，一般至多有 6 个） |
  | <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Delete</kbd> | 👍 重启，（在桌面环境下，需要确认是否注销）                   |



> 💡 **提示**
>
> 如果你对所输入的命令用法不太了解，你可以通过执行 `<command> --help` 来查看详细的用法与说明；如果仍然对其不够熟悉，请通过 `man <command>` 命令来查询更为详细的帮助手册。具体内容，请跳转至 🔖[帮助手册](Manual.md) 章节。
>
> 
>
> 关于更多 Shell 命令的内容，你还可以通过扩展文档中的 🔖 [Shell 命令速查表](../Extra/Shell-Command.md)，可以快速找到更多的命令。
