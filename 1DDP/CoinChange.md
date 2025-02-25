# 322. Coin Change
Difficulty: Medium

## QUESTION

You are given an integer array `coins` representing coins of different denominations (e.g. 1 dollar, 5 dollars, etc) and an integer `amount` representing a target amount of money.

Return the fewest number of coins that you need to make up the exact target amount. If it is impossible to make up the amount, return `-1`.

You may assume that you have an unlimited number of each coin.

### EXAMPLE

```
Input: coins = [1,5,10], amount = 12
Output: 3
```

```
Input: coins = [2], amount = 3
Output: -1
```

```
Input: coins = [1], amount = 0
Output: 0
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        # Recursive DFS function to find the minimum number of coins
        def dfs(amount):
            # Base case: if amount is zero, no more coins are needed
            if amount == 0:
                return 0

            # Initialize result with a large number (infinity)
            res = 1e9

            # Try each coin to reduce the amount
            for coin in coins:
                if amount - coin >= 0:
                    # Recursive call and add one coin to the count
                    res = min(res, 1 + dfs(amount - coin))

            # Return the minimum coins found for the current amount
            return res

        # Get the minimum coins needed for the given amount
        minCoins = dfs(amount)

        # If no valid solution found, return -1
        return -1 if minCoins >= 1e9 else minCoins
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        # Initialize the dp array where dp[i] represents the minimum coins needed for amount i
        # Set all values to amount + 1 (a value greater than the maximum possible coins needed)
        dp = [amount + 1] * (amount + 1)
        
        # Base case: 0 coins are needed to make amount 0
        dp[0] = 0

        # Iterate through all amounts from 1 to the target amount
        for a in range(1, amount + 1):
            # Iterate through all coin denominations
            for c in coins:
                # If the coin can be used (doesn't make the amount negative)
                if a - c >= 0:
                    # Update dp[a] with the minimum coins needed
                    dp[a] = min(dp[a], 1 + dp[a - c])

        # If dp[amount] is still the initialized value, no valid combination exists
        return dp[amount] if dp[amount] != amount + 1 else -1
```
