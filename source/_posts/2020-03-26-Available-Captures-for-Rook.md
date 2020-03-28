---
title: Leetcode[999] Available Captures for Rook
date: 2020-03-26 22:37:54
urlname: available-captures-for-root
categories:
- leetcode
tags:
- leetcode
- easy
---

>[999. Available Captures for Rook](https://leetcode.com/problems/available-captures-for-rook/) 题目要求8x8的棋局上，Rook上下左右移动可以补货的黑色卒数量，只要遍历上下左右位置即可。实际题目并没有什么含量，奈何把黑色卒缩写理解成‘B’,稍微折腾了一下，并给题目点了个👎。

<!-- more -->

```java
public int numRookCaptures(char[][] board) {
        int row = board.length;
        int col = board[0].length;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (board[i][j] == 'R') {
                    return canCapture(board, row, col, i, j);
                }
            }
        }
        return 0;
    }

    private int canCapture(char[][] board, int row, int col, int i, int j) {
        int num = 0;
        int up = i - 1;
        while (up >= 0) {
            char c = board[up][j];
            if (c == '.') {
                up--;
            } else {
                if (c == 'p') {
                    num++;
                }
                break;
            }
        }
        int right = j + 1;
        while (right < col) {
            char c = board[i][right];
            if (c == '.') {
                right++;
            } else {
                if (c == 'p') {
                    num++;
                }
                break;
            }
        }

        int left = j - 1;
        while (left >= 0) {
            char c = board[i][left];
            if (c == '.') {
                left--;
            } else {
                if (c == 'p') {
                    num++;
                }
                break;
            }
        }

        int down = i + 1;
        while (down < row) {
            char c = board[down][j];
            if (c == '.') {
                down++;
            } else {
                if (c == 'p') {
                    num++;
                }
                break;
            }
        }
        return num;
    }
```