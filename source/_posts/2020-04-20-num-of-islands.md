---
title: Leetcode[200] Num of Islands
date: 2020-04-20 00:05:34
urlname: num-of-islands
categories:
- leetcode
tags:
- leetcode
- backtracking
- dfs
- medium
---
>[Leetcode200 Num of Islands](https://leetcode.com/problems/number-of-islands/)
题目定义一个二维面板表示的地图，其中‘0’代表海洋，‘1’表示陆地，上下左右相邻的陆地构成一个小岛，问这个地图中独立的小岛数量，很经典的回溯dfs类题目。

<!--more-->

#### 思路
- 遍历整个面板，如果面板元素grid[i][j]未访问，且是陆地，则可以进行dfs遍历，标记改陆地所相邻的所有陆地，遍历完成，独立小岛数量+1
- 可以用boolean数组标记是否访问，也可以将陆地改变为海洋（考虑题目是否允许改动面板）

#### 算法实现
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