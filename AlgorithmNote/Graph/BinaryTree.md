# Binary Tree
## Preorder Traversal
## Inorder Traversal
## Postorder Traversal
## Levelorder Traversak
## Two Recursive Mindsets for binary tree problems
When solving binary tree problems with recursion, there are two main ways of thinking:
1. Traversal-style (visit the whole tree once)
   - You "walk" the tree and do something at each node
   - The recursive function usually has no return value
   - You rely on external / global variables to accumulate the result
> Traversal-style function naming & signarure conventions
```
void traversal(TreeNode root, ...) {...}
```
- The function just visit nodes and updates outside variables
> Backtracking
```
void backtrack(State state, ...){...}
```
- modify a "path" or "state"
- pushing / popping / undoing changes
- updating global answers

2. Divide and conquer / subproblem style  
   - you treat each node as the root of subproblem
   - the recursive function usually returns a value
   - the return value is the answer to that subproblem, which is combine up the tree
> Dynamic programming
```
int dp(int i, int j, ...) {...}
```
- Returns a value: the answer for subproblem(i, j, ...)
