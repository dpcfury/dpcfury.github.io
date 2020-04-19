---
title: Leetcode[516] Longest Palindromic Subsequence
date: 2020-04-06 22:11:08
urlname: longest-palindromic-subsequence
categories:
- leetcode
tags:
- leetcode
- dynamic programming
- medium
---
>[Leetcode 516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)
这题也是经典的动态规划题，同样遵循通用的解体思路，首先是理解题目的意思，然后在寻找问题规模缩小的入口，慢慢推导出子问题和最优结构，从而总结出表达式，然后自底向上实现。

<!--more -->

#### 思路
首先，回文字符串的特点要理解，对单个字符而言，都是长度为1的回文串；
其次，我们能够缩小的范围只有字符中字串的大小，即字串的开始和终止位置；
令dp[i][j]为字符串subString(i,j+1)中，包含的回文子穿最大长度；
进而，可以发现，计算dp[i][j]的方法如下：
- case0: s.charAt[i] == s.charAt[j]，则dp[i][j] = dp[i+1][j-1] +2
- case1: s.charAt[i] != s.charAt[j]，则dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1])

那么最终的我们求的dp[0][s.length()-1]即为最终的解。

但是，我们如何自底向上进行求解呢，已知dp[i][i]的初始值为1，我们可以从不同长度的回文串开始着手，
首先求长度间隙为1（即dp[i][i]），然后长度间隙为2 ... 到长度间隙为n-1。一步一步，慢慢实现自底向上。

#### 算法实现
下面给出我个人实现的一种算法，目前并未进行优化，理论上这种时间遍历的消耗可以提前削减
```java
public int longestPalindromeSubseq(String s) {
        if (s == null) return 0;
        int n = s.length();
        int[][] dp = new int[n][n];
        for (int i = 0; i < n; i++) dp[i][i] = 1;
        for (int len = 1; len <= n - 1; len++) {
            for (int start = 0; start + len < n; start++) {
                int end = start + len;
                if (s.charAt(start) == s.charAt(end)) {
                    if (start + 1 == end) {
                        dp[start][end] = 2;
                    } else {
                        dp[start][end] = dp[start + 1][end - 1] + 2;
                    }

                } else {
                    dp[start][end] = Math.max(dp[start + 1][end], dp[start][end - 1]);
                }
            }
        }

        return dp[0][n - 1];
    }
```
