---
title: C++中如何输出char型指针地址
date: 2017-03-12 07:07:40
categories:
- 技术
tags:
- c++
comments: true
---

+ C++ 中std::cout对于char类型的指针，将默认输出这个类型指向的字符串，也就是：
```
char * temp = "Hello, CPP.\n";
std::cout << temp << endl;

OUTPUT:  Hello, CPP
```
那么我们要想输出这个字符串的地址怎么办？？？
+ 首先，我们要清楚为什么会这样呢，因为如果是int类型的指针就不会呀。这是因为C++标准库中IO类对'<<'运算符进行了重载，因此呢， 在遇到 char * 类型数据的时候，就自动使用相应的重载函数，把那个char型指针指向的字符串输出了。
+ 那么，How 2 solve this problem? 类型转换呀，你把它转换为int型指针？ 这是可以的， 但是这时下策。使用'static_cast<const void *>',把char型指针转换为无类型指针。如下：
```
char * temp = "Hello, CPP.\n";
std::cout << temp << endl;
std::cout << static_cast<const void *>(temp) << endl;
OUTPUT:  
Hello, CPP
某个地址
```
+ 一些学渣还会有疑问，static_cast<>()这是什么鬼？那么拾起那么《C++ primer》，你会发现C++还是很有趣的。
