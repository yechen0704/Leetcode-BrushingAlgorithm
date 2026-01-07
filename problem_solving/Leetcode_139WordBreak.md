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
                return memo[index] = true; // ----i---index----  i prev has been check, i to index check by set, back of i call dfs check
            }
        }
        return memo[index] = false;
    }
}
```

---
## Approach 2: Bottom-up DP

DP thinking is essentially the same as recursive thinking
> 💡 **Key Insight**
> recursion + memo : starts from a given position and explores all possible subsequent paths.
If one path reaches the end of the string, the result is propagated back from bottom to top.
> DP : If the future state represents the same subproblem and is reachable from the current state,
the current state is marked as valid and the feasibility is propagated forward.
In the end, we only need to check the final state.
```java
DFS + Memo                                       DP
dfs(0):                                       dp[0]✅
   cut on i=4                                 0-4 check by dict => dp[4]✅
      dfs(4):                                 4-8 check by dict => dp[8]✅
         cut on i=8                           return dp[8]
            dfs(8):✅
         so dfs(4) ✅
      so dfs(0)✅
```
1. DP definition: dp[i] - on i position,the prefix s[0..i) can be segmented using words in the dictionary.
2. DP Initialization : dp[0] = true - The empty string can always be segmented without using any words.
3. DP state transition equation : dp[i] = (s[j..i) ∈ dict && dp[j] == true)
4. DP return : dp[n]
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        HashSet<String> set = new HashSet<>();
        for (String w : wordDict) {
            set.add(w);
        }

        for (int i = 0; i < s.length(); i++) {
            // if (!dp[i]) continue;    
            for (int j = i + 1; j <= s.length(); j++) {
                if (dp[i] && set.contains(s.substring(i, j))) {
                    dp[j] = true;
                }
            }
        }
        return dp[s.length()];
    }
}
```
Recursive + memo
**TC** : O(n)cur level loop + O(n)recursive inner layer loop = O(n^2)
**SC** : O(n) memo + O(n) stack memory = O(n)

DP
**TC**: O(n^2) 2 layer loop
**SC**: O(n) dp

Ignore substring copy and set dict space

Even if they both have O(n^2) time complexity and O(n) space complexity, but actually when we run this two algorithm on leetcode, the recursive performance more well
Why❓
Although both approaches have the same asymptotic complexity,
top-down DP often runs faster in practice because it evaluates only reachable states and can terminate early,
while bottom-up DP enumerates all states regardless of necessity.

---
## Approach 3: Trie-based Search
