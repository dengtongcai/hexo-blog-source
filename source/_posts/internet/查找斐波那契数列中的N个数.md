layout: '[layout]'
title: 查找斐波那契数列中的N个数
date: 2013-06-01 21:31:09
categories: 代码
tags: [算法,lintcode]
---
查找斐波纳契数列中第 N 个数。 
所谓的斐波纳契数列是指： 
前2个数是 0 和 1 。 
第 i 个数是第 i-1 个数和第i-2 个数的和。 
斐波纳契数列的前10个数字是： 
0, 1, 1, 2, 3, 5, 8, 13, 21, 34 …
```
class Solution {
    /**
     * @param n: an integer
     * @return an integer f(n)
     */
    public int fibonacci(int n) {
        int a = 0;
        int b = 1;
        int c = 0;

        if (n == 1) { //n=1时，数列中为0.
            c = 0;
        } else if (n == 2) { //n=2时数列中为1.
            c = 1;
        } else {
            for (int i = 0; i < (n - 2); i++) { //从n=3开始，满足数列的规律。
                c = a + b;
                a = b;
                b = c;
            }
        }

        return c;
    }
}
```