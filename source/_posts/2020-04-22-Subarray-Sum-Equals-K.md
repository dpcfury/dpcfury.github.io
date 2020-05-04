---
title: Leetcode[560] Subarray Sum Equals K
date: 2020-04-22 22:16:46
urlname: subarray-sum-equals-k
categories:
- leetcode
tags:
- leetcode
- prefix sum
- medium
---
>[Leetcode 560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。这题是比较典型的前缀和应用类问题，最粗暴的就是O(n^3)的暴力求解。

<!--more-->
#### 思路
approach1 优化求子数组和过程
- 令sum(i) 为遍历到nums第i个元素的前缀和，那么计算 子数组nums[j]-> nusm[k]的和直接等于 nums[k]-nums[i]
- 遍历所有可能的j、k组合，统计所有符合的子数组

approach2 利用map存储
在一次遍历的过程中，记录出现前缀和sum的次数，如果map.containsKey(sum-k)：
- 说明前面出现了nums[i]-nums[j]=k的情况，则可以累加这个sum-k出现的次数

#### 算法实现
```java
public int subarraySum(int[] nums, int k) {
        // 关键思路， sum(i) sum(j) sum(i-j)之间的关系
        if (nums == null || nums.length == 0) return 0;
        HashMap<Integer, Integer> map = new HashMap<>();
        int sum = 0;
        int count = 0;
        map.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (map.containsKey(sum - k)) {
                count += map.get(sum - k);
            }
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
```
