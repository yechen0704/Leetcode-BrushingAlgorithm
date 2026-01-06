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
   ｜--------->✅s : a p p l e p e n a p p l e                   dict : ["apple", "pen", "ap"]
   ｜              /    |     \
   ｜        apple/     |pen   \ap
   ｜            /      |       \
   ｜-----> ✅penapple   ❌      plepenapple
   ｜       /    \                /  |  \
   ｜   pen/      \apple         ❌  ❌  ❌ <= Whichone won't work here
   ｜     /        \
   ｜---> ✅apple      ❌
   ｜    /
   ｜   / apple
   ｜  /
   ✅"" <= index = s.len
*/
class Solution {
    Set<String> dict;
    public boolean wordBreak(String s, List<String> wordDict) {
        dict = new HashSet<>(wordDict);
        return dfs(s, 0);
    }

    private boolean dfs(String s, int i) {
        if (i == s.length()) return true;
        for (int j = i + 1; j <= s.length(); j++) {
            if (dict.contains(s.substring(i, j)) && dfs(s, j)) {
                return true;
            }
        }
        return false;
    }
}
```
---
## Approach 2: Bottom-up DP
---
## Approach 3: Trie-based Search
