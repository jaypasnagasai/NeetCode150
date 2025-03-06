# 518. Coin Change II
Difficulty: Medium

## QUESTION

You are given an integer array `coins` representing coins of different denominations (e.g. 1 dollar, 5 dollars, etc) and an integer `amount` representing a target amount of money.

Return the number of distinct combinations that total up to `amount`. If it's impossible to make up the amount, return `0`.

You may assume that you have an unlimited number of each coin and that each value in `coins` is unique.

### EXAMPLE

```
Input: amount = 4, coins = [1,2,3]
Output: 4
```

```
Input: amount = 7, coins = [2,4]
Output: 0
```

## SOLUTION


### APPROACH: RECURSION

```python
from typing import List

class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        # Sort coins to ensure a consistent order of processing
        coins.sort()

        def dfs(i, a):
            # If the amount becomes 0, we found a valid combination
            if a == 0:
                return 1
            # If we have exhausted all coins, return 0 (no valid way)
            if i >= len(coins):
                return 0

            res = 0
            if a >= coins[i]:  # Only consider coins that do not exceed the amount
                # Option 1: Skip the current coin and move to the next
                res = dfs(i + 1, a)
                # Option 2: Use the current coin and reduce the remaining amount
                res += dfs(i, a - coins[i])
            return res

        return dfs(0, amount)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
from typing import List

class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        # Initialize a DP array where dp[a] represents the number of ways to make amount 'a'
        dp = [0] * (amount + 1)
        dp[0] = 1  # Base case: There is one way to make amount 0 (by using no coins)

        # Iterate through the coins (using each coin type one by one)
        for i in range(len(coins) - 1, -1, -1):
            for a in range(1, amount + 1):
                # If the coin value is <= current amount, add the ways to make (a - coin value)
                dp[a] += dp[a - coins[i]] if coins[i] <= a else 0
        
        # Return the number of ways to make 'amount' using given coins
        return dp[amount]
```
