---
title: 力扣中国[62] Joseph Circle Problem
date: 2020-03-30 21:46:19
urlname: joseph-circle-problem
categories:
- leetcode
tags:
- Math
- easy
---

>[剑指offer Joseph Problem](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)
题目不用多介绍，经典的约瑟夫环问题，求解的过程主要包含两种。一种是直观的构造循环链表，通过多次多次遍历，求的最终结果，时间复杂度为O(mn),另一种就是通过数学分析，找出递归的规律，然后使用递归或者动归求解。

<!--more-->

#### 思路

令f(n-1,m) 为n-1个人，报数m，最终留下的胜者坐标，那么f(n,m)的的胜者同样是f(n-1,m)胜者，但是其下坐标遍了，那么上一轮中胜者的坐标对应应该是(f(n-1,m)+m) % n

即关系式为：f(n,m) = (f(n-1,m)+m) % n

针对0和1个数的情况，胜者坐标都为0
因此解法可写为:
```java
public int lastRemaining(int n, int m) {
        int dp = 0;
        for (int i = 2; i <= n; i++) {
            dp = (dp + m) % i;
        }
        return dp ;
    }
```

>对于这个推到技巧，在知乎看到这么一个总结，感觉比较实用

递归问题分3步走：
1、递归收敛：由于m是不变的，所以只能通过n将规模不断缩小
2、找出口：当递归收敛到最小单位时，能得到一个出口。即当n=1时，胜者的位置为0
3、找规律：分析已知条件，与我们需要结果的关联。
