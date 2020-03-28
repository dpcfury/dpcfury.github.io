---
title: Leetcode[108] Convert Sorted Array to Binary Search Tree
date: 2020-03-22 13:36:55
urlname: convert-sorted-array-to-binary-search-tree
categorites:
- leetcode
tags:
- leetcode
- BST
- easy
---

>[Leetcode 108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)
题目意思：通过一个有序的数组，构建一个高度平衡的二叉搜索树。具体解法比较直观的就是，通过二分法，将数组从中间一份为二，前半部分为左子树，右半部分为右子树，中间元素为树的根，这样递归去构建左右子树技能得到最终的解。

<!-- more -->

```java
public TreeNode sortedArrayToBST(int[] nums) {
        if (nums == null || nums.length == 0) return null;

        return sortedArrayToBST(nums, 0, nums.length - 1);
    }

    private TreeNode sortedArrayToBST(int[] nums, int i, int j) {
        if (j < i) return null;
        int m = (i + j) / 2;
        TreeNode root = new TreeNode(nums[m]);
        root.left = sortedArrayToBST(nums, i, m - 1);
        root.right = sortedArrayToBST(nums, m + 1, j);
        return root;
    }
```