---
title: leetcode[67]add binay
date: 2020-03-10 22:28:44
urlname: leetcode-add-binary
categories:
- leetcode
tags:
- leetcode
- easy
---
 >[67. Add Binary](https://leetcode.com/problems/add-binary/)
  这题非常中规中矩，组织好逻辑就可以一步到位，就是简单的运算
  <!--more-->

 ```java
 public String addBinary(String a, String b) {
        int i = a.length() - 1;
        int j = b.length() - 1;
        boolean carry = false;
        StringBuilder str = new StringBuilder();
        while (i >= 0 && j >= 0) {
            if (a.charAt(i) == '1' && b.charAt(j) == '1') {
                if (carry) {
                    str.append('1');
                } else {
                    str.append('0');
                    carry = true;
                }

            } else if (a.charAt(i) == '1' || b.charAt(j) == '1') {
                if (carry) {
                    str.append('0');
                } else {
                    str.append('1');
                }
            } else {
                char c = carry ? '1' : '0';
                str.append(c);
                carry = false;
            }
            i--;
            j--;
        }
        if (i < 0 && j < 0) {
            if (carry)
                str.append('1');
        } else if (i < 0)
            str.append(add(b, j, carry));
        else
            str.append(add(a, i, carry));


        return str.reverse().toString();
    }

    private String add(String s, int j, boolean carry) {
        StringBuilder str = new StringBuilder();
        for (; j >= 0; j--) {
            char c = s.charAt(j);
            if (carry) {
                str.append(c == '1' ? '0' : '1');
                if (c == '0')
                    carry = false;
            } else {
                str.append(c == '1' ? '1' : '0');
            }
        }
        if (carry)
            str.append('1');
        return str.toString();
    }
 ```
