# Leetcode 322 Coin Change
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

---
## Key Observation
It's easy to think about, if we choose more large at first, can easy to reach the minimal piece goal.  
But this problem cannot be solved using greedy strategy because it does not satisfy the greedy-choice property
> A locally optimal choice does not always lead to a globally optimal solution
> For example : n = 8, coins =[1,4,5] greedy : [5,1,1,1] but actually : [4, 4]
So this question still need smart exhausted iteration.
---

## Code
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount == 0) return 0;
        if (coins.length == 0) return 0;
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        for (int i = 0; i <= amount; i++) {
            for (int coin : coins) {
                // if current coint over current amount, we will abandon this coin 
                if (i - coin < 0) continue;
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```
**Time Complexity:**
O(n)
**Spcae Complexity**
O(n)
