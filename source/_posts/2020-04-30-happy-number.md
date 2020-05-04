---
title: Leetcode[202] happy number
date: 2020-04-30 17:19:35
urlname: happy-number
categories:
- leetcode
tags:
- leetcode
- linked list
- easy
---
>[Leetcode 202. Happy Number](https://leetcode.com/problems/happy-number/)

<!--more -->


#### 思路
链表循环问题，如果转换不能走到1，是否一定会走入循环的过程，还是走入无限大，通过数学论证，一定不会会越来越大，那么只要判断是否走入循环或者走到最终结果1，这里直观的可以通过set几何进行判断是否进入了循环情况，也可以通过快慢指针的方式进行实现。

#### 算法实现
```java
public boolean isHappy(int n) {
        HashSet<Integer> cache = new HashSet<>();
        while (true) {
            int sum = sumOfDigits(n);
            if (sum == 1) return true;
            else if (cache.contains(sum)) break;
            else n = sum;

            cache.add(sum);
        }
        return false;
    }

    private int sumOfDigits(int temp) {
        int sum = 0;
        while (temp != 0) {
            int x = temp % 10;
            sum += x * x;
            temp = temp / 10;
        }
        System.out.println(sum);
        return sum;
    }
```

第二种思路，通过快慢指针，如果循环肯定能够相遇，如果相遇点不是1，肯定是出现了循环，直接返回False
```java
private int sumOfDigits(int temp) {
        int sum = 0;
        while (temp != 0) {
            int x = temp % 10;
            sum += x * x;
            temp = temp / 10;
        }
//        System.out.println(sum);
        return sum;
    }

    public boolean isHappy(int n) {
        int slow = n;
        int fast = sumOfDigits(n);
        while (slow != fast) {
            slow = sumOfDigits(slow);
            fast = sumOfDigits(sumOfDigits(fast));
        }
        return slow == 1;
    }
```
