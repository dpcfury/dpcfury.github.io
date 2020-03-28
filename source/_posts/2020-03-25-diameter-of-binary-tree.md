---
title: Leetcode[543] Diameter of Binary Tree
date: 2020-03-25 19:55:19
urlname: leetcode-diameter-of-binary-tree
categories:
- leetcode
tags:
- leetcode
- binary tree
- easy
---

>[543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
题目要求二叉树的最大直径，所谓直径就是这颗树的任意两个叶子结点连接，所能经过的最多节点数。分析这个路径的可能构成情况，可能是存在于左子树、右子树，或者存在于左右子树+根节点。所以对于当前任意节点，这颗树的最大直径，可以分为包含这个节点的路径情况：即左右子树高度+1，不包含的情况，即左子树的最大直径、或右子树的最大直径。

<!-- more-->

```java
public int diameterOfBinaryTree(TreeNode root) {
        if (root == null)
            return 0;
        int max = getDepth(root.left) + getDepth(root.right);
        int leftMax = diameterOfBinaryTree(root.left);
        int rightMax = diameterOfBinaryTree(root.right);
        if (leftMax > max)
            max = leftMax;
        if (rightMax > max)
            max = rightMax;
        return max;
    }

    public int getDepth(TreeNode node) {
        if (node == null)
            return 0;
        else
            return Math.max(getDepth(node.left), getDepth(node.right)) + 1;
    }
```