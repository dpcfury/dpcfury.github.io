---
title: Leetcode[212] Word Search II
date: 2020-04-15 22:08:25
urlname: word-searchII
categories:
- leetcode
tags:
- backtracking
- trie
- matrix
- hard
---
>[Leetcode 212. Word Search II](https://leetcode.com/problems/word-search-ii/)
题目要求在二维字符数组中搜索存在的单词，这题从**word search**延伸出来，不过这次不仅是判断是否存在，而是要求得所有可搜索串联的单词，自然而然会想到回溯，但是时间肯定会超时，关键就在于如何进行剪枝。

<!--more-->

#### 思路
思路一：在回溯过程中，对超过单词集最大长度的中间结果，直接停止搜索，但是36个case还有3个没过，仍然超时。
思路二：构建单词集的前缀树，对搜索过程中遇到的非已有前缀情况，直接停止搜索，这种剪枝更为合适。

#### 算法实现
```java
public List<String> findWords(char[][] board, String[] words) {
        List<String> res = new ArrayList<>();
        if (board == null || board[0].length == 0) return res;
        int row = board.length;
        int col = board[0].length;
        HashSet<String> set = new HashSet<String>(Arrays.asList(words));

        // 构建前缀字典树
        TrieTree root = new TrieTree();
        for (String s : words)
            root.insert(s);

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (board[i][j] >= 'a' && board[i][j] <= 'z') {
                    backtracking(board, row, col, i, j, res, new StringBuilder(), set, root.root);
                }
            }
        }

        return res;
    }

    private boolean backtracking(char[][] board, int row, int col, int i, int j, List<String> res,
                                 StringBuilder stringBuilder, HashSet<String> set, TrieNode cur) {
        if (set.contains(stringBuilder.toString()) && !res.contains(stringBuilder.toString())) {
            res.add(stringBuilder.toString());
            return true;
        } else if (i < 0 || i >= row || j < 0 || j >= col) {
            return false;
        }
        if (board[i][j] >= 'a' && board[i][j] <= 'z' && cur.children[board[i][j] - 'a'] != null) {
            TrieNode p = cur.children[board[i][j] - 'a'];
            stringBuilder.append(board[i][j]);
            board[i][j] ^= 256;
            boolean flag = false;
            flag = backtracking(board, row, col, i, j + 1, res, stringBuilder, set, p);
            flag |= backtracking(board, row, col, i, j - 1, res, stringBuilder, set, p);
            flag |= backtracking(board, row, col, i - 1, j, res, stringBuilder, set, p);
            flag |= backtracking(board, row, col, i + 1, j, res, stringBuilder, set, p);
            board[i][j] ^= 256;
            stringBuilder.deleteCharAt(stringBuilder.length() - 1);
            if (flag) return true;
        }

        return false;
    }

    static class TrieNode {
        char c;
        TrieNode[] children;
        boolean isWord;

        public TrieNode(char c) {
            this.c = c;
            children = new TrieNode[26];
        }
    }

    static class TrieTree {
        TrieNode root;

        public TrieTree() {
            root = new TrieNode('#');
        }

        private void insert(String word) {
            TrieNode cur = root;
            char[] chs = word.toCharArray();
            for (int i = 0; i < chs.length; i++) {
                if (cur.children[chs[i] - 'a'] == null) {
                    cur.children[chs[i] - 'a'] = new TrieNode(chs[i]);
                }
                cur = cur.children[chs[i] - 'a'];
            }
            cur.isWord = true;
        }
    }
```