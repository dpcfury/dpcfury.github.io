---
title: Leetcode[79] Word Search
date: 2020-04-10 21:13:12
urlname: word-search
categories:
- leetcode
tags:
- leetcode
- backtracking
- medium
---
>[Leetcode 79. Word Search](https://leetcode.com/problems/word-search/)
给定一个二维网格和一个单词，找出该单词是否存在于网格中。单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

<!--more-->

#### 思路
利用回溯的方法，递归进行搜索求解，从一个格子（单个字符已经匹配）出发，查看剩余字符是否在上下左右的搜索路径中能够满足。

#### approach1 原始写法
```java
public boolean exist(char[][] board, String word) {
        if (board == null || board.length == 0 || word == null) return false;
        int row = board.length;
        int col = board[0].length;
        if (word.length() == 0) return true;
        int[][] directions = {{0, 1}, {0, -1}, {-1, 0}, {1, 0}};
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (board[i][j] == word.charAt(0)) {
                    boolean[][] visited = new boolean[row][col];
                    if (traversal(board, word, 0, i, j, directions, visited)) return true;
                }
            }
        }
        return false;
    }

    private boolean traversal(char[][] board, String word,
                              int index, int i, int j, int[][] directions, boolean[][] visited) {
        if (index == word.length()) {
            return true;
        } else if (index > word.length()) return false;
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) return false;
        if (visited[i][j] || board[i][j] != word.charAt(index)) return false;
        visited[i][j] = true;
        for (int[] dir : directions) {
            int curX = i + dir[0];
            int curY = j + dir[1];
            if (traversal(board, word, index + 1, curX, curY, directions, visited)) return true;
        }
        visited[i][j] = false;
        return false;
    }
```

#### approach2 优化写法
```java
public boolean exist(char[][] board, String word) {
        char[] w = word.toCharArray();
        for (int y=0; y<board.length; y++) {
            for (int x=0; x<board[y].length; x++) {
                if (exist(board, y, x, w, 0)) return true;
            }
        }
        return false;
    }

    private boolean exist(char[][] board, int y, int x, char[] word, int i) {
        if (i == word.length) return true;
        if (y<0 || x<0 || y == board.length || x == board[y].length) return false;
        if (board[y][x] != word[i]) return false;
        board[y][x] ^= 256;
        boolean exist = exist(board, y, x+1, word, i+1)
                || exist(board, y, x-1, word, i+1)
                || exist(board, y+1, x, word, i+1)
                || exist(board, y-1, x, word, i+1);
        board[y][x] ^= 256;
        return exist;
    }
```