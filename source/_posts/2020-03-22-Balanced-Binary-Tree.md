---
title: Leetcode[110] Balanced Binary Tree
date: 2020-03-22 21:24:11
urlname: leetcode-balanced-binary-tree
categories:
- leetcode
tags:
- leetcode
- binary tree
- easy
---

>[Leetcode 110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)
题目给出一个二叉树，需要判断这颗树是不是平衡二叉树，即对树的每个节点而言，其左右子树高度差不能超过1。第一感觉就是使用递归，从当前节点判断，除了要左右两颗子树都是平衡二叉树之外，还需要一个条件，即左右子树的高度不能像差超过1，因此需要计算树的高度。

<!-- more -->

```java
public boolean isBalanced(TreeNode root) {
        if (root == null) return true;
        else if (isBalanced(root.left) && isBalanced(root.right))
            return Math.abs(heightOfTree(root.left) - heightOfTree(root.right)) <= 1;
        else
            return false;

    }

    private int heightOfTree(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int ldep = heightOfTree(root.left);
        int rdep = heightOfTree(root.right);
        if (ldep > rdep) {
            return ldep + 1;
        } else {
            return rdep + 1;
        }

    }
```
