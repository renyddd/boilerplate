---
title: first about Linux
date: 2019-06-26 23:18:42
tags:
---

# first about Linux
## 计算机的组成
我想将计算机的发展历史放到后面来讲，因为自己很想通读计算机历史，弄清每个重要人物的故事。也因为最近想去好好读传记，那这就是个更棒的行为了。

那么现在还是先说说计算机的组成。

现在的计算机还是遵循存储程序结构，及冯诺依曼结构（Von Neumann architecture），或称为普林斯顿结构（Princeton architecture），此为一种将程序指令存储器和数据存储器合并在一起的计算机设计结构。

区别于哈佛架构（Harvard architecture）的，将程序指令存储和数据存储分开的存储结构。

冯诺依曼架构将计算机分为四个主要组成部分：算数逻辑单元（arithmetic logic unit，ALU）、控制电路（control unit）、存储器（memory）及输入输出设备（input and output devices）。
![](https://s1.51cto.com/images/blog/201906/28/633e22b915351a2fb9ccc0cb0ae3de03.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

如果看到这里突然想到 CPU（Central Processing Unit） 为何物，那么：The main difference between CPU and ALU is that the CPU is an electronic circuit that handles instructions to operate the computer， while the ALU is a subsystem of the CPU that performs arithmetic and logical operations.  [[1] PEDIAA](https://pediaa.com/difference-between-cpu-and-alu/) 总之，ALU 是 CPU 的子部分，CPU 专注于处理指令的及时处理并准确地执行，而 ALU 侧重于数学和逻辑推理。

## 内核

首先分享一篇不错的博客：
https://www.ibm.com/developerworks/cn/linux/l-linux-kernel/index.html

我们所谓的完整的操作系统是，kernel + application。而狭义上的 OS 仅仅指 kernel。

内核（kernel）也是个应用程序，它是用来管理软件发出的数据 I/O 要求的程序，并将这些要求转译给 CPU 及其计算机组件。

它是为众多计算机软件提供对计算机硬件的安全访问的软件，由内核决定一个程序在什么时候对什么硬件部分操作多长时间。

由于直接操作计算机硬件很复杂，所以内核提供一种硬件的抽象，来完成此类操作。

我们的 Linux 内核结构在硬件之上，抽象出接口系统调用（System call）来实现操作系统的功能。

the operating system is an interface that allows the application programs to access hardware resources. The kernel is the core of an operating system. The operating system performs major tasks of a computer system such as memory management, process management, securing the data and many more. System call and library call are two terms associated with operating systems. [[2] PEDIAA](https://pediaa.com/difference-between-cpu-and-alu/))

![](https://s1.51cto.com/images/blog/201906/28/c9ed365c762034ebfa5555aeca76f8da.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

## Linux 发行版
Linux 发行版（Linux distribution）是基于 Linux kernel ，由软件组成的操作系统，且用户用软件包管理系统进行应用软件的管理。

软件包管理系统作用：提供在操作系统中安装、升级、卸载目标软件的方法，并提供对系统所有软件状态信息的查询。

在 GNU/Linux 操作系统中，最为常用的两类软件包管理工具为 RPM   与 DPKG。

RPM ，Redhat Package Manager

DPKG ，Debian Package

## 开源协议

这里先连接上别人的文章吧
https://www.jianshu.com/p/a57c13631d5e
等到自己用时，有需求了再来更深入的了解。

## Linux 的哲学思想
- 一切皆文件：把几乎所有资源统统抽象为文件形式：包括硬件设备，甚至通信接口
- 由众多功能单一的程序组成：一个程序只做一件事，并且做好；组合小程序完成复杂任务；
- 尽量避免跟用户交互：目标：易于以编程的方式实现自动化任务；
- 使用文本文件保存配置信息；

未来有更深入的了解时，便会加上自己的见解。

## Linux 的目录

首先来了解一些特殊的目录：

| . |代表此层目录  |
| --- | --- |
| .. | 代表上层目录  |
| - |代表前一个工作目录  |
| ~ |代表目前使用者的家目录  |

还是再来连接上参考资源 [鸟哥的 Linux 私房菜 5.3 Linux 目录配置](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/content/44.html#ps2)

[Filesystem Hierarchy Standard](http://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.pdf)
![](https://s1.51cto.com/images/blog/201906/28/4279591e6a199fff31fad1a7f92e6021.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)
