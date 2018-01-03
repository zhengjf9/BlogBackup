---
title: 算法题目--Search a 2D Matrix II
date: 2018-01-03
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/search-a-2d-matrix-ii/description/

## 题目描述

Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

For example,

Consider the following matrix:

<!--more-->

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given **target** = `5`, return `true`.

Given **target** = `20`, return `false`.

## 心路历程

leetcode给出了两种解法。第一种是暴力解法，时间复杂度m*n,不太可取。第二种很巧妙，时间复杂度只有m + n。用的策略是从最后一行开始比较，若小于目标，往前移；若大于目标，行数减1。自己想到的方法是使用两层循环，第一层对行进行遍历，第二层在行内采用二分法。时间复杂度为mlog n。可以采用一些优化来加速算法。如行的第一个数就大于目标了，那显然结果是false；如果行的最后一个数已经小于目标，那么这一行也没有必要二分了。

## 解决方案

```
#include<iostream>
#include<vector>
using namespace std;
class Solution {
public:
	bool searchMatrix(vector<vector<int>>& matrix, int target) {
		int m = matrix.size();
		if (m == 0) return false;//这一句和下面第二句一定要加，处理矩阵为空的情况
		int n = matrix[0].size();
		if (n == 0) return false;//
		for (int i = 0; i < m; i++) {
			if (matrix[i][0] > target) return false;//第一个元素已经大于target，肯定找不到
			if (matrix[i][n - 1] < target) continue;//改行最后一个元素小于target，继续下一行
			if (matrix[i][0] == target || matrix[i][n - 1] == target)
				return true;
			int start = 0;
			int end = n - 1;
			int middle = (start + end) / 2;
			while (1) {
				if (start == end || start == middle) {//无法再二分了
					if (matrix[i][start] == target || matrix[i][end] == target)
						return true;
					else break;
				}
				if (matrix[i][middle] == target) return true;
				if (matrix[i][middle] > target) {//中间元素比target大
					end = middle - 1;
				}
				else {
					start = middle + 1;
				}
				middle = (start + end) / 2;
			}
		}
		return false;
	}
};
int main() {
	vector<vector<int>> matrix;
	vector<int> row1 = { 1, 4,  7, 11, 15 };
	vector<int> row2 = { 2, 5,  8, 12, 19 };
	vector<int> row3 = { 3, 6,  9, 16, 22 };
	vector<int> row4 = { 10, 13, 14, 17, 24 };
	vector<int> row5 = { 18, 21, 23, 26, 30 };
	matrix.push_back(row1);
	matrix.push_back(row2);
	matrix.push_back(row3);
	matrix.push_back(row4);
	matrix.push_back(row5);
	Solution solution;
	cout << solution.searchMatrix(matrix, 25) << endl;
	system("pause");
}
```