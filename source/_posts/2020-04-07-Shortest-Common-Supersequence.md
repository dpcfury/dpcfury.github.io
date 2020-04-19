---
title: Leetcode[1092] Shortest Common Supersequence
date: 2020-04-07 23:56:26
urlname: shortest-common-supersequence
categories:
- leetcode
tags:
- leetcode
- dynamic programming
- hard
---
>[Leetcode 1092. Shortest Common Supersequence](https://leetcode.com/problems/shortest-common-supersequence/)
这道题延伸自最短编辑距离问题（Edit Distance），题目唯一描述不同的是，需要求出最终的结果，而不仅仅是最短的长度。结题的思路还是先去找问题的收敛入口，进而找到子问题和最优结构，推导出表达式后，再自定向上求解。

<!--more-->

#### 思路
和普通的do不同，这里需要计算出最终的superSequence结果，乍一看会有点懵，但是其实道理相同，还是
先令dp[i][j] 为str1前i个字符和str2的前j个字符串，其最短的superSequence长度；
那么表达式为：
- case0: s.charAt[i] == s.charAt[j]，则dp[i][j] = dp[i+1][j-1] +1
- case1: s.charAt[i] != s.charAt[j]，则dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1])+1

然后从后往前，分批便利str1和str2，如果当前两个字符串位置上的字符相同，就把字符添加到结果，如果不同，选择dp[i-1][j] 和dp[i][j-1]中小的那个，添加对应的字符即可。

#### 算法实现

approach1 : 最初的写法，空间会超
```java
public String shortestCommonSupersequence1(String str1, String str2) {
        int m = str1.length();
        int n = str2.length();
        String[][] dp = new String[m + 1][n + 1];
        for (int i = 0; i <= m; i++) {
            dp[i][0] = str1.substring(0, i);
        }
        for (int j = 0; j <= n; j++) {
            dp[0][j] = str2.substring(0, j);
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                char c1 = str1.charAt(i - 1);
                char c2 = str2.charAt(j - 1);
                if (c1 == c2) dp[i][j] = dp[i - 1][j - 1] + c1;
                else {
                    String s1 = dp[i][j - 1];
                    String s2 = dp[i - 1][j];
                    String s3 = dp[i - 1][j - 1];
                    String res = "";
                    if (s1.length() < s2.length()) res = s1 + c2;
                    else res = s2 + c1;
                    if (s3.length() + 2 < res.length()) res = s3 + c1 + c2;
                    dp[i][j] = res;
                }
            }
        }

        return dp[m][n];
    }
```

approach2: 优化写法
```java
public String shortestCommonSupersequence(String str1, String str2) {
        int i = str1.length();
        int j = str2.length();
        int[][] dp = shortestCS(str1, str2);
        StringBuilder str = new StringBuilder();
        while (i > 0 && j > 0) {
            if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                str.append(str1.charAt(i - 1));
                i--;
                j--;
            } else if (dp[i - 1][j] < dp[i][j - 1]) {
                str.append(str1.charAt(i - 1));
                i--;
            } else {
                str.append(str2.charAt(j - 1));
                j--;
            }
        }
        while (i > 0) {
            str.append(str1.charAt(i - 1));
            i--;
        }
        while (j > 0) {
            str.append(str2.charAt(j - 1));
            j--;
        }

        return str.reverse().toString();


    }
```