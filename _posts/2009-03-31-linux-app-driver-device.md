---
title: "大致理清了 Linux 应用、驱动、设备的关系"
date: 2009-03-31 15:05
categories:
- Linux
tags:
- arm
- driver
---

今天嵌入式实验课，研究了一下老师写的 TC 驱动程序（就是利用定时计数器让
LED 狂闪），加上以前看的一点入门的设备驱动知识，大致理清了 Linux
应用、驱动、设备三者的关系，不知道想得对不对。

首先，有 Udev 的话，插入设备时系统会自动在 /dev
目录下生成设备文件，我们的板子上好像没有 Udev，所以自己
mknod，也好指定主设备号。

然后，写驱动，用 register\_chrdev
函数注册一个设备驱动，传入的三个参数分别为
主设备号、设备名、文件操作。文件操作是一个之前定义的 file\_operations
结构体，用来保存各种设备操作的函数的指针，其中重要的一个设备操作是
ioctl，其中传入的操作类型 cmd 是个 int
类型的参数，也就是说，对于设备的操作类型可以有 2 的 32 次方种……

最后，应用程序首先打开 /dev 目录下的设备文件，然后用 ioctl
函数调用驱动中的文件操作，传入的三个参数分别为：设备文件的文件标识符，操作类型，传入的参数。

这样，应用程序就可以通过操作设备文件来操作设备了！

呵呵，不知道理解得对不对，有什么谬误欢迎指点。
