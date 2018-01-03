---
title: 算法题目--Maximum Length of Repeated Subarray
date: 2018-01-02
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/maximum-length-of-repeated-subarray/description/

## 题目描述

Given two integer arrays `A` and `B`, return the maximum length of an subarray that appears in both arrays.

**Example 1:**

```
Input:
A: [1,2,3,2,1]
B: [3,2,1,4,7]
Output: 3
Explanation: 
The repeated subarray with maximum length is [3, 2, 1].

```

<!--more-->

**Note:**

1. 1 <= len(A), len(B) <= 1000
2. 0 <= A[i], B[i] < 100

## 心路历程

最长回文子串，最长回文子序列，最长公共子串，最长公共子序列都是学习动态规划老生常谈的问题了。本题类似于最长公共子串，可以通过画二维矩阵的方法，找到最长的对角线来解决。

## 解决方案

```
#include<iostream>
#include<vector>
#include<cstring>
using namespace std;
class Solution {
public:
	int findLength(vector<int>& A, vector<int>& B) {
		int lenA = A.size();
		int lenB = B.size();
		int max = 0;
		int * temp = new int[lenA];
		memset(temp, 0, sizeof(int)*lenA);//最开始写成sizeof(temp)报错
		for (int i = 0; i < lenB; i++) {
			for (int j = lenA - 1; j >= 0; j--) {
				if (B[i] == A[j]) {
					if (j == 0) {
						temp[0] = 1;
						if (max < temp[0]) max = temp[0];
					}
					else {
						temp[j] = temp[j - 1] + 1;
						if (max < temp[j]) max = temp[j];
					}
				}
				else temp[j] = 0;
			}
		}
		delete temp;
		return max;
	}
};
int main() {
	vector<int> A = { 1,2,3,2,1 };
	vector<int> B = { 3,2,1,4,7 };
	Solution solution;
	cout << solution.findLength(A, B)<<endl;
	system("pause");
}
```