---
title: Leetcode [1248] Count Number of Nice Subarrays
date: 2020-04-21 22:08:36
urlname: count-number-of-nice-subarrays
categories:
- leetcode
tags:
- leetcode
- sliding window
- medium
- array
---
>[Leetcode 1248. Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays/)
题目要求计算给定数组nums中，满足包含K个奇数元素的子数组的个数，这个题目比较容易想到滑动窗口的思路，关键是对前后偶数元素的计算和边界的控制。

<!--more -->

#### 思路
- 首先统计出每个奇数元素在数组中的出现位置，并记录在中间数组oddIndex中
- 遍历每组K个奇数组合，即oddIndex[i] 与oddIndex[i+k-1],通过计算左右边界与前后奇数元素的间隔，就能得到前后偶数的个数，相乘就能得到组合的子数组个数


#### 算法实现
```java
 public int numberOfSubarrays(int[] nums, int k) {
        int len = nums.length;
        int count = 0;
        int oddNum = 0;
        int[] oddIndex = new int[len + 2];
        for (int i = 0; i < len; i++) {
            if (nums[i] % 2 == 1) oddIndex[++oddNum] = i;
        }
        oddIndex[0] = -1; // 左边界
        oddIndex[oddNum + 1] = len; // 右边界

        for (int i = 1; i + k <= oddNum + 1; i++) {
            count += (oddIndex[i] - oddIndex[i - 1]) * (oddIndex[i + k] - oddIndex[i + k - 1]);
        }

        return count;
    }
```

类似题目：[Leetcode 560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)