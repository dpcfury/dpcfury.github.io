---
title: Leetcode[105] Construct Binary Tree from Preorder and Inorder Traversal
date: 2020-04-11 20:50:44
urlname: construct-binary-tree-from-preorder-and-inorder-traversal
categories:
- leetcode
tags:
- leetcode
- binary tree
- medium
---
>[Leetcode 105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
题目要求：根据一棵树的前序遍历与中序遍历构造二叉树。ps：树中没有重复的元素，属于很基本的二叉树问题。

<!--more-->
#### 思路
给定二叉树的前序遍历数组和中序遍历数组，可以明确指导前序数组的第一个元素，肯定是当前这颗树的根,随后在中序数组中找到这个根元素，将中序数组就可以切分为左子树和右子树，然后递归去求构造左子树和右子树即可。

#### 算法实现
```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || inorder == null || preorder.length == 0 || inorder.length == 0)
            return null;
        else
            return buildTree(
                    preorder, 0, preorder.length - 1,
                    inorder, 0, inorder.length - 1);
    }

    private TreeNode buildTree(int[] preorder, int start1, int end1, int[] inorder, int start2, int end2) {
        if (start1 > end1) return null;
        TreeNode root = new TreeNode(preorder[start1]);
        int index = start2;
        while (inorder[index] != preorder[start1]) index++;
        root.left = buildTree(preorder, start1 + 1, start1 + index - start2, inorder, start2, index - 1);
        root.right = buildTree(preorder, start1 + index - start2 + 1, end1, inorder, index + 1, end2);
        return root;
    }
```