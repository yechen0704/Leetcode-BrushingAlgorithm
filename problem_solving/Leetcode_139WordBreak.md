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
This thinking also can be optimized

we can use index + word.len to avoid for loop unnecessary index

```java
class Solution {
    List<String> words;
    Boolean[] memo;
    String s;

    public boolean wordBreak(String s, List<String> wordDict) {
        this.s = s;
        this.words = wordDict;
        memo = new Boolean[s.length() + 1];
        return dfs(0);
    }

    private boolean dfs(int index) {
        if (index == s.length()) return true;
        if (memo[index] != null) return memo[index];

        for (String w : words) {
            int len = w.length();
            if (index + len <= s.length() &&
                s.startsWith(w, index) &&
                dfs(index + len)) {
                return memo[index] = true;
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

Ignoring the cost of substring copying and the space used by the dictionary, both algorithms have O(n²) time complexity and O(n) space complexity.

However, when running these two approaches on LeetCode, the recursive (top-down DP) solution performs better in practice.

Why❓
Although both approaches have the same asymptotic complexity,
top-down DP often runs faster in practice because it evaluates only reachable states and can terminate early,
while bottom-up DP enumerates all states regardless of necessity.

---
## Approach 3: Trie-based search + memo
Even though the previous algorithm is already good enough — a top-down recursion with memoization can easily beat most submissions on LeetCode — we can still ask: where can we optimize further in practice?

The common implementation uses substring + HashSet.

HashSet.contains() is amortized O(1), but only after you already have the key.

The expensive part is creating the substring:

substring(i, j) allocates a new String (in modern Java it usually copies characters),

and computing the hash for that new string typically costs O(length) because it must iterate over the characters.

So each membership check is closer to O(L), where L = j - i + 1, not truly O(1).
When you do this inside a double loop over (i, j), the real runtime can feel much worse than the “clean” O(n²) analysis.

To optimize this, we can use a Trie.

A Trie avoids substring creation and hashing.
Instead of generating every possible substring and asking “is it in the set?”, we walk the trie character by character and only follow paths that exist in the dictionary. The moment the next character does not match any trie edge, we stop immediately (early cut).

Example:
s = "aaab"
dict = ["aaaa", "aaab"]

Starting from index 0:

We walk a -> a -> a -> b in the trie.

When we reach the 4th character, we can directly know "aaab" is a word (isWord = true).

If the string were "aaac" instead, the trie would fail at the last step (c edge does not exist) and we would stop right away — without creating "aaac" as a substring or hashing it.

```java
class Solution {
    static class TrieNode {
        TrieNode[] next = new TrieNode[26];
        boolean isWord;
    }

    public boolean wordBreak(String s, List<String> wordDict) {
        TrieNode root = buildTrie(wordDict);
        int n = s.length();
        boolean[] dp = new boolean[n + 1];
        dp[n] = true;

        for (int i = n - 1; i >= 0; i--) {
            TrieNode cur = root;
            for (int j = i; j < n; j++) {
                int idx = s.charAt(j) - 'a';
                if (idx < 0 || idx >= 26 || cur.next[idx] == null) break;
                cur = cur.next[idx];

                if (cur.isWord && dp[j + 1]) {
                    dp[i] = true;
                    break; // early stop
                }
            }
        }
        return dp[0];
    }

    private TrieNode buildTrie(List<String> wordDict) {
        TrieNode r = new TrieNode();
        for (String w : wordDict) {
            TrieNode cur = r;
            for (int k = 0; k < w.length(); k++) {
                char ch = w.charAt(k);
                int idx = ch - 'a';
                if (cur.next[idx] == null) cur.next[idx] = new TrieNode();
                cur = cur.next[idx];
            }
            cur.isWord = true;
        }
        return r;
    }
}
```
TC : O(words.length * maxLen)buidTrie + O(n^2) 2 for loop
SC : O(word.length * maxLen)buildTrie + O(n) dp array
