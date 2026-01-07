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
If you have played *The Legand of Zelda*, congratulations - you've just made it out of the beginner village.

But this is still not enough. When you submit the solution, you may find that some test cases exceed the time limit.

```java

/**
   Let's deep dive into this test case

                  a a a a a a a a b                           dict : ["a", "aa", "aaa"]
                     /       |     \
                   a/      aa|      \aaa
                  /          |       \
             aaaaaaab     &aaaaaab    $aaaaab
             /  |  \
           a/ aa|    \aaa                                    & - 7 a's
           /    |     \                                      $ - 6 a's
    &aaaaaab  $aaaaab   aaaab  

   In this case, different branch have same subtree, it's duplicate
   so we can use memorization to cut edge
*/

class Solution {
    HashSet<String> set;
    Boolean[] memo; 
    public boolean wordBreak(String s, List<String> wordDict) {
        memo = new Boolean[s.length() + 1];
        set = new HashSet<>(wordDict);
        return dfs(s, 0);
    }

    private boolean dfs(String s, int index){
        if (index == s.length()) return memo[s.length()] = true;
        if (memo[index] != null) return memo[index];

        for (int i = index + 1; i <= s.length(); i++) {
            if (set.contains(s.substring(index, i)) && dfs(s, i)) {
                return memo[i] = true; // ----i---index----  i prev has been check, i to index check by set, back of i call dfs check
            }
        }
        return memo[index] = false;
    }
}
```

---
## Approach 2: Bottom-up DP
---
## Approach 3: Trie-based Search
