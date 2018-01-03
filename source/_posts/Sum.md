---
title: 算法题目--Two Sum
date: 2017-09-08 21:20:34
tags:
     - Leetcode
     - 算法
---

## 题目来源

[https://leetcode.com/problems/two-sum/description/](https://leetcode.com/problems/two-sum/description/)

## 题目描述

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

<!--more-->

Example:

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## 心路历程

看到这道题目，自然而然就想到最暴力的方法——二重循环。这种解法大家都会，如果止步于此，那就没有进步。于是就解读了leetcode给出的几种方法，发现可以做的改进还挺多。

## 解决方案

1.最直接的方法，是采用二重循环进行遍历，当然是有代价的，时间复杂度为O(n​2​​)，空间复杂度为O(1)，好在这道题并没有对运行时间严格控制，该方法行得通。C++实现代码如下：

```
#include<iostream>
#include<vector>
using namespace std;
class Solution {
public:
	vector<int> twoSum(vector<int>& nums, int target) {
		for (int i = 0; i < nums.size(); i++) {       //二重循环
			for (int j = i + 1; j < nums.size(); j++) {
				if (target == nums[i] + nums[j]) {
					vector<int> result;
					result.push_back(i);
					result.push_back(j);
					return result;
				}
			}
		}
	}
};
int main() {
	vector<int> nums;
	nums.push_back(2);
	nums.push_back(7);
	nums.push_back(11);
	nums.push_back(15);
	int target = 9;
	Solution solution;
	vector<int> result = solution.twoSum(nums, target);
	cout << "The first index is " << result[0] << endl;
	cout << "The second index is " << result[1] << endl;
	getchar();
}
```

2.第二种方法来自leetcode，采用哈希表的方法，节省了一重遍历的时间，时间复杂度为O(n)。同时，哈希表的建立需要空间，使得空间复杂度变为O(n)。在本解中，我们看到了map的强大作用，containsKey方法判断是否存在某个key，get方法返回对应key 的value。Java实现代码如下：

```
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
            return new int[] { i, map.get(complement) };
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
```

3.第三种方法来自leetcode，同样采用哈希表的方法。不同的是，在建表的过程不断地“往回看”，一旦发现解，即返回结果。这种方法的复杂度仍与2中相同，但实际中性能应该比2要好。Java实现代码如下：

```
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
```