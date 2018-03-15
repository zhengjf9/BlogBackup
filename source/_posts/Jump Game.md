---
title: Jump Game
date: 2018-01-21
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/jump-game/description/

## 题目描述

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

For example:
A = `[2,3,1,1,4]`, return `true`.

A = `[3,2,1,0,4]`, return `false`.

<!--more-->

## 心路历程

动态规划问题。可以使用一个变量_max，用来记录到某一步的时候能够往前到达最远的距离。比如，

[2,3,1,1,4]中，第0步能够到达的最远距离是2，第3步能够到达的最远距离是1 + 3 = 4；第2步能够到达的最远距离是max（4， 2 + 1） = 3……每一步得到的最远距离如果比步数小或者相等，说明走到这里的时候已经无法继续。否则可以继续往前走。

## 解决方案

```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
class Solution {
public:
	bool canJump(vector<int>& nums) {
		int _max = -1;
		for (int i = 0; i < nums.size(); i++) {
			int cur = i + nums[i];
			_max = max(_max, cur);
			if (_max >= nums.size() - 1) return true;
			if (_max <= i) return false;
		}
		return false;
	}
};
int main() {
	vector<int>nums = { 3,2,2,0,2,1 };
	Solution solution;
	bool res = solution.canJump(nums);
	cout << res << endl;
	system("pause");
}
```



