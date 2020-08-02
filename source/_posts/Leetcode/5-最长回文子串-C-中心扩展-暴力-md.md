---
title: '[5. 最长回文子串] C + 中心扩展 + 暴力'
date: 2020-08-02 21:48:21
categories:
- 技术
tags:
- leetcode
- 数据结构与算法
comments: true
---
## Leetcode No.5

+ <b>给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。</b>

<b>示例:</b>
```c
示例 1：
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。

示例 2：
输入: "cbbd"
输出: "bb"

return [0, 1].
```
<img src="https://pic.leetcode-cn.com/15a55cf50b8c3e9ad272f581e5b7174c94fcbbdbadb990a7a11b0c22b4cb361f-image.png" alt="result" height="175" width="475">

## 解题思路
1. 没有思路，只有暴力解，以每个点为中心向两边扩散；
2. 但是条件1中又需要考虑奇数偶数的情况，原本想的是每次传个left和right，但是题解搞了一种骚操作，就是通过中心点i和长度len，直接求出left和right

## 复杂度分析
时间复杂度： O(n^2)
空间复杂度： O(n)

## 代码
```c
//start: 19:35
// 解前思路：没啥思路，弱点就在字符串这块，难，先暴力一下：以每个点作为中心点来判断
#define MAX(x, y) (x > y) ? x : y
int loopPalindrome(char* s, int l, int r) {
    if (r >= strlen(s)) return 0;
    int len = 0;
    while (l >= 0 && r <= strlen(s) && s[l] == s[r]) {
        l--;
        r++;
        len = r - l - 1;
    }
    return len;
}

char * longestPalindrome(char * s){
    if (s == NULL) return NULL;
    int r = 0;
    int l = 0;
    int len = strlen(s);
    for (int i = 0; i < len; i++) {
        int len1 = loopPalindrome(s, i, i);
        int len = loopPalindrome(s, i, i + 1);
        int maxLen = MAX(len1, len);
        if (maxLen > (r - l + 1)) {
            l = i - (maxLen - 1) / 2;
            r = i + maxLen / 2;
        }
    }
    char* res = (char*)malloc(sizeof(char) * (r - l + 2));
    memcpy(res, &s[l], (r - l + 1));
    res[r - l + 1] = '\0';
    return res;
}

作者：liangzelang
链接：https://leetcode-cn.com/problems/longest-palindromic-substring/solution/5-zui-chang-hui-wen-zi-chuan-c-zhong-xin-kuo-zhan-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
