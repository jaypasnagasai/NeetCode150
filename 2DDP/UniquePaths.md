# 62. Unique Paths
Difficulty: Medium

## QUESTION

There is an `m x n` grid where you are allowed to move either down or to the right at any point in time.

Given the two integers `m` and `n`, return the number of possible unique paths that can be taken from the top-left corner of the grid (`grid[0][0]`) to the bottom-right corner (`grid[m - 1][n - 1]`).

You may assume the output will fit in a 32-bit integer.

### EXAMPLE

```
Input: m = 3, n = 6
Output: 21
```

```
Input: m = 3, n = 3
Output: 6
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        
        def dfs(i, j):
            # If we reach the bottom-right corner, count 1 valid path
            if i == (m - 1) and j == (n - 1):
                return 1
            # If out of bounds, return 0 (invalid path)
            if i >= m or j >= n:
                return 0
            # Recursive calls: move right and move down
            return dfs(i, j + 1) + dfs(i + 1, j)
        
        # Start DFS from the top-left corner (0,0)
        return dfs(0, 0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # Initialize a 1D DP array with all values set to 1
        # This represents the last row of the grid, where only one way exists (moving right)
        dp = [1] * n

        # Iterate from the second last row to the top row (bottom-up approach)
        for i in range(m - 2, -1, -1):
            # Iterate from the second last column to the leftmost column
            for j in range(n - 2, -1, -1):
                # Update DP value: dp[j] stores the number of paths from this cell
                # Sum of right cell (dp[j + 1]) and below cell (dp[j])
                dp[j] += dp[j + 1]
                
        # The first cell (top-left) contains the total number of unique paths
        return dp[0]
```

### APPROACH: MATH

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # If there's only one row or one column, there's only one unique path
        if m == 1 or n == 1:
            return 1
        
        # Ensure m is the larger value to minimize calculations (optimization)
        if m < n:
            m, n = n, m

        res = 1  # Stores the computed result
        j = 1  # Counter for division operations

        # Compute the binomial coefficient C(m + n - 2, n - 1) using iterative multiplication
        for i in range(m, m + n - 1):
            res *= i  # Multiply numerator
            res //= j  # Divide denominator to avoid overflow
            j += 1  # Increment denominator index

        return res  # Return the computed number of unique paths
```
