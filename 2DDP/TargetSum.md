# 494. Target Sum
Difficulty: Medium

## QUESTION

You are given an array of integers `nums` and an integer `target`.

For each number in the array, you can choose to either add or subtract it to a total sum.

- For example, if `nums = [1, 2]`, one possible sum would be `"+1-2=-1"`.

If `nums=[1,1]`, there are two different ways to sum the input numbers to get a sum of `0`: `"+1-1"` and `"-1+1"`.

Return the number of different ways that you can build the expression such that the total sum equals `target`.

### EXAMPLE

```
Input: nums = [2,2,2], target = 2
Output: 3
```

## SOLUTION


### APPROACH: RECURSION

```python
from typing import List

class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        
        def backtrack(i, total):
            # Base case: if we reach the end of the array, check if sum equals target
            if i == len(nums):
                return 1 if total == target else 0
            
            # Recursive case: explore both adding and subtracting current number
            return (backtrack(i + 1, total + nums[i]) +  # Choose '+'
                    backtrack(i + 1, total - nums[i]))   # Choose '-'
                
        # Start the backtracking from index 0 with an initial sum of 0
        return backtrack(0, 0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
from typing import List
from collections import defaultdict

class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        # Dictionary to store the number of ways to reach a particular sum
        dp = defaultdict(int)
        dp[0] = 1  # Base case: one way to make sum 0 (by doing nothing)

        # Iterate through each number in the list
        for num in nums:
            next_dp = defaultdict(int)  # New dictionary for the next state
            
            # Update possible sums based on previous states
            for total, count in dp.items():
                next_dp[total + num] += count  # Adding num to the sum
                next_dp[total - num] += count  # Subtracting num from the sum
            
            dp = next_dp  # Move to the next state
        
        # Return the number of ways to reach the target sum
        return dp[target]
```
