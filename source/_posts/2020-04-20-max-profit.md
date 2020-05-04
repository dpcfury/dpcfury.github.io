---
title: Leetcode[121] Best Time to Buy and Sell Stock
date: 2020-04-20 21:28:10
urlname: best-time-to-buy-and-sell-stock
categories:
- leetcode
tags:
- leetcode
- dynamic programming
- easy
---
>[Leetcode 121 Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
抛售股票的最原始题，只能抛售一次，问能够获取的最大利润，这里给出的是一种比较直观的方法，没有使用dp进行求解。

<!--more-->

#### 思路
- 记录当前出现的最小元素，用当前元素减去最小元素，如果收益更大，则更新全局最大利润
- 如果当前元素比之前出现的最小元素还小，则更新当前最小元素为当前遍历的元素

#### 算法实现
```java
public int maxProfit(int[] prices) {
        //7,1,5,3,6,4  -> 5
        // 7,6,4,3,1 ->0

        if (prices == null || prices.length == 0) return 0;
        int min = prices[0];
        int maxProfit = Integer.MIN_VALUE;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] - min > maxProfit) maxProfit = prices[i] - min;
            if (prices[i] < min) min = prices[i];
        }
        return maxProfit;
    }
```