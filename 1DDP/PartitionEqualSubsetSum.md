# 416. Partition Equal Subset Sum
Difficulty: Medium

## QUESTION

You are given an array of positive integers `nums`.

Return `true` if you can partition the array into two subsets, `subset1` and `subset2` where `sum(subset1) == sum(subset2)`. Otherwise, return `false`.

### EXAMPLE

```
Input: nums = [1,2,3,4]
Output: true
```

```
Input: nums = [1,2,3,4,5]
Output: false
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        # If the sum of elements is odd, it's impossible to partition into equal subsets
        if sum(nums) % 2:
            return False
        
        # Recursive DFS function to check if a subset with the target sum exists
        def dfs(i, target):
            # If we've considered all elements, check if target sum is achieved
            if i >= len(nums):
                return target == 0
            
            # If the target becomes negative, the subset is invalid
            if target < 0:
                return False

            # Either exclude or include the current element
            return dfs(i + 1, target) or dfs(i + 1, target - nums[i])
        
        # Start DFS with the target being half of the total sum
        return dfs(0, sum(nums) // 2)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def canPartition(self, nums: list[int]) -> bool:
        # Calculate the total sum of the array
        total = sum(nums)
        
        # If total is odd, partitioning into two equal subsets is impossible
        if total % 2 != 0:
            return False

        # Target sum for each subset
        target = total // 2
        
        # Bitmask DP approach: Initialize a bitset where only the 0th bit is set
        dp = 1 << 0
        
        # Process each number in the list
        for num in nums:
            # Update bitmask by shifting bits to include the current num
            dp |= dp << num
            
        # Check if the target sum is achievable
        return (dp & (1 << target)) != 0
```
