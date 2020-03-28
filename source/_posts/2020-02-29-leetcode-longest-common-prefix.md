---
title: leetcode[14]longeest common prefix
date: 2020-02-29 21:29:02
urlname: leetcode-longest-common-prefix
categories:
- leetcode
tags:
- leetcode
- easy
---
>[14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)
这题比较有意思，我开始给出的算法属于很直接的那种，针对规则编码，其实这个问题可以进行总结和分析，会有更深的收获
<!--more-->

```java
public String longestCommonPrefix(String[] strs) {

        int i = 0;
        String result = "";
        if (strs.length == 0)
            return result;
        boolean flag = true;
        while (flag) {
            char c = i < strs[0].length() ? strs[0].charAt(i) : ' ';
            for (String s : strs) {

                if (i == s.length() || s.charAt(i) != c) {
                    flag = false;
                    break;
                }
            }
            if (flag)
                result = result + c;
            i++;
        }
        return result;
    }
```

> 官方解法
#### approach 1. Horizontal Scan
```java
 public String longestCommonPrefix(String[] strs) {
    if (strs.length == 0) return "";
    String prefix = strs[0];
    for (int i = 1; i < strs.length; i++)
        while (strs[i].indexOf(prefix) != 0) {
            prefix = prefix.substring(0, prefix.length() - 1);
            if (prefix.isEmpty()) return "";
        }
    return prefix;
}
```
> 比较形式化的防范，从approach1 的LCP公式衍生而来
#### approach 2. Divide and Conquer
```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";    
        return longestCommonPrefix(strs, 0 , strs.length - 1);
}

private String longestCommonPrefix(String[] strs, int l, int r) {
    if (l == r) {
        return strs[l];
    }
    else {
        int mid = (l + r)/2;
        String lcpLeft =   longestCommonPrefix(strs, l , mid);
        String lcpRight =  longestCommonPrefix(strs, mid + 1,r);
        return commonPrefix(lcpLeft, lcpRight);
   }
}

String commonPrefix(String left,String right) {
    int min = Math.min(left.length(), right.length());       
    for (int i = 0; i < min; i++) {
        if ( left.charAt(i) != right.charAt(i) )
            return left.substring(0, i);
    }
    return left.substring(0, min);
}
```
>更脱俗的一个解法是，利用binary search的思路，缩小搜索范围
#### approach 3. binary search
```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0)
        return "";
    int minLen = Integer.MAX_VALUE;
    for (String str : strs)
        minLen = Math.min(minLen, str.length());
    int low = 1;
    int high = minLen;
    while (low <= high) {
        int middle = (low + high) / 2;
        if (isCommonPrefix(strs, middle))
            low = middle + 1;
        else
            high = middle - 1;
    }
    return strs[0].substring(0, (low + high) / 2);
}

private boolean isCommonPrefix(String[] strs, int len){
    String str1 = strs[0].substring(0,len);
    for (int i = 1; i < strs.length; i++)
        if (!strs[i].startsWith(str1))
            return false;
    return true;
}
```
