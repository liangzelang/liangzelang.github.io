---
title: 1.two sum
date: 2016-12-26 23:35:23
categories:
- 技术
tags:
- leetcode
- 数据结构与算法
comments: true
---
+ Given an array of integers, return indices of the two numbers such that they add up to a specific target.

+ You may assume that each input would have exactly one solution.

<b>Example:</b>
```c
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

# 解析
<!--more-->
+ 翻译：给定一个整型数组，一个特定目标值，返回两个数组下标，使得对应数组元素相加为特定目标值。
+ 思路：
1. 用了一个笨方法，用了2个for循环，最糟糕的情况时间复杂度为O（n^2）.使用两个循环，相加判断是否为特定目标值即可
## C/C++

```cpp

// Note: The returned array must be malloced, assume caller calls free().

int* twoSum(int* nums, int numsSize, int target) {
    int *returnArray=NULL;
    returnArray = (int *)malloc(2*sizeof(int));
    for(int i=0;i<numsSize;i++) {
        for(int j=0;j<i;j++) {
            int temp = nums[i]+nums[j];
            if(temp==target) {
                returnArray[0]=j;
                returnArray[1]=i;
                return returnArray;
            }
        }
        for(int j=i+1;j<numsSize;j++) {
            int temp = nums[i]+nums[j];
            if(temp==target) {
                returnArray[0]=i;
                returnArray[1]=j;
                return returnArray;
            }
        }
        
    }
    return 0;
        
}
```
## Python
## Java
