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
       

```
