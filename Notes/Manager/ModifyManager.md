## 权限管理

### 💡  学前需知

权限（Permissions）：控制对文件及目录的访问、创建、修改、删除等一系列操作

| 权限             | 文件               | 目录                        |
| ---------------- | ------------------ | --------------------------- |
| r（**R**EAD）    | 读取内容           | 查看目录下的内容            |
| w（**W**RITE）   | 编辑 / 修改内容    | 创建 / 删除目录下的任一内容 |
| x（E**X**ECUTE） | 作为可执行文件运行 | 可直接切换至此目录          |

### 1. 查看权限

> 执行 `ls` 仅可查看目录下存放的内容 ；
>
> 执行 `ll` 可查看更为详细的内容（如：所属用户、所属组、权限、文件大小、修改日期等）

示例，长列表一般分为以下几部分组成：

```shell
[test@localhost ~]$ ll
总用量 0
drwxr-xr-x. 2 test testGroup 6 11月  4 20:30 公共
drwxr-xr-x. 2 test testGroup 6 11月  4 20:30 模板
drwxr-xr-x. 2 test testGroup 6 11月  4 20:30 视频
drwxr-xr-x. 2 test testGroup 6 11月  4 20:30 图片
drwxr-xr-x. 2 test testGroup 6 11月  4 20:30 文档
drwxr-xr-x. 2 test testGroup 6 11月  4 20:30 下载
drwxr-xr-x. 2 test testGroup 6 11月  4 20:30 音乐
drwxr-xr-x. 2 test testGroup 6 11月  4 20:30 桌面
```

- 【⚠ **非常重要**】`drwxr-xr-x.` ：类型 / 权限
  - `d`：表示类型，如：`b ` 为块设备，`d` 为目录，`l ` 为链接，`-` 为文件……
  - `rwx|r-x|r-x`：由三个字母一组所组成的三组不同的权限，三组分别对应
    - `rwx`：用户权限【`u`（**u**ser）】（对应所属用户 `test` 的权限）
    - `r-x`：组权限【`g`（**g**roup）】
    - `r-x`：其它用户/组权限【`o`（**o**ther）】
- `test`：所属用户
- `testGroup`：所属组
- `11月  4 20:30`：修改时间
- 文件 / 目录名称

### 2. 权限的匹配原则

- 权限匹配优先级**（顺序：所属用户 → 所属组 → 其它用户）**

  - 例 1：已知某服务器上新建了`user1、user2、user3`三个用户，其中 `user1、user2` 属于 `usergroup` 组，现有 `test.txt` 文件及对应属性，如下：

    ```shell
    -r--rw-r--. 1 user1 usergroup 20 11月  8 02:12 test.txt
    ```

    问：`user1` 和 ` user3` 用户能否写入和查看 `test.txt` 文件？

      ```shell
    # 使用 user1 用户
    [user1@localhost test]$ echo "---" >> test.txt
    -bash: test.txt: 权限不够
    
    # 使用 user2 用户
    [user2@localhost test]$ echo "---" >> test.txt
    [user2@localhost test]
      ```

  - 分析：如上结果，**由于`user1` 用户权限中没有写权限**，因此会导致因权限不足而无法写入的结果；而 `user2` 用户与 `user1` 同属于一个组（`usergroup`），因此 `user2` 可以写入 `test.txt` 文件

- **当前用户**下若**没有**所属用户**（当前用户）**的对应权限，则无法进行对应操作

  - 例 2：接例 1，现在 `user1` 修改了`test.txt`文件权限（如下所示），

    ```shell
    -rw-rw-r--. 1 user1 usergroup 20 11月  8 02:12 test.txt
    ```

     那么： ` user3` 用户能否写入和查看 `test.txt` 文件？

    ```shell
    # 使用 user3 用户
    [user3@localhost test]$ echo "user3 is done" >> test.txt
    bash: test.txt: 权限不够
    [user3@localhost test]$ cat test.txt
    bash: test.txt: 权限不够
    ```

  - 分析：如上结果，**由于 `user3` 不属于 `usergroup` 组，且其他用户/组均没有任何权限**，因此 `user3` 既无法查看又无法编辑此文件

- **同组**的**另一用户**若没有所属组的对应权限，则无法进行对应操作

  - 例 3：接例 1，现有文件 `err.log` 存放于此目录，具体如下：

    ```shell
    -rw-r--r--. 1 user1 usergroup  0 11月  8 02:44 err.log
    ```

    问：`user2` 用户能否写入 `err.log` 文件？

    ```shell
    [user2@localhost test]$ echo "---" >> err.log
    bash: err.log: 权限不够
    ```

  - 分析：虽然 `user2` 用户属于 `usergroup` 组，**但由于该用户组中没有写权限**，因此无法写入 `err.log` 文件

- 每个文件跟从父目录权限（即不管该文件的权限本身，**只要拥有父目录权限都能删除父目录下的文件**）

  - 例 4：接例 1，现新建 `Trash` 目录，目录内有存放 `abc.bin` 文件，具体如下：

    ```shell
    drwx------. 2 user1 usergroup 21 11月  8 02:35 Trash
    
    # Trash 目录内部文件结构
    ----------. 1 user1 usergroup 0 11月  8 02:35 abc.bin
    ```

    问：`user1` 能否删除 `abc.bin` 文件？

    ```shell
    [user1@localhost test]$ rm -rf Trash/abc.bin
    [user1@localhost test]$ ll Trash/
    总用量 0
    ```

  - 分析：虽然 `user1` 没有 `abc.bin` 文件的任何权限，**但 `Trash` 目录下 `user1` 拥有全部权限**，因此可以直接删除 `abc.bin` 文件


### 3. 编辑权限 [chmod]

- 要编辑某文件或目录的权限，一般使用 `chmod`指令执行

- 格式：`chmod [选项|参数] [文件 / 目录]`

- 具体设置选项、参数

  | 选项/参数               | 注释                                                         | 选项及参数示例                                               |
  | ----------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | `<u=?>,<g=?>,<o=?>`     | `?` 表示赋予的权限                                           | `u=rw, g=x, o=`<br />所属用户可读可写，所属组可执行，其它无权限 |
  | `<u/g/o><+/-><r/w/x/s>` | `+`：增加权限<br />`-`：减少权限<br /><br />*（其中的 `s` 为特殊权限）* | 1. `u+x`/`+x`：为当前用户添加可执行权限<br />2. `o-w`：为其它用户及组删除写权限<br />3. `g+r`：为当前所属组添加读权限 |
  | `[s]<r><w><x>`          | 每个`[]`表示对应的**数字权限**：<br />数字规则：<br />(1) 每位数字表示 u+g+o 的总和<br />(2) 每位数字由 `r=4, w=2, o=1, -=0` 相加而成<br />(3) 其中的 `[s]` 为特殊权限，<br />     `4` = 用户，`2` = 组，`1` = 其它人，`0` = 无 | 1. `421` => `r--|-w-|--x`<br />2. `777`=> `rwx|rwx|rwx`<br />3. `000`=> `---|---|---`<br />4. `644` => `rw-|r--|r--`<br />5. `750` => `rwx|r-x|---`<br />6\*. `4664` ==> `rwS|rw-|r--`<br />7\*. `2600` ==> `-w-|rwS|r--` |

### 4. 修改所属用户及所属组 [chown]

- 格式：`chown [User]:[Group] <文件/目录>`
- 提示：若只修改所属用户或所属组时，可直接执行以下目录：
  - 仅修改所属用户：`chown [User]: <文件/目录>`
  - 仅修改所属组：`chown :[Group] <文件/目录>`  / `chgrp [group] <文件/目录> `

### 5. 特殊权限（第四权限）

- **S**UID（用户）：**任何一个命令如果可执行且同时带有 suid 权限**，任何用户运行这条命令时都会以 owner 的身份运行

  - 示例：`/etc/shadow` 文件与`/usr/bin/passwd` 文件

    ```shell
    -rwsr-xr-x. 1 root root       34512 8月  12 2018 passwd
    ```

    - 虽然 `/etc/shadow` 文件只有 root 用户拥有权限，但每个管理员用户（包括 root）都可以运行`passwd` 命令（即运行 `/usr/bin/passwd` 文件）进行修改指定用户的密码

    > 💡 **扩展：S 和 s**
    >
    > 区别：当某个拥有特殊权限的文件的权限存在 `x` 权限，则为 `s`（小写），反之为`S`（大写）
    >
    > 示例：如下两个文件，`a.sh`有执行权限， `b.sh` 没有执行权限
    >
    > ```shell
    > -rwxr--r--. 1 root  root       0 11月  8 03:26 a.sh
    > -rw-r--r--. 1 root  root       0 11月  8 03:26 b.sh
    > ```
    >
    > 为两个文件都添加 suid 权限，得到如下结果：
    >
    > ```shell
    > [root@localhost test]# chmod u+s {a,b}.sh
    > [root@localhost test]# ll
    > 总用量 4
    > -rwsr--r--. 1 root  root       0 11月  8 03:26 a.sh
    > -rwSr--r--. 1 root  root       0 11月  8 03:26 b.sh
    > ```
    >
    > **注意：添加后的 suid 权限，文件名会变为红底白字**

- **S**GID（组）：对目录来说，成为一个**共享目录**。<u>当同组用户在共享目录内新建文件时，任何同组用户都可进行修改</u>

  - 例：现有一个带有特殊权限 `shared` 文件夹，具体如下：

    ```shell
    drwxrwsr--. 2 user  usergroup  6 11月  8 03:32 shared
    ```

    问： `user1` 和 `user2` （两个用户都属于 `usergroup` 组）能否新建文件？

    ```shell
    ## 使用 user1 用户
    [user1@localhost shared]$ touch a.txt
    [user1@localhost shared]$ ll
    总用量 0
    -rw-rw-r--. 1 user1 usergroup 0 11月  8 03:39 a.txt
    
    ## 使用 user2 用户
    [user2@localhost shared]$ touch b.txt
    [user2@localhost shared]$ ll
    总用量 0
    -rw-rw-r--. 1 user1 usergroup 0 11月  8 03:39 a.txt
    -rw-rw-r--. 1 user2 usergroup 0 11月  8 03:39 b.txt
    ```

  - 结果：`user1` 和  `user2` 可以在 `shared` 目录下新建文件，且新建文件的**所属用户为当前用户，所属组为当前用户的所属组**

- S**t**icky（sbi**t**）（其它用户组）：仅针对目录，拥有此权限则为**共享目录（多出现于 temp 目录）**，但**只有所属用户拥有写入权限**，其他用户无法写入

- 示例：为三个文件 `1.txt`、`2.txt`、`3.txt` 添加特殊权限

  ```shell
  -rw-r--r--. 1 root root    0 11月  8 04:31 1.txt
  -rw-r--r--. 1 root root    0 11月  8 04:31 2.txt
  -rw-r--r--. 1 root root    0 11月  8 04:31 3.txt
  ```

  为三个文件分别添加权限：

  ```shell
  [root@localhost ~]# chmod u=rw,g=r,o=r 1.txt	# 给所属用户添加特殊权限
  [root@localhost ~]# chmod 2644 2.txt			# 给所属组添加特殊权限
  [root@localhost ~]# chmod o+t 3.txt				# 给其他用户组添加特殊权限
  [root@localhost ~]# ll
  总用量 8
  -rwSr--r--. 1 root root    0 11月  8 04:31 1.txt
  -rw-r-Sr--. 1 root root    0 11月  8 04:31 2.txt
  -rw-r--r-T. 1 root root    0 11月  8 04:31 3.txt
  -rw-------. 1 root root 1423 11月  5 09:37 anaconda-ks.cfg
  -rw-r--r--. 1 root root 1578 11月  5 09:58 initial-setup-ks.cfg
  drwxr-sr-x. 2 root root  117 11月  8 04:11 test
  ```

  > 注：三个文件所对应的权限标识不同

