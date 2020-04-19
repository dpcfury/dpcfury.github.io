---
title: Leetcode[42] Trapping Rain Water
date: 2020-04-04 09:13:04
urlname: trapping-rain-water
categories:
- leetcode
tags:
- leetcode
- hard
---
>[Leetcode 42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
题目给定一个整形数组，代表高低相间的柱状体，问下完雨之后，这些柱状体之间的缝隙空间能装载多少水。这里提供自己比较喜欢的一种直观解法，理解非常方便。

<!--more -->

#### 思路
从每个柱体进行考虑，这一格柱体能承载多少水，是由其左侧最高的柱体+ 右侧最高的柱体+本身的柱体高度决定，只要能计算出每个柱体能承载多少水，就能计算出总的盛水量。
实现方式：
- 从左往右遍历，计算出每个柱体左边最高的柱体，记为leftMax[i]；
- 从右往左遍历，计算出每个柱体右侧最高的柱体，记为rightMax[i];
- 从迁往后遍历，计算每个柱体盛水量

#### 代码实现
```java
public int trap(int[] height) {
        if (height == null || height.length == 0) return 0;
        int[] leftMax = new int[height.length];
        int[] rightMax = new int[height.length];
        int max1 = height[0];
        for (int i = 0; i < height.length; i++) {
            max1 = Math.max(max1, height[i]);
            leftMax[i] = max1;
        }

        int max2 = height[height.length - 1];
        for (int j = height.length - 1; j >= 0; j--) {
            max2 = Math.max(max2, height[j]);
            rightMax[j] = max2;
        }

        int num = 0;
        for (int i = 0; i < height.length; i++) {
            int min = Math.min(leftMax[i], rightMax[i]);
            if (min > height[i]) num += min - height[i];

        }
        return num;
    }
```