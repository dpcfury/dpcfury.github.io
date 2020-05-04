---
title: Leetcode[33]Search in Rotated Sorted Array
date: 2020-04-27 21:42:15
urlname: search-in-rotated-sorted-array
categories:
- leetcode
tags:
- leetcode
- binary search
- medium
---

>[Leetcode 33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
题目：假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

<!--more-->

#### 思路
遇到有序的数组，时间复杂度又是要求logn，自然而然会想到** binary search ** ，但是此题不同的是，数组是一半一半的有序，而不是完整的一个有序数组，那么如何在这样的数组上进行二叉搜索呢，核心是判断[start,mid]元素是处于有序状态，还是[mid, end]处于有序状态，再分情况去更新搜索的左右边界。

#### 算法实现
```java
public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) return -1;
        int start = 0, end = nums.length - 1;
        while (start <= end) {
            int mid = (start + end) / 2;
            if (nums[mid] == target)
                return mid;

            if (nums[start] <= nums[mid]) {
                if (target < nums[mid] && target >= nums[start])
                    end = mid - 1;
                else
                    start = mid + 1;
            }

            if (nums[mid] <= nums[end]) {
                if (target > nums[mid] && target <= nums[end])
                    start = mid + 1;
                else
                    end = mid - 1;
            }

        }

        return -1;
    }
```