---
title: Leetcode[1162] As Far from Land as Possible
date: 2020-03-29 21:44:36
urlname: leetcode-as-far-from-land-as-possible
categoried:
- leetcode
tags:
- leetcode
- BFS
- medium
---

>[Leetcode 1162. As Far from Land as Possible](https://leetcode.com/problems/as-far-from-land-as-possible/)
题目的意思需要多读几次，最终的目的就是求离陆地最远的海洋距离。这类题目可以属于一道简单的bfs题目。leetcode中海洋陆地问题的题目很多，大多都是bfs或是dfs题目。这里再简单啰嗦一下bfs的思路。首先我们选取一些特征一致的点作为起始目标，然后从这些目标开始向四周扩散一步，四周的点只要在数组边界之内并且其特征与初始特征不一致，我们就将其变为与起始点同一特征（染色）。同时将这些新染色的点作为起始再向四周扩散一步，执行相同操作，直到全部同化完成。

<!-- more -->

#### 思路
  1. 先找出所有陆地的坐标，作为bfs的起始条件。
  2. 通过所有的陆地坐标向外bfs。直到找到最后一块海洋为止。这期间所用到的步数即是答案，也即从某个初始点扩散到这块海洋，所需要的最长距离。

```java
static class Point {
        int x;
        int y;
        int dist;

        Point(int x, int y, int dist) {
            this.x = x;
            this.y = y;
            this.dist = dist;
        }
    }

    public int maxDistance(int[][] grid) {
        int n = grid.length;
        int maxDistance = -1;
        Queue<Point> queue = new LinkedList<>();
        int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        boolean[][] visited = new boolean[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    queue.offer(new Point(i, j, 0));
                }
            }
        }
        // 如果没有陆地或者全是陆地，直接返回-1
        if (queue.size() == 0 || queue.size() == n * n) return maxDistance;
        while (!queue.isEmpty()) {
            Point p = queue.poll();
            if (p.x < 0 || p.x > grid.length - 1 || p.y < 0 || p.y > grid[0].length - 1)
                continue;
            if (!visited[p.x][p.y]) {
                visited[p.x][p.y] = true;
                maxDistance = Math.max(maxDistance, p.dist);
                for (int[] dir : directions) {
                    int curX = p.x + dir[0];
                    int curY = p.y + dir[1];
                    queue.offer(new Point(curX, curY, p.dist+ 1));
                }
            }
        }
        return maxDistance;
    }


```

