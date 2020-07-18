---
title: 442.find all duplicates in an array
date: 2016-12-23 10:09:57
categories:
- 技术
tags:
- leetcode
- 数据结构与算法
comments: true
---

+ Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

+ Find all the elements that appear twice in this array.

+ Could you do it without extra space and in O(n) runtime?

<b>Example:</b>
```
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
```
# 解析
+ 翻译：一个整型数组，[1,n]其中n为数组大小，其中有些元素出现2次，有些出现1次，求出现2次的元素，0(n)复杂度
<!--more-->
+ 思路：将数组作为下标，使用正负法
## C/C++
```cpp
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* findDuplicates(int* nums, int numsSize, int* returnSize) {
    int *result=NULL;
    result = (int *)malloc(numsSize*sizeof(int));
    int temp=0;
    *returnSize=0;
    for(int i=0;i<numsSize;i++) {
        if(nums[abs(nums[i])-1]<0) {
            result[temp]=abs(nums[i]);
            temp++;
            *returnSize = temp;
        } else {
            nums[abs(nums[i])-1]=-nums[abs(nums[i])-1];
        }
    }
    return result;
}
```
## Python
## Java
# 疑问
不是很明白为什么一下代码在用例[4,3,2,7,8,2,3,1]，程序ok，但是用例[3,11,8,16,4,15,4,17,14,14,6,6,2,8,3,12,15,20,20,5]却失败。

+ <b>原因:</b>之所以这样是在malloc时错误，采用(*returnSize)*sizeof(int)，但是这个时候*returnSize并没有值。所以改为使用正确的numsSize*sizeof(int),但是总觉得这样比较浪费内存。
```cpp
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* findDuplicates(int* nums, int numsSize, int* returnSize) {
    int *result=NULL;
    result = (int *)malloc((*returnSize)*sizeof(int));
    int temp=0;
    *returnSize=0;
    for(int i=0;i<numsSize;i++) {
        if(nums[abs(nums[i])-1]<0) {
            result[temp]=abs(nums[i]);
            temp++;
            *returnSize = temp;
        } else {
            nums[abs(nums[i])-1]=-nums[abs(nums[i])-1];
        }
    }
    
    return result;
}
```
