---
title: 461.hamming distance 
date: 2016-12-23 10:11:23
categories:
- 技术
tags:
- leetcode
- 数据结构与算法
comments: true
---


+ The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

+ Given two integers x and y, calculate the Hamming distance.

<b>Note</b>:
0 ≤ x, y < 2^31.

<b>Example</b>
```
Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```

# 解析
+ <b>翻译</b>：Hanmming Distance 就是两个整数[0 2^31]二进制表示对应位上不同位数的个数。给定X,Y求取两者的hamming distance
<!--more-->
+ <b>思路</b> 用数组表示整数二进制，使用循环比较

## C/C++
```cpp
void dis(int n,char a[]) {
    char i=0;
    while(n) {
        a[i]=n%2;   //得到低位余数
        n=n/2;      //得到商
        i++;
    }
}

int hammingDistance(int x, int y) {
    char x_dis[32]={0};
    char y_dis[32]={0};
    int k=0;
    int temp=0;
    dis(x,x_dis);
    dis(y,y_dis);
        for(int i=0;i<32;i++) {
            if(x_dis[i]!=y_dis[i]) temp++;
        }
    return temp;
}

```
## Python
## Java

# 疑问
我最开始的思路就是不用指定二进制表示的位数，使用strlen（）来获得数组大小，但是失败，以下为错误代码，希望有人不吝赐教。
```cpp
void dis(int n,char a[]) {
    char i=0;
    while(n) {
        a[i]=n%2;   //得到低位余数
        n=n/2;      //得到商
        i++;
        if(n==0)
            a[i]='\0';
    }
    
}

int hammingDistance(int x, int y) {
    char x_dis[]={0};
    char y_dis[]={0};
    int k=0;
    int temp=0;
    dis(x,x_dis);
    dis(y,y_dis);
    if((strlen(x_dis))>=(strlen(y_dis))) {
        for(int i=0;i<strlen(y_dis);i++) {
            if(x_dis[i]!=y_dis[i]) temp++;
        }
        for(int i=strlen(y_dis);i<strlen(x_dis);i++) {
            if(x_dis[i]!=0) temp++;
        }
    } else {
        for(int i=0;i<strlen(x_dis);i++) {
            if(y_dis[i]!=y_dis[i]) temp++;
        }
        for(int i=strlen(x_dis);i<strlen(y_dis);i++) {
            if(y_dis[i]!=0) temp++;
        }
    }
    
    return temp;
}


```
