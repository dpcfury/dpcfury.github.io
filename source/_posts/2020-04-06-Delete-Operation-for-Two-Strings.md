---
title: Leetcode[583] Delete Operation for Two Strings
date: 2020-04-06 20:19:30
urlname: delete-operation-for-two-strings
categories:
- leetcode
tags:
- leetcode 
- medium
- dynamic programming
---

>[ Leetcode 583 Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/)
题目和经典的Edit Distance 属于同一类问题，只是允许的操作变限制为了删除操作，问需要两个单词需要最少的删除操作，能够变为相同的字符串。核心还是如何找到一个问题规模收敛的入口，从而在问题规模缩小的过程中，找到递推的规律，进而推导出表达式，再利用自顶向下或自底向上的方法去实现。

<!--more-->

#### 思路
给定word1和word2，通过最少的删除操作，使得两个单词最终变为相同的字符串，先从特殊的情况开始考虑，如果某个单词为空串（长度为0），另一个不为空串，则需要的最少操作步骤为不为空的单词长度。从中又可以感受到一点动态规划的味道。
继续分析，假设令dp[i][j] 为word1前i个字符串 和word2前j个字符串，变成相同字符串需要的最少删除次数，那么：
- case1:  word1.charAt(i-1) == word2.charAt(j-1)，那么dp[i][j] = dp[i-1][j-1]
- case2:  word1.charAt(i-1) != word2.charAt(j-1)：
    - 可能1: dp[i][j] = dp[i-1][j]+1  // 通过word2insert一个字符
    - 可能2: dp[i][j] = dp[i][j-1] +1 // 通过word1insert一个字符

#### 代码实现
```java
public int minDistance(String word1, String word2) {
        if (word1 == null || word2 == null) return 0;
        int m = word1.length();
        int n = word2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1))
                    dp[i][j] = dp[i - 1][j - 1];
                else {
                    dp[i][j] = Math.min(dp[i][j - 1], dp[i - 1][j]) + 1;
                }
            }
        }

        return dp[m][n];
    }
```
