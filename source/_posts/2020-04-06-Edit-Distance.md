---
title: Leetcode[72] Edit Distance
date: 2020-04-06 09:50:05
urlname: edit-distance
categpries:
- leetcode
tags:
- leetcode
- dynamic programming
- hard
---

>[Leetcode 72. Edit Distance](https://leetcode.com/problems/edit-distance/)
经典的动态规划题目，求从一个字符串通过三种操作转换为另一个字符串的短编辑记录（最少的转换步骤），允许的操作包括：1. 插入；2. 删除；3. 替换。这题的核心是还是如何找一个收敛的入口，在问题规模缩小过程中，找到递推的规律，再通过自底向上或自顶向下的方式实现即可。

<!--more-->

#### 思路
给定的word1，word2，在极限情况下，假设word2位空字符串（长度为0），那么word1 转换为word2，需要进行word1.length()次删除操作。假设word1为空字符串（长度为0），需要进行word2.length（）次删除操作。这么一看，是不是有点递推边界的味道。
继续往下看，假设令dp[i][j] 为从word1的前i个字符转换为word2前j个字符所需的最短编辑记录，那么如何计算dp[i][j]：
- case1:  word1.charAt(i-1) == word2.charAt(j-1)，那么dp[i][j] = dp[i-1][j-1]
- case2:  word1.charAt(i-1) != word2.charAt(j-1)：
    - 可能1: dp[i][j] = dp[i-1][j]+1  // 通过remove
    - 可能2: dp[i][j] = dp[i][j-1] +1 // 通过insert或replace
    - 可能3: dp[i][j] = dp[i-1][j-1]+1 //通过insert或replace

#### 算法实现
```java
public int minDistance(String word1, String word2) {
        if (word1 == null || word2 == null) return 0;
        int m = word1.length();
        int n = word2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int j = 1; j <= n; j++) {
            dp[0][j] = j;
        }
        for (int i = 1; i <= m; i++) {
            dp[i][0] = i;
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i - 1][j - 1]), dp[i][j - 1]) + 1;
                }

            }
        }
        return dp[m][n];
    }
```