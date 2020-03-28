---
title: leetcode[112] path-sum
date: 2020-03-17 21:44:21
urlname: leetcode-path-sum
categories:
- leetcode
tags:
- easy
- tree
---
>[112. Path Sum](https://leetcode.com/problems/path-sum/)
 这题属于Tree类最基本的题目，只要画好图，考虑好递归和边界即可
<!--more-->

```java
public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) {
            return false;
        }
        if (root.left == null && root.right == null) {
            return sum - root.val == 0;
        }

        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
    }
```