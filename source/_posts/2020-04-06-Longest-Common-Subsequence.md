---
title: Leetcode[1143] Longest Common Subsequence
date: 2020-04-06 21:02:23
urlname: longest-common-subsequence
categpries:
- leetcode
tags:
- leetcode
- dynamic programming
- medium
---
[Leetcode 1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
题目和字符串编辑问题类似，同样属于非常经典的动态规划，只要我们找到其中问题规模的收敛入口，继而推导出递归表达式，通过自底向下的方式进行实现。

<!--more -->

#### 思路
给定两个字符串，text1和text2，求其中最长的公共字符子序列长度。为什么说这道题目一样很经典，子序列的问题，通常我们都能一下找到问题规模缩小的入口，即将字符串的规模缩小，再通过比较指定位置字符的是否相同，将问题规模缩小到子问题的求解过程中。
我们令dp[i][j] 为text1前i个字符和text2的前j个字符，其最长公共子序列的长度，那么：
- case1:  text1.charAt(i-1) == text2.charAt(j-1)，那么dp[i][j] = dp[i-1][j-1] + 1
- case2:  text1.charAt(i-1) != word2.text2(j-1)：
    - 可能1: dp[i][j] = dp[i-1][j]  // 最长为text1前i-1字符串和text2前j个字符
    - 可能2: dp[i][j] = dp[i][j-1] // 最长为text1前i字符串和text2前j-1个字符
    - 可能3: dp[i][j] = dp[i-1][j-1] // 最长为text1前i-1字符串和text2前j-1个字符
    即 dp[i][j] = max(dp[i][j-1], dp[i-1][j], dp[i-1][j-1])


#### 代码实现
```java
public int longestCommonSubsequence(String text1, String text2) {
        if (text1 == null || text2 == null) return 0;
        int m = text1.length();
        int n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1))
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                else {
                    dp[i][j] = Math.max(Math.max(dp[i - 1][j - 1], dp[i - 1][j]), dp[i][j - 1]);
                }

            }
        }
        return dp[m][n];
    }
```