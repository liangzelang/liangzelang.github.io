---
title: C++指针注意事项
date: 2016-12-29 23:34:47
categories:
- 技术
tags:
- c++
comments: true
---

+ 一定要在对指针使用解除运算符（*）之前，将指针初始化为一个确定的适当的地址，这是关于使用指针的金科玉律。

+ 使用delete的关键在于，将他用于new分配的内存。这并不意味这要使用用于new的指针，而是用于new分配的地址。
<b>Example:</b>
```cpp
int *ps = new int;    //allocate memory
int *pq = ps;         //set second pointer to same block
delete pq;			  //delete with second pointer
```
以上方法为同一使用new分配的空间，创建了两个指针，通常不会这么做，这将增加删除同一内存块两次的可能性。遵守

+ 使用new和delete时，需要以下规则。
1. 不要使用delete来释放不是new分配的内存；
2. 不要使用delete释放同一个内存块两次；
3. 如果使用new []为数组分配内存，则应使用delete []来释放
4. 如果使用new []为一个实体分配内存，则应使用delete来释放
5. 对空指针使用delete是安全的
