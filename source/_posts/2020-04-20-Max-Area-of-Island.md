---
title: Leetcode[695] Max Area of Island
date: 2020-04-20 20:49:19
urlname: max-area-of-island
categories:
- leetcode
tags:
- leetcode
- dfs
- medium
---
>[Leetcode 695. Max Area of Island]()
题目和 **Leetcode[200] Num of Islands**属于同一类题目，只是要求得内容为独立小岛最大的面积。将题目定义为dfs类型比backtracking类型更为合适。

<!--more-->

#### 思路
- 遍历面板，只要遇到1元素，则开始dfs
- dfs过程：
    - 如果坐标超标或元素不为1，返回0
    - 返回 1+ 四个方向dfs结果
- dfs完成更新全局Max值

#### 算法实现
```java
int max = Integer.MIN_VALUE;

    public int maxAreaOfIsland(int[][] grid) {
        if (grid == null || grid[0].length == 0) return 0;
        int row = grid.length;
        int col = grid[0].length;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == 1)
                    max = Math.max(max, dfs(grid, row, col, i, j));
            }
        }
        return max;
    }

    private int dfs(int[][] grid, int row, int col, int i, int j) {
        if (i < 0 || i >= row || j < 0 || j >= col || grid[i][j] != 1) return 0;
        grid[i][j] = 0;
        int res = 1;
        res += dfs(grid, row, col, i, j + 1);
        res += dfs(grid, row, col, i, j - 1);
        res += dfs(grid, row, col, i - 1, j);
        res += dfs(grid, row, col, i + 1, j);
        return res;
    }
```