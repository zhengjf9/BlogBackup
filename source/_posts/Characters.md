---
title: 算法题目--Longest Substring Without Repeating Characters
date: 2017-12-19 22:06:13
tags: 
     - Leetcode
     - 算法
---

## 题目来源

> https://leetcode.com/problems/longest-substring-without-repeating-characters/description/

## 题目描述

Given a string, find the length of the **longest substring** without repeating characters.

<!--more-->

**Examples:**

Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.

Given `"bbbbb"`, the answer is `"b"`, with the length of 1.

Given `"pwwkew"`, the answer is `"wke"`, with the length of 3. Note that the answer must be a **substring**, `"pwke"` is a *subsequence* and not a substring.

## 心路历程

这道的解法有很多种，可以参见上面给出的链接中的Solution。自己想到的方法跟题目Solution相似，也就是用哈希表的方法。从头开始对字符串进行遍历，若当前字母从未出现过（通过查map），则直接将此位置减去开始位置（start），并与之前的结果（result）比较取最大值。注意开始位置是会改变的，也就是出现重复字母的时候，要将重复字母的下标值与之前的开始位置比较取最大值。这样做就能够保证出现重复字母后时从上一个重复字母的位置后面开始计数的。

## 解决方案

```
#include<iostream>
#include<string>
#include<map>
#include<algorithm>
using namespace std;
class Solution {
public:
	int lengthOfLongestSubstring(string s) {
		int length = s.size();
		int result = 0;
		map<char, int> map;
		int start = 0;
		for (int i = 0; i < length; i++) {
			bool change = false;
			if (map.find(s[i]) != map.end()) {//判断该字母是否出现过
				start = max(start, map[s[i]]);//改变开始位置
				change = true;
			}
			result =  max(result, i - start + 1);//结果比较
			if (change == false)
				map.insert(pair<char, int>(s[i], i + 1));//将新出现的字母插入
			else
				map[s[i]] = i + 1;//更新旧字母的下标值
		}
		return result;
	}
};
int main() {
	string s;
	cin >> s;
	Solution solution;
	cout << solution.lengthOfLongestSubstring(s)<<endl;
	system("pause");
}
```

## 反思

解这道题的过程中，遇到了一些困惑。比如，对C++中map的使用。

不妨猜猜下面的程序输出的结果是什么。

```
#include<iostream>
#include<map>
using namespace std;
int main(){
	map<char,int> map;
	map.insert(pair<char, int>('a', 1));
	cout<<map['a']<<endl;
	map.insert(pair<char, int>('a', 2));
	cout<<map['a']<<endl;
}
```

都是1！也就是说，对于map来说，两次插入同样的key，第一次插入的value并不会被改变。这是我一直的误区。因此，当想要改变map中某个key的值时，千万不能采用再次insert的方式，而要采用map[key] = value的方式去改。解决方案中的change变量就是用来判断map中是否已经出现你某个字母的。在Java中，使用HashMap就没有这个问题。重新插入可以改变之前key的值。在C++中貌似找不到类似的用法。有个头文件hash_map好像已经弃用了。

写到这里，又发觉change其实是多余的。因为不管key是否已经出现，map都可以用map[key] = value的方式来插入。程序可以精简一点。

```
class Solution {
public:
	int lengthOfLongestSubstring(string s) {
		int length = s.size();
		int result = 0;
		map<char, int> map;
		int start = 0;
		for (int i = 0; i < length; i++) {
            
			if (map.find(s[i]) != map.end()) {
				start = max(start, map[s[i]]);
				
			}
			result =  max(result, i - start + 1);
				map[s[i]] = i + 1;
		}
		return result;
	}
};
```







