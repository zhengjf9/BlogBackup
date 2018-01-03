---
title: 算法题目--Target Sum
date: 2018-01-03
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/target-sum/description/

## 题目描述

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols `+` and `-`. For each integer, you should choose one from `+` and `-` as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

<!--more-->

**Example 1:**

```
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

**Note:**

1. The length of the given array is positive and will not exceed 20.
2. The sum of elements in the given array will not exceed 1000.
3. Your output answer is guaranteed to be fitted in a 32-bit integer.

## 心路历程

这道题我想了很久没想出好的解法，leetcode后面的答案要么复杂度太高，要么很难理解。在网上找到一种很简洁的答案，讲得非常好！引用在下面。

## 解决方案

出处：http://blog.csdn.net/u014593748/article/details/70185208

**编程思路：**

令dp[i][j表示前i个数和为j的方案数。 
则状态转移方程为dp[i][j]=dp[i−1][j−a[i]]+dp[i−1][j+a[i]] 
为了避免负数的情况，可以对原始问题进行如下转化：

- 令P为序列中符号为正的数字的集合，N为序列中符号为负的数字的集合，则
- sum(P)−sum(N)=target
- sum(P)+sum(N)+sum(P)−sum(N)=target+sum(P)+sum(N)
- 即2∗sum(P)=target+sum(P)+sum(N)
- 即在原序列中寻找一个正数集合P，使得sum(P)=(target+sum(P)+sum(N))/2

根据上面推导过程可以看出，target+sum(P)+sum(N)一定要是偶数，否者不存在符合条件的P。 
则令dp[i][j]表示序列中从0到j的子序列中，其和为j的组合数，则有如下状态转移方程：

- dp[i][j]=dp[i−1][j]+dp[i−1][j−nums[i]]
- dp[i][j]=0,j=0 
  其中，前半部分代表不选择nums[i]的情况，后半部分代表选择nums[i]的情况。

```
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int s) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        //(s + sum) & 1，判断s + sum的奇偶；(s + sum) >> 1，即(s + sum)/2
        return sum < s || (s + sum) & 1 ? 0 : subsetSum(nums, (s + sum) >> 1); 

    }   
    int subsetSum(vector<int>& nums, int s) {
        int dp[s + 1] = { 0 };
        dp[0] = 1;
        for (int n : nums)
            for (int i = s; i >= n; i--)
                dp[i] += dp[i - n];
        return dp[s];
    }
};
```

