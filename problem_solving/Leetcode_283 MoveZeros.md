# Leetcode 283 Move Zeros
---

## Problem Summary
Given an integer array nums, move all 0 to the end of array  
maintain relative order of non-zero elements  
in-place change

--- 
## Key Observation & Implemention
### Solution 1, swap + for loop
```
    f                     f                 f             f=n-1          f=n
  0 1 0 3 12 -f&s-> 1 0 0 3 12 --> 1 3 0 0 12 -> 1 3 12 0 0  -> 1 3 12 0 0
  s                   s                s              s                s

if (nums[fast] == 0):
  // keep move fast
else
  // exchange fast ele and slow ele
  // keep move fast & slow
```
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length;
        int slow = 0, fast = 0;
        if (n == 0) return;

        while (fast < n) {
            if (nums[fast] != 0) {
                swap(nums, fast, slow);
                slow++;
                fast++;
            } else {
                fast++;
            }
        }
    }

    private void swap(int[] nums, int fast, int slow) {
        int temp = nums[fast];
        nums[fast] = nums[slow];
        nums[slow] = temp;
    }
}
```
TC : O(n)
SC : O(1)

### Solution 2 : fill ele
``````
