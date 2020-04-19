---
title: Leetcode[445] Add Two Numbers II
date: 2020-04-14 22:40:22
urlname: add-two-numsII
categories:
- leetcode
tags:
- leetcode
- linked-list
- medium
---
>[Leetcode 445. Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii/)
题目要计算两个链表表示的整数相加，其本质是两个字符串的数字相加，转化为链表的形式即可，可以灵活的利用字符串的翻转，切记直接用整数int或long来存储链表表示的数值容易溢出，转换为字符串是较为合适的方法。

<!--more-->

#### 思路
- 遍历两组链表，获取数字的字符串形式
- 转化为字符串形式的两数相加
- 字符串遍历，头插法生成链表

#### 算法实现
目前写的有点啰嗦，时间比较敢写的，后续再优化TODO
```java
static class ListNode {
        int val;
        ListNode next;

        public ListNode(int x) {
            this.val = x;
        }
    }

    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // 思路：先整成数字，再去按位生成链表
        String numA = generateNum(l1);
        String numB = generateNum(l2);
        String sum = addNum(numA, numB);
        ListNode head = new ListNode(0);
        char[] chs = sum.toCharArray();
        for (int i = 0; i < chs.length; i++) {
            ListNode p = new ListNode(chs[i] - '0');
            p.next = head.next;
            head.next = p;
        }
        return head.next;
    }

    private String addNum(String num1, String num2) {
        int carry = 0;
        int index = 0;
        StringBuilder res = new StringBuilder();
        int min = Math.min(num1.length(), num2.length());
        while (index < min) {
            int sum = num1.charAt(index) - '0' + num2.charAt(index) - '0' + carry;
            res.append(sum % 10);
            carry = sum / 10;
            index++;
        }

        while (index >= num2.length() && index < num1.length()) {
            int sum = num1.charAt(index) - '0' + carry;
            res.append(sum % 10);
            carry = sum / 10;
            index++;
        }
        while (index >= num1.length() && index < num2.length()) {
            int sum = num2.charAt(index) - '0' + carry;
            res.append(sum % 10);
            carry = sum / 10;
            index++;
        }
        if (carry != 0) res.append(carry);

        return res.toString();
    }

    private String generateNum(ListNode head) {
        StringBuilder res = new StringBuilder();
        if (head == null) return res.toString();
        ListNode p = head;
        while (p != null) {
            res.append(p.val);
            p = p.next;
        }
        return res.reverse().toString();
    }
```