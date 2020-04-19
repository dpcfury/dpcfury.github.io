---
title: Leetcode[22] Generate Parentheses
date: 2020-04-09 22:34:06
urlname: generate-parentheses
categories:
- leetcode
tags:
- backtracking
- leetcode
- medium
---
>[Leetcode 22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)
这题要求生成指定对数的括号，通常可以直接对应到回溯来做，只要控制好边界，再引入合法括号的判断，基本记得到最终解，如果时间效率优化的话，可以考虑进行剪枝。

<!--more-->

#### 思路
利用一个temp（StringBuilder）来存放临时的解，如果temp的长度达到了2 * n 并且是合法的括号，那么就把这个临时解存放到结果集中。回溯完成，删除最后一个字符，进行下一轮搜索。


#### 算法实现
```java
public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<>();
        if (n < 0) return res;
        backtracking(res, n, new StringBuilder());
        return res;
    }

    public void backtracking(List<String> res, int n, StringBuilder str) {
        if (str.length() == n * 2 && validParent(str.toString())) {
            res.add(str.toString());
        }
        if (str.length() > n * 2) return;
        char[] pat = {'(', ')'};
        for (char c : pat) {
            backtracking(res, n, str.append(c));
            str.deleteCharAt(str.length() - 1);
        }

    }

    private boolean validParent(String str) {
        if (str == null || str.length() == 0) return false;
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) == '(') stack.add('C');
            else if (str.charAt(i) == ')') {
                if (!stack.isEmpty()) stack.pop();
                else return false;
            } else return false;
        }
        return stack.isEmpty();
    }
```