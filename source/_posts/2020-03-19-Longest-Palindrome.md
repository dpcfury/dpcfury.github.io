---
title: leetcode[409]Longest Palindrome
date: 2020-03-19 23:01:25
urlname: leetcode-longest-palindrome
categories:
- leetcode
tags:
- leetcode
- array
- easy
---

> [409. Longrest Palindrome](https://leetcode.com/problems/longest-palindrome/)
给定一个字符串，计算其包含的字符可组成的最长回文字符串，主要是观察下规律，看如何用字符去组织回文字符串，针对单数的字符、双数的字符，应该如何处理。个人给出的代码其实很臃肿，可以优化的地方很多，往上有种bitwise的做法，不是很理解，暂且不放上来。

<!--more-->

```java
public int longestPalindrome(String s) {
        HashMap<Character, Integer> count = new HashMap<>();
        for (char c : s.toCharArray()) {
            if (count.containsKey(c)) {
                int n = count.get(c);
                count.put(c, n + 1);
            } else {
                count.put(c, 1);
            }
        }
        boolean once = false;
        int num = 0;
        for (Map.Entry<Character, Integer> entry : count.entrySet()) {
            if (entry.getValue() == 1) {
                if (once) continue;
                else {
                    num++;
                    once = true;
                }
            } else {
                int n = entry.getValue();
                if (n % 2 == 0) num += n;
                else {
                    if (once) {
                        num += (n - 1);
                    } else {
                        num += n;
                        once=true;
                    }
                }
            }

        }
        return num;
    }
```
