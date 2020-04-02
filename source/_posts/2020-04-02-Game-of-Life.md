---
title: Leetcode[289] Game of LifeGame of Life
date: 2020-04-02 22:44:34
urlname: leetcode-game-of-life
categories:
- leetcode
tags:
- leetcode
- medium
- bit
---

>[Leetcode 289. Game of Life](https://leetcode.com/problems/game-of-life/)
题目给定一个二维面板，每个格子中的细胞存活由0，1代表，给定一定的繁衍规律，希望在面板上直接更新细胞的下一次存活状态。题目特别强调的是in-place修改，以及边界控制。

<!--more -->

#### 思路

由于不能使用额外的存储空间标记细胞的上一轮状态，所以想到了，有没有社么办法，先在遍历过程中，将值做一个转换，在计算每个细胞的下一个状态时，同时还能保持记录上一轮状态。此时，题目的重点就是：如何提供一种编码方式，将细胞的更新值和上一轮状态保留，后续再进行一次遍历，即获取最终的值。

分析值变化情况：
- 0 -> 1 利用101标记，上一轮状态为0，更新状态为1
- 1 -> 0 利用10 标记，上一轮状态为1，更新状态为0
- 0 -> 0 不变
- 1 -> 1 不变

再加上八个方向的边界控制，题目的解法如下：
```java
 public void gameOfLife(int[][] board) {
        if (board == null || board.length == 0) return;
        int row = board.length;
        int col = board[0].length;
        int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}, {-1, -1}, {-1, 1}, {1, -1}, {1, 1}};

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                int numOfOne = 0;
                int numOfZero = 0;
                for (int[] dir : directions) {
                    int curX = i + dir[0];
                    int curY = j + dir[1];
                    if (inArea(curX, curY, row, col)) {
                        int value = board[curX][curY];
                        if (value == 1 || value == 10) numOfOne++;
                        else numOfZero++;
                    }
                }

                if (board[i][j] == 0 && numOfOne == 3) board[i][j] = 101; // 0->1
                if (board[i][j] == 1 && (numOfOne < 2 || numOfOne > 3)) board[i][j] = 10; // 1->0
            }
        }

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                int value = board[i][j];
                if (value == 101) board[i][j] = 1;
                if (value == 10) board[i][j] = 0;
            }

        }
    }

    private boolean inArea(int x, int y, int row, int col) {
        if (x < 0 || x >= row) return false;
        if (y < 0 || y >= col) return false;
        return true;
    }
```
