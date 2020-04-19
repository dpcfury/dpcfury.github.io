---
title: Leetcode[55] Jump Game
date: 2020-04-17 23:08:22
urlname: jump-game
categories:
- leetcode
tags:
- leetcode
- dynamic programming
---

>[Leetcode 55. Jump Game](https://leetcode.com/problems/jump-game/)
经典的动态规划题，解题思路比较容易，按递归式实现即可，基本的算法时间复杂度o(n * 2)，其实可以降低为o(n)，遍历当前的最可达范围，只要能够触及到终点，就可以收敛返回。


<!--more-->
#### 算法实现
approach1
```java
public boolean canJump(int[] nums) {
        if (nums == null || nums.length == 0) return false;
        int n = nums.length;
        boolean[] dp = new boolean[n];
        dp[0] = true;
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && j + nums[j] >= i) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[n - 1];
    }
```

approach2
```java
public boolean canJump(int[] nums) {
        int index = 0;
        int max = 0;
        int target = nums.length - 1;
        if (target == 0) return true;
        while (index <= max) {
            max = Math.max(nums[index] + index, max);
            index++;
            if (max >= target) return true;

        }
        return false;
    }
````
