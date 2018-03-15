---
title:  Assign Cookies
date: 2018-01-21
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/assign-cookies/description/

## 题目描述

Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie. Each child i has a greed factor gi, which is the minimum size of a cookie that the child will be content with; and each cookie j has a size sj. If sj >= gi, we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

**Note:**
You may assume the greed factor is always positive. 
You cannot assign more than one cookie to one child.

<!--more-->

**Example 1:**

```
Input: [1,2,3], [1,1]

Output: 1

Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.

```

**Example 2:**

```
Input: [1,2], [1,2,3]

Output: 2

Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 
You have 3 cookies and their sizes are big enough to gratify all of the children, 
You need to output 2.
```

## 心路历程

采用贪心算法。将曲奇和孩子的贪心值分别按从大到小的顺序进行排列。然后分别遍历，用曲奇中的最大值不断去满足贪心值中能够满足的最大值，知道曲奇用完或者贪心值用完为止。

## 解决方案

```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
bool cmp(int  a, int b)
{
	return a > b;
}
class Solution {
public:
	int findContentChildren(vector<int>& g, vector<int>& s) {
		int glen = g.size();
		int slen = s.size();
		int res = 0;
		sort(g.begin(), g.end(), cmp);
		sort(s.begin(), s.end(), cmp);
		int i = 0, j = 0;
		for (; i < slen && j < glen;) {
			if (s[i] >= g[j]) {
				res++;
				i++;
				j++;
			}
			else j++;
		}
		return res;
	}
};
int main() {
	vector<int> g = { 1,2 };
	vector<int> s = { 1,2,3 };
	Solution solution;
	int res = solution.findContentChildren(g, s);
	cout << res << endl;
	system("pause");
}
```



