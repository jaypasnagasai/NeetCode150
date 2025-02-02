# 121. Best Time to Buy and Sell Stock
Difficulty: Easy

## QUESTION

You are given an integer array `prices` where `prices[i]` is the price of NeetCoin on the `ith` day.

You may choose a single day to buy one NeetCoin and choose a different day in the future to sell it.

Return the maximum profit you can achieve. You may choose to not make any transactions, in which case the profit would be `0`.

### EXAMPLE

```
Input: prices = [10,1,5,6,7,1]
Output: 6
```

```
Input: prices = [10,8,7,5,2]
Output: 0
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # Initialize the maximum profit to 0
        res = 0

        # Iterate through each possible buying day
        for i in range(len(prices)):
            buy = prices[i]  # Set the buying price

            # Iterate through each possible selling day after the buying day
            for j in range(i + 1, len(prices)):
                sell = prices[j]  # Set the selling price
                res = max(res, sell - buy)  # Update max profit

        return res  # Return the maximum profit found
```

### APPROACH: TWO POINTER

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # Two-pointer approach: 'l' represents the buying day, 'r' represents the selling day
        l, r = 0, 1  # Initialize left (buying) and right (selling) pointers
        maxP = 0  # Variable to store the maximum profit

        # Iterate through the price list while 'r' stays within bounds
        while r < len(prices):
            if prices[l] < prices[r]:  # If we can make a profit (buy low, sell high)
                profit = prices[r] - prices[l]  # Calculate profit
                maxP = max(maxP, profit)  # Update maximum profit if it's higher
            else:
                l = r  # Move the left pointer to the right, as we found a new minimum price
            r += 1  # Always move the right pointer to explore new selling opportunities
        
        return maxP  # Return the maximum profit found
```

### APPROACH: DYNAMIC PROGRAMMING

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # Initialize max profit as 0
        maxP = 0
        
        # Track the minimum buying price seen so far
        minBuy = prices[0]

        # Iterate through the prices to find the maximum profit
        for sell in prices:
            maxP = max(maxP, sell - minBuy)  # Calculate profit and update maxP
            minBuy = min(minBuy, sell)  # Update minBuy if a lower price is found
        
        return maxP  # Return the maximum profit
```
