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
---
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
---

## Essence of Algorithm & Recursion
1. Essence of algorithm: in the end, most algorithm are a form of exhaustive search
2. Recursion is powerful way to organize that search
3. The most effective way to understand recursion : think it a as operating on a tree
### Two mindsets for writing recursive algorithms
1. Decompose the problem ("problem decomposition" / "Divide and conquer" / "DP style")
2. Traverse the tree ("traversal" / DFS / backtracking style)
Almost every recursive solution you see is one of these two
### Example - Binary Tree Max Depth (Leetcod3e 104)
#### Decomposition
```java

// Online Java Compiler
// Use this editor to write, compile and run your Java code online

class Main {
    static class TreeNode{
        int val;
        TreeNode left;
        TreeNode right;
        public TreeNode (int val, TreeNode left, TreeNode right) {
            this.val = val;
            this.left = left;
            this.right = right;
        }
    }
    
    private static int maxDepth(TreeNode root) {
        if (root == null) return 0;
        int depth = decomp(root);
        return depth;
    }
    
    private static int decomp(TreeNode root) {
        if (root == null) return 0;
        int leftDecomp = decomp(root.left);
        int rightDecomp = decomp(root.right);
        return Math.max(leftDecomp, rightDecomp) + 1;
    }
    public static void main(String[] args) {
        TreeNode root4 = new TreeNode(4, null, null);
        TreeNode root7 = new TreeNode(7, null, null);
        TreeNode root6 = new TreeNode(6, null, null);
        TreeNode root5 = new TreeNode(5, null, root7);
        TreeNode root2 = new TreeNode(2, root4, null);
        TreeNode root3 = new TreeNode(3, root5, root6);
        TreeNode root1 = new TreeNode(1, root2, root3);
        
        int depth = maxDepth(root1);
        System.out.println(depth);
        
    }
}
```
#### Traversal
```java
static int res = 0;
private static void traverse(TreeNode root, int depth) {
        if (root == null) {
            res = Math.max(depth, res);
            return;
        }
        traverse(root.left, depth + 1);
        traverse(root.right, depth + 1);
}
```
---
## Practical checklist for recursion
1. Tree?
   - real tree or an implicit "decision tree"
2. Which mindset?
   - Do I need to return a value from subproblems? -> decomp
   - Do I just need a search / enumerate / collect? -> tracersal
3. If decomposition
   - clearly define the function: input & output
   - Use the definition to express subproblems
   - Combine subproblem results
4. If traversal:
   - write a void recursive function
   - Maintain global / external state (like res, poath, depth)
   - Decide at which position (pre / in / post / leaf) to update results
