---
title: 412. fizz buzz
date: 2016-12-23 10:09:06
categories:
- 技术
tags:
- leetcode
- 数据结构与算法
comments: true
---

+ Write a program that outputs the string representation of numbers from 1 to n.

+ But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

<b>Example:</b>
```cpp

n = 15,

Return:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```
# 解析
+ 翻译：写一个程序输出1到n的字符串表达，但是 如果是3的倍数，不输出对应数字的字符串，而是输出“Fizz” ，如果是5的倍数，不输出对应数字的字符串，而是输出“Buzz”， 若同时是3和5的倍数，则输出 “FizzBuzz”。
<!--more-->
+ 思路：很简单的一道题，唯一的难点就是这是一个字符串数组，分别对char **和char * 进行malloc。还有就是sprintf把整型转换为字符串。
## C/C++
+ c

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
char** fizzBuzz(int n, int* returnSize) {
     char **returnArray ;  //数组的指针
     returnArray = (char **)malloc(n*sizeof(char **));
     for(int i=0;i<n;i++) {
         returnArray[i] = (char *)malloc(8*sizeof(char *));
     }
     for(int i=0;i<n;i++) {
         if(((i+1)%3==0)&&((i+1)%5!=0)) {
             returnArray[i] = "Fizz";
         } else if(((i+1)%3!=0)&&((i+1)%5==0)) {
             returnArray[i] = "Buzz";
         } else if(((i+1)%3==0)&&((i+1)%5==0)) {
             returnArray[i] =  "FizzBuzz";
         } else {
             sprintf(returnArray[i],"%d",i+1);
            // returnArray[i] = "Fizz";
         }
         
     }
     *returnSize = n;
     return returnArray;
}
```
## Python
## Java
