---
title: 算法题目--Binary Tree Paths
date: 2018-01-06
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/binary-tree-paths/description/

## 题目描述

Given a binary tree, return all root-to-leaf paths.

For example, given the following binary tree:

<!--more-->

```
   1
 /   \
2     3
 \
  5

```

All root-to-leaf paths are:

```
["1->2->5", "1->3"]
```

## 心路历程

题意比较简单。就是打印出一=一棵树从根节点到叶子节点的所有路径。利用递归来实现。每次都是先递归左子树再递归右子树。每次递归要将已经遍历得到的子串作为参数进行传递。

## 解决方案

```
#include<iostream>
#include<vector>
#include<string>
using namespace std;

struct TreeNode {
     int val;
     TreeNode *left;
     TreeNode *right;
     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
	vector<string> result;
	vector<string> binaryTreePaths(TreeNode* root) {
		if (root == NULL) {
			return result;
		}
		search(root, "");
		return result;
	}
	void search(TreeNode * node, string str) {
		if (node == NULL) {
			result.push_back(str);
			return;
		}
		if (node->left == NULL && node->right == NULL) {
			if(str == "")
			    result.push_back(str + to_string(node->val));
			else
				result.push_back(str + "->" + to_string(node->val));
			return;
		}
		if (node->left != NULL) {
			if(str == "")
			    search(node->left, str + to_string(node->val));
			else
				search(node->left, str + "->" + to_string(node->val));
		 }
		if (node->right != NULL) {
			if (str == "")
			   search(node->right, str + to_string(node->val));
			else
			   search(node->right, str + "->" + to_string(node->val));
		}
	}
};
int main() {
	TreeNode* leaf = new TreeNode(5);
	TreeNode* left = new TreeNode(2);
	left->right = leaf;
	TreeNode* right = new TreeNode(3);
	TreeNode* root = new TreeNode(1);
	root->left = left;
	root->right = right;
	Solution solution;
	vector<string> result = solution.binaryTreePaths(root);
	for (int i = 0; i < result.size(); i++)
		cout << result[i] << endl;
	system("pause");
	return 0;
}
```