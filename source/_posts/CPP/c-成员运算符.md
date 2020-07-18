---
title: c++成员运算符
date: 2016-12-29 23:49:13
categories:
- 技术
tags:
- c++
comments: true
---
总结一下成员运算符的简单知识。

+ 成员运算符有直接成员运算符 `.` 和间接成员运算符 `->` 

+ 直接成员运算符 ‘.’
直接成员运算符主要是在直接引用，类本身以及内部引用时使用。人话就是对象使用
+ 间接成员运算符 ‘->’
间接成员运算符主要是在间接引用时使用。人话就是指向对象的指针使用

+ 有时，C++新手在指定结构成员时，搞不清楚何时应使用句点运算符，何时应使用箭头运算符。规则很简单，如果结构标识符是结构名称，则使用句点运算符（直接成员运算符）；如果标识符是指向结构的指针，则使用箭头运算符。

<b>Example:</b>

```cpp
//利用结构，演示直接成员运算符和间接成员运算符的用法

#include <iostream>

struct inflatable
{
	char name[20];
	float volume;
	double price;
};

int main()
{
	using namespace std;
	inflatable *ps = new inflatable;
	cout << "enter name of inflable item: ";
	cin.get(ps->name,20);
	cout << "enter volume in cubic feet : ";
	cin >> (*ps).volume;
	cout << "enter price : $";
	cin >> ps->price;
	cout << "Name: " << (*ps).name <<endl;
	cout << "volume : "<< ps->volume <<"cubic feet\n";
	cout << "Price: $" << ps->price <<endl;
	system("pause");
	delete ps;
	
	return 0;
}
```
