
# Maximum Product of Splitted Binary Tree (LeetCode 1339)
---
## Problem Restatement
We are given the root of a binary tree where each node contains a positive value.

We must split this tree into two subtrees by removing exactly one edge, then compute the product of the sum of the two resulting subtrees.

Return the maximum product modulo 1e9+7.

```java
/**
  Original Tree
      1                       2        1
     / \                     / \        \
    2   3    --(1-2)-->     4   5        3
   / \  /                    (11)       /   (10)  = 11 * 10 = 110
  4   5 6                              6

            --(1-3)-->       1          3
                            /           /
                           2           6
                          / \
                         4   5
                          (12)        (9)    = 12 * 9 = 108
            ......
*/
```
