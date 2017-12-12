---
title: Delete and Earn
date: 2017-12-05
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/delete-and-earn/

## 题目描述

Given an array nums of integers, you can perform operations on the array.
In each operation, you pick any nums[i] and delete it to earn nums[i] points. After, you must delete every element equal to nums[i] - 1 or nums[i] + 1.
You start with 0 points. Return the maximum number of points you can earn by applying such operations.

<!--more-->

Example 1:

> Input: nums = [3, 4, 2] Output: 6 Explanation:  Delete 4 to earn 4
> points, consequently 3 is also deleted. Then, delete 2 to earn 2
> points. 6 total points are earned.

Example 2:

> Input: nums = [2, 2, 3, 3, 3, 4] Output: 9 Explanation:  Delete 3 to
> earn 3 points, deleting both 2's and the 4. Then, delete 3 again to
> earn 3 points, and 3 again to earn 3 points. 9 total points are
> earned.

Note:

The length of nums is at most 20000.
Each element nums[i] is an integer in the range [1, 10000].
## 心路历程

这道题是leetcode上面位于动态规划目录下的一道题，难度为Medium。刚开始看到这道题的时候，没什么思路，找不到很好的子问题。所以写了一个遍历并递归的算法，输入测试用例倒没什么问题，但是提交后运行只通过了10个测试样例，后面的全部超时。如下图：

![错误答案](http://img.blog.csdn.net/20171205155901822?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemhlbmdqZjk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
于是看了答案，确实有一种醍醐灌顶的感觉。先把所有的数从小到大排个序（因为数的范围有限，所以可以使用桶排序）。这样，问题就变成从左往右依次考虑是否加入某个数。过程中使用use和avoid两个变量来分别记录加入和不加入当前数所得到的值。接着考虑下一个数，若与前一个数不连续。可以直接加入；若与前一个数连续，就要加上前一个数的avoid值。这里有一点要注意，每个数都有可能重复，但是它们要不全部加入，要么全部不加，这是因为当加入第一个这种数时，它的左右邻都会被删除，所以其它的相同的数没办法再被删除（因为没有邻居了）。leetcode上给了Java和Python的解法，下面给出了类似的C++解法。

## 解决方案

```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
class Solution {
public:
	int deleteAndEarn(vector<int>& nums) {
		int count[10001] = { 0 };
		for (int i = 0; i < nums.size(); i++) count[nums[i]]++;
		int avoid = 0;
		int use = 0;
		int prev = -1;
		for (int k = 0; k <= 10000; ++k) {
			if (count[k] > 0) {
				int m = max(avoid, use);
				if (k - 1 != prev) {
					use = k * count[k] + m;
					avoid = m;
				}
				else {
					use = k * count[k] + avoid;
					avoid = m;
				}
				prev = k;
			}
		}
		return max(avoid, use);
	}
};
int main() {
	vector<int> nums;
	nums.push_back(3);
	nums.push_back(4);
	nums.push_back(2);
	Solution solution;
	int result = solution.deleteAndEarn(nums);
	cout << result<<endl;
	system("pause");
	return 0;
}

```