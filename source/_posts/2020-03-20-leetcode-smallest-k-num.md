---
title: Leetcode.cn 最小的k个数
date: 2020-03-20 21:50:59
urlname: leetcode-cn-k-smallest-num
categories:
- leetcode
tags:
- leetcode
- heap sort
- priority queye
---

>[Leetcode中国每日一题](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/submissions/)取自剑指offer
题目意思就是从一个一维数组中，找出k个最小的数，每次看到这个K，最容易想到的就是堆排序，通过构造大顶堆或小顶堆分别来找最小的k个数和最大的k个数。而Java中天生有支持堆排序的数据结构，PriorityQueue，只要指定K容量的堆，通过比较堆顶元素与当前数组遍历元素，如果小于堆顶，则堆顶出列，插入当前遍历元素，遍历完成之后，堆中的K个数即我们要找的最小K个数。

<!-- more -->

```java
public int[] getLeastNumbers(int[] arr, int k) {
        int[] result = {};
        if (arr == null || k > arr.length || k == 0) return result;
        PriorityQueue<Integer> queue = new PriorityQueue<>(k, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2.compareTo(o1);
            }
        });
        for (int i = 0; i < arr.length; i++) {
            if (queue.size() != k) {
                queue.offer(arr[i]);
            } else {
                if (arr[i] < queue.peek()) {
                    queue.poll();
                    queue.offer(arr[i]);
                }
            }
        }
        result = new int[k];
        for (int i = 0; i < k; i++) {
            result[i] = queue.poll();
        }
        return result;
    }
```

