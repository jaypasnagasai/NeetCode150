# 55. Jump Game
Difficulty: Medium

## QUESTION

You are given an integer array `nums` where each element `nums[i]` indicates your maximum jump length at that position.

Return `true` if you can reach the last index starting from index `0`, or `false` otherwise.

### EXAMPLE

```
Input: nums = [1,2,0,1,0]
Output: true
```

```
Input: nums = [1,2,1,0,1]
Output: false
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        # Recursive DFS function to check if we can reach the last index
        def dfs(i):
            # Base case: If we've reached the last index, return True
            if i == len(nums) - 1:
                return True
            
            # Determine the farthest we can jump from index i
            end = min(len(nums) - 1, i + nums[i])

            # Try all possible jumps from i+1 to the maximum reachable index
            for j in range(i + 1, end + 1):
                if dfs(j):  # If any path reaches the last index, return True
                    return True
            
            # If no valid jump reaches the last index, return False
            return False

        # Start DFS from index 0
        return dfs(0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        n = len(nums)
        # Create a DP array where dp[i] indicates if index i can reach the last index
        dp = [False] * n
        dp[-1] = True  # The last index is always reachable from itself

        # Iterate backwards from second last index to the first index
        for i in range(n - 2, -1, -1):
            # Determine the furthest we can jump from index i
            end = min(n, i + nums[i] + 1)

            # Check if any of the reachable indices lead to the last index
            for j in range(i + 1, end):
                if dp[j]:  # If any reachable index can reach the end, mark dp[i] as True
                    dp[i] = True
                    break  # No need to check further

        # Return whether index 0 can reach the last index
        return dp[0]
```

### APPROACH: GREEDY

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        # Initialize the goal as the last index
        goal = len(nums) - 1

        # Traverse the array from right to left
        for i in range(len(nums) - 2, -1, -1):
            # If we can jump from index i to the goal or beyond, update the goal
            if i + nums[i] >= goal:
                goal = i

        # If the goal is reduced to 0, we can reach the last index
        return goal == 0
```
