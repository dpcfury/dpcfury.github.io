---
title: Leetcode[198] House Robber
date: 2020-03-24 22:10:43
urlname: leetcode-house-robber
categories:
- leetcode
tags:
- leetcode
- easy
- dynamic programming
---

>[198. House Robber](https://leetcode.com/problems/house-robber/)
题目描述，给定一组序列，对其中的每一个元素，可以选择拿或者不拿，但是拿了之后，后相邻的下一个元素就不能拿，求能够拿的最大数量。属于特别典型的dp问题，推到出递归式即可。这题在国内还换了种说法，变成了[按摩师](https://leetcode-cn.com/problems/the-masseuse-lcci/)，内容完全一样。

<!-- more-->

#### 写法一
```java
public int massage(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        int[] dp = new int[nums.length + 1];
        dp[0] = 0;
        dp[1] = nums[0];
        for (int i = 2; i <= nums.length; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i-1]);
        }
        return dp[nums.length];
    }
```

#### 写法二
```java
public int rob(int[] nums) {
        if (nums.length == 0)
            return 0;
        int[] max = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            if (i == 0)
                max[i] = nums[i];
            else if (i == 1)
                max[i] = Math.max(nums[0], nums[1]);
            else
                max[i] = Math.max(max[i - 1], max[i - 2] + nums[i]);
        }
        return max[nums.length - 1];
    }
```
