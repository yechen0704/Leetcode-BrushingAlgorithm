# Leetcode 5 Longest Palindromic Substring
---

## Problem Description
Return the longest palindromic substring

---
## Key Observartion
1. Palindromic can be `aba`, also can be `aa`
2. substring is part consecutive of string
---
## Core Algorithm
### Solution 1: Brute force
It's easy to think about brute force, for loop all substring  
Check is palin or not

babad
b✅        a✅      b✅      a✅      d✅
ba❌       ab❌     ba❌     ad❌
bab✅      aba✅    bad❌
baba❌     abad❌
babad❌

TC : O(n^3)  
SC : O(n)

### Solution 2 : Expand Around center
center can be even(i, i+1) and odd(i)  
- even center : n - 1
- odd center : n
- Total center : 2n - 1

for loop center, and from inside to outside to check palindromic
```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        String res = "";
        for (int i = 0; i < n; i++) {

            String oddCenter = check(s, i, i);
            if (oddCenter.length() > res.length()) {
                res = oddCenter;
            } 
            if (i != n - 1) {
                String evenCenter = check(s, i, i + 1);
                if (evenCenter.length() > res.length()) {
                    res = evenCenter;
                }
            }
            
        }
        return res;
    }

    private String check(String s, int c1, int c2) {
        if (c1 > c2) return "";
        while (c1 >= 0 && c2 < s.length() && s.charAt(c1) == s.charAt(c2)) {
            c1--;
            c2++;
        }
        return s.substring(c1 + 1, c2);
    }
}
```
