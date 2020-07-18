---
title: 414. the third maxium number
date: 2016-12-28 23:26:04
categories:
- 技术
tags:
- leetcode
- 数据结构与算法
comments: true
---

+ Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).
<b>Example 1:</b>
```c
Input: [3, 2, 1]

Output: 1

Explanation: The third maximum is 1.
```
<b>Example 2:</b>
```c
Input: [1, 2]

Output: 2

Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
```
<b>Example 3:</b>
```c
Input: [2, 2, 3, 1]

Output: 1

Explanation: Note that the third maximum here means the third maximum distinct number.
Both numbers with value 2 are both considered as second maximum.
```

# 解析
+ 翻译：给一个非空整型数组，返回第三大的数字，如果不存在则返回最大的数字，要求时间复杂度为0（n）
<!--more-->
+ 思路：初始化数组temp[3]，循环比较给定数组中的数字，把循环过程中前三数字放在temp[3]中，最后检查temp数组有没有被填满，如果有就说明第三大的数字存在，如果没有就不存在，按要求返回数值即可。
## C/C++

```c
int thirdMax(int* nums, int numsSize) {
    int flag,flag1,flag2=0;
    int temp[3]={ -2147483648, -2147483648, -2147483648};
    for(int i=0;i<numsSize;i++) {
       if(nums[i]>temp[2]) {
           if(flag2==1) {
               if(flag1==1){
                temp[0] = temp[1];
                temp[1] = temp[2];
                temp[2] = nums[i];
                flag =1;
               } else {
                //temp[0] = temp[1];
                temp[1] = temp[2];
                temp[2] = nums[i];
                flag1 =1;   //标记该位被占 
               }
           } else {
              temp[2] = nums[i];
              flag2=1;
           }
       } else if(nums[i]==temp[2]) { 
           flag2=1;   //表示这一位被占
       } else if((nums[i]<temp[2])&&(nums[i]>temp[1])) {
           if(flag1==1) {
               temp[0] = temp[1];
               temp[1] = nums[i];
               flag=1;
           } else {
               temp[1] = nums[i];
               flag1=1;
           }
       } else if(nums[i]==temp[1]) {
           flag1=1;    //表示此位被占
       } else if((nums[i]<temp[1])&&(nums[i]>temp[0])) {
           if(flag ==1) {
               temp[0]=nums[i];
           } else {
               temp[0]=nums[i];
               flag=1;
           }
       } else if(nums[i]==temp[0]) {
           flag=1;
       }
    }
    if(flag==1) 
        return temp[0];
    else
        return temp[2];
}
```

## Python
## Java

# 注意
这道题在leetcode中难度级别为easy，但是ac率只有20%多，根据自己做这道题感觉最大的原因不是没有思路，而且细节处理上，特别是如果确定是否存在第三大的数字，还有就是int范围等。
