---
title: Leetcode[1111] Maximum Nesting Depth of Two Valid Parentheses Strings
date: 2020-04-01 23:18:21
urlname: Maximum-Nesting-Depth-of-Two-Valid-Parentheses-Strings
categories:
- leetcode
tags:
- leetcode
- trick
- medium
---
>[Leetcode 1111. Maximum Nesting Depth of Two Valid Parentheses Strings](https://leetcode.com/problems/maximum-nesting-depth-of-two-valid-parentheses-strings/)
题目的描述很长，但是表达的意思我理解就是，怎么均分一下A B两个子括号，这边借用的是交替均分‘（’的方法，这题有点偏trick，👎的比例相对较高，不做过多细节描述。

<!--more-->

#### 方法一：用栈进行括号匹配
思路及算法

要求划分出使得最大嵌套深度最小的分组，我们首先得知道如何计算嵌套深度。我们可以通过栈实现括号匹配来计算：

维护一个栈 s，从左至右遍历括号字符串中的每一个字符：

- 如果当前字符是 (，就把 ( 压入栈中，此时这个 ( 的嵌套深度为栈的高度；

- 如果当前字符是 )，此时这个 ) 的嵌套深度为栈的高度，随后再从栈中弹出一个 (。

下面给出了括号序列 (()(())()) 在每一个字符处的嵌套深度：

>括号序列   ( ( ) ( ( ) ) ( ) )
下标编号   0 1 2 3 4 5 6 7 8 9
嵌套深度   1 2 2 2 3 3 2 2 2 1 

知道如何计算嵌套深度，问题就很简单了：只要在遍历过程中，我们保证栈内一半的括号属于序列 A，一半的括号属于序列 B，那么就能保证拆分后最大的嵌套深度最小，是当前最大嵌套深度的一半。要实现这样的对半分配，我们只需要把奇数层的 ( 分配给 A，偶数层的 ( 分配给 B 即可。对于上面的例子，我们将嵌套深度为 1 和 3 的所有括号 (()) 分配给 A，嵌套深度为 2 的所有括号 ()()() 分配给 B。


```java
public int[] maxDepthAfterSplit(String seq) {
        int[] res = new int[seq.length()];
        for (int i = 0, depth = 0; i < seq.length(); i++) {
            if (seq.charAt(i) == '(') {
                res[i] = depth++ & 1;
            } else {
                res[i] = --depth & 1;
            }
        }
        return res;
    }
```


#### 方法二：找规律
思路及算法

我们还是使用上面的例子 (()(())())，但这里我们把 ( 和 ) 的嵌套深度分成两行：

>括号序列   ( ( ) ( ( ) ) ( ) )
下标编号   0 1 2 3 4 5 6 7 8 9
嵌套深度   1 2 - 2 3 - - 2 - -
嵌套深度   - - 2 - - 3 2 - 2 1 
有没有发现什么规律？

- 左括号 ( 的下标编号与嵌套深度的奇偶性相反，也就是说：

    - 下标编号为奇数的 (，其嵌套深度为偶数，分配给 B；

    - 下标编号为偶数的 (，其嵌套深度为奇数，分配给 A。

    - 右括号 ) 的下标编号与嵌套深度的奇偶性相同，也就是说：

- 下标编号为奇数的 )，其嵌套深度为奇数，分配给 A；

    - 下标编号为偶数的 )，其嵌套深度为偶数，分配给 B。

    - 这样以来，我们只需要根据每个位置是哪一种括号以及该位置的下标编号，就能确定将对应的对应的括号分到哪个组了。
