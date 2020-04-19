---
title: Leetcode[887] Super Egg Drop
date: 2020-04-11 13:28:55
urlname: super-egg-drop
categories:
- leetcode
tags:
- leetcode
- dynamic programming
- binary search
- hard
---
>[Leetcode 887. Super Egg Drop](https://leetcode.com/problems/super-egg-drop/)
题目：K 个鸡蛋，并可以使用一栋从 1 到 N  共有 N 层楼的建筑。
每个蛋的功能都是一样的，如果一个蛋碎了，你就不能再把它掉下去。
你知道存在楼层 F ，满足 0 <= F <= N 任何从高于 F 的楼层落下的鸡蛋都会碎，从 F 楼层或比它低的楼层落下的鸡蛋都不会破。
每次移动，你可以取一个鸡蛋（如果你有完整的鸡蛋）并把它从任一楼层 X 扔下（满足 1 <= X <= N）。
你的目标是确切地知道 F 的值是多少。

求：无论 F 的初始值如何，你确定 F 的值的最小移动次数是多少？
<!--more-->

#### 思路
题目只要推导推导，很快会发现这题是动态规划的味道，令dp[K][N]为给定K个鸡蛋，测量N层楼需要的最少移动次数，可以推导出表达式：
```java
dp(K,N)= V 1≤X≤N min(max(dp(K−1,X−1),dp(K,N−X)))
```

但是问题是，如果通过一个一个遍历的方式，寻找这个满足最少条件的的X，时间复杂度为O(k*N*N),提交会TLL

结合官网给出的数学分析，可以将寻找这个X的过程通过二分实现，进而将时间复杂度降低为O(K∗NlogN).

![数学分析](https://leetcode.com/problems/super-egg-drop/Figures/891/sketch.png)

#### 算法实现
```java
public int superEggDrop(int K, int N) {
        int[][] dp = new int[K + 1][N + 1];
        for (int i = 1; i <= N; i++) dp[1][i] = i;
        for (int i = 2; i <= K; i++) {
            for (int j = 1; j <= N; j++) {
                dp[i][j] = j;
                int low = 1, high = j;
                while (low < high) {
                    int mid = (low + high) / 2;
                    if (dp[i - 1][mid - 1] < dp[i][j - mid]) low = mid + 1;
                    else high = mid;
                }
                dp[i][j] = Math.min(dp[i][j], Math.max(dp[i - 1][high - 1], dp[i][j - high]) + 1);
            }

        }
        return dp[K][N];
    }
```

题目其实还有O(KN)的实现方式，这个记个TODO，后续跟进