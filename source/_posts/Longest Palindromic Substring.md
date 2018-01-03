---
title: 算法题目--Longest Palindromic Substring
date: 2017-12-27
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/longest-palindromic-substring/description/

## 题目描述

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

<!--more-->

**Example:**

```
Input: "babad"

Output: "bab"

Note: "aba" is also a valid answer.
```

**Example:**

```
Input: "cbbd"

Output: "bb"
```

## 心路历程

本题可以利用动态规划来解决。如果一个字符串是回文的，它的子串也是回文的。利用这一性质，可以用一个二维布尔数组dp[i][j]来代表下标i到j的子串是否回文。i和j都从小遍历，若i等于j或者i - j == 1,置为true；其他情况下只有是s[i] == s[j]并且去除是s[i]、s[j]的子串也回文才是true。

## 解决方案

```

#include<iostream>
#include<string>
#include<algorithm>
using namespace std;
class Solution {
public:
	string longestPalindrome(string s) {
	    int n = s.size();
		int **dp = new int*[n];       
		for (int i = 0; i < n; i++)
			dp[i] = new int[n];
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++)
				dp[i][j] = 0;
		}
		int max_len = 1;
		int start = 0;
		for (int i = 0; i<s.size(); i++)
		{
			for (int j = 0; j <= i; j++)
			{
				if (i - j<2)
					dp[j][i] = (s[i] == s[j]);
				else
					dp[j][i] = (s[i] == s[j] && dp[j + 1][i - 1]);
				if (dp[j][i] && max_len<(i - j + 1))
				{
					max_len = i - j + 1;
					start = j;
				}
			}
		}
		return s.substr(start, max_len);
	}
};
int main() {
	Solution solution;
	string str = "bxaddayb";
	string result = solution.longestPalindrome(str);
	cout << result<<endl;
	system("pause");
}
```