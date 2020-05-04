---
title: Leetcode[45] Jump Game II
date: 2020-05-04 17:15:14
urlname: jump-gameII
categories:
- leetcode
tags:
- leetcode
- greedy
- hard
---
>[Leetcode 45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)
题目：
给定一个非负整数数组，你最初位于数组的第一个位置。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
你的目标是使用最少的跳跃次数到达数组的最后一个位置。
示例:

输入: [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
<!--more-->

#### 思路
- 利用动态规划，算法时间复杂度为O(n^2)
- 利用贪心算法，更新遍历每步能走到的最远距离

#### 算法实现
approach1: 动态规划
```java
public int jump(int[] nums) { //dp 超时
        if (nums == null || nums.length == 0) return 0;
        int[] dp = new int[nums.length];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (j + nums[j] >= i) {
                    dp[i] = Math.min(dp[i], dp[j] + 1);

                }
            }

        }
        return dp[nums.length - 1];
    }
```

approach2：贪心
```java
 public int jump1(int[] nums) { // greedy
        int n = nums.length;
        int step = n == 1 ? 0 : 1;
        int max = nums[0];
        int start = 1;
        while (max < n - 1) {
            int curMax = max;
            while (start <= curMax) {
                if (start + nums[start] > max) max = start + nums[start];
                start++;
            }
            start = curMax + 1;
            step++;

        }
        return step;
    }
```