# Leetcode 76 Minimum Window Substring
**Topic : HashMap, sliding window**
**Difficulty: Hard**  
---

## Problem description
Given Two string s and t of lengths m and n respectively.  
return minimum window substring of s, such that every character in t (including duplicates) is included in the window.  
If there is no such substring, return the ""  
Tip : testcases will be generated such that answer is unique

---
## Key Observation
1. we need target t map between element with frequency.
2. we need windows element map with frequency
3. we need count window and target together
4. we need return substring, so we need minLen and start position also

target map we can build like:  
``` 
key | freq
A   |  1
B   |  1
C   |  1

s = A D O B E C O D E B A N C
    l         r => first window valid
1. other chars doesn't matter
2. ABC both got 1, total needed count = 3,window valid
3. we need left + length to cut substring
```

So for now we have high level logic
1. Count target map
2. start sliding window, check right char
   - if (right char belong to map && map.get(rChar) > 0) // still need on number
     - count++
     - map.put(rChar, rCharCount - 1)  
   - while (count == len) // satisfied window required, it keeps move until unsatified. So use while loop
     - len update
     - leftC = s.charAt(left)
     - if (map.contains(leftC))
       - map.put(leftC, count + 1)
       - if (map.get(leftC > 0)) count--;

## Java Code
```java
class Solution {
    public String minWindow(String s, String t) {
        HashMap<Character, Integer> targetCount = new HashMap<>();
        for (int i = 0; i < t.length(); i++) {
            char charCur = t.charAt(i);
            targetCount.put(charCur, targetCount.getOrDefault(charCur, 0) + 1);
        }

        // sliding Window
        int count = 0;
        int right = 0, left = 0;
        int len = Integer.MAX_VALUE;
        int startPos = 0;
        while (right < s.length()) {
            char cur = s.charAt(right);
            if (targetCount.containsKey(cur)) {
                targetCount.put(cur, targetCount.get(cur) - 1);
                
                if (targetCount.get(cur) >= 0){
                    count++;
                }
                
            }

            while (count == t.length()) {
                if (right - left + 1 < len) {
                    len = right - left + 1;
                    startPos = left;
                }
                
                char shrink = s.charAt(left);
                if (targetCount.containsKey(shrink)) {
                    targetCount.put(shrink, targetCount.get(shrink) + 1);
                    if (targetCount.get(shrink) > 0) count--;
                }
                left++;
            }
            right++;
        }
        return len == Integer.MAX_VALUE ? "" : s.substring(startPos, startPos + len);
        
    }
}
```
**Time Complexity**
build map : O(m)  
for loop : O(2n)  amortized analysis  
Total : O(n + m)  
**Space Complexity**
HashMap : O(m)  
Total :  O(m)
