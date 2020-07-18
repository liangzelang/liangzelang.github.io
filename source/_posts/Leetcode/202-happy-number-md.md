---
title: 202.happy number
date: 2016-12-23 10:08:24
categories:
- 技术
tags:
- leetcode
- 数据结构与算法
comments: true
---

+ write an algorithm to determine if a number is 'happy'

+ A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay),or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

<b>Example:</b> 19 is a happy number

```
           1^2 + 9^2 = 82
           8^2 + 2^2 = 68
           6^2 + 8^2 = 100
           1^2 + 0^2 + 0^2 = 1
```

# 解析
+ <b>翻译</b>： 判断一个正整数是否为一个happy-number，一个正整数各位的平方和代替次正整数，不断迭代，如果迭代出1，则为happy-number，否则不是而且会出现无限循环。
<!--more-->
+ <b>思路</b>：写一个函数分离正整数的各位，计算平方和并返回，主函数中需要有两类迭代，一类迭代每次迭代一次，一类迭代两次，这两如果出现无限重复循环肯定OK；循环结束的条件为迭代结果为1（这就是happy number）或者两类迭代结果相等（出现无限重复循环，不是happy number）。

## C/C++

```cpp
	int sum(int n) {
	    int i=0;
	    int sum=0;
	    while(n) {
	        i=n%10;
	        n=n/10;
	        sum=sum+i*i;
	    }
	    return sum;
	}

	bool isHappy(int n) {
	    int temp=n;
	    bool overflag=true;
	    bool flag=true;
	    int temp1= temp;   
	    while(overflag) {
	      temp = sum(temp); 
	      temp1=sum(temp1);
	      temp1=sum(temp1);
	      if(temp1==1) {       
	          flag=true;
	          break;
	      }
	      if(temp==temp1) {   
	          flag=false;
	          break;
	      }
	    }
	    return flag;
	}
```

## Python
## Java
