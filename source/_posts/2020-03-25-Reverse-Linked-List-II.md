---
title: Leetcode[92] Reverse Linked List II
date: 2020-03-25 21:35:07
urlname: leetcode-reverse-linked-list-II
categories:
- leetcode
tags:
- leetcode
- linkedlist
- medium
- stack
---

>[Leetcode 92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
题目要求在一次遍历完成对链表中m-n的元素进行反转，由于是单链表，涉及到这种顺序的饭庄，首先会想到栈，将m-n的元素放入栈中，再记录m-1个元素的位置，通过出栈再重新链接，即可得到反转后的链表。

<!-- more-->

```java
public ListNode reverseBetween(ListNode head, int m, int n) {
        if (head == null) return head;
        Stack<ListNode> stack = new Stack<>();
        ListNode newHead = new ListNode(0);
        newHead.next = head;
        ListNode before = newHead;
        ListNode p = newHead;
        int count = 0;
        while (p != null) {
            before = p;
            p = p.next;
            count++;
            if (count == m) break;

        }

        while (count <= n && p != null) {
            stack.add(p);
            p = p.next;
            count++;
        }

        while (!stack.isEmpty()) {
            ListNode node = stack.pop();
            before.next = node;
            before = node;
        }

        before.next = p;
        return newHead.next;
    }
```