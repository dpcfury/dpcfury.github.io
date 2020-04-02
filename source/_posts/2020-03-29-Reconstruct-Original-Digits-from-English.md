---
title: Leetcode[423] Reconstruct Original Digits from English
date: 2020-03-29 13:49:52
urlname: reconstruct-original-digits-from-english
categories:
- leetcode
tags:
- leetcode
- medium
---

>[423. Reconstruct Original Digits from English](https://leetcode.com/problems/reconstruct-original-digits-from-english/)
题目的意思第一遍读，以为是和编解码同类型的题目，想用回溯去尝试，后来发现给定的一个条件是input长度小于5000，那就说明可能存在重复的数字，回溯走不通。再看看discuss，发现这题主要是在找规律，即分析每个数字英文所包含的字母和其对应关系，再从input中判断特征字母的出现次数，计算每个数字的出现次数。

<!--more-->


如果我们用numbers数组存储出现的数字个数，其中下标表示着对应0-9的数字，那么我们会有下面这个结果：

| 数字 | 数字的个数|
| ------ | ------ | ------ |
| 0 | numbers[0] = ‘z’的个数 |
| 2 | numbers[2] = ‘w’的个数 |
| 4 | numbers[4] = ‘u’的个数 |
| 6 | numbers[6] = ‘x’的个数 |
| 8 | numbers[8] = ‘g’的个数 |

而其他的数字个数，比如5的个数，因为‘f’的数目由four和five的数目组成，而我们已知four的数目为numbers[4]，所以numbers[5] = ‘f’的个数-number[4]。其他的数字同样处理。
最后得到了下面的映射关系：

| 数字 | 数字的个数|
| ------ | ------ | ------ |
| 5 | numbers[5] = ‘f’的个数 - numbers[4] |
| 3 | numbers[3] = ‘h’的个数 - numbers[8] |
| 7 | numbers[7] = ‘s’的个数 - numbers[6] |
| 1 | numbers[1] = ‘o’的个数 - numbers[0] - numbers[2] - numbers[4] |
| 9 | numbers[9] = ‘i’的个数 - numbers[5] - numbers[6] - numbers[8] |


于是乎，代码写成下面这种格式：
```java
public String originalDigits(String s) {
        HashMap<Character, Integer> count = new HashMap<>();
        int[] nums = new int[10];

        for (char c : s.toCharArray()) {
            count.put(c, count.getOrDefault(c, 0) + 1);
        }

        nums[8] = count.getOrDefault('g', 0);
        nums[6] = count.getOrDefault('x', 0);
        nums[4] = count.getOrDefault('u', 0);
        nums[2] = count.getOrDefault('w', 0);
        nums[0] = count.getOrDefault('z', 0);
        nums[5] = count.getOrDefault('f', 0) - nums[4];
        nums[3] = count.getOrDefault('h', 0) - nums[8];
        nums[7] = count.getOrDefault('s', 0) - nums[6];
        nums[1] = count.getOrDefault('o', 0) - nums[0] - nums[2] - nums[4];
        nums[9] = count.getOrDefault('i', 0) - nums[5] - nums[6] - nums[8];

        StringBuilder str = new StringBuilder();
        for (int i = 0; i < 10; i++) {
            while (nums[i] > 0) {
                str.append((char) ('0' + i));
                nums[i]--;
            }
        }

        return str.toString();

    }
```

看到discuss中有给出的拓扑排序解法，但是这题给人的感觉更倾向于找规律，因此不再讨论另外的解法。

