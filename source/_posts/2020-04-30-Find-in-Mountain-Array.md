---
title: Leetcode[1095] Find in Mountain Array
date: 2020-04-30 00:10:36
urlname: find-in-mountain-array
categories:
- leetcode
tags:
- leetcode
- binary search
- interactive problem
- hard
---
>[Leetcode 1095. Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array/)
题目：给定一个山脉数组，以及一个目标数，判断山脉数组中是否包含这个数，对查询的操作只能通过MoutainArray接口，并且超过指定的查找次数就会提示超时，属于interactive problem。

<!--more -->
#### 思路
想到很快的能够找到对应元素，以及山脉数组的特征，会想到二分查找，核心是：
- 找到山峰，切分为两个子有序数组
- 分别再二分查找
- 缓存已查询的结果

#### 算法实现
大晚上下班赶着写的，目前还比较糙，后续待优化
```java
HashMap<Integer, Integer> cache = new HashMap<>();
    int len = 0;

    public int findInMountainArray(int target, MountainArray mountainArr) {
        len = mountainArr.length();
        int low = 0, high = len - 1;
        int peek = -1;
        while (low <= high) {
            int mid = (low + high) >> 1;
            int value = get(mid, mountainArr);
            int right = mid + 1 < len ? get(mid + 1, mountainArr) : 0;
            if (value < right) {
                low = mid + 1;
            } else high = mid - 1;


        }
        peek = low;


        low = 0;
        high = peek;
        while (low <= high) {
            int mid = (low + high) >> 1;

            int value = get(mid, mountainArr);

            if (value == target) return mid;
            else if (value > target) high = mid - 1;
            else low = mid + 1;
        }

        low = peek;
        high = len - 1;
        while (low <= high) {
            int mid = (low + high) >> 1;
            int value = get(mid, mountainArr);
            if (value == target) return mid;
            else if (value > target) low = mid + 1;
            else high = mid - 1;
        }


        return -1;
    }

    int get(int index, MountainArray array) {
        if (index < 0 || index >= len) return 0;
        else {
            if (cache.containsKey(index)) return cache.get(index);
            else {
                int value = array.get(index);
                cache.put(index, value);
                return value;
            }
        }
    }
```