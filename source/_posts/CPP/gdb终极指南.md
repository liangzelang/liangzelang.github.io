---
title: gdb调试指南
date: 2020-11-16 23:35:23
categories:
- 技术
tags:
- gdb
- 调试与定位
- C++
comments: true
---


### gdb基本入门用法
1. gdb调试编译

* gdb调试需要使用的额外的信息，因此在编译程序的时候需要加上 **`-g`** 参数

2. gdb启动调试
```
* gdb <your_program>
```
3. gdb基本调试命令

* 启动gdb调试：`r`
* 启动程序：
* 添加断点：
* 查看断点：
* 单步执行：
* 跳过当前函数：
* 进入函数(steps into)：
* 查看当前函数调用栈：

懂了以上的基本方法，基本就可以自己愉快的玩耍了。

### gdb进阶用法
1. 多线程调试

* 查看进程：`info inferiors`
* 查看线程：`info threads`
* 查看线程栈结构：`bt`
* 切换线程：`thread n` (n表示第几个线程)

### 参考
[100个gdb技巧](https://wizardforcel.gitbooks.io/100-gdb-tips/content/index.html)