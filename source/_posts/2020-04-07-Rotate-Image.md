---
title: Leetcode[48] Rotate Image
date: 2020-04-07 23:50:07
urlname: rotate-image
categories:
- leetcode
tags:
- leetcode
- matrix
- medium
---
>[Leetcode 48. Rotate Image](https://leetcode.com/problems/rotate-image/)
题目要求在原地对二维矩阵中的元素进行顺时针90度的旋转，而且不能借助额外的辅助空间（比如创建一个同等大小的二维数组）。这题通过纸上多画图尝试尝试，就能找到规律，我们所能做的操作只有行和列操作，以及围绕对角线的操作，试着试着这题的答案就可以出来了。

<!--more-->

#### 思路
对二维数组的行进行围绕中间轴的上线交换，再围绕对角线对元素进行交换，就能得到最终的结果。

#### 算法实现
```java
public void rotate(int[][] matrix) {
        if (matrix == null) return;
        int n = matrix.length;

        for (int i = 0; i < n / 2; i++) {
            int[] temp = matrix[i];
            matrix[i] = matrix[n - i - 1];
            matrix[n - i - 1] = temp;
        }

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

    } 
```