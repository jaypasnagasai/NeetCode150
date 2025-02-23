# 213. House Robber II
Difficulty: Medium

## QUESTION

You are given an integer array `nums` where `nums[i]` represents the amount of money the `ith` house has. The houses are arranged in a circle, i.e. the first house and the last house are neighbors.

You are planning to rob money from the houses, but you cannot rob two adjacent houses because the security system will automatically alert the police if two adjacent houses were both broken into.

Return the maximum amount of money you can rob without alerting the police.

### EXAMPLE

```
Input: nums = [3,4,3]
Output: 4
```

```
Input: nums = [2,9,8,3,6]
Output: 15
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        # If there's only one house, return its value
        if len(nums) == 1:
            return nums[0]

        # Recursive DFS function to explore robbing options
        # 'i' is the current house index
        # 'flag' indicates if the first house was robbed (to handle circular constraint)
        def dfs(i, flag):
            # Base case: If index exceeds bounds or last house is blocked due to first being robbed
            if i >= len(nums) or (flag and i == len(nums) - 1):
                return 0

            # Choose the maximum between:
            # 1. Skipping the current house
            # 2. Robbing the current house and adding its value
            return max(dfs(i + 1, flag), 
                       nums[i] + dfs(i + 2, flag or i == 0))

        # Run two scenarios:
        # 1. Start robbing from the first house (setting flag to True)
        # 2. Start robbing from the second house (avoiding the circular constraint)
        return max(dfs(0, True), dfs(1, False))
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    
    def rob(self, nums: List[int]) -> int:
        # Handle case when there's only one house
        if len(nums) == 1:
            return nums[0]
        
        # Since houses are in a circle, we can't rob both first and last house
        # Calculate max by either:
        # 1. Excluding the first house
        # 2. Excluding the last house
        # 3. Robbing only the first house if it's the maximum
        return max(nums[0], self.helper(nums[1:]), 
                            self.helper(nums[:-1]))

    def helper(self, nums):
        # Initialize two variables:
        # rob1 -> Max amount robbed excluding the previous house
        # rob2 -> Max amount robbed including the previous house
        rob1, rob2 = 0, 0

        # Iterate through the list of houses
        for num in nums:
            # Calculate the new maximum by deciding to rob current house or skip it
            newRob = max(rob1 + num, rob2)

            # Update rob1 and rob2 for the next iteration
            rob1 = rob2  # rob1 takes the previous rob2 value
            rob2 = newRob  # rob2 holds the new max value

        # rob2 holds the final maximum amount robbed
        return rob2
```
