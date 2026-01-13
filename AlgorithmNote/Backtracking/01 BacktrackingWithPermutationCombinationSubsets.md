# Backtraking on Permutation / Combination / Subsets
## Basic Concepts
1. Substring (子串) - contiguous
     - abcde : "a" "bc" "cde" "abcde" ...
2. Subarray (子数组) - contiguous
     - [1,2,3,4] : [1],[2,3],[3,4],[1,2,3,4]
3. Subsequence (子序列) - ❌contiguous
     - “abcde” : "ace", "bdf" ...
     - keep order
4. Subset (子集) - ❌contiguous
     - {1,2,3} : {}, {2}, {3,2} ...
     - don't need keep order
```
combination/subset                               permutation
                []                                                []
            /   |   \                                           /  |  \ 
           1    2    3                                        1    2    3
          /     |     \                                      /     |     \
         []     []    []                                    []     []    []
        / \     |                                          /  \    / \   / \
       2  3     3                                          2  3   1   3  1  2
      /    \    |                                         /   /   |   |   \   \
     []    []   []                                       []   []  []  []  []  []
     /                                                  /     /   |    |    \   \
    3                                                  3     2    3    1     2   1
   /                                                  /     /     |    |      \    \
  []                                                 []    []     []   []     []    []
       
1. order doesn't matter                              1. Order matter
2. forward only to avoid duplicate                   2. pick any unuse element
3. [1,3] = [3,1]                                     3. [1,3] != [3,1]
4. node number = 2^n                                 4. node number = n!
```
---
## Subset : no duplicate no reuse
Leetcode_78 subset
1. colleact on each node
2. keep track path element
3. keep forward from current level index
```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> subsets(int[] nums) {
        res = new ArrayList<>();
        // choice or not : 2^n
        recursive(nums, 0, new ArrayList<Integer>());
        return res;
    }
    private void recursive(int[] nums, int index, List<Integer> path) {
        res.add(new ArrayList<>(path));

        for (int i = index; i < nums.length; i++) {
            int cur = nums[i];
            path.add(cur);
            recursive(nums, i + 1, path);
            path.removeLast();
        }
    }
}
```
---
## combination : no duplicate no reuse
Leetcode 77 Combination
1. num of combination : C(n, k) = n! / (k!(n - k)!)
2. in code, keep forward can avoid duplicate
3. for each position, coose or not, recursive to next position
4. collect on len = k
```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> combine(int n, int k) {
        res = new ArrayList<>();
        recursive(n, k, 1, new ArrayList<>());
        return res;
    }

    private void recursive(int n, int k, int i, List<Integer> path) {
        if (path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int start = i; start <= n; start++) {
            path.add(start);
            recursive(n, k, start + 1, path);
            path.removeLast();
        }
    }
} 
```
---
## permutation : no duplicate no reuse
Leetcode 46 permutations
1. cannot keep forward to avoid duplicate, need extra space to store used element 
2. collect element on end of track, track = nums.len
```java
class Solution {
    boolean[] used;
    List<List<Integer>> res;
    public List<List<Integer>> permute(int[] nums) {
        int n = nums.length;
        used = new boolean[n];
        res = new ArrayList<>();
        recursive(nums, new ArrayList<>());
        return res;
    }

    private void recursive(int[] nums, List<Integer> path) {
        int n = nums.length;
        if (path.size() == n) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < n; i++) {
            if (used[i]) continue;
            used[i] = true;
            path.add(nums[i]);
            recursive(nums, path);
            path.removeLast();
            used[i] = false;
        }
    }
}
```
---
## subset/combination : duplicate no reuse
```
     nums = [1, 2, 2']
               []
          /     |     \
         1      2      2'❌
      /    \        /     \
     2      2'❌   1       2
   [1,2]   [1,2']  [2'1]   [2',2]

1. observe the tree have duplicate part all caused by 2'
2. need cutting branch
3. check duplicate element, and skip
4. sort array to let duplicated element adjacency
```

```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        res = new ArrayList<>();
        res.add(new ArrayList<>());

        Arrays.sort(nums);
        backtrack(nums, 0, new ArrayList<Integer>());
        return res;
    }

    private void backtrack(int[] nums, int startIndex, List<Integer> path) {
        // exit
        if (startIndex >= nums.length) return;

        for (int i = startIndex; i < nums.length; i++) {
            if (i > startIndex && nums[i] == nums[i - 1]) {
                continue;
            }
            path.add(nums[i]);
            res.add(new ArrayList<>(path));
            backtrack(nums, i + 1, path);
            path.remove(path.size() - 1);
        }
    }
}
```

**Leetcode 40 Combination SumII**  
find all unique combinations in candidates in candidates where the candidate numbers sum to target
```
     [1, 2, 2'] sum=3
     Tree total as upper one
1. sort array, to make same number adjancency
2. cutting edge same as previous num
3. collect node with sum = target
```
```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        res = new ArrayList<>();
        Arrays.sort(candidates);
        recursive(candidates, 0, 0, target, new ArrayList());
        return res;
    }

    private void recursive(int[] nums, int sum, int startPos, int target, List<Integer> path) {
        if (sum == target){
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = startPos; i < nums.length && sum + nums[i] <= target; i++) {
            if (i > startPos && nums[i] == nums[i-1]) {
                continue;
            }

            sum += nums[i];
            path.add(nums[i]);
            recursive(nums, sum, i + 1, target, path);
            sum -= nums[i];
            path.remove(path.size()-1);
        }

    }
}
```
---
## permutation : duplicate no reuse
#### Leetcode 47 : Permutations II
```
[1,2,2']
                    []
               /     |     \ 
              1      2      2'
             /       |       \
           [1]      [2]       [2']
         /    \    /    \    /    \
        2      2'  1    ❌2' 1      2
       /      /   |      |   \      \
     [1,2] [1,2'] [2,1] [2.2'] [2',1] [2',2]
    /        /    |      |       \       \
   ❌2'      2    ❌2'     1        2       1
  /        /      |      |         \        \
[1,2,2'] [1,2',2] [2,1,2'] [2,2',1] [2',1,2] [2',2,1]
  #        #         $         *        $        *

2,2' order cause duplicate
think : how to cut edge avoid this situation
❌means keep curernt element not show after previous same element

So, the logic to cutting edge:
if (i > startPos && nums[i] == nums[i - 1] && used[nums[i - 1]]) continue;
```

```java
class Solution {
    List<List<Integer>> res;
    boolean[] used;
    public List<List<Integer>> permuteUnique(int[] nums) {
        res = new ArrayList<>();
        Arrays.sort(nums);
        used = new boolean[nums.length];
        recursive(nums, 0, new ArrayList<>());
        return res;
    }

    private void recursive(int[] nums, int startPos, List<Integer> path) {
        if (path.size() == nums.length) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1]) continue;

            path.add(nums[i]);
            used[i] = true;
            recursive(nums, i, path);
            used[i] = false;
            path.remove(path.size() - 1);
        }
    }
}
```

***Optimization***
How about [2,2',2'']?
```
                    []
               /     |     \ 
              2      2'❌   2''❌
             /       |       \
           [2]      [2']       [2'']
         /    \    /    \    /    \
        2'   2''  2      2'' 2      2'
       /      /   |      |   \      \
     [2,2'][2,2''][2',2][2',2''][2'',2] [2'',2']
      #       %      #      *       %      *

We can see, we can't keep any duplicate element in same level
so, used[n - 1] not enough 
how to pass status from 2 -> 2' -> 2''
                        T -> ❌ -> ❓
                        ❌ -> ❌ -> ❌ means : if we don't use previous first ele, we can not use any followed

So:
if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) continue;
```

---
## subset/combination : no duplicate reuse
---
## combination : no duplicate reuse
