---
title: Leetcode[26] Merge k Sorted Lists
date: 2020-04-26 20:28:38
urlname: merge-k-sorted-list
categories:
- leetcode
tags:
- leetcode
- linkedlist
- hard
---
>[Leetcode 26. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
题目：合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

<!--more-->

#### 思路
将所有链表中的元素遍历存入有限队列中，再通过头插法进行重组链表，时间复杂度O(kN * logkN),空间复杂度O(kN)，其他则可以两两合并，或者分治归并，这里偷懒写的是优先队列的形式。

#### 算法实现
```java
public ListNode mergeKLists(ListNode[] lists) {
        ListNode head = new ListNode(-1);
        PriorityQueue<ListNode> queue = new PriorityQueue<>(new Comparator<ListNode>() {
            @Override
            public int compare(ListNode o1, ListNode o2) {
                return o2.val - o1.val; // 降序
            }
        });

        for (ListNode p : lists) {
            while (p != null) {
                queue.offer(p);
                p = p.next;
            }
        }

        while (!queue.isEmpty()) {
            ListNode p = queue.poll();
            p.next = head.next; // 头插法
            head.next = p;
        }


        return head.next;
    }
```
