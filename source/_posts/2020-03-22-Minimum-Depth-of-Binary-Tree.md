---
title: Leetcode[111] Minimum Depth of Binary Tree
date: 2020-03-22 21:46:45
urlname: leetcode-minimum-depth-of-binary-tree
categories:
- leetcode
tags:
- leetcode
- easy
- binary tree
---

>[Leetcode 111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
题目要求计算二叉树从根节点开始，到叶子阶段的最短距离。解法同样是左右子树递归进行计算，但是要注意为空情况下，子树的最小深度应该设置为无穷大，而不是0。

<!-- more-->

```java
public int minDepth(TreeNode root) {
        if(root == null ) return 0;
        if (root.left == null && root.right == null) return 1;
        int minLeft = root.left != null ? minDepth(root.left) : Integer.MAX_VALUE;
        int minRight = root.right != null ? minDepth(root.right) : Integer.MAX_VALUE;
        return Math.min(minLeft, minRight) + 1;
    }
```
