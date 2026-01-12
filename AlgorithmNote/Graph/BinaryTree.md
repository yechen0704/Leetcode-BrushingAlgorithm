# Binary Tree
## Preorder Traversal
```java
// Online Java Compiler
// Use this editor to write, compile and run your Java code online
import java.util.*;
class Main {
    static class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;
        public TreeNode(int val, TreeNode left, TreeNode right) {
            this.val = val;
            this.left = left;
            this.right = right;
        }
    }
    
    public static List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        helper(root, res);
        return res;
    }
    
    private static void helper(TreeNode root, List<Integer> res) {
        if (root == null) return;
        res.add(root.val);
        if (root.left != null) helper(root.left, res);
        if (root.right != null) helper(root.right, res);
    }
    public static void main(String[] args) {
        TreeNode root4 = new TreeNode(4, null, null);
        TreeNode root5 = new TreeNode(5, null, null);
        TreeNode root6 = new TreeNode(6, null, null);
        TreeNode root2 = new TreeNode(2, root4, root5);
        TreeNode root3 = new TreeNode(3, root6, null);
        TreeNode root1 = new TreeNode(1, root2, root3);
        List<Integer> ans = preorderTraversal(root1);
        for (int i : ans){
            System.out.print(i);
        }
    }
}
```
## Inorder Traversal
```java
private static void helper(TreeNode root, List<Integer> res) {
        if (root == null) return;
        
        if (root.left != null) helper(root.left, res);
        res.add(root.val);
        if (root.right != null) helper(root.right, res);
    }
```
## Postorder Traversal
```java
private static void helper(TreeNode root, List<Integer> res) {
        if (root == null) return;
        
        if (root.left != null) helper(root.left, res);
        if (root.right != null) helper(root.right, res);
        res.add(root.val);
    }
```
## Levelorder Traversal
```java
    public static List<Integer> levelTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if (root == null) return res;
        queue.add(root);
        while(!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            res.add(cur.val);
            if (cur.left != null) queue.add(cur.left);
            if (cur.right != null) queue.add(cur.right);
        }
        return res;
    }
```
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
> Binary tree "subproblem" recursion:
```
int maxDepth(TreeNode root) {...}
boolean isBalanced(TreeNode root) {...}
int sumTree(TreeNode root) {...}
```
- Function name describes "what this subproblem computes"
- The return value = subproblem's result
## Notes - Posyorder Position, subtree Problems & problem Decomposition
1. Key Idea :
   - When dealing with subtree-based problems, the natural approach is
     > Given the recursive function a return value and do the main work in the postorder position (after visiting children)
   - Because at that point you have access to the information returned by both left and right subtrees
## DP / Backtracking / DFS
> DP : focus on whole tree (decomposition problem / divide and conquer)
> Backtracking algorithm: focus on branches (paths/decision sequences)
> DFS: Focus on nodes (access traversal behavior)
```java
// DFS 算法把「做选择」「撤销选择」的逻辑放在 for 循环外面
void dfs(Node root) {
    if (root == null) return;
    // 做选择
    print("enter node %s", root);
    for (Node child : root.children) {
        dfs(child);
    }
    // 撤销选择
    print("leave node %s", root);
}

// 回溯算法把「做选择」「撤销选择」的逻辑放在 for 循环里面
void backtrack(Node root) {
    if (root == null) return;
    for (Node child : root.children) {
        // 做选择
        print("I'm on the branch from %s to %s", root, child);
        backtrack(child);
        // 撤销选择
        print("I'll leave the branch from %s to %s", child, root);
    }
}
```

## Quick Sort = preorder traversal & Merge Sort = postorder traversal
### Quick Sort
```java
quickSort(arr, lo, hi):
    if lo >= hi: return
    p = partition(arr, lo, hi)
    quickSort(arr, lo, p-1)
    quickSort(arr, p+1, hi)
```
1. Partition first everytime
2. recursive to left and right interval
### Merge Sort
```java
mergeSort(arr, lo, hi):
    if lo >= hi: return
    mid = (lo + hi) / 2
    mergeSort(arr, lo, mid)
    mergeSort(arr, mid+1, hi)
    merge(arr, lo, mid, hi)
```
