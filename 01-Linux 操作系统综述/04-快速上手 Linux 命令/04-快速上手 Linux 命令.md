## 用户和密码：

修改密码：

passwd，其实就是 password 的简称。

```
# passwd
Changing password for user root.
New password:
```



添加用户：

```
useradd <username> 创建一个用户
passwd <username>  设置用户的密码
```



命令帮助：

通过 -h 参数看一下，它的意思是 help

还可以通过 `man useradd` 来查看更加详细的文档

```
[root@deployer ~]# useradd -h
Usage: useradd [options] LOGIN
       useradd -D
       useradd -D [options]


Options:
  -g, --gid GROUP               name or ID of the primary group of the new account
```



通过文件来查看用户:

Linux 是: “命令行 + 文件” 的模式，通过命令创建的用户都存放在 `/etc/passwd` 文件中。

/root 和 /home/cliu8 是什么呢？它们分别是 root 用户和 cliu8 用户的主目录。主目录是用户登录进去后默认的路径。

/bin/bash 配置交互式命令行

```
# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
......
cliu8:x:1000:1000::/home/cliu8:/bin/bash


# cat /etc/group
root:x:0:
......
cliu8:x:1000:
```

`cliu8:x:1000:` ，其中 `cliu8` 是用户名，`x` 的位置是密码，后边分别是 用户ID 和 组ID `/etc/group`

```
->/etc cat group
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,lijh
```



切换目录和查看文件:

`cd` ==> `change directory`

`ls` ==> `list` 列出当前目录下的文件

==> `.` 代表当前目录 ==> `..` 代表上一级目录

==> `ls -l` 用列表的方式列出文件

```
# ls -l
drwxr-xr-x 6 root root    4096 Oct 20  2017 apt
-rw-r--r-- 1 root root     211 Oct 20  2017 hosts
```

`字段1` : `drwxr-xr-x` ==> 文件类型 + 文件权限

`字段2`: `6` ==> 硬链接数目

`字段3` : 第一个 `root` 是所属用户

`字段4` : 第二个 `root` 是所属组

`字段5` : 文件大小

`字段6` : 文件修改日期

`字段7` : 文件名



`字段1` 详细解释:

`drwxr-xr-x` 第一个字符 `d` 表明这是一个目录 `directory`

文件权限: `-rw-r--r--` 第一个字符 `-` 表明这是一个普通文件

`rwx` ==> 分别标识 读 写 执行。字母，就说明有这个权限；如果是横线，就是没有这个权限

三个为一组，分别表示 `用户权限` `组权限` `其他用户权限`

`-rw-r–r--` 就可以翻译为，这是一个普通文件，对于所属用户，可读可写不能执行；对于所属的组，仅仅可读；对于其他用户，也是仅仅可读。

改变文件权限: `chmod 711 hosts`



## 安装包安装软件

安装软件:（使用安装包安装）

Linux 系统分为两大体系: `CentOS` 和 `Ubuntu` 

`CentOS` : 使用 `rpm` 格式的安装包文件 `rpm -i jdk-XXX_linux-x64_bin.rpm`

`Ubuntu` : 使用 `deb` 格式的安装包文件 `dpkg -i jdk-XXX_linux-x64_bin.deb`

其中 `-i` 就是 `install` 的意思



查看安装程序列表:

`CentOS` : `rpm -qa`  `(q = query ; a = all ; query all)`

`Ubuntu` :  `dpkg -l` `(l = list)`



搜索某个软件是否已经安装:

`CentOS` : `rpm -qa | grep jdk` 搜索是否安装了 `jdk`，`grep` 会展示出含有 `jdk` 字串的行

`Ubuntu` : `dpkg -l | grep jdk` 搜索是否安装了 `jdk`，`grep` 会展示出含有 `jdk` 字串的行



使用 `more` 和 `less` 可以进行分页:

`more` : 只能向后翻页 ; `less` : 前后都能翻页 ; `q` 退出 `quit`

如: `dpkg -l | more` 或 `dpkg -l | less`



卸载软件: （没有软件管家的情况下）

`CentOS` : `rpm -e`  ==> `erase`

`Ubuntu` : `dpkg -r` ==> `remove`



## 软件管家安装软件

配置软件管家的服务商（配置文件路径）：

`CentOS` : `/etc/yum.repos.d/CentOS-Base.repo`

`Ubuntu` : `/etc/apt/sources.list`



搜索软件商店的软件：

`CentOS` : `yum search <software_name>`

`Ubuntu` : `apt-cache search <software_name>`

再用 `grep` `more` `less` 过滤



安装软件:（使用软件管家）

`CentOS` : `yum install <software_name>`

`Ubuntu` : `apt-get install <software_name>`

对于 `Linux` 中安装一个软件后:

1. 主执行文件会放在 /usr/bin 或者 /usr/sbin 下面
2. 库文件会放在 /var 下面
3. 配置文件会放在 /etc 下面



## 通过解压文件安装软件

安装 `zip` 压缩文件的解压工具

```shell
yum install zip.x86_64 unzip.x86_64
apt-get install zip unzip
```

解压 `tar.gz` 压缩文件

`tar xvzf jdk-XXX_linux-x64_bin.tar.gz`



## 配置环境变量

只在当前打开的命令行生效（使用 `export` 命令）

在 `PATH` 中，每个路径之间使用 `:` 分割开，并且在最后将 `PATH` 中原有的值拼接上去

```
export JAVA_HOME=/root/jdk-XXX_linux-x64
export PATH=$JAVA_HOME/bin:$PATH
```



让环境变量永久生效：

在家目录中的 `.bashrc` 文件中配置环境变量，该文件也是一个脚本文件，每次打开一个新的命令行的时候自动执行，也可以 `source .bashrc` 来手动执行。

如果用的 `shell` 是 `zsh` 那么就有对应的 `.zshrc` ，每个用户都有一个自己的 `.bashrc`

`source <filename>` 命令：`source filename [argements]Read and execute commands from filename in the current shell environment and return the exit status of the last command executed from filename.`执行 `filename` 文件中的指令，并配置到当前的 `shell` 环境中

查看所有文件:`ls -la` `（a = all）`



## 运行程序

运行程序有三种方式

1. 在终端打开，而当终端关闭的时候，程序的运行也会停止
2. 让程序在后台运行，通过 `kill -9` 命令来关闭程序
3. 以 `服务` 的形式来运行程序



在终端打开的时候，运行程序：

1. 通过 `./filename` 来执行当前目录下拥有 `x` 权限的文件（程序）
2. 在 `PATH` 中配置该文件，那么直接输入文件名就可以执行该文件了



在后台运行程序：

`nohup` 命令 `(no hang up)` `不挂起`

最后的 `&` 符号表示让该程序在后台执行

`nohup command >out.file &`

- `1` 表示标准输出文件描述符
- `2` 表示标准错误输出文件描述符
- `2>&1` 表示把标准输出文件和标准错误输出文件合并在一起文件中

`nohup command >out.file 2>&1 &`



以服务的形式来运行程序

1. 在 `Ubuntu` 中，一个程序要变成 `服务` 是因为在目录 `/lib/systemd/system` 文件下会有一个 `xxx.service` 的文件，该文件定义了 `xxx` 程序如何启动，如何关闭
2. 启动服务: `systemctl start <service_name>` 
3. 关闭服务: `systemctl enable <service_name>`



1. 在 `CentOS` 中的 `MySQL` : 
2. 在 CentOS 里有些特殊，MySQL 被 Oracle 收购后，因为担心授权问题，改为使用 MariaDB，它是 MySQL 的一个分支。通过命令yum install mariadb-server mariadb进行安装，命令systemctl start mariadb启动，命令systemctl enable mariadb设置开机启动。同理，会在 /usr/lib/systemd/system 目录下，创建一个 XXX.service 的配置文件，从而成为一个服务



## 关机和重启

1. 关机：`shutdown -h now`
2. 重启: `reboot`



## 关闭程序（进程）

1. `ps -ef |grep 关键字 |awk '{print $2}'|xargs kill -9`
2. `awk` 用来处理文本，其中 `$2` 表示文本的第二列，也就是 `进程ID` `PID`
3. `xargs` 用来传递参数给后边的 `kill -9` 命令

![image-20220729181826204](04-%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B%20Linux%20%E5%91%BD%E4%BB%A4.assets/image-20220729181826204.png)
