---
title: Leetcode[980] Unique Paths III
date: 2020-04-19 21:53:16
urlname: unique-pathIII
categories:
- leetcode
tags:
- leetcode
- backtracking
- hard
- dynamic programming
---
>[Leetcode 980. Unique Paths III](https://leetcode.com/problems/unique-paths-iii/)
这题要求从二维面板中，找到所有不同的从起点到终点的遍历路径，要求每条路径正好包含一次每个0元素，结题基本套路还是利用回溯的方法，不同的是需要考虑走过路径的0的个数，而且不能重复。

<!--more-->

#### 思路
基本算法框架还是回溯，但是回溯的时候增加一个变量，就是障碍格子的数量，当搜索路径中已经包含了 ** 格子总数** - ** 障碍个数** -1 时，并且当前准备搜索的点是终点，说明找到了合法解。dp的解法则是定义了dp[r][c][todo] 则是定义了，在grid[r][c]这个点上，在还有todo个空格灭有搜索情况下，有几条独立的到终点路径，然后利用memorization记录中间结果，减少dfs的次数。

#### 算法实现
这里给出个人蹩脚的实现，暂时还未做优化：
```java
int count = 0;

    public int uniquePathsIII(int[][] grid) {
        if (grid == null || grid.length == 0) return count;
        int row = grid.length;
        int col = grid[0].length;
        int startX = 0;
        int startY = 0;
        int numOfObstacle = 0;

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == 1) {
                    startX = i;
                    startY = j;
                } else if (grid[i][j] == -1) numOfObstacle++;
            }
        }
        boolean[][] visited = new boolean[row][col];
        dfs(grid, row, col, startX, startY, numOfObstacle, visited, new LinkedList<>());
        return count;
    }

    private void dfs(int[][] grid, int row, int col, int i, int j, int numOfObstacle, boolean[][] visited, LinkedList<int[]> path) {
        if (i < 0 || i >= row || j < 0 || j >= col) return;
        if (grid[i][j] == 2 && path.size() == row * col - numOfObstacle - 1) {
            count++;
            for (int[] index : path) {
//                grid[index[0]][index[1]] = -1;
                System.out.print("(" + index[0] + ", " + index[1] + "), ");
            }
            System.out.println();
            return;
        } else if (grid[i][j] == 2) return;
        if (!visited[i][j] && grid[i][j] != -1) {
            visited[i][j] = true;
            System.out.println("(" + i + ", " + j + ")");
            path.add(new int[]{i, j});
            dfs(grid, row, col, i, j + 1, numOfObstacle, visited, path);
            dfs(grid, row, col, i, j - 1, numOfObstacle, visited, path);
            dfs(grid, row, col, i - 1, j, numOfObstacle, visited, path);
            dfs(grid, row, col, i + 1, j, numOfObstacle, visited, path);
            visited[i][j] = false;
            path.removeLast();
        }
    }
```

优化后：
```java
int count = 0;

    public int uniquePathsIII(int[][] grid) {
        if (grid == null || grid.length == 0) return count;
        int row = grid.length;
        int col = grid[0].length;
        int startX = 0;
        int startY = 0;
        int numOfObstacle = 0;

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == 1) {
                    startX = i;
                    startY = j;
                } else if (grid[i][j] == -1) numOfObstacle++;
            }
        }
        dfs(grid, row, col, startX, startY, row * col - 1 - numOfObstacle);
        return count;
    }

    private void dfs(int[][] grid, int row, int col, int i, int j, int todo) {
        if (i < 0 || i >= row || j < 0 || j >= col) return;
        if (grid[i][j] == 2 && todo == 0) {
            count++;
            return;
        } else if (grid[i][j] == 2) return;
        if (grid[i][j] != 3 && grid[i][j] != -1) {
            grid[i][j] = 3;
            todo--;
            dfs(grid, row, col, i, j + 1, todo);
            dfs(grid, row, col, i, j - 1, todo);
            dfs(grid, row, col, i - 1, j, todo);
            dfs(grid, row, col, i + 1, j, todo);
            grid[i][j] = 0;
            todo++;
        }
    }
```
优化后时间超过 100%，空间超过33%