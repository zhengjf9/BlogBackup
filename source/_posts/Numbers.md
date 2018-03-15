---
title: 算法题目--Add Two Numbers
date: 2017-09-19 20:49:47
tags:
    - Leetcode
    - 算法
---

## 题目来源

> https://leetcode.com/problems/add-two-numbers/description/

## 题目描述

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

<!--more-->

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
```

## 心理历程

这道题在leetcode上的难度标示为Medium，但感觉不是太难，主要考查结构体和链表的运用。其中有些特殊情况，是在提交之后报错才知道的。比如，要考虑最高位的进位。还有一个关键点，就是两个链表长度不一致时，要把比较短的那个链表高位用0来代替。

## 解决方案

1.自己写的C++版解法，遇到的一个坑是，结构体等于NULL的时候，不能再指向下一个节点。（一开始天真地以为NULL的下一个节点还是NULL）

```
#include<iostream>
using namespace std;
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};

class Solution {
public:
	ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
		ListNode* head = new ListNode(0);
		ListNode* result = head;
		int flag = 0;
		int add1, add2, sum, rem;//加数1，加数2，和，余数
		int carry = 0;//进位
		while (l1 != NULL || l2 != NULL) {//只要任意一个链表不为空则继续
			if (l1 == NULL) add1 = 0; else add1 = l1->val;//空的链表赋值为0
			if (l2 == NULL) add2 = 0; else add2 = l2->val;
			sum = add1 + add2 + carry;
			carry = sum / 10;
			rem = sum % 10;
			if (flag == 0) { //链表头直接赋值
				head->val = rem;
				flag++;
			}
			else { //新建节点
				head->next = new ListNode(rem);
				head = head->next;
			}
			if (l1 != NULL)l1 = l1->next;
			if (l2 != NULL) l2 = l2->next;
		}

		if (carry == 0){}//处理最高位
		else {
			head->next = new ListNode(carry);
			head = head->next;
		}
		return result;
	}
};
int main() {
	ListNode* l1 = new ListNode(0);
	ListNode* l2 = new ListNode(0);
	ListNode* arg1 = l1;
	ListNode* arg2 = l2;
	l1->val = 2;
	l1->next = new ListNode(4);
	l1 = l1->next;
	l1->next = new ListNode(3);
	l2->val = 5;
	l2->next = new ListNode(6);
	l2 = l2->next;
	l2->next = new ListNode(4);
	Solution solution;
	ListNode* result = solution.addTwoNumbers(arg1, arg2);
	while (result != NULL) {
			cout << result->val;
		result = result->next;
	}
	cout << endl;
	system("pause");
}
```

2.第二种解决方案来自leetcode，用Java写的，思路与第一种基本一致。主要的优化是采用了虚拟链表头，使得循环时不需要进行一次判断，最后返回链表头的下一个节点即可。这种思路值得借鉴。代码如下：

```
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
}
```