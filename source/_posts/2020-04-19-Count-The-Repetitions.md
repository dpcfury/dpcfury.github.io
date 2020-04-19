---
title: Leetcode[466] Count The Repetitions
date: 2020-04-19 14:21:14
urlname: count-the-repetitions
categories:
- leetcode
tags:
- leetcode
- dynamic programming
- 循环节
- hard
---
>[Leetcode 466. Count The Repetitions](https://leetcode.com/problems/count-the-repetitions/)
题目大意是：定义S = [s,n] 表示字符串S由字符串s重复n次构成。 例如["abc", 3] ="abcabcabc"
另一方面，我们定义字符串s1可以从s2得到，如果我们可以通过从s2中移除一些字符得到s1。例如根据定义，“abc”可以从“abdbec”得到，但是“acbbe”则不行。
给定两个非空字符串s1和s2（每个最多100个字符），两个整数 0 ≤ n1 ≤ 10^6 和 1 ≤ n2 ≤ 10^6。现在考虑字符串S1和S2，其中S1=[s1,n1] 并且 S2=[s2,n2]。计算满足[S2,M]可以从S1得到的最大整数M。

<!--more-->
#### 思路
题目意思较为难懂，进过几次阅读后，大致明白了这个意思，也能体会到题目背后的核心是如何可用循环重复字符串进行规律的总结，而不是使用暴力的方法求解，结题思路参考自leetcode disscuss中一个解答，核心是发现规律，利用循环节的概念，将s1 * n1 这种长串中出现的反复规律找出来，减少循环遍历的时间损耗：步骤如下：
- 遍历S1，追踪每次匹配结束，s2中最后一个字符的下标
- 持续遍历，只直到发现s2中的结束坐标重复出现，则：
    - 将S1 分为循环前的部分、循环体、结尾部分
        - 计算循环体中循环节的出现次数，需要的s1个数和匹配的s2个数
        - 编列循环前和循环后的情况，补全可匹配的s2个数


#### 算法实现
approach1 原始的brute force解法
```java
public int getMaxRepetitions1(String s1, int n1, String s2, int n2) {
        // 0->n1次，计算最少的需要多少字符串才能
        int[] dp = new int[n1 + 1];
        for (int i = 1; i <= n1; i++) {
            dp[i] = contSubsequence(s2, build(s1, i));

        }

        return dp[n1] / n2;
    }


    public int contSubsequence(String s, String t) { //判断 t中能包含几个s序列
        int i = 0, j = 0;
        int count = 0;
        while (j < t.length()) {
            if (i == s.length()) {
                count++;
                i = 0;
                continue;
            }
            if (s.charAt(i) == t.charAt(j)) {
                i++;
                j++;
            } else {
                j++;
            }
        }

        if (i == s.length()) {
            count++;
        }
        return count;
    }


    private String build(String s, int n) {
        StringBuilder str = new StringBuilder();
        for (int i = 0; i < n; i++) str.append(s);
        return str.toString();
    }
```

approach2 引用的循环节解法
```java
public int getMaxRepetitions(String s1, int n1, String s2, int n2) {
        int nn1 = s1.length();
        int nn2 = s2.length();

        // records[0][i]标识第i个s1用完s2匹配的位置
        // records[1][i] 标识使用i个s1能匹配的s2个数
        int[][] records = new int[2][n1+1]; 
        Arrays.fill(records[0], -1);
        int index2 = 0, count = 0;
        int cycleStart = -1, cycleEnd = -1, cycleCount = -1;
        for (int i = 1; i <= n1; i++) {
            for (int j = 0; j < nn1; j++) {
                if (s1.charAt(j) == s2.charAt(index2)) {
                    index2++;
                }
                if (index2 == nn2) {
                    index2 = 0;
                    count++;
                }
            }
            for (int k = 1; k < i; k++) {
                if (records[0][k] == index2) {
                    // we find the cycle, record start/end/count and then quit the loop
                    cycleStart = k;
                    cycleEnd = i;
                    cycleCount = count - records[1][k];
                }
            }
            if (cycleStart != -1) {
                break;
            }
            records[0][i] = index2;
            records[1][i] = count;
        }

        if (cycleStart == -1) {
            return count / n2;
        }

        int res = 0;
        // calculate cycle
        int cycleN = (n1 - cycleStart) / (cycleEnd - cycleStart);
        res = cycleN * cycleCount;
        // snitich pre-cycle and post-cycle parts
        res += records[1][n1 - cycleN * (cycleEnd - cycleStart)];

        return res / n2;
    }
```
