# What is Dynamic Programming?
Dynamic Programming is a tecnique for solving problems that ask for some kind of optimal value (min / max / best)  
**Examples:**
- longest increasing subsequence
- minimum edit distance
- minimum coins to make a given amount
**Core Idea**
DP = smart exhaustive search
Instead of blindly exploring all possibles,
we use structure in the problem to reuse results of subproblems
**Key concepts**
1. Overlapping subproblems
2. optimal substructure
3. state & choices
4. recurrence relation / state transition
5. top-down with memorization vs. bottom-up DP table
---

## Example 1 : Fibonacci
What is Fibonacci

```
       0      n=0 // base case
f(n) = 1      n=1 // base case
       f(n - 1) + f(n - 2) n>1 // state transition equation
```
### Brute-force recursion (exponential)
```
/**
        5
       / \
      4   3
     / \ / \
    3  2 2  1
   / \ ......
  2  1 ......
*/
class Main {
    
    private static int recursion(int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        return recursion(n - 1) + recursion(n - 2);
    }
    public static void main(String[] args) {
        int res = recursion(6);
        System.out.println(res);
    }
}
```
**Time Complexity**
O(2^n) for every layer
Sum of the first n terms
Sn​=a+ar+ar2+⋯+arn−1

**Space Complexity**
O(n) - recursive stack space

### Recursive + memo (cut edge)
```
    private static int recursionMemo(int n) {
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1);
        helper(n, memo);
        return memo[n];
    }
    
    private static int helper(int n, int[] memo) {
        if (n == 0 || n == 1) {return n;}
        if (memo[n] != -1) return memo[n];
        
        // not visited
        memo[n] = helper(n - 1, memo) + helper(n - 2, memo);
        return memo[n];
    }
```
**Time Complexity**
O(n) 

**Space Complexity**
O(n) - recursive stack space

> Up for now, this two solutions all from top to bottom
### DP Table (from bottom to top) 
```java
    /**
        DP Table
    */
    private static int dp(int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
    public static void main(String[] args) {
        int res = dp(6);
        System.out.println(res);
    }
```
**Time Complexity**
O(n) 

**Space Complexity**
O(n) - dp array

### DP (compressing DP table to O(1))
```
    private static int dp(int n) {
        if (n == 0) return 0;
        if (n == 1) return 1;
        int dp0 = 0, dp1 = 1;
        for (int i = 2; i <= n; i++) {
            int dp = dp0 + dp1;
            dp0 = dp1;
            dp1 = dp;
        }
        return dp1;
    }
    public static void main(String[] args) {
        int res = dp(6);
        System.out.println(res);
    }
```
**Time Complexity**
O(n) 

**Space Complexity**
O(1)
