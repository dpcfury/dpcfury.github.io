---
title: Leetcode[168] Excel Sheet Column Title
date: 2020-03-27 19:38:32
urlname: leetcode-excel-sheet-column-title
categories:
- leetcode
tags:
- leetcode
- easy
---

>(Leetcode 168. Excel Sheet Column Title)[https://leetcode.com/problems/excel-sheet-column-title/]
题目求数字到Excel sheet编号的转换，其实本质就是进制的转换，但是需要注意的是，这边的A是代表1，z代表的是26，如果直接取余数，会发现结果查一位，这里有个技巧就是，每次将初数-1操作,可以看成逻辑上将A从0开始算。

<!--more-->

```java
public String convertToTitle(int n) {
        StringBuilder str = new StringBuilder();
        while (n != 0) {
            n--;
            int num = n % 26;
            str.append((char) (num + 65));
            n = n / 26;
        }
        return str.reverse().toString();

    }
```
