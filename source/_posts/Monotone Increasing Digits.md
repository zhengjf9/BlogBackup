---
title: 算法题目--Monotone Increasing Digits
date: 2017-12-27
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/monotone-increasing-digits/description/

## 题目描述

Given a non-negative integer `N`, find the largest number that is less than or equal to `N` with monotone increasing digits.

(Recall that an integer has *monotone increasing digits* if and only if each pair of adjacent digits `x` and `y` satisfy `x <= y`.)

<!--more-->

**Example 1:**

```
Input: N = 10
Output: 9

```

**Example 2:**

```
Input: N = 1234
Output: 1234

```

**Example 3:**

```
Input: N = 332
Output: 299
```

**Note:** `N` is an integer in the range `[0, 10^9]`.

## 心路历程

本题在leetcode上为中等难度。主要思路不难，是利用贪心算法。但是解题的过程中很难考虑全面，提交过好几次之后再根据错误的样例对算法进行修正，最终得到正确答案。大致做法：从最高位开始遍历，分3种情况：（1）当前小于下一位，正是想要的结果，不需要任何处理；（2）当前大于下一位，则当前减1，后面全部置为9；（3）当前等于下一位，这是最复杂最容易漏掉的情况。继续往后比较，若还相等，就一直继续，若遇到比当前大的，就停止比较，不需要做什么，若遇到比当前小的，就把当前减1，后面全部置为9。

## 解决方案

```
#include<iostream>
#include<sstream>
using namespace std;
class Solution {
public:
	int monotoneIncreasingDigits(int N) {
		stringstream s;
		s << N;
		string str = s.str();
		int length = str.size();
		for (int i = 0; i < length - 1; i++) {
			if (str[i] > str[i + 1]) {//前面大于后面
				str[i] = str[i] - 1;
				for (int j = i + 1; j < length; j++)
					str[j] = '9';
				return atoi(str.c_str());
			}
			else if (str[i] == str[i + 1]) {//前面等于后面
				for (int j = i + 2; j < length; j++) {//需要继续往后比较
					if (str[i] > str[j]) {//如果找到一个比当前小的，那么当前减1，后面变9
						str[i] = str[i] - 1;
						for (int k = i + 1; k < length; k++)
							str[k] = '9';
						return atoi(str.c_str());
					}
					if (str[i] < str[j]) break;//如果找到一个比当前大的，停止向后比较
				}
			}
		}
		return atoi(str.c_str());
	}
};
int main() {
	Solution solution;
	int N = 668841;
	cout<<solution.monotoneIncreasingDigits(N)<<endl;
	system("pause");
}
```