---
title: kernel开发环境
date: 2021-06-27 15:16:39
tags:
---


1. 编译
就是常规的make menuconfig/ make / make bzImage套路，最终生成一个bzImage镜像文件

2. 制作根文件系统
略有一些复杂： 
 - 创建一个目录x
 - 将一些要放入根文件系统的工具编译好，放入x目录中。通常是一些系统启动前需要使用的工具(这些工具一般拷贝busybox的二进制可执行文件会比较简单)
 - 将x目录用cpio指令打包成一个img文件：  find . | cpio -o -Hnewc | gzip -9 > y.img

3. 使用qemu将上述两个镜像加载并启动：  qemu -kernel bzImage -initrd y.img
