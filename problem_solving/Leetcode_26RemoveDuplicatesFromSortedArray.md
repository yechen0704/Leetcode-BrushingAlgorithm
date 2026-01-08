# 26. Remove Duplicates from Sorted Array
---

## Problem Summary
Giben a sorted array `nums` (non-decreasing)  
remove the duplacates **in-place** such that each unique element appear only once

- The relative order of element should be kept
- After removal, the first k elements of nums should contain the unique elements
- element beyond index k - 1 can be ignore
- return k, the number of unique elements
---


## Key observation
1. Array is sorted, so duplicated element is consective
2. We need O(1) extra space, so use variable pointer. We need mark position pointer and element pointer, so **Two pointer**
```java
/**
  s: this point to need be exchange element
  0 0 1 1 2 2 3 3 4
    f: this point to followed different elment
for (fast from 1 to n-1)
  if (num[fast] != nums[slow])
    slow++
    nums[slow] = nums[fast]
The number of unique element is slow + 1
*/
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        if (n == 0) return 0;
        int slow = 0, fast = 1;
        while (fast < n) {
            if (nums[slow] != nums[fast]) {
                slow++;
                nums[slow] = nums[fast];
            }
            fast++;
        }
        return slow + 1;
    }
}
```
**Time Complexity:**
O(n)  
**Space Complexity:**
O(1)  
