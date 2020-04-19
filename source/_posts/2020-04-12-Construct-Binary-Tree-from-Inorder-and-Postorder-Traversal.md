---
title: Leetcode[106] Construct Binary Tree from Inorder and Postorder Traversal
date: 2020-04-12 12:08:34
urlname: construct-binary-tree-from-inorder-and-postorder-traversal
categories:
- leetcode
tags:
- leetcode
- binary tree
- medium
---
[Leetcode 106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
题目要求：根据一棵树的中序遍历与后序遍历构造二叉树。ps：树中没有重复的元素，属于很基本的二叉树问题。

<!--more-->
#### 思路
给定二叉树的后序遍历数组和中序遍历数组，可以明确指导后序数组的第一个元素，肯定是当前这颗树的根,随后在中序数组中找到这个根元素，将中序数组就可以切分为左子树和右子树，然后递归去求构造左子树和右子树即可。

#### 算法实现
```java
public TreeNode buildTree(int[] inorder, int[] postorder) {
        if (inorder == null || postorder == null || inorder.length == 0 || postorder.length == 0)
            return null;
        else
            return buildTree(
                    inorder, 0, inorder.length - 1,
                    postorder, 0, postorder.length - 1);
    }

    private TreeNode buildTree(int[] inorder, int start1, int end1, int[] postorder, int start2, int end2) {
        if (start1 > end1) return null;
        TreeNode root = new TreeNode(postorder[end2]);
        int index = start1;
        while (inorder[index] != postorder[end2]) index++;
        root.left = buildTree(inorder, start1, index - 1, postorder, start2, start2 + index - start1 - 1);
        root.right = buildTree(inorder, index + 1, end1, postorder, start2 + index - start1, end2 - 1);
        return root;
    }
```