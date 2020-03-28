---
title: Leetcode[200] Number of Islands
date: 2020-03-22 20:39:04
urlname: leetcode-number-of-islands
categories:
- leetcode
tags:
- leetcode
- backtracking
- medium
---

>[Leetcode 200. Number of Islands](https://leetcode.com/problems/number-of-islands/)
题目意思：一个二维字符数组，其中‘1’代表的是陆地，上下左右相邻的陆地构成一个岛，求解这幅地图中的独立小岛个数。这题和回溯的相似度很高，但是比回溯要更为简单，直观的解法就是，找到小岛的一块陆地，从上下左右分别去拓展这个小岛，通过一个标记二维数组标记当前的拓展情况，如果已经拓展则进行忽略，当二维字符数组的每个元素都遍历完成，求得小岛数就是最终的答案。

<!-- more-->

```java
public int numIslands(char[][] grid) {
        int num = 0;
        if (grid == null || grid.length==0) return num;
        int row = grid.length;
        int col = grid[0].length;
        boolean[][] visited = new boolean[row][col];
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == '1' && !visited[i][j]) {
                    traversal(grid, row, col, i, j, visited);
                    num++;
                }
            }
        }
        return num;
    }

    private void traversal(char[][] grid, int row, int col, int i, int j, boolean[][] visited) {
        if (i < 0 || i >= row || j < 0 || j >= col || grid[i][j] != '1') {
            return;
        } else {
            if (!visited[i][j]) {
                visited[i][j] = true;
                traversal(grid, row, col, i - 1, j, visited);
                traversal(grid, row, col, i + 1, j, visited);
                traversal(grid, row, col, i, j - 1, visited);
                traversal(grid, row, col, i, j + 1, visited);
            }

        }
    }
```
