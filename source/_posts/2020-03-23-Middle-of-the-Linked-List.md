---
title: Leetcode[876] Middle of the Linked List
date: 2020-03-23 22:35:01
urlname: leetcode-middle-of-the-linked-list
categories:
- leetcode
tags:
- leetcode
- linked list
- easy
---

>[Leetcode 876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)
题目目的是求一个非空链表中，从中间元素开始往后的截取一半列表。比较直观的就是设立两个元素以相差一倍的速度进行遍历，当跑的快的达到null，或者即将为null，再返回慢的元素或慢元素的下一个位置。

<!-- more -->

```java
public ListNode middleNode(ListNode head) {
        ListNode p = head;
        ListNode q = head.next;
        while (q != null) {
            if (q == null || q.next == null) break;
            p = p.next;
            q = q.next.next;
        }
        return q == null ? p : p.next;

    }
```