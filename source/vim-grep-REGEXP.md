---
title: vim_grep_REGWXP
date: 2019-07-15 14:49:23
tags:
---
## bash 的一些特性
首先要注意一件重要的事，命令行接口和 shell 是不同的，命令行接口仅仅提供了一个使用 shell 的方法。然而 shell 却是可编程的，因此我们才能通过命令来跟内核交流。

我们再来区分几个概念：终端，是个能输入输出的环境。控制台是物理的终端，即为物理存在的实体。shell 是独特的程序，负责解析用户提供的命令。

why bash? 在类 Unix 系统上有大量的 shell 程序，可能最常见的版本就是 bash 了，它代表着 "Bourne again shell"，单词上有那么点书呆子的玩法。不得不说最早的 Unix Shell 是 "Bourne shell"，由 Stephen Bourne 在 1971 年与贝尔实验室命名。

### bash 特性之：hash 命令
hash 为 shell 的内嵌命令，用以记录或显示程序的地址，以键值对的方式缓存此前的命令查找结果与哈希表中，为命令提供完整的路径名。
```shell
help hash
```
### bash特性之：变量
变量名：指向内存空间中的某段起始地址。
变量赋值（赋值时不需要加'$'）：
```shell
name=value
```
此时应注意，变量名等于号赋值间都不能加等号。
bash 中变量无需事先声明，相当于声明与赋值同时进行。变量名仅包括大小写字母 a-z，数字 0-9  与下划线。支持字母与下划线开头，不能使用保留字与标点，因为这对于 bash 有某些特殊意义。当然，还需要做到见名知意。
而**访问变量**你需要加上 '$'，例如如下脚本：
```shell
#!/bin/bash
name="Hello World"
echo $name
```
运行脚本将输出变量的值。下来是关于花括号定界 '${NAME}'，脚本改为如下，在变量 world 后加上一个 s。
```shell
#!/bin/bash
name="Hello World"
echo  "I say $names"
```
期待输出为：I say Hello Worlds
可结果是：I say
之后给输出加上变量的定界，echo "I say ${name}s"，则为期待输出。

**bash 变量类型**
- 本地变量：作用域进位当前 shell 进程。
- 环境变量：作用域为当前 shell 进程及其子进程。
- 局部变量：作用域仅为某代码片段即函数上下文。

环境变量赋值：
```shell
#!/bin/bash
export name=value
# 或者
declare -x name=value
```
撤销变量
```shell
unset name
# 应当注意此处非变量引用，无需加 $
```
当我们希望变量的值不能被改变时，即可使用**只读变量**，存活时间为当前 shell 进程的生命周期，随 shell 的终止而终止，定义方式如下：
```shell
readonly name
# 或者
declare -r name
# 无法重新复制，也无法撤销
# -bash: unset: name: cannot unset: readonly variable
```
### bash 特性之：多命令执行
方法一，无逻辑关系：
~]# COMMAND1; COMMAND2; COMMAND3; ... 
当我们需要进行关机操作时，sync; sync; shutdown now

方法二，与逻辑：
~]# COMMAND1 &&& COMMAND2
遵循短路法则，即 COMMAND1 为假则不会执行 COMMAND2

方法三，或逻辑：
~]# COMMAND1 || COMMAND2
同样遵循短路法则，即 COMMAND1 为真则不执行 COMMAND2，示例
```shell
id $username || useradd $username
```
## shell 脚本编程
我们先来看看 shell 脚本编程的几个特点：解释运行、过程式编程语言、依赖于外部程序文件运行。

首先，编程语言根据运行方式可分为编译运行与解释运行，前者为源代码经过编译器编译为程序文件；而后者则先启动解释器，由解释器边解释源代码边运行。

再来，根据编程模型可分为过程式编程语言与面向对象编程语言，既然程序=指令+数据，则前者以指令为中心来组织代码，数据服务于代码；而后者，以数据为中心来组织代码，围绕数据组织代码。

最后，根据其编程过程中，功能的实现是调用库还是外部的程序文件可来理解，shell 脚本编程利用系统上的命令及编程组件进行编程；而后者则为完整编程。
### 我们为什么需要脚本
通常 shell 是交互式的，意味着它接受你的命令作为输入并执行。有时我们希望能像例行公事一样地执行一大堆命令，为避免我们每次在终端一遍遍地敲击命令，shell 也能将文件作为它的输入，并执行。这帮我们避免了重复性的工作，而这些文件就被称为 shell scripts，通常都是以 .sh 结尾。
### 你的第一个 shell 脚本
脚本文件的第一行，shebang，指出解释器路径即指明解释执行当前脚本的解释器文件，需顶格。
常见的解释器：
```shell
#!/bin/bash
```
或者是 python
```shell
#!/bin/python
```
常用编辑器 vim，执行命令```vim myshell.sh```进入后点击 a 进入输入模式，编辑如下内容。
```shell
#!/bin/bash
echo "hello world"
```
输入完毕，点击 Esc 键进入编辑模式，在点击 : 键进入末行模式，输入 wq 保存并推出文件编辑。

脚本运行，赋予执行权限或直接运行解释器。
前者```chmod a+x /root/myshell.sh```
后者```bash ./myshell.sh```

注意：脚本中除了 shebang 以外，以 # 开头的行皆为注释；shell 脚本的运行是通过运行一个子 shell 进程实现的。

## 文本处理工具
Linux 上文本处理三剑客：
- grep，egrep，fgrep：文本过滤工具
- sed：stream editor，流编辑器
- awk：文本报告生成器，格式化文本
## 正则表达式
Regular Expression，REGEXP
由一类特殊字符及文本字符所编写的模式，其中有些字符不表示其字面意义，而是用于表示控制或通配的功能。一般分为两类：基本正则表达式(BRE)、扩展正则表达式(ERE，即 extended 之意。)
### grep
Global search REgular expression and Print out the line. 文本搜索工具，根据用户指定的模式（过滤条件）对目标文本逐行进行匹配检查，并打印匹配到的行。

模式（pattern）：由正则表达式的元字符及文本字符所编写出的过滤条件。

grep [OPTIONS] PATTERN [FILE]...
其中常用参数有：
-i：ignore-case，忽略字符的大小写 
-o：仅显示匹配到的字符本身
-v，--invert-match：显示不能被模式匹配到的行
-E：支持使用扩展正则表达式
-q，--quiet：静默模式，即不输出任何信息，但命令 echo $? 可查看是否查询成功

-A #：after 之意，再多显示后 # 行
-B #：before 之意，再显示前 # 行
-C #：context 之意，再多显示前后文各 # 行
### 基本正则表达式元字符
其分为四大类：字符匹配、匹配次数、位置锚定、分组及引用，下面一一道来。

**字符匹配：**
|.| 匹配任意单个字符 |
|--|--|
| [ ]  | 匹配指定范围内的任意单个字符 |
| [^] | 匹配指定范围外的任意单个字符 |
几种特殊格式：[a-z], [A-Z], [a-z0-9], [[:upper:]], [[:lower:]], [[:alpha:]], [[:digit:]], [[:alnum:]] 所有字母和数字, [[:space:]] 所有空白字符, [[punct]] 所用标点。

**匹配次数：** 用在要指定其出现的次数的字符后面，用于限制其前面字符的出现次数，默认工作于贪婪模式。
| * | 匹配其前出现的字符任意次，即 0 次 1 次或多次 |
|--|--|
| .* | 匹配任意长度的任意字符 |
| \\? | 匹配其前出现的字符 0 次或 1 次，即其前的字符可有可无 |
| \\+ | 匹配其前出现的字符 1 次或多次，即其前的字符至少要出现一次 |
| \\{m\\} | 匹配其前的字符 m 次 |
| \\{m,n\\} | 匹配其前的字符至少 m 次，至多 n 次 |
| \\{0,n\\} |  匹配其前的字符至多 n 次 |
| \\{m,\\} |  匹配其前的字符至少 m 次 |

**位置锚定：**
| ^ | 行首锚定，用于模式的最左侧 |
|--|--|
| $ | 行尾锚定，用于模式的最右侧 |
| 单词的定义 | 由非特殊字符组成的连续字符串 |
| \\< 或 \b | 词首锚定，用于模式的最左侧 |
| \\> 或 \b | 词尾锚定，用于模式的最右侧 |
再加上几个常用位置锚定的模式，\\<PATTERN\\> 用于精确匹配单词，^$ 则用于匹配空白行。

**分组及引用**  
\\(\\)：将一个或多个字符捆绑在一起，当做一个整体进行处理。因 () 对于 bash 有特殊意义，所以需要转义。
同时注意，组括号中的模式匹配到的内容会被正则表达式引擎自动记录在内部变量中，这些变量为 \1 从左侧起，第一个左括号以及与之匹配的右括号之间的模式所匹配到的字符； \2 从左侧其第二个... 以此类推。

练习题，编辑一下文本内容，保存为 love.txt
```
He likes his lover.
He loves his lover.
She likes her liker.
She loves her liker.
```
分别键入以下命令以了解后向引用。
```shell
grep "l..e.*l..e" love.txt
grep "\(l..e\).*\1" love.txt
```
第一条命令可描述为： l 与两个任意字符后面加一个 e，其后跟任意次数任意字符，再跟上 l 与两个任意字符后面加一个 e，所匹配到的行。
第二条命令可描述为： l 与两个任意字符后面加一个 e，其后跟任意次数任意字符，再引用前面的分组括号中的模式所匹配到的字符，所匹配到的行。

命令执行结果如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190714233113578.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzM5MTMxMA==,size_16,color_FFFFFF,t_70)
那么现在来描述**后向引用** ：引用前面的第#个分组括号中的模式所匹配到的字符。
### egrep
支持扩展正则表达式实现类似于 grep 的文本过滤功能，等同于 grep -E。
egrep [OPTIONS] PATTERN [FILE...]
### 扩展正则表达式元字符
其分为五大类：字符匹配、匹配次数、位置锚定、分组及引用、或，下面一一道来。

**字符匹配：**
|.| 匹配任意单个字符 |
|--|--|
| [ ]  | 匹配指定范围内的任意单个字符 |
| [^] | 匹配指定范围外的任意单个字符 |

**匹配次数：**
| * | 匹配其前出现的字符任意次，即 0 次 1 次或多次 |
|--|--|
| .* | 匹配任意长度的任意字符 |
| ? | 匹配其前出现的字符 0 次或 1 次，即其前的字符可有可无 |
| + | 匹配其前出现的字符 1 次或多次，即其前的字符至少要出现一次 |
| {m} | 匹配其前的字符 m 次 |
| {m,n} | 匹配其前的字符至少 m 次，至多 n 次 |
| {0,n} |  匹配其前的字符至多 n 次 |
| {m,} |  匹配其前的字符至少 m 次 |
相比正则表达式，少了转移符的存在。

**位置锚定：**
| ^ | 行首锚定，用于模式的最左侧 |
|--|--|
| $ | 行尾锚定，用于模式的最右侧 |
| \\< 或 \b | 词首锚定，用于模式的最左侧 |
| \\> 或 \b | 词尾锚定，用于模式的最右侧 |

**分组及引用：**
()：分组：括号内的模式匹配到的字符会被记录于正则表达式引擎的内部变量中，后向引用：\1, \2, ...

**或：**
a|b：a 或者 b，例如：C|cat 意为 C 或 cat，而 (C|c)at 为 cat 或 Cat。
### fgrep
fgrep 则不支持正则表达式元字符，当无需用到元字符去编写模式时，使用 fgrep 效率更高。

## vim 编辑器
vim 是一款优秀的文本编辑器，那么文本意为纯文本信息，例如 ASCII text 或者 Unicode。而文本编辑器有两种，行编辑器与全屏编辑器，前者代表为 sed 后者代表有 nano 与 vim。

再说说 vi 所代表的的意思，visual Interface，而 vim 代表着 vi 的增强版 Vi IMproved。

既然 vim 是模式化编辑器，我们来看看它的基本模式：编辑模式（命令模式）、输入模式（插入模式）、末行模式（内置的命令行接口）

### 打开文件：vim  [OPTIONS]  [FILE]...
先介绍如下几个参数：
- +#：打开文件后，直接让光标出现在第#行的行首
- +/PATTERN：打开文件后，直接让光标处于第一个被 PATTERN 匹配到的行的行首
- +：打开文件后，光标出现在最后一行行首

### 转换模式：默认进入编辑模式。
| 由编辑模式进入输入模式 | 键入以下命令 |
|--|--|
| i | insert，在光标所在处输入 |
| a | append，在光标所在处后方输入 |
| o | 在光标所在处下方打开新的一行 |
| I | 大写 i，在光标所在行的行首输入 |
| A | 在光标所在行的行尾输入 |
| O | 在光标所在处的上方打新的一行 |

而由输入模式进入编辑模式，敲击 **Esc** 键。
由末行模式进入编辑模式，同样敲击 **Esc** 键。

| 关闭文件 | 键入一下命令 |
|--|--|
| ZZ | 编辑模式下，保存并退出 |
| :q | 末行模式下的退出 |
| :!q | 强制退出，不保存此前的编辑 |
| :wq | 保存并退出 |
| :x | 保存并退出 | 
| :w /PATH/TO/SOMEFILE | 保存为新文件，跟上路径 |
| :r /PATH/FROM/ SOMEFILE | 将指定文件的文本读入指定位置 |

### 光标的跳转
使用 vim 时，双手尽可能多的停留在键盘核心区域，因此你需要熟练掌握。如果你需要通过游戏来练习，那就推荐给你 GitHub 上的开源项目 [PacVim](https://github.com/jmoon018/PacVim) ，在吃豆人的游戏中熟练掌握 vim 编辑器。

字符间跳转：h，j，k，l，分别对应方向键左，下，上，右。当然 #COMMAND 意为向某方向跳转#个字符。
| 单词间跳转| 由非特殊字符组成的连续字符串 |
|--|--|
| w | 跳转至下一个单词的词首 |
| e | 当前或后一个单词的词尾 |
| b | 当前或前一个单词的词首 |
| #COMMAND | 跳转#指定个数的单词 |
接下，
| 行首行尾、行间 | 以及句间、段间的跳转 |
|--|--|
| ^ | 跳转至行首的第一个非空白字符 |
| 0 | 跳转至行首 |
| $ | 跳转至行尾 |
| #G | 跳转至由#指定的行 |
| 1G，gg | 跳转至首行 |
| G | 跳转至最后一行 |
|（ | 跳转至此句或前一句的句首 |
| ）| 跳转至下一句句首或词句句尾 |
| { 与 } | 即可实现段间的跳转 |

### vim 编辑模式下的命令：
| 编辑模式下输入 | 功用| 
|--|--|
| x | 删除光标所在处的字符 |
| #x | 删除光标所在处起始的#个字符 |
| xp | 交换光标所在处与其后面的字符的位置 |
| r | replace，替换命令，后面跟上要替换的字符 |
| d | 删除命令，可结合光标跳转命令，实现范围删除 |
| dCOMMAND | d$, d^, dw, de, db, dd 删除删除整行，注意删除内容将放置缓冲区 |
| p | paste，黏贴命令，缓冲区中的内容若为整行在黏贴在光标向所在行的下方，否则在后方 |
| y | yank，复制命令，工作行为相似于 d 命令 |
一些其他的编辑操作：
进入可视化模式，v：按字符选定，V：按行选定。选定后即可结合编辑命令使用。
u：撤销操作。#u：以为撤销此前的#个命令。
Ctrl + r：撤销此前的撤销操作
. ：重复执行前一个编辑操作
再介绍 vim 的自带练习教程，直接执行 vimtutor 即可进入教程。

**vim 末行模式：** 内建的命令行接口。
1、地址定界：start_pos[,end_pos]
其中，#：表示特定的行，. 表示当前行，% 全文。
2、查找：
/PATTERN：从光标所在处开始向文件尾部查找能被当前模式匹配到的所有字符串
?PATTERN：从光标所在处开始向文件首部查找能被当前模式匹配到的所有字符串
对结果键入 n 则表示，跳转至与命令方向相同的下一个，N 则表示与命令方向相反的上一个。
3、查找并替换，s
s/要查找的内容/替换为的内容/修饰符
要查找的内容：可使用正则表达式
替换为的内容：则不能使用正则表达式，前部分若使用分组符号即可后向引用，或使用 & 直接引用模式匹配到的全部文本。
修饰符：i：忽略大小写，g：全局替换
可以把分隔符替换为其他非常用字符，避开对路径分隔符的转义，例如 s@@@
举例说明：
```
%s@\<t\([[:alpha:]]\+\)\>@T\1@g
全局查找以小写 t 开头的单词，并将之替换为大写 T
%s@\<t[[:alpha:]]\+\>@&er@g
全局查找以小写 t 开头的单词，并给之后面加上 er
```
### vim 的多文件、多窗口功能
多文件：vim FILE1 FILE2 ... ，而在编辑模式下:next，:prev，:first，:last 实现文件间的切换。此时的 q 为退出当前文件，wqall，wall，qall! 则为对应含义。

多窗口：加上参数 -o：小写 o 为水平分隔窗口，-O：大写 O 为垂直分隔窗口。在窗口间切换，则先 Ctrl + w，后再加方向键。不过同时，单个文件也可分割为多个窗口，Ctrl + w，s 进行水平分隔。而 Ctrl + w，v 进行垂直分隔。
