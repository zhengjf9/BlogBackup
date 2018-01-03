---
title: 算法题目--Min Cost Climbing Stairs
date: 2018-01-02
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/min-cost-climbing-stairs/description/

## 题目描述

On a staircase, the `i`-th step has some non-negative cost `cost[i]` assigned (0 indexed).

Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.

<!--more-->

**Example 1:**

```
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.

```

**Example 2:**

```
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].
```

**Note:**

1. `cost` will have a length in the range `[2, 1000]`.
2. Every `cost[i]` will be an integer in the range `[0, 999]`.

## 心路历程

使用动态规划解决。从序列后面开始往前计算最小代价。比如：

1, 100, 1, 1, 1, 100, 1, 1, 100, 1可以看成：

1, 100, 1, 1, 1, 100, 1, 1, 100, 1，0，0

初始化right1,right2为0，表示最右面两个台阶的代价为0。往前看是1，这时候假设已经到达这一台阶，有两种选择，因此选择小的一种（此处都是0）。继续往前看，此时right1和right2也都往前移了一步，right1等于1，right1等于之前的right1（0）。好了，再看现在的100，同样有两种选择，继续选择小的。再往前一步，right1变成100，right2变成1……重复此操作，直到最前面。

## 解决方案

```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
class Solution {
public:
	int minCostClimbingStairs(vector<int>& cost) {
		int length = cost.size();
		int right1 = 0;
		int right2 = 0;
		for (int i = length - 1; i >= 0; i--) {
			int cur = cost[i] + min(right1, right2);
			right2 = rigth1;
			right1 = cur;
		}
		return min(right1, right2);
	}
};
int main() {
	Solution solution;
	vector<int> test1 = { 1, 100, 1, 1, 1, 100, 1, 1, 100, 1 };
	cout << solution.minCostClimbingStairs(test1) << endl;
	vector<int> test2 = { 10,15,20};
	cout << solution.minCostClimbingStairs(test2) << endl;
	system("pause");
	return 0;
}
```