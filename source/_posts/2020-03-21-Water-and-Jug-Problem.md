---
title: letcode[365]Water and Jug Problem
date: 2020-03-21 15:59:14
urlname: leetcode-water-jug-problem
categories:
- leetcode
tags:
- leetcode
- medium
- math
---

>[Leetcode 365. Water and Jug Problem](https://leetcode.com/problems/water-and-jug-problem/)
 题目直面意思，通过两个容器，是否可以计算出指定体积的水，一开始以为什么规律题，折腾了半天，后面看解题思路。这类题目其实属于数学题，即通过机选x,y的最大公约数，并看Z是否能整出这个最大公约数进行判断。

<!-- more-->

####  参考思路：
 https://zhuanlan.zhihu.com/p/68987055

#### 问题转换为gcd
 即如何求两个正整数的最大公约数，思路如下：https://blog.csdn.net/wujingchangye/article/details/88542193
 > 
 1. 假设有两个数a和b，其中a是不小于b的数，记a被b除的余数为r，则a = b*q + r。
 2. 假设a和b的一个约数为u，那么a和b都能被u整除，则a = su b = tu，s和t都是整数。
 3. r = a - bq = su - (tu)q = (s - tq)u，所以r也能被u整除
 4. 所以a和b的任一约数同时也是r的约数（每次取余，直到余数为0）。**

```java
public static int gcd(int p, int q) {
    if(q == 0) {
        return p;
    }
    int r = p % q;
    return gcd(q, r);
}
```

#### 题目完整解答
```java
public boolean canMeasureWater(int x, int y, int z) {
        if (x + y < z) return false;

        if (x == z || y == z || x + y == z) return true;
        return z % gcd(x, y) == 0;
    }

    public int gcd(int x, int y) {
        if (x == 0)
            return y;
        if (y == 0)
            return x;

        return gcd(y, x % y);
    }
```

