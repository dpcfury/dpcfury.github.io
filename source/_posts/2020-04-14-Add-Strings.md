---
title: Leetcode[415] Add Strings
date: 2020-04-14 23:14:03
urlname: add-strings
categories:
- leetcode
tags:
- leetcode
- string
- easy
---
>[Leetcode 415. Add Strings](https://leetcode.com/problems/add-strings/)
题目：给定两个非负的整数（字符串形式），不利用java的BigInteger库，实现两个整数的相加，结果仍以字符串形式表示。

<!--more-->

#### 思路
- 翻转字符串
- 从地位到高位计算，记住进位
- 注意边界情况，两数长短不一致的处理

#### 算法实现
```java

    public String addStrings(String num1, String num2) {
        int carry = 0;
        int index = 0;
        num1 = new StringBuilder(num1).reverse().toString();
        num2 = new StringBuilder(num2).reverse().toString();
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

        return res.reverse().toString();
    }
```