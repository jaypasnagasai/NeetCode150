# 312. Burst Balloons
Difficulty: Hard

## QUESTION

You are given an array of integers `nums` of size `n`. The `ith` element represents a balloon with an integer value of `nums[i]`. You must burst all of the balloons.

If you burst the `ith` balloon, you will receive `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then assume the out of bounds value is 1.

Return the maximum number of coins you can receive by bursting all of the balloons.

### EXAMPLE

```
Input: nums = [4,2,3,7]
Output: 167
Explanation:
nums = [4,2,3,7] --> [4,3,7] --> [4,7] --> [7] --> []
coins =  4*2*3    +   4*3*7   +  1*4*7  + 1*7*1 = 143
```

## SOLUTION


### APPROACH: RECURSION

```python
from typing import List

class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        # Add virtual boundary balloons with value 1 at the start and end
        nums = [1] + nums + [1]

        def dfs(nums):
            # Base case: If only boundary balloons remain, no coins can be collected
            if len(nums) == 2:
                return 0

            maxCoins = 0
            # Iterate over possible balloons to burst
            for i in range(1, len(nums) - 1):  
                # Calculate coins obtained by bursting nums[i]
                coins = nums[i - 1] * nums[i] * nums[i + 1]
                # Recursively compute coins for remaining balloons
                coins += dfs(nums[:i] + nums[i + 1:])
                # Track maximum coins obtained
                maxCoins = max(maxCoins, coins)
            return maxCoins

        return dfs(nums)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def maxCoins(self, nums):
        n = len(nums)
        # Add virtual boundary balloons with value 1 at the start and end
        new_nums = [1] + nums + [1]

        # Initialize DP table: dp[l][r] represents the max coins for bursting balloons in range [l, r]
        dp = [[0] * (n + 2) for _ in range(n + 2)]

        # Process subproblems from smaller ranges to larger ones (bottom-up approach)
        for l in range(n, 0, -1):  # Start from the last valid left boundary and move up
            for r in range(l, n + 1):  # Right boundary moves from left to the end
                for i in range(l, r + 1):  # Try bursting each balloon in range [l, r]
                    coins = new_nums[l - 1] * new_nums[i] * new_nums[r + 1]  # Coins from bursting nums[i]
                    coins += dp[l][i - 1] + dp[i + 1][r]  # Add coins from left and right partitions
                    dp[l][r] = max(dp[l][r], coins)  # Store the max coins possible for this range

        return dp[1][n]  # The result is stored in the full range (1 to n)
```

