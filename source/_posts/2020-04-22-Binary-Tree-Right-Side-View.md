---
title: Leetcode [199]  Binary Tree Right Side View
date: 2020-04-22 21:40:14
urlname: binary-tree-right-side-view
categories:
- leetcode
tags:
- leetcode 
- binary tree
- dfs
- bfs
---

>[Leetcode 199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
求从右侧看一颗二叉树，所能看到的元素列表: 例如下面的二叉树，对应输出为[1, 3, 5]
    
         1            <---
       /   \
      2     3         <---
       \     \
        5     4       <---

<!--more-->
#### 思路
- DFS，先遍历右子树
- BDF，每一层的最后一个元素

#### 算法实现
```java
public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;
        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            res.add(queue.getLast().val);
            List<TreeNode> temp = new ArrayList<>();
            while (!queue.isEmpty()) {
                TreeNode node = queue.poll();
                if (node.left != null) temp.add(node.left);
                if (node.right != null) temp.add(node.right);

            }
            for (TreeNode node : temp) queue.add(node);
        }
        return res;
    }
```