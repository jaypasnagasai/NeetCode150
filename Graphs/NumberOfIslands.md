# 200. Number of Islands
Difficulty: Medium

## QUESTION

Given a 2D grid `grid` where `'1'` represents land and `'0'` represents water, count and return the number of islands.

An island is formed by connecting adjacent lands horizontally or vertically and is surrounded by water. You may assume water is surrounding the grid (i.e., all the edges are water).

### EXAMPLE

```
Input: grid = [
    ["0","1","1","1","0"],
    ["0","1","0","1","0"],
    ["1","1","0","0","0"],
    ["0","0","0","0","0"]
  ]
Output: 1
```

```
Input: grid = [
    ["1","1","0","0","1"],
    ["1","1","0","0","1"],
    ["0","0","1","0","0"],
    ["0","0","0","1","1"]
  ]
Output: 4
```

## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        # Possible moves in four directions
        directions = [[1, 0], [-1, 0], [0, 1], [0, -1]]
        # Number of rows and columns
        ROWS, COLS = len(grid), len(grid[0])
        # Count of islands
        islands = 0

        def dfs(r, c):
            # Return if out of bounds or cell is water
            if (r < 0 or c < 0 or r >= ROWS or 
                c >= COLS or grid[r][c] == "0"):
                return

            # Mark the current cell as visited
            grid[r][c] = "0"
            # Explore all four directions
            for dr, dc in directions:
                dfs(r + dr, c + dc)

        # Traverse the grid
        for r in range(ROWS):
            for c in range(COLS):
                # If land is found, initiate DFS and increment island count
                if grid[r][c] == "1":
                    dfs(r, c)
                    islands += 1

        return islands
```

### APPROACH: BREADTH FIRST SEARCH

```python
from collections import deque
from typing import List

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        # Directions for moving up, down, left, and right
        directions = [[1, 0], [-1, 0], [0, 1], [0, -1]]
        # Dimensions of the grid
        ROWS, COLS = len(grid), len(grid[0])
        # Counter for islands
        islands = 0

        def bfs(r, c):
            # Initialize a queue for BFS
            q = deque()
            # Mark the starting cell as visited
            grid[r][c] = "0"
            q.append((r, c))

            # Perform BFS
            while q:
                row, col = q.popleft()
                # Explore all four directions
                for dr, dc in directions:
                    nr, nc = row + dr, col + dc
                    # Check boundaries and if it's land
                    if (nr < 0 or nc < 0 or nr >= ROWS or
                        nc >= COLS or grid[nr][nc] == "0"):
                        continue
                    # Mark as visited and add to queue
                    grid[nr][nc] = "0"
                    q.append((nr, nc))

        # Traverse every cell in the grid
        for r in range(ROWS):
            for c in range(COLS):
                # If it's land, initiate BFS and increment island count
                if grid[r][c] == "1":
                    bfs(r, c)
                    islands += 1
        
        return islands
```

### APPROACH: DISJOINT SET UNION

```python
from typing import List

class DSU:
    def __init__(self, n):
        # Initialize parent and size arrays
        self.Parent = list(range(n + 1))
        self.Size = [1] * (n + 1)

    def find(self, node):
        # Path compression
        if self.Parent[node] != node:
            self.Parent[node] = self.find(self.Parent[node])
        return self.Parent[node]

    def union(self, u, v):
        # Union by size
        pu = self.find(u)
        pv = self.find(v)
        if pu == pv:
            return False
        if self.Size[pu] >= self.Size[pv]:
            self.Size[pu] += self.Size[pv]
            self.Parent[pv] = pu
        else:
            self.Size[pv] += self.Size[pu]
            self.Parent[pu] = pv
        return True

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        # Get the number of rows and columns
        ROWS, COLS = len(grid), len(grid[0])
        # Create a DSU for all cells
        dsu = DSU(ROWS * COLS)

        # Convert 2D coordinates to 1D index
        def index(r, c):
            return r * COLS + c

        # Possible directions
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        # Count of islands
        islands = 0

        # Traverse the grid
        for r in range(ROWS):
            for c in range(COLS):
                if grid[r][c] == '1':
                    islands += 1
                    # Union adjacent land cells
                    for dr, dc in directions:
                        nr, nc = r + dr, c + dc
                        if (nr < 0 or nc < 0 or nr >= ROWS or
                            nc >= COLS or grid[nr][nc] == "0"):
                            continue
                        if dsu.union(index(r, c), index(nr, nc)):
                            islands -= 1

        return islands
```
