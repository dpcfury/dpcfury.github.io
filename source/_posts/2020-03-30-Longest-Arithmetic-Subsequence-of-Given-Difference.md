---
title: Leetcode[1218] Longest Arithmetic Subsequence of Given Difference
date: 2020-03-30 23:14:37
urlname: longest-arithmetic-subsequence-of-given-difference
categories:
- leetcode
tags:
- leetcode
- dynamic programming
- medium
---

>[Leetcode 1218. Longest Arithmetic Subsequence of Given Difference](https://leetcode.com/problems/longest-arithmetic-subsequence-of-given-difference/)
题目阅读下来，看到子序列这种问题，立马就会想到动态规划，首先会去想如何缩小问题的规模。从这个差来看，对于每一个遍历的数n而言，如果能知道以 n-difference结尾的子序列长度，就能计算以n为结尾的子序列长度，由于差值不是坐标，可以用HashMap存储dp的中间值。

<!-- more-->

```java
 public int longestSubsequence(int[] arr, int difference) {
        if (arr == null || arr.length == 0) return 0;
        HashMap<Integer, Integer> map = new HashMap<>();
        int max = 1;
        map.put(arr[0], 1);
        for (int i = 1; i < arr.length; i++) {
            if (map.containsKey(arr[i] - difference)) {
                map.put(arr[i], map.get(arr[i] - difference) + 1);
                max = Math.max(map.get(arr[i]), max);

            } else {
                map.put(arr[i], 1);
            }
        }
        return max;
    }
```
