---
title: Leetcode [151] Reverse Words in a String
date: 2020-04-10 21:01:09
urlname: reverse-words-in-a-string
categories:
- leetcode
tags:
- leetcode
- String
- medium
---
>[ Leetcode 151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)
给定一个字符串，逐个翻转字符串中的每个单词。要求是去除前后的空格，并且单词间间隔一个空格，很常规的字符串题目。

<!--more-->
#### 思路
从字符串的尾部开始便利，遇到空格跳过，遇到非空格，开始遍历合法的单词，并利用substring存入到中间结果集中，最后统一处理拼接字符串，记得去除尾部的空格。

#### 算法实现
```java
public String reverseWords(String s) {
        if (s == null || s.length() == 0) return "";
        StringBuilder str = new StringBuilder();
        List<String> res = new ArrayList<>();
        int i = s.length() - 1;
        while (i >= 0) {
            if (s.charAt(i) == ' ') {
                i--;
                continue;
            }
            int j = i;
            while (j >= 0 && s.charAt(j) != ' ') j--;
            res.add(s.substring(j + 1, i + 1));
            i = j;
        }

        for (String word : res) str.append(word).append(' ');
        if (str.length() != 0) str.deleteCharAt(str.length() - 1);
        return str.toString();
    }

```