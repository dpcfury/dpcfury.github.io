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

除了DFS+BFS，这题还可以用UnionFind实现，关于并查集的定义和理解，参考https://juejin.im/post/5d8e66fdf265da5b633cc8db ，后续补充

```java
class UnionFind {
        int count;
        int[] parent;
        int[] rank; // 表示以 i 为根节点的集合的层数，即树的高度

        public UnionFind(char[][] grid) {
            count = 0;
            int m = grid.length;
            int n = grid[0].length;
            parent = new int[m * n];
            rank = new int[m * n];
            for (int i = 0; i < m; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (grid[i][j] == '1') {
                        parent[i * n + j] = i * n + j;
                        ++count;
                    }
                    rank[i * n + j] = 0;
                }
            }
        }

        public int find(int i) {
            if (parent[i] != i) parent[i] = find(parent[i]); // 路径压缩
            return parent[i];
        }

        public void union(int x, int y) {
            int rootx = find(x); //找出x元素位于的集合的根元素
            int rooty = find(y);
            if (rootx != rooty) {
                if (rank[rootx] > rank[rooty]) { //rootx元素所在的集合的层数大于rooty元素所在的集合的层数
                    parent[rooty] = rootx; //rooty集合的根节点父亲指针指向rootx集合的根节点
                } else if (rank[rootx] < rank[rooty]) {
                    parent[rootx] = rooty;
                } else {
                    parent[rooty] = rootx; // 层数相同，随意
                    rank[rootx] += 1;
                }
                --count;
            }
        }

        public int getCount() {
            return count;
        }
    }

    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int nr = grid.length;
        int nc = grid[0].length;
        int num_islands = 0;
        UnionFind uf = new UnionFind(grid);
        for (int r = 0; r < nr; ++r) {
            for (int c = 0; c < nc; ++c) {
                if (grid[r][c] == '1') {
                    grid[r][c] = '0';
                    if (r - 1 >= 0 && grid[r-1][c] == '1') {
                        uf.union(r * nc + c, (r-1) * nc + c);
                    }
                    if (r + 1 < nr && grid[r+1][c] == '1') {
                        uf.union(r * nc + c, (r+1) * nc + c);
                    }
                    if (c - 1 >= 0 && grid[r][c-1] == '1') {
                        uf.union(r * nc + c, r * nc + c - 1);
                    }
                    if (c + 1 < nc && grid[r][c+1] == '1') {
                        uf.union(r * nc + c, r * nc + c + 1);
                    }
                }
            }
        }

        return uf.getCount();
    }

```