# 45. Jump Game II
Difficulty: Medium

## QUESTION

You are given an array of integers `nums`, where `nums[i]` represents the maximum length of a jump towards the right from index `i`. For example, if you are at `nums[i]`, you can jump to any index `i + j` where:

- `j <= nums[i]`
- `i + j < nums.length`

You are initially positioned at `nums[0]`.

Return the minimum number of jumps to reach the last position in the array (index `nums.length - 1`). You may assume there is always a valid answer.

### EXAMPLE

```
Input: nums = [2,4,1,1,1,1]
Output: 2
```

```
Input: nums = [2,1,2,1,0]
Output: 2
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        # Recursive DFS function to find the minimum jumps required
        def dfs(i):
            # Base case: If we reach the last index, no more jumps are needed
            if i == len(nums) - 1:
                return 0
            # If we can't move forward, return infinity (impossible path)
            if nums[i] == 0:
                return float('inf')

            # Determine the farthest we can jump from index i
            end = min(len(nums) - 1, i + nums[i])

            # Initialize the minimum jumps needed
            res = float('inf')

            # Try all possible jumps and choose the minimum
            for j in range(i + 1, end + 1):
                res = min(res, 1 + dfs(j))

            return res

        # Start DFS from index 0
        return dfs(0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        n = len(nums)
        # Initialize the DP array with a large value (representing infinite jumps)
        dp = [1000000] * n
        dp[-1] = 0  # No jumps needed from the last index

        # Iterate backwards through the array
        for i in range(n - 2, -1, -1):
            # Determine the farthest we can jump from index i
            end = min(n, i + nums[i] + 1)

            # Check all reachable positions and find the minimum jumps required
            for j in range(i + 1, end):
                dp[i] = min(dp[i], 1 + dp[j])

        # Return the minimum jumps needed from the start index
        return dp[0]
```

### APPROACH: GREEDY

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        # Initialize variables
        res = 0  # Count of jumps
        l = r = 0  # Left and right boundaries of the current jump range

        # Continue jumping until we reach or exceed the last index
        while r < len(nums) - 1:
            farthest = 0  # Track the farthest index reachable in the next jump

            # Iterate over the current jump range and find the farthest jump
            for i in range(l, r + 1):
                farthest = max(farthest, i + nums[i])

            # Move to the next jump range
            l = r + 1
            r = farthest
            res += 1  # Increment jump count

        return res  # Return the minimum jumps needed to reach the last index
```
