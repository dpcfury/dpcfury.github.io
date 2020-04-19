---
title: Leetcode[1339] Maximum Product of Splitted Binary Tree
date: 2020-04-19 16:37:45
urlname: maximum-product-of-splitted-binary-tree
tags:
---
>[Leetcode 1339. Maximum Product of Splitted Binary Tree](https://leetcode.com/problems/maximum-product-of-splitted-binary-tree/)
题目给定一课二叉树，选择一个边进行切分，整棵树变成两部分，要求分开的两个字数乘积最大。

<!--more-->

#### 思路
无论是切分哪条边，整颗树肯定是分为了一颗字子树，以及包含这个边上连父节点的子树，利用dfs的思路，遍历每个节点，都可以计算其子树的和于包含自己的子树和乘积，并更新全局最大。核心是先通过一次遍历求得整个树的整体的综合，减去子树的和，就能得到包含父节点那棵树的和，此外，因为树的规模还不小，乘积大小也较大，考虑double类型，并且最好将子树和进行memorization。

#### 算法实现
```java
double max = 0, totalSum = 0;
    HashMap<TreeNode, Double> memo = new HashMap<>();

    public int maxProduct(TreeNode root) {
        if (root == null) return 0;
        totalSum = sum(root);
        memo.put(null, 0.0);
        dfs(root);
        return (int) (max % 1000000007);
    }

    private void dfs(TreeNode root) {
        if (root == null) return;
        double leftSum = memo.get(root.left);
        double rightSum = memo.get(root.right);
        if ((totalSum - leftSum) * leftSum > (totalSum - rightSum) * rightSum) {
            max = Math.max(max, (totalSum - leftSum) * leftSum);
        } else {
            max = Math.max(max, (totalSum - rightSum) * rightSum);

        }
        dfs(root.left);
        dfs(root.right);
    }

    private double sum(TreeNode root) {
        if (root == null) return 0;
        double leftSum = sum(root.left);
        double rightSum = sum(root.right);
        double sum = leftSum + rightSum + root.val;
        memo.put(root, sum);
        return sum;
    }

```