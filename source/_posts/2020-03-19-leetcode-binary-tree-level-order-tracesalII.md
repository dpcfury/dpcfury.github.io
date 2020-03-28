---
title: leetcode[107]Binary Tree Level Order Travesal II
date: 2020-03-19 21:03:44
urlname: leetcode-binary-tree-level-order-travesalII
categories:
- leetcode
tags:
- leetcode
- binary tree
- easy

---

>[107. Binary Tree Level Order Travesal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)
题目意思就是二叉树的层次遍历换了种说法，从上往下的层次遍历，感觉上变成了从下往上的层次遍历，其实按照从上往下的顺序正常层次遍历，然后对每一层获取的结果列表进行reverse即可。

<!--more-->
```java
public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> results = new ArrayList<>();
        if (root == null)
            return results;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            List<TreeNode> layer = new ArrayList<>();
            List<Integer> temp = new ArrayList<>();
            while (!queue.isEmpty()) {
                TreeNode node = queue.poll();
                temp.add(node.val);
                layer.add(node);
            }
            for (TreeNode node : layer) {
                if (node.left != null)
                    queue.offer(node.left);
                if (node.right != null)
                    queue.offer(node.right);
            }
            results.add(temp);
        }
        Collections.reverse(results);
        return results;
    }
```
