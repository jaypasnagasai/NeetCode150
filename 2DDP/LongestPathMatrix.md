# 329. Longest Increasing Path in a Matrix
Difficulty: Hard

## QUESTION

You are given a 2-D grid of integers `matrix`, where each integer is greater than or equal to `0`.

Return the length of the longest strictly increasing path within `matrix`.

From each cell within the path, you can move either horizontally or vertically. You may not move diagonally.

### EXAMPLE

```
Input: matrix = [[5,5,3],[2,3,6],[1,1,1]]
Output: 4
```

```
Input: matrix = [[1,2,3],[2,1,4],[7,6,5]]
Output: 7
```

## SOLUTION


### APPROACH: RECURSION

```python
from typing import List

class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        ROWS, COLS = len(matrix), len(matrix[0])  # Get matrix dimensions
        directions = [[-1, 0], [1, 0], [0, -1], [0, 1]]  # Possible movement directions (Up, Down, Left, Right)

        def dfs(r, c, prevVal):
            # Base case: Out of bounds or not increasing
            if (min(r, c) < 0 or r >= ROWS or 
                c >= COLS or matrix[r][c] <= prevVal):
                return 0
            
            res = 1  # Start length as 1 (current cell)
            for d in directions:  # Explore all four directions
                res = max(res, 1 + dfs(r + d[0], c + d[1], matrix[r][c]))  # Take max increasing path
            return res  # Return the longest path from this cell

        LIP = 0  # Longest Increasing Path tracker
        for r in range(ROWS):  # Iterate through all matrix cells
            for c in range(COLS):
                LIP = max(LIP, dfs(r, c, float('-inf')))  # Compute longest path from each cell

        return LIP  # Return the maximum length found
```

### APPROACH: DYNAMIC PROGRAMMING

```python
from typing import List

class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        ROWS, COLS = len(matrix), len(matrix[0])  # Get matrix dimensions
        dp = {}  # Memoization dictionary to store longest increasing path (LIP) from each cell

        def dfs(r, c, prevVal):
            # Base case: Out of bounds or not an increasing path
            if (r < 0 or r >= ROWS or c < 0 or 
                c >= COLS or matrix[r][c] <= prevVal):
                return 0
            
            # If the result is already computed for this cell, return it
            if (r, c) in dp:
                return dp[(r, c)]

            res = 1  # Initialize LIP length from current cell

            # Explore all four possible directions and take the maximum LIP
            res = max(res, 1 + dfs(r + 1, c, matrix[r][c]))  # Down
            res = max(res, 1 + dfs(r - 1, c, matrix[r][c]))  # Up
            res = max(res, 1 + dfs(r, c + 1, matrix[r][c]))  # Right
            res = max(res, 1 + dfs(r, c - 1, matrix[r][c]))  # Left
            
            # Store the result in the memoization dictionary
            dp[(r, c)] = res
            return res

        # Compute the longest increasing path from each cell
        for r in range(ROWS):
            for c in range(COLS):
                dfs(r, c, -1)
        
        # Return the maximum value from the memoized results
        return max(dp.values())
```

### APPROACH: TOPOLOGICAL SORT (KAHN'S ALGORITHM)

```python
from typing import List
from collections import deque

class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        ROWS, COLS = len(matrix), len(matrix[0])  # Get matrix dimensions
        directions = [[-1, 0], [1, 0], [0, -1], [0, 1]]  # Possible movement directions (Up, Down, Left, Right)
        
        # Step 1: Compute in-degree for each cell
        indegree = [[0] * COLS for _ in range(ROWS)]
        
        for r in range(ROWS):
            for c in range(COLS):
                for d in directions:
                    nr, nc = d[0] + r, d[1] + c
                    # If a neighbor cell has a smaller value, it points to the current cell
                    if (0 <= nr < ROWS and 0 <= nc < COLS and 
                        matrix[nr][nc] < matrix[r][c]):
                        indegree[r][c] += 1  # Increase in-degree for the current cell

        # Step 2: Initialize queue with cells that have in-degree 0 (smallest values in increasing paths)
        q = deque()
        for r in range(ROWS):
            for c in range(COLS):
                if indegree[r][c] == 0:
                    q.append([r, c])  # Push cells with no dependencies

        # Step 3: Perform Topological Sorting (BFS) to count the longest path layers
        LIS = 0  # Longest Increasing Path length
        while q:
            for _ in range(len(q)):  # Process all nodes at the current level
                r, c = q.popleft()
                for d in directions:  # Check all possible movements
                    nr, nc = r + d[0], c + d[1]
                    if (0 <= nr < ROWS and 0 <= nc < COLS and 
                        matrix[nr][nc] > matrix[r][c]):  # Ensure increasing path
                        indegree[nr][nc] -= 1  # Reduce in-degree of the neighbor
                        if indegree[nr][nc] == 0:  # If in-degree becomes 0, add to queue
                            q.append([nr, nc])
            LIS += 1  # Increment LIS count for each BFS level
        
        return LIS  # Return the longest increasing path length
```
