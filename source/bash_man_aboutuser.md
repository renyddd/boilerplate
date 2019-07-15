---
title: secon-about-Linux
date: 2019-07-01 18:44:50
tags:
---
## 关于命令
命令的语法通用格式：
**~]#  COMMAND OPTIONS ARGUMENTS**

### COMMAND
把该命令启动为一个进程，命令运行中的特性由 OPTIONS 进行修正，并且作用在所给的参数上。

**发起一个命令**，相当于请求内核将某个二进制程序运行为一个进程。
此时，程序 --> **进程：**
- 程序是由一组特别的指令构成的可执行二进制文件，目的是完成您电脑上的一些特定的工作。
- 进程是某个特定程序的执行，他有被限定的寿命周期。[guru99](
https://www.guru99.com/program-vs-process-difference.html)

命令本身是一个可执行的二进制文件，其还同时可能调用**共享库文件。**

- [root@study ~]# file /lib   -->  /lib: symbolic link to **usr/lib**

- [root@study ~]# file /lib64  --> /lib64: symbolic link to **usr/lib64**

- /usr/local/lib


**Linux supports two classes of libraries**, namely:
- Static libraries?– are bound to a program statically at compile time.
- Dynamic or shared libraries?– are loaded when a program is launched and loaded into memory and binding occurs at run time. [tecmint](https://www.tecmint.com/understanding-shared-libraries-in-linux/)
- 共享库无执行入口，只可被调用。

多数的程序文件都存放在：/bin, /sbin, /usr/bin, /usr/sbin
- [root@study ~]# file /bin --> /bin: symbolic link to **usr/bin**

- [root@study ~]# file /sbin --> /sbin: symbolic link to **usr/sbin**

- 注意：并非所有命令都有在某目录下的可执行文件。

命令必须遵循特定的格式规范：ELF（Linux）

- ELF is the abbreviation for **Executable and Linkable Format**?, and defines the structure for binaries, libraries, and core files. [linux-audit](https://linux-audit.com/elf-binaries-on-linux-understanding-and-analysis/)
- ~] file /bin/ls （下文）
- ~] readelf -a /bin/ls  （查看文件结构）

**命令的分类：**

**内置命令( built-in command)：** 内置在 shell 解释器中的 Linux/Unix 命令 ，优点：提高性能 ：但我们经常从 RAM 中执行指令时，这将减少从磁盘读取的延迟，从而提高性能（increase performance。）[linuxnix](https://www.linuxnix.com/what-is-a-built-in-command-in-linux/)
**列出内置命令**  
~]# compgen -b
**检查是否为内置命令** 
~]# type COMMAND

**外部命令( program executables [ file system command])** 
独立的可执行文件，文件名即程序名
当我们执行一个命令的时候，Linux 会在存储在环境变量 $PATH 的目录中，从左至右的寻找某个具体的可执行文件。
当然你想看看 $PATH 的 directories，~]# echo $PATH

**关于 shell（壳程序） ：** One important thing to note is that the command line interface is different from the shell, it only provides a means for you to access the shell. The shell, which is also programmable then makes it possible to communicate with the kernel using commands. [tecmint](https://www.tecmint.com/understanding-different-linux-shell-commands-usage/)
壳程序 shell 是独特的程序，负责解析用户提供的命令。

**the difference between shell and command line：** The “shell” is software that lets you interact with your computer via a “command line” — a text-only, line-based, input feed. [quora](https://www.quora.com/What-is-the-difference-between-command-line-and-shell)
Short answer from [stackExchange](https://askubuntu.com/questions/506510/what-is-the-difference-between-terminal-console-shell-and-command-line)
- terminal：test input/output enviornment
- console: physical terminal
- shell: command line interface

**why bash？** On Unix-y systems, there are a gazillion shells. Probably the most common modern one is “bash” which stands for “Bourne again shell”, a little nerdy play on words. (One of the firs Unix shells was the “Bourne shell” named after Jason Bourne. Hm, no, named after Stephen Bourne at Bell labs in like 1971. [quora](https://www.quora.com/What-is-the-difference-between-command-line-and-shell)

**type 命令** 
显示指定命令的类型，?It displays if command is an alias,shell function, shell builtin, disk file, or shell reserved word. [sharadchhetri](https://sharadchhetri.com/2014/04/04/type-command-display-information-command-type-linux/)
SYNOPSIS： type [-afptP] name [name ...]

### OPTIONS
指定命令的运行特性；或者说是调整命令运行时所要执行的代码。

选项有两种**表现形式**
- 长选项: --word, 例如: --help, --human-readable, 注意：长选项不能合并。
- 短选项: -C, 例如: -l, -d, 注意：有些命令的选项没有 - ，同一命令使用多个短选项，多数可合并。

注意：有些选项可以带参数，此称为选项参数。

### ARGUMENTS
命令的作用对象；或者说是命令对什么生效

注意：不同命令的参数，带多个参数一空白符分隔

### 获取命令的帮助
内部命令：help COMMAND

**外部命令：**
- 命令自带简要格式的使用帮助： ~]# COMMAND --help
- 使用手册  manual：~]# man COMMAND
- 获取命令的在线文档：~]# info COMMAND
- 很多应用程序自带帮助文档：/usr/share/doc/APP-VERSION

### 关于 man 
**位置：**/usr/share/man
**布局:**
- NAME: 功能性说明
- SYNOPSIS: 语法格式
- DESCRIPTION: 描述
- OPTIONS： 选项
- EXAMPLES： 使用示例
- AUTHOR: 作者
- BUGS: 报告程序 bug 的方式
- SEE ALSO： 参考

**Sections（章节、区块）**
/usr/share/man/man* 压缩格式的文件，存储省空间而已。读取类似 zcat 命令。
| 1 | user commands（一般命令）  |
| --- | --- |
| 2 | system calls（系统调用）  |
| 3 | library functions（库函数）  |
| 4 | special files（特殊文件及设备文件）  |
| 5 | file formats（文件格式和约定）  |
| 6 | games（游戏）  |
| 7 | miscellany（杂项）  |
| 8 | system-administration commands, daemons（系统管理命令和进程守护）|

默认1~8顺序查看，可指定章节查看

**查看命令章节**  
~]# whatis COMMAND
whatis - display manual page descriptions

## 此部分相关命令

### pwd
print name of current/working directory（显示工作目录)
**SYNOPSIS：**   pwd [OPTION]...

~]# pwd -L: Prints the symbolic path.
~]# pwd -P: Prints the actual path.
The $PWD variable value is same as pwd -L. [geeksforgeeks](https://www.geeksforgeeks.org/pwd-command-in-linux-with-examples/)
**例如：**
[root@study bin]# file /bin
/bin: symbolic link to ‘usr/bin’
[root@study bin]# pwd -L
/bin
[root@study bin]# pwd -P
/usr/bin
[root@study bin]# echo $PWD
/bin
[root@study bin]#

### file
determine file type（查看文件内容类型）
**SYNOPSIS:** file [FILE]...

### cd
 change  the  current  directory  to dir. 
**SYNOPSIS:** cd [dir]
- 不带参数时：返回家目录，在 bash 中，~表示家目录
- cd ~：切回自己的家目录
- cd ~USERNAME：切换至指定用户的家目录
- cd -：在上一次所在目录与当前目录之间来回切换

**相关的环境变量**
- $PWD：当前工作目录
- $OLDPWD：上一次的工作目录

### ls
list directory contents
**SYNOPSIS:** ls [OPTION]... [FILE]...
- -a: 显示所有文件，包括隐藏文件
- -l: --long，长格式列表，及显示文件的详细属性信息
	drwxr-xr-x.  2 root root   159 1月  26 18:35 account
    d：文件类型，-，d，b，c，l，s，p
    rwxr-xr-x
    左三位：文件属主的权限
    中三位：文件属组的权限
    右三位：其用户的权限
    2：数字表示文件被硬链接的次数
    root：文件的属主
    root：文件的属组
    159：数字表示文件的大小，字节是单位
    1月  26 18:35：文件最近一次被修改的时间
    account：文件名
- -h：--human-readable，对文件大小单位换算，换算后的结果可能会不太精确
- -d：查看目录自身，而非其内部的文件列表
- -r：reverse，逆序显示
- -R：recursive，递归显示
- -t： sort by modification time, newest first

### cat
concatenate files and print on the standard output（文本文件查看工具）
**SYNOPSIS:** cat [OPTION]... [FILE]...
- -n：给显示的文本行编号
- -E：显示行结束符$
- ~]# cat /etc/fatab /etc/passwd(将文件链接一起显示)
- copy the contents：~]# cat /etc/fsatb > cattest.txt
- append the contents：~]# cat file1 >> file2
- tac 逆序显示文本

### echo
display a line of text
**SYNOPSIS:** echo [SHORT-OPTION]... [STRING]...

- -n 输出字符串后不换行
[root@study ~]# echo -n "hello" 
hello[root@study ~]# 
- -e 让转义字符生效（enable interpretation of backslash escapes）
[root@study ~]# echo -e "hello\nworld"
hello
world
[root@study ~]# 

### shutdown
关机或重启命令
**SYNOPSIS:** shutdown [OPTIONS...] [TIME] [WALL...]

- -P, wer-off（选项）
- -h, halt. [the diference between halt and shutdown commands](https://unix.stackexchange.com/questions/8690/what-is-the-difference-between-halt-and-shutdown-commands)
- -r，reboot
- -k，just kidding，只发关机信息
- -c，cancel，取消关机计划

TIME：+m，值从现在开始，m 分钟后进行，now 代表着 +0，若果没有则默认为 +1。

### which
显示命令的整个路径
**SYNOPSIS:** which [options] [--] programname [...]

### whereis
定位命令的二进制文件、源文件或者帮助手册。
具体按需进行 man whereis

### who
显示谁登录

## Bash 的基础特性
Bash 是一款兼容 sh 命令语言的解释器，通过标准输入或是文件来读取命令。同时 Bash 也集合了 ksh 和 csh 有用的特性。
### 命令历史
shell 进程会在其会话中保存此前用户提交过的命令历史。

~]# history  显示或是操作命令历史列表。
几个相关的**环境变量：**
- HISTSIZE: shell 进程可保留的历史命令条数
- HISTFILE: 持久保存命令历史的文件：.bash_history
- HISTFILESIZE: 命令历史文件的大小
**参数：**
- -c：清空命令历史
- -r：从文件读取命令历史至列表
- -w：把列表追加至文件
- history #：显示最近#条命令
**Run Commands From Your History: ** 

 - !#：再次执行历史列表中的第#条
 - !!：再次执行上条命令
 - !STRING：再次执行列表中最近一个以 STRING 开头的命令
- !-#：再次执行导数第#个命令
- You can append a?**:p**?to any of the above expansions and bash will print the command to the terminal without running it.?[How-to Geek](https://www.howtogeek.com/howto/44997/how-to-use-bash-history-to-improve-your-command-line-productivity/)
**调用上调命令的参数**
- 先按下 Esc 键，松开后再按 .
- COMMAND !$ ，这样只是引用命令行的最后一个参数
- COMMAND !* ，引用上条命令的全部参数
**控制历史命令的记录方式**
环境变量：HISTCONTROL
- ignoredups：忽略重复的命令
- ignorespace：忽略以空白字符开头的命令
- ignoreboth：以上同时生效
### 补全机制
shell 程序在接收到用户执行命令的请求，分析完成之后，最左侧的字符串会被当做命令。
命令补全：根据 PATH 中设置的变量，自左向右地搜索目录下的文件名；给定的打头字符串如果能唯一标识某命令程序文件，则直接补全，否在再次敲击 Tab 会给出列表。
路径补全：在给定的起始路径下，以对应路径下的打头字符串来逐一匹配起始路径下的每个文件。
### 命令行展开
- ~：自动展开为用户的家目录，或是指定用户的家目录。
- {}：可承载一个以逗号分隔的路径列表，并将其展开为多个路径。
**例：**/tmp/a{1,2}  --> /tmp/a1 , /tmp/a2
### 命令的执行状态、结果
bash 通过**状态返回值**来输出此结果：
- 成功：0
- 失败：1-255
命令执行完成后，其状态返回值会保存在 bash 的特殊变量 **$?** 中，但需立即获取。
**引用命令的执行结果**
~]# echo $(COMMAND)
### 引用
命令的引用``，**两个反引号**
``` shell
[root@study ~]# echo `whatis echo`
echo (1) - display a line of text echo (1p) - write arguments to standard output echo (3x) - curses input options
[root@study ~]#
```

此外引用是对引用对象为**变量**时而言的：
```sehll
root@study ~]# echo '$PATH'
$PATH
[root@study ~]# echo "$PATH"
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
[root@study ~]# 
```
### 快捷键
- Ctrl + a：跳转至命令行**行首**
- Ctrl + e：跳转至命令行**行尾**
- Ctrl + u：删除行首至光标所在处之间所有字符
- Ctrl + k：删除光标所在处至行尾的所有字符
- Ctrl + l：清屏，相当于 clera 命令

### globbing 文件名通配
匹配模式：元字符

Before we move to file globbing, let’s understand what are wildcard patterns, these are the patterns containing strings like??,  * .
File globbing is the operation that recognises these patterns and does **the job of file path expansion.** [geeksforgeeks](https://www.geeksforgeeks.org/file-globbing-linux/)
- \* ：匹配任意长度的任意字符
- ? ：匹配任意单个字符
- [] ：匹配指定范围内的任意单个字符
- [^] ： 匹配指定范围外的任意单个字符

几种**特殊格式**
[a-z], [A-Z], [a-z0-9]
[[:upper:]]：所有大写字母
[[:lower:]]：所有小写字母
[[:alpha:]]：所有字母
[[:digit:]]：所有数字
[[:alnum:]]：所有字母和数字
[[:space:]]：所有空白字符
[[:punct:]]：所有标点

### IO 管道及重定向
程序的**数据流：**
- 输入的数据流：标准输入（stdin），键盘，0
- 输出的数据流：标准输出（stdout），显示器，1
- 错误的输出流：错误输出（stderr），显示器，2
[文章分享](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-i-o-redirection)

**IO 重定向：**
输出重定向：
- > ，覆盖输出
- \>\>，追加输出
- 2>, 2>>，错误输出流重定向
- &>, &>>，合并正常、错误输出流
- 或者 COMMAND >> /PATH/TO/SOMEWHERE 2>&1

特殊设备： **/dev/null**
**输入重定向：<**
**tr：**
 translate or delete characters.  
 tr [OPTION]... SET1 [SET2]
 把输入数据当中的字符，凡是在 SET1 定义的范围内出现的，统统转换为 SET2 出现的字符。
 用法一：
 
```
~]# tr  SET1  SET2  <  /PATH/FROM/SOMEFILE
```
用法二：
```
~]# tr  -d  SET1  <  /PATH/FROM/SOMEFILE
```
**<<：here document**
```
~]# cat  <<  EOF
~]# cat  >  /PATH/TO/SOMEFILT  << EOF
```
EOF 只是结束输入的标识，可以替换为任意字符。End Of File

**管道（pipe）**
链接程序，将前一个命令的输出，直接定向后一个程序当做输入当做数据流
```
~]#  COMMAND1 | COMMAND2 | COMMAND3 | ...
```
 **tee:**
 read from standard input and write to standard output and files.
 -a, --append：append to the given FILEs, do not overwrite
 
## 此部分相关命令
### 文本查看
more, less, head, tail
### stat
显示文件或文件系统的状态；
文件有**两类数据：**
- 元数据：metadata
- 数据：data
**时间戳（timestamp）：**
- Access: 2019-07-03 10:18:03.172099206 +0800
- Modify: 2019-07-03 10:18:03.175099206 +0800
- Change: 2019-07-03 10:18:03.175099206 +0800
- Birth: -

### touch
change file timestamps；Update the access and modification times of each FILE to the current time.
touch [OPTION]... FILE...
- -c：不创建新文件
- -a：仅修改 access time
- -m：仅修改 modify time
- -t STAMP  [[CC]YY]MMDDhhmm[.ss]

### cp
copy files and directories；
**SYNOPSIS：**
- cp [OPTION]... [-T] SOURCE DEST
- cp [OPTION]... SOURCE... DIRECTORY
- cp [OPTION]... -t DIRECTORY SOURCE...

Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.
- -i：交互式复制，即覆盖之前提醒用户确认
- -f：force，强制覆盖目标文件
- -r，-R：递归复制目录
- -d：复制符号链接本身，而非指向的源文件
- -a：-dR --preserve=all，archive，用于实现归档
- --preserve=
 mode：权限
 ownership：属主属组
 imestamps：时间戳
 xattr：扩展属性
 links：符号链接
 all：上述所有
 
 
## 用户、组和管理权限
既然我们的操作系统是 multi-task 和 multi-users 多人多任务，那就必须要进行用户、组和权限管理。
每个使用者都有用户标识和密码，**3A：**
- 认证：Authentication
- 授权：Authorization
- 审计：Audition
### user
**用户类别：**
管理员
普通用户：有系统用户和登录用户两类
   
 **用户标识：**
UserID，UID 为16bit 二进制数字：0~65535
其中，管理员为 0
普通用户：1~65535：
系统用户：1-499（CentOS6），1-999（CentOS7）
登录用户：500-60000（CentOS6），1000-60000（CentOS7）

**名称解析：** Username <--> UID ，根据名称解析库进行：/etc/passwd
### group
**组类别1：**
管理员组
普通用户组：有系统用户组和登录用户组两类
组标识： 
管理员组：0
普通用户组 ：1~65535：
系统用户组：1-499（CentOS6），1-999（CentOS7）
登录用户组：500-60000（CentOS6），1000-60000（CentOS7）
名称解析：Groupname <--> GID，根据解析库：/etc/group

**组类别2：**
用户的基本组
用户的付家组

**组类别3：**
用户私有组：组名相同，且只包含一个用户
公共组：组内包含多个用户
### 认证消息
通过对比事先的存储，检查与登录时提供的信息是否一致
/etc/passwd
/etc/shadow

Linux 上的**加密算法**
- md5：message digest, 128bits
- sha 系列：secure hash algorithm; (sha1sum, sha224sum, sha256sum, sha384sum, sha512sum)
```
~]# echo "I can see the shadow" | md5sum
```
查看**用户信息库：**
```
~]# whatis passwd
~]# man 5 passwd
```
信息如下：
name:password:UID:GID:GECOS:directory:shell
- name：用户名
- passwd：可以使加密的密码，也可以时占位符 x
- UID, GID
- GECOS：注释信息
- directory：用户家目录信息
-shell：用户的默认 shell，登录时默认打开的 shell 程序
```
~]# whatis shadow
~]# man 5 shadow
```
### 安全上下文
进程以发起者的身份运行，进程对文件的访问权限，取决于发起此进程的用户的权限。

此时解释**系统用户：**为了能够让后台进程或服务类进程以非管理员的身份运行，通常需要为此创建多个普通用户，但这类用户从不用登录系统。

**id：**
print real and effective user and group IDs

**su：**
run a command with substitute user and group ID
SYNOPSIS：
su [options...] [-] [user [args...]]
DESCRIPTION：
When called without arguments su defaults to running an interactive shell as root.
OPTIONS：
- -, -l, --login：Starts the shell as login shell with an environment similar to a real login:登录式切换：会通过读取目标用户的配置文件来重新初始化
- -c command, --command=command：Pass command to the shell with the -c option.参考 man 文档
