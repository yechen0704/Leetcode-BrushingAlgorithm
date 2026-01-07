
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
## Key Observatrions
1. No matter which edge we remove, the two resulting subtree sums always add up to the total sum of the original tree. If one part has sum sub, the other part must be total - sub, so the product is sub * (total - sub).
2. We must compare the raw products (without modulo) when searching for the maximum. Taking % MOD too early can change the ordering and lead to the wrong answer. Only apply modulo once at the end when returning the result.

---
## Code (java)
```java
class Solution {
    long maxProduct = 0l;
    long totalSum = 0l;
    long mod = (long)(1e9 + 7);
    public int maxProduct(TreeNode root) {
        totalSum = countSum(root);
        dfs(root);
        return (int)(maxProduct % mod);
    }

    private long countSum(TreeNode root) {
        if (root == null) return 0;
        return root.val + countSum(root.left) + countSum(root.right);
    }

    private long dfs(TreeNode root) {
        if (root == null) return 0l; // bottom of the subtree
        long curSum = root.val + dfs(root.left) + dfs(root.right); // root bring it's subtree as a part
        long curProd = curSum * (totalSum - curSum); // count product
        maxProduct = Math.max(maxProduct, curProd); // update max product
        return curSum; // current subtree maybe be part of others
    }
}
```
**Time Complexity:**
countSum - O(n)
dfs - O(n) dfs visit each node one time, other operation are all O(1)
Total TC : O(n)
**Space Complexity:**
variable are O(1) SC
countSum - O(h) stack space
- IF balance tree O(h) = O(logn)
dfs - O(h) stack space
Total SC : O(h)

---
## Dry Run
```
    1
   / \
  2  3
/ \  /
4  5 6
```  
countSum
countSum(1)
  countSum(2)
    countSum(4) return 4
    countSum(5) return 5
  return 4 + 5 + 2 = 11
  countSum(3)
    countSum(6) return 6
  return 3 + 6 = 9
11 + 9 + 1 = 21
```
DFS Dry Run

maxProd: -> 68 -> 80 -> 110 ->

total sum = 21
```
dfs(1)
  === 1.left
  dfs(2)
    dfs(4)
      curSum = 4
      curProd = 4 * 17 = 68
    dfs(5)
      curSum = 5
      curProd = 5 * 16 = 80
  curSum = 2 + 4 + 5 = 11
  curProd = 11 * 10 = 110
  === 1.right
  dfs(3)
    dfs(6)
      curSum = 6
      curProd = 6 * 15 = 90
  curSum = 3 + 6 = 9
  curProd = 9 * 12 = 108
curSUm = 1 + 11 + 9 = 21
curProd = 21 * 0 = 0
```

  
