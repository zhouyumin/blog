---
title: 最长递增子序列nlogn法
date: 2021-03-05 12:03:03
tags: 
	- 算法
categories: 算法
---

### 思路

使用dp[i]来记录所有长度为i+1的递增子序列中最小的那个序列的尾数

由定义知dp数组必然是一个递增数组, 可以用 maxL 来表示最长递增子序列的长度

对数组进行迭代, 依次判断每个数num将其插入dp数组相应的位置

1. num > dp[maxL], 表示num比所有已知递增序列的尾数都大, 将num添加入dp
    数组尾部, 并将最长递增序列长度maxL加1
2. dp[i-1] < num <= dp[i], 只更新相应的dp[i]

<!-- more -->

### 代码

```java
public int lengthOfLIS(int[] nums) {
    int maxL = 0;
    int l, r, mid;
    int[] dp = new int[nums.length];
    for(int num :nums) {
        l = 0;
        r = maxL;
        //二分查找，可以调用Arrays.binarySearch
        while(l < r) {
            mid = (l + r) / 2;
            if(dp[mid] < num) {
                l = mid + 1;
            }else {
                r = mid;
            }
        }
        dp[l] = num;
        if(l == maxL) {
            maxL++;
        }
    }
    return maxL;
}
```

> https://leetcode-cn.com/problems/longest-increasing-subsequence/comments/10526