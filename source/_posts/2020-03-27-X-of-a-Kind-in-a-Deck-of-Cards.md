---
title: Leetcode[914] X of a Kind in a Deck of Cards
date: 2020-03-27 21:17:06
urlname: leetcode-x-of-a-kind-in-a-deck
categories:
- leetcode
tags:
- leetcode
- easy
- gcd
---

>[Leetcode 914. X of a Kind in a Deck of Cards](https://leetcode.com/problems/x-of-a-kind-in-a-deck-of-cards/)
题目的直白含义就是：给出一堆数组，判断是否有一种划分方式，让每个分组内的元素都相同，每个组的元素个数页相同。暴力的解法比较直观，但是背后的含义其实是：对于每一种的牌的数量Ni，如果能够满足按x进行分组，那么Ni % x ==0,那么对于不同种类的牌而言，是否存在x这种划分，就变成，这组牌的数量的最大公约数是否满足大于等于 2，由此变为如何求一组数的最大公约数。

<!-- more-->

```java
 public boolean hasGroupsSizeX(int[] deck) {
        HashMap<Integer, Integer> count = new HashMap<>();
        for (int i : deck) count.put(i, count.getOrDefault(i, 0) + 1);
        int g = -1;
        for (Map.Entry<Integer, Integer> entry : count.entrySet()) {
            if (g == -1) {
                g = entry.getValue();
            } else {
                g = gcd(g, entry.getValue());
            }
        }
        return g >= 2;
    }

    private int gcd(int x, int y) {
        if (x == 0) return y;
        else return gcd(y % x, x);
    }
```

