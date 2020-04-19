---
title: Leetcode[56]. Merge Intervals
date: 2020-04-16 21:17:19
urlname: merge-nitervals
categories:
- leetcode
tags:
- leetcode
- medium
---
>[Leetcode 56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)
区间合并的题目，题目意思很容易明白，印象中这道题目在校招笔试就出现过，很中规中矩的题目，思路大致就是先对区间按起点排序，再按终点排序，最后遍历和合并区间即可。

<!--more-->
#### 思路
- 区间排序
- 区间遍历，如果当前区间起点小于等于上一个区间的终点，则目前扩展到的最大范围是max（maxEnd， 当前节点的end）

#### 算法实现
当前写法
```java
static class Interval {
        int start;
        int end;

        public Interval(int start, int end) {
            this.start = start;
            this.end = end;
        }

        @Override
        public String toString() {
            return "Interval{" +
                    "start=" + start +
                    ", end=" + end +
                    '}';
        }
    }

    public int[][] merge(int[][] intervals) {
        if (intervals == null || intervals.length == 0) return new int[][]{};
        List<Interval> temp = new ArrayList<>();
        for (int i = 0; i < intervals.length; i++) {
            temp.add(new Interval(intervals[i][0], intervals[i][1]));
        }

        temp.sort(new Comparator<Interval>() {
            @Override
            public int compare(Interval o1, Interval o2) {
                if (o1.start < o2.start) return -1;
                else if (o1.start > o2.start) return 1;
                else {
                    return o1.end - o2.end;
                }
            }
        });

        List<int[]> res = new ArrayList<>();
        int start = temp.get(0).start;
        int end = temp.get(0).end;
        for (Interval interval : temp) {
            if (interval.start <= end) {
                end = Math.max(end, interval.end);
            } else {
                res.add(new int[]{start, end});
                start = interval.start;
                end = interval.end;
            }
        }
        res.add(new int[]{start, end});
        int[][] finalRes = new int[res.size()][2];
        for (int i = 0; i < res.size(); i++) finalRes[i] = res.get(i);
        return finalRes;
    }
```

两年前给出的解法：
```java
public List<Interval> merge(List<Interval> intervals) {
        if (intervals.size() <= 1)
            return intervals;

        // Sort by ascending starting point using an anonymous Comparator
        intervals.sort((i1, i2) -> Integer.compare(i1.start, i2.start));

        List<Interval> result = new LinkedList<Interval>();
        int start = intervals.get(0).start;
        int end = intervals.get(0).end;

        for (Interval interval : intervals) {
            if (interval.start <= end) // Overlapping intervals, move the end if needed
                end = Math.max(end, interval.end);
            else {                     // Disjoint intervals, add the previous one and reset bounds
                result.add(new Interval(start, end));
                start = interval.start;
                end = interval.end;
            }
        }

        // Add the last interval
        result.add(new Interval(start, end));
        return result;
    }
```