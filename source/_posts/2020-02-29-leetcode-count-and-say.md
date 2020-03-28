---
title: leetcode[38] count and say
date: 2020-02-29 14:50:22
urlname: count_and_say
categories:
- leetcode
tags:
- leetcode
- easy

---

 >[13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
 这道题最重要的还是能读懂，英文意思就是，每一当前n计算得到的结果，取决于之n-1计算得到的记过，所以第一感觉是递归。按照题目的滚则，“1”就是一个1，“2”就是一个2名， “1121”就是两个1，一个2，一个1，这么去报数。   
 <!--more-->
 ```java
 public String countAndSay(int n) {
        if (n == 1)
            return "1";
        String s = countAndSay(n - 1) + '*';
        int count = 1;
        String t = "";
        for (int i = 0; i < s.length() - 1; i++) {
            if (s.charAt(i) == s.charAt(i + 1)) {
                count++;
            } else {
                t = t + count + s.charAt(i);
                count = 1;
            }
        }
        return t;
    }
 ```


