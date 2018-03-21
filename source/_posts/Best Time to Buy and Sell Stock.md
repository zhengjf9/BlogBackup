---
title: 算法题目--Best Time to Buy and Sell Stock
date: 2018-01-21
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/

## 题目描述

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

<!--more-->

**Example 1:**

```
Input: [7, 1, 5, 3, 6, 4]
Output: 5

max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)

```

**Example 2:**

```
Input: [7, 6, 4, 3, 1]
Output: 0

In this case, no transaction is done, i.e. max profit = 0.
```

## 心路历程

简单的动态规划问题。对于每一天的股价，我们可以计算它和前面最小的股价的差值。最大的差值就是结果。因此，需要在每一步中记录最小值。并计算距离与之前的最大值作比较。

## 解决方案

```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
class Solution {
public:
	int maxProfit(vector<int>& prices) {
		int len = prices.size();
		if (len == 0 || len == 1) return 0;
		int _min = prices[0];
		int _max = -INFINITY;
		for (int i = 1; i < len; i++) {
			_max = max(_max, prices[i] - _min);
			_min = min(_min, prices[i]);
		}
		if (_max < 0) return 0;
		return _max;
	}
};
int main() {
	vector<int> prices = { 7, 1, 5, 3, 6, 4 };
	Solution solution;
	int res = solution.maxProfit(prices);
	cout << res << endl;
	system("pause");
}
```



