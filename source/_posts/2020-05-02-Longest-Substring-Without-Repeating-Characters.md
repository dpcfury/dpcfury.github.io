---
title: Leetcode[3] Longest Substring Without Repeating Characters
date: 2020-05-02 10:42:49
urlname: longest-substring-without-repeating-characters
categories:
- leetcode
tags:
- leetcode
- sliding window
- two pointers
- hash table
---
>[Leetcode 3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
题目：给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。注意是子串，即元素是下标是连续的，不是子序列（下标可不连续）。

<!--more-->
#### 思路
- 暴力方法就是去遍历，时间复杂度是O(n^3)，列所有的可能组合，并且进行字符重复判断O(n).
- 滑动窗口，保持一个不包含重复字符的滑动窗口，如遇到重复，则将左侧窗口不断删除，直到将右侧遇到的重复字符在窗口中删除，并在过程中更新子串的最大长度，优化点：
    - 使用HashSet进行重复元素判断
    - 使用HashMap记录字符上次出现的下标


#### 算法实现
```java
public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) return 0;
        int maxLen = Integer.MIN_VALUE;
        int left = 0, right = 0;
        HashSet<Character> set = new HashSet<>();
        char[] chs = s.toCharArray();
        while (left <= right && right < s.length()) {
            if (set.contains(chs[right])) {
                while (chs[left] != chs[right]) {
                    set.remove(chs[left++]);
                }
                set.remove(chs[left++]);

            } else {
                maxLen = Math.max(maxLen, right - left + 1);
                set.add(chs[right++]);
            }
        }
        return maxLen;
    }
```