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
1. other char doesn't matter
2. ABC both got 1, total needed count = 3,window valid
3. we need left + length to cut substring
```

So for now we have high level logic
1. Count target map
2. start sliding window, check right char
   - if (right char belong to map && map.get(rChar) > 0) // still need on number
     - count++
     - map.put(rChar, rCharCount - 1)
    
    
