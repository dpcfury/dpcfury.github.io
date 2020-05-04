---
title: Leetcode[21] Merge Two Sorted Lists
date: 2020-05-01 15:30:47
urlname: merge-two-sorted-lists
categories:
- leetcode
tags:
- leetcode
- linkedlist
- easy
---
>[Leetcode 21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
五一轻松一下，题目：和并两个有序的链表，注意细节，争取一把过。

<!--more-->

#### 思路
- 两头遍历，按小的先插
- 注意合并过程中一个插完，一个还剩一部分的情况

#### 算法实现
```java
 public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(-1);
        ListNode p = l1;
        ListNode q = l2;
        ListNode node = head;
        while (p != null && q != null) {
            if (p.val < q.val) {
                node.next = p;
                node = p;
                p = p.next;
            } else {
                node.next = q;
                node = q;
                q = q.next;
            }
        }

        if (p != null) node.next = p;
        if (q != null) node.next = q;

        return head.next;
    }
```