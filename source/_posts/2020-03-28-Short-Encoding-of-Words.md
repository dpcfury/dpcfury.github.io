---
title: Leetcode[820] Short Encoding of Words
date: 2020-03-28 20:44:24
urlname: leetcode-short-encoding-of-words
categories:
- leetcode
tags:
- leetcode
- tries
- suffix
- medium
---

>[Leetcode 820. Short Encoding of Words](https://leetcode.com/problems/short-encoding-of-words/)
题目给出一组字符串数组，计算最短的编码字符串长度。编码的方式就是通过‘#‘号分割，再加一个索引，能够从编码字符串中解码初原始字符串。读一下题目，大致可以知道这题的意思是和后缀有关，因为如果一个单词是其他单词的后缀，它的长度肯定不会被计算入加密字符串。因此，先给字符串数组排个序，让长的字符串在前，短的在后，通过每次比较当前字符串和已构造的加密字符对比，含是否是已有字符串的后缀。

<!-- more-->

#### approach 1

```java
public int minimumLengthEncoding(String[] words) {
        if (words == null | words.length == 0) return 0;
        String[] strs = new String[words.length];
        Arrays.sort(words, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o2.length() - o1.length();
            }
        });
        for (int i = 0; i < words.length; i++) {
            if (i == 0) strs[i] = words[i] + '#';
            else {
                int index = strs[i - 1].indexOf(words[i]);
                if (index != -1) { // 看能否匹配到#
                    if (strs[i - 1].charAt(index + words[i].length()) == '#') {
                        strs[i] = strs[i - 1];
                        continue;
                    }
                }
                strs[i] = strs[i - 1] + words[i] + '#';
            }
        }
        return strs[words.length - 1].length();
    }
```

#### approach 2
> 从leetcode 官网给的solution也可以看出来，既然本质是剔除那些本身是其他单词后缀的单词。提到后缀，其实变相的就是逆前缀，而提到前缀就会想到Trie字典树，字典树的分支就是26个字母分叉，对于不是其他单词后缀的单词，其对应在字典树中一定能从根遍历到叶子结点。因此遍历字符串数组的过程中，可以去构造逆向的前缀树，然后统计能走到叶子结点的字符串长度和即可。

```java
class Solution {
    public int minimumLengthEncoding(String[] words) {
        TrieNode trie = new TrieNode();
        Map<TrieNode, Integer> nodes = new HashMap();

        for (int i = 0; i < words.length; ++i) {
            String word = words[i];
            TrieNode cur = trie;
            for (int j = word.length() - 1; j >= 0; --j)
                cur = cur.get(word.charAt(j));
            nodes.put(cur, i);
        }

        int ans = 0;
        for (TrieNode node: nodes.keySet()) {
            if (node.count == 0) // 走到叶子结点
                ans += words[nodes.get(node)].length() + 1;
        }
        return ans;

    }
}

class TrieNode {
    TrieNode[] children;
    int count;
    TrieNode() {
        children = new TrieNode[26];
        count = 0;
    }
    public TrieNode get(char c) {
        if (children[c-'a'] == null) {
            children[c-'a'] = new TrieNode();
            count++;
        }
        return children[c - 'a'];
    }
}
```

