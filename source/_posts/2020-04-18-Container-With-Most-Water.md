---
title: Leetcode[11] Container With Most Water
date: 2020-04-18 08:53:37
urlname: container-with-most-water
categories:
- leetcode
tags:
- leetcode
- two pointers
- medium
---
>[Leetcode 11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
题目是经典的双指针问题，给定一个高度的数组，求能够围成的长方形最大面积，很中规中矩，从这题可以延伸到trapping rain waterh那道题。

<!--more-->
#### 思路
直观上肯定是希望长和高都尽量大，容易得到一个面积更大的长方形，那么是不是外圈的更容易是最终解呢，这不一定，可能内圈的长方形高度很高。因此，结题思路其实是设置两个指针，从外往里遍历求解，如果左侧指针的高度小于右侧，则将左侧指针左移，如果右侧指针高度小于左侧，则将右侧指针左移,因为当前搜索空间中，这个面积取决于短的那个高度，所以哪个短，就尝试移动哪个获取更大面积的可能。


#### 算法实现
```java
public int maxArea(int[] height) {
        int left = 0, right = height.length - 1;
        int max = 0;
        while (left < right) {
            int len = right - left;
            int h = Math.min(height[left], height[right]);
            max = Math.max(max, len * h);
            if (height[left] < height[right]) left++;
            else right--;
        }
        return max;
    }
```