---
title: Leetcode[730] Count Different Palindromic Subsequences
date: 2020-04-08 23:11:29
urlname: count-different-palindromic-subsequences
categorie:
- leetcode
tags:
- leetcode
- palindromic
- hard
- dynamic programming
---
>[Leetcode 730. Count Different Palindromic Subsequences](https://leetcode.com/problems/count-different-palindromic-subsequences/)
这题是从Longest Palinromic Subsequence延伸而来，同样属于动态规划问题，但是不同的是在递归式的推导上，由于子问题的回文串数量和前后位置的字符串是有关联的，难点在这个规律的发现。

<!--more-->
#### 算法实现
```java
public int countPalindromicSubsequences(String s) {
        if (s == null) return 0;
        int n = s.length();
        int[][] dp = new int[n][n];
        for (int i = 0; i < n; i++) dp[i][i] = 1;
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i < n; i++) {
                int end = i + len - 1;
                if (end < n) {
                    if (s.charAt(i) == s.charAt(end)) {
                        dp[i][end] = dp[i][end - 1] + dp[i + 1][end] + 1;
                    } else {
                        dp[i][end] = dp[i][end - 1] + dp[i + 1][end] - dp[i + 1][end - 1];
                    }
                }
            }
        }
        return dp[0][n - 1] % 1000000007;
    }
```