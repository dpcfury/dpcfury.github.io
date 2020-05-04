---
title: Leetcode[46] Permutations
date: 2020-04-25 21:10:51
urlname: permutations
categories:
- leetcode
tags:
- leetcode
- backtracking
---

>[Leetcode 46. Permutations](https://leetcode.com/problems/permutations/)
题目：给定一个 **没有重复 数字的序列**，返回其所有可能的全排列。

<!--more-->

#### 思路
不带重读的全排列问题，这种permutations、Subsets、Combinations问题，基本都可以沿用回溯的方法去搜索求解，具有比较一致的重复套路：[通用回溯思路](https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning))。

但是深入理解回溯和搜索空间以及优化，对后面的结题其实有非常大的帮助,这里贴出力扣中国给出的社区解答：[官网解答](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/)


#### 算法实现
approach1：原始通用解法
```java
public List<List<Integer>> permute(int[] nums) {
   List<List<Integer>> list = new ArrayList<>();
   // Arrays.sort(nums); // not necessary
   backtrack(list, new ArrayList<>(), nums);
   return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums){
   if(tempList.size() == nums.length){
      list.add(new ArrayList<>(tempList));
   } else{
      for(int i = 0; i < nums.length; i++){ 
         if(tempList.contains(nums[i])) continue; // element already exists, skip
         tempList.add(nums[i]);
         backtrack(list, tempList, nums);
         tempList.remove(tempList.size() - 1);
      }
   }
}
```

approach2：优化的swap解法
```java
public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> list = new ArrayList<>();
        // Arrays.sort(nums); // not necessary
        backtrack(list, 0, nums);
        return list;
    }

    private void backtrack(List<List<Integer>> list, int start, int[] nums){
        if(start == nums.length){
            List<Integer> tmpList = new ArrayList<>();
            for(int i:nums)
                tmpList.add(i);
            list.add(new ArrayList<>(tmpList));
        } else{
            for(int i = start; i < nums.length; i++){
                swap(start,i,nums);
                backtrack(list, start+1, nums);
                swap(i,start,nums);
            }
        }
    }
    private void swap(int i,int j, int[] nums){
        int temp = nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }
```

