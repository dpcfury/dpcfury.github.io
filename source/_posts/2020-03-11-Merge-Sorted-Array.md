---
title: leetcode[88] Merge Sorted Array
date: 2020-03-11 20:28:25
urlname: leetcode-merge-sorted-array
categories:
- leetcode
tags:
- leetcode
- easy
---
> [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
记得好像是剑指offer上一个很基础的题目，非常适合休闲和找状态，特别干了一天比较累的情况下刷着玩玩，思路也很简单，就是避免数组内的移动，从后往前安排元素，
时间复杂度控制在o(m+n)
<!--more-->

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;
        while (i >= 0 && j >= 0) {
            if (nums1[i] >= nums2[j]) {
                nums1[k] = nums1[i];
                i--;
            } else {
                nums1[k] = nums2[j];
                j--;
            }

            k--;
        }

        if (i < 0) {
            for (; k >= 0; k--) {
                nums1[k] = nums2[j--];
            }
        }
    }
```
