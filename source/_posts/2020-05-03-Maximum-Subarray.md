---
title: Leetcode[53] Maximum Subarray
date: 2020-05-03 21:02:19
urlname: maximum-subarray
categories:
- leetcode
tags:
- leetcoe
- array
- easy
---
>[Leetcode 53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
题目：给定一个整数数组 ** nums ** ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

<!--more-->

#### 思路
- 遍历数组，追踪当前能获取的数组和，如果之前的sum已经小于等于0，那么最大的子数组不应该从之前开头，此时更新子数组的开头为当前元素
- 持续更新子数组最大的sum记录

#### 算法实现
approach1：
```java
public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        int curSum = 0;
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {

            if (curSum <= 0) {
                curSum = nums[i];
            } else {
                curSum += nums[i];
            }
            max = Math.max(max, curSum);
        }
        return max;
    }
```
类似滚动数组的实现方法：https://leetcode-cn.com/problems/maximum-subarray/solution/zui-da-zi-xu-he-by-leetcode-solution/
```golang
func maxSubArray(nums []int) int {
    max := nums[0]
    for i := 1; i < len(nums); i++ {
        if nums[i] + nums[i-1] > nums[i] {
            nums[i] += nums[i-1]
        }
        if nums[i] > max {
            max = nums[i]
        }
    }
    return max
}
```

approach2：
线段数的解法 TODO，了解线段树的作用


