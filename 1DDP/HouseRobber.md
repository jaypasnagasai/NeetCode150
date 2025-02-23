# 198. House Robber
Difficulty: Medium

## QUESTION

You are given an integer array `nums` where `nums[i]` represents the amount of money the `ith` house has. The houses are arranged in a straight line, i.e. the `ith` house is the neighbor of the `(i-1)th` and `(i+1)th` house.

You are planning to rob money from the houses, but you cannot rob two adjacent houses because the security system will automatically alert the police if two adjacent houses were both broken into.

Return the maximum amount of money you can rob without alerting the police.

### EXAMPLE

```
Input: nums = [1,1,3,3]
Output: 4
```

```
Input: nums = [2,9,8,3,6]
Output: 16
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        # Recursive DFS function to explore all robbing options
        def dfs(i):
            # Base case: If index is beyond the list, no money can be robbed
            if i >= len(nums):
                return 0
            # Choose the maximum between:
            # 1. Skipping the current house (dfs(i + 1))
            # 2. Robbing the current house and skipping the next (nums[i] + dfs(i + 2))
            return max(dfs(i + 1),
                       nums[i] + dfs(i + 2))

        # Start the DFS from the first house
        return dfs(0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        # Initialize two variables:
        # rob1 -> Max amount robbed excluding the previous house
        # rob2 -> Max amount robbed including the previous house
        rob1, rob2 = 0, 0

        # Iterate through each house in the list
        for num in nums:
            # Calculate the new max by deciding to rob current house or skip it
            temp = max(num + rob1, rob2)

            # Update rob1 and rob2 for the next iteration
            rob1 = rob2  # rob1 takes the previous rob2 value
            rob2 = temp  # rob2 holds the new max value

        # rob2 holds the final maximum amount robbed
        return rob2
```

