# Word Break (LeetCode 139)

## Problem Restatement
Given a string `s` and a dictionary `wordDict`, determine whether `s` can be segmented
into a sequence of one or more dictionary words.

At each position in the string, we decide **whether to cut**, and if so, **how far**.

---

## Solution Overview

| Approach | Paradigm | Key Idea | Time | Space | Trade-off |
|--------|----------|----------|------|-------|-----------|
| Recursive + Memo | Top-down DP | DFS on index with caching | O(n²) | O(n) | Simple & intuitive |
| DP Table | Bottom-up DP | Build validity from left to right | O(n²) | O(n) | Iterative & stable |
| Trie-based | DS + Search | Prefix matching with pruning | O(n·L) | O(n + dict) | Faster lookups, more setup |

---
## Core abstraction

At first glance, our intuition is to try matching each word in wordDict against the string s.
Whenever we find a match, we recursively move to the next position and repeat the process.

we solve the same subproblem repeatedly. => **recursive**

---
## Approach 1: Recursive + Memo (Top-down DP)
#### Pure Recursive
```java
/**
    s : a p p l e p e n a p p l e                   dict : ["apple", "pen", "ap"]
r1        /    |     \
    apple/     |pen   \ap
        /      |       \
    penapple   ❌      plepenapple
*/
```
---
## Approach 2: Bottom-up DP
---
## Approach 3: Trie-based Search
