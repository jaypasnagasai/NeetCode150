# 309. Best Time to Buy and Sell Stock with Cooldown
Difficulty: Medium

## QUESTION

You are given an integer array `prices` where `prices[i]` is the price of NeetCoin on the `ith` day.

You may buy and sell one NeetCoin multiple times with the following restrictions:

- After you sell your NeetCoin, you cannot buy another one on the next day (i.e., there is a cooldown period of one day).
- You may only own at most one NeetCoin at a time.

You may complete as many transactions as you like.

Return the maximum profit you can achieve.

### EXAMPLE

```
Input: prices = [1,3,4,0,4]
Output: 6
```

```
Input: prices = [1]
Output: 0
```

## SOLUTION


### APPROACH: RECURSION

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        
        def dfs(i, buying):
            # Base case: if we reach the end of the price list, return 0 (no profit)
            if i >= len(prices):
                return 0
            
            # Option 1: Cooldown (skip this day)
            cooldown = dfs(i + 1, buying)

            if buying:
                # Option 2 (when buying): Buy at current price and move to the next day
                buy = dfs(i + 1, not buying) - prices[i]
                return max(buy, cooldown)  # Choose the best option
            else:
                # Option 2 (when selling): Sell at current price, skip the next day due to cooldown
                sell = dfs(i + 2, not buying) + prices[i]
                return max(sell, cooldown)  # Choose the best option
        
        # Start DFS from day 0 with the option to buy
        return dfs(0, True)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        
        # Variables to store previous states (bottom-up DP)
        dp1_buy, dp1_sell = 0, 0  # dp1_buy: previous buy state, dp1_sell: previous sell state
        dp2_buy = 0  # dp2_buy: two days before buy state (to handle cooldown)

        # Iterate from the last day to the first day (reverse order)
        for i in range(n - 1, -1, -1):
            # Buying state: max of buying today (subtract price) or continuing the previous state
            dp_buy = max(dp1_sell - prices[i], dp1_buy)

            # Selling state: max of selling today (add price and apply cooldown) or continuing the previous state
            dp_sell = max(dp2_buy + prices[i], dp1_sell)

            # Shift values for the next iteration
            dp2_buy = dp1_buy
            dp1_buy, dp1_sell = dp_buy, dp_sell

        # The maximum profit with a buy option on day 0 is stored in dp1_buy
        return dp1_buy
```
