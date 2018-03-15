---
title: 算法题目--Maximum Product Subarray
date: 2018-01-06
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/maximum-product-subarray/description/

## 题目描述

Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array `[2,3,-2,4]`,
the contiguous subarray `[2,3]` has the largest product = `6`.

<!--more-->

## 心路历程

动态规划问题。使用两个数组记录从开始到i的乘积最大值还有最小值。每次最大值从当前值，当前值与上一轮的最大值的乘积，当前值与上一轮的最小值的乘积中选最大的产生。最小值同理。

## 解决方案

```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
class Solution {
public:
	int maxProduct(vector<int>& nums) {
		int len = nums.size();
		int * _max = new int[len];
		int * _min = new int[len];
		int result = nums[0];
		_max[0] = _min[0] = nums[0];
		for (int i = 1; i < len; i++) {
			_max[i] = max(max(_max[i - 1] * nums[i], _min[i - 1] * nums[i]), nums[i]);
			_min[i] = min(min(_max[i - 1] * nums[i], _min[i - 1] * nums[i]), nums[i]);
			result = max(result, _max[i]);
		}
		return result;
	}
};
int main() {
	vector<int> nums = { 2, 3, -2, 4 };
	Solution solution;
	cout << solution.maxProduct(nums) << endl;
	system("pause");
	return 0;
}
```