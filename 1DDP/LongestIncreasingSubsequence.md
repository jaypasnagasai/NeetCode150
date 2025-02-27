# 300. Longest Increasing Subsequence
Difficulty: Medium

## QUESTION

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from the given sequence by deleting some or no elements without changing the relative order of the remaining characters.

For example, `"cat"` is a subsequence of `"crabt"`.

### EXAMPLE

```
Input: nums = [9,1,4,2,3,3,7]
Output: 4
```

```
Input: nums = [9,1,4,2,3,3,7]
Output: 4
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        # Recursive DFS function to find the length of the longest increasing subsequence
        def dfs(i, j):
            # Base case: If index reaches the end, return 0
            if i == len(nums):
                return 0

            # Case 1: Not including the current element
            LIS = dfs(i + 1, j)

            # Case 2: Include the current element if it's greater than the previous element in LIS
            if j == -1 or nums[j] < nums[i]:
                LIS = max(LIS, 1 + dfs(i + 1, i))

            return LIS

        # Start DFS from index 0 with an initial invalid index (-1) for comparison
        return dfs(0, -1)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def lengthOfLIS(self, nums):
        # Get the length of the input list
        n = len(nums)

        # Initialize a memoization table with -1
        # memo[i][j+1] stores the result of dfs(i, j)
        memo = [[-1] * (n + 1) for _ in range(n)]  

        # Recursive DFS function with memoization
        def dfs(i, j):
            # Base case: If index reaches the end, return 0
            if i == n:
                return 0
            
            # If result is already computed, return it
            if memo[i][j + 1] != -1:
                return memo[i][j + 1]

            # Case 1: Not including the current element
            LIS = dfs(i + 1, j)

            # Case 2: Include the current element if it's greater than the previous one
            if j == -1 or nums[j] < nums[i]:
                LIS = max(LIS, 1 + dfs(i + 1, i))

            # Store result in memoization table
            memo[i][j + 1] = LIS
            return LIS

        # Start DFS from index 0 with an initial invalid index (-1) for comparison
        return dfs(0, -1)
```

### APPROACH: BINARY SEARCH

```python
from bisect import bisect_left

class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        # Initialize the dp list, which will store the smallest ending elements of increasing subsequences
        dp = []
        dp.append(nums[0])  # Start with the first element
        
        # Variable to track the length of the Longest Increasing Subsequence (LIS)
        LIS = 1

        # Iterate through the nums array
        for i in range(1, len(nums)):
            if dp[-1] < nums[i]:  # If current number extends the LIS
                dp.append(nums[i])  
                LIS += 1  # Increase LIS length
                continue

            # If nums[i] is smaller, find its position using binary search
            idx = bisect_left(dp, nums[i])
            dp[idx] = nums[i]  # Replace the element at found position

        # Return the length of the longest increasing subsequence
        return LIS
```
