---
title: leetcode[836] Rectangle Overlap
date: 2020-03-18 21:48:28
urlname: leetcode-rectangl-overlap
categories:
- leetcode
tags:
- leetcode
- array
- easy
---

>[836. Rectangle](https://leetcode.com/problems/rectangle-overlap/)
给出两个矩形的左下角坐标，以及右上角坐标，判断两个矩形是否重叠，只要从横坐标和纵坐标构成的线段判断是否重叠，再做逻辑与运算，即能判断长方形是否重叠，只要整理好判断条件即可

<!--more-->

```java
public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
        int x1 = rec1[0];
        int y1 = rec1[1];
        int x2 = rec1[2];
        int y2 = rec1[3];

        int x11 = rec2[0];
        int y11 = rec2[1];
        int x22 = rec2[2];
        int y22 = rec2[3];

        boolean xOverlap = false;
        boolean yOverlap = false;
        xOverlap = (x11 >= x1 && x11 < x2) || (x22 > x1 && x22 <= x2) || (x11 <= x1 && x22 >= x2);
        yOverlap = (y11 >= y1 && y11 < y2) || (y22 > y1 && y22 <= y2) || (y11 <= y1 && y22 >= y2);

        return xOverlap && yOverlap;
    }
```

其实在这种判断条件比较多的情况下，可以看看是否可以用取反进行优化，比如，用判断x轴和y轴不重叠的条件，再取非：

```java
public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
        int x1 = rec1[0];
        int y1 = rec1[1];
        int x2 = rec1[2];
        int y2 = rec1[3];

        int x11 = rec2[0];
        int y11 = rec2[1];
        int x22 = rec2[2];
        int y22 = rec2[3];

        boolean xOverlap = false;
        boolean yOverlap = false;
        xOverlap = !(x22 <= x1 || x11 >= x2);
        yOverlap = !(y22 <= y1 || y11 >= y2);

        return xOverlap && yOverlap;
    }
```
