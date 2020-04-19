---
title: Leetcode[8]String to Integer (atoi)
date: 2020-04-03 22:41:07
urlname: string-to-integer
categories:
- leetcode
tags:
- leetcode
- medium
---
>[Leetcode 8. String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)
题目要实现一个 atoi 函数，使其能将字符串转换成整数。确切的说，这种字符的类型五花八门，有很多中形式，除了正负号的判断，还需要判断数字的合法性，数字的长度是否超越32位整数的边界等，属于细节题，不做过多的描述，比较好的方式是不用借助太多的标记为，而是分阶段进行处理，先处理空格，再处理符号，再处理数字。

<!--more -->

```java
public int myAtoi(String str) {
        if(str.isEmpty()) {
            return 0;
        }

        int index=0;
        int startIndex=0;
        boolean negative=false;
        while(str.length()>index && str.charAt(index)==' ') {
            index++;
        }
        if(index>=str.length()) {
            return 0;
        }

        if(str.charAt(index)=='+') {
            index++;
        } else if(str.charAt(index)=='-') {
            index++;
            negative=true;
        }

        if(index>=str.length()) {
            return 0;
        }

        if(Character.isDigit(str.charAt(index))==false) {
            return 0;
        } else {
            startIndex=index;
        }
        int endIndex=startIndex;
        for(int i=startIndex; i<str.length(); i++) {
            if(str.charAt(i)>='0' && str.charAt(i)<='9') {
                endIndex++;
            } else {
                break;
            }
        }
        double num=0;
        for(int i=startIndex; i<endIndex; i++) {
            num=(num*10)+Double.parseDouble(Character.toString(str.charAt(i)));
        }
        if(num>Integer.MAX_VALUE) {
            return (negative)?Integer.MIN_VALUE:Integer.MAX_VALUE;
        }

        return negative?(int)(-1*num):(int)num;
    }
```