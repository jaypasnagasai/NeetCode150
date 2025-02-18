# 695. Max Area of Island
Difficulty: Medium

## QUESTION

You are given a matrix `grid` where `grid[i]` is either a `0` (representing water) or `1` (representing land).

An island is defined as a group of `1`'s connected horizontally or vertically. You may assume all four edges of the grid are surrounded by water.

The area of an island is defined as the number of cells within the island.

Return the maximum area of an island in `grid`. If no island exists, return `0`.

### EXAMPLE

```
Input: grid = [
  [0,1,1,0,1],
  [1,0,1,0,1],
  [0,1,1,0,1],
  [0,1,0,0,1]
]
Output: 6
```

## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        # Get the dimensions of the grid
        ROWS, COLS = len(grid), len(grid[0])
        # Keep track of visited cells to avoid revisiting
        visit = set()

        # Depth-first search to calculate area of an island
        def dfs(r, c):
            # Return 0 if out of bounds, water, or already visited
            if (r < 0 or r == ROWS or c < 0 or
                c == COLS or grid[r][c] == 0 or
                (r, c) in visit):
                return 0
            # Mark current cell as visited
            visit.add((r, c))
            # Area = 1 (current land) + area of neighbors
            return (1 
                    + dfs(r + 1, c) 
                    + dfs(r - 1, c) 
                    + dfs(r, c + 1) 
                    + dfs(r, c - 1))

        # Track the maximum area of any island
        area = 0
        # Explore each cell; update maximum area found
        for r in range(ROWS):
            for c in range(COLS):
                area = max(area, dfs(r, c))
        return area
```

### APPROACH: BREADTH FIRST SEARCH

```python
from collections import deque

class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        # Possible movements in four directions
        directions = [[1, 0], [-1, 0], [0, 1], [0, -1]]
        # Get rows and columns count
        ROWS, COLS = len(grid), len(grid[0])
        # Keep track of maximum island area
        area = 0

        # Breadth-first search to compute area of the connected component
        def bfs(r, c):
            # Initialize queue and mark the starting cell as visited
            q = deque()
            grid[r][c] = 0
            q.append((r, c))
            # Start with area = 1 for the current land cell
            res = 1

            # Process the queue
            while q:
                row, col = q.popleft()
                # Explore neighbors in all four directions
                for dr, dc in directions:
                    nr, nc = row + dr, col + dc
                    # Skip invalid or water cells
                    if (nr < 0 or nc < 0 or nr >= ROWS or
                        nc >= COLS or grid[nr][nc] == 0):
                        continue
                    # Mark as visited and increment area count
                    q.append((nr, nc))
                    grid[nr][nc] = 0
                    res += 1
            # Return area of this island
            return res

        # Check each cell in the grid
        for r in range(ROWS):
            for c in range(COLS):
                # If it's land, perform BFS and update maximum area
                if grid[r][c] == 1:
                    area = max(area, bfs(r, c))

        return area
```

### APPROACH: DISJOINT SET UNION

```python
from typing import List

class DSU:
    def __init__(self, n):
        # Parent array for Union-Find
        self.Parent = list(range(n + 1))
        # Size array to track component sizes
        self.Size = [1] * (n + 1)

    def find(self, node):
        # Path compression for efficient find operation
        if self.Parent[node] != node:
            self.Parent[node] = self.find(self.Parent[node])
        return self.Parent[node]

    def union(self, u, v):
        # Find the root of both nodes
        pu = self.find(u)
        pv = self.find(v)
        # If already in the same component, return False
        if pu == pv:
            return False
        # Union by size: attach smaller tree to larger tree
        if self.Size[pu] >= self.Size[pv]:
            self.Size[pu] += self.Size[pv]
            self.Parent[pv] = pu
        else:
            self.Size[pv] += self.Size[pu]
            self.Parent[pu] = pv
        return True
    
    def getSize(self, node):
        # Find the root and return the size of the component
        par = self.find(node)
        return self.Size[par]

class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        # Get grid dimensions
        ROWS, COLS = len(grid), len(grid[0])
        # Create a DSU for all grid cells
        dsu = DSU(ROWS * COLS)

        # Convert 2D coordinates to 1D index
        def index(r, c):
            return r * COLS + c

        # Possible movement directions
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        # Variable to track the maximum area
        area = 0

        # Traverse the grid
        for r in range(ROWS):
            for c in range(COLS):
                # If the cell is land, process it
                if grid[r][c] == 1:
                    # Check neighboring cells and union if they are land
                    for dr, dc in directions:
                        nr, nc = r + dr, c + dc
                        if (nr < 0 or nc < 0 or nr >= ROWS or
                            nc >= COLS or grid[nr][nc] == 0):
                            continue
                        dsu.union(index(r, c), index(nr, nc))

                    # Update the maximum island area found
                    area = max(area, dsu.getSize(index(r, c)))

        return area
```
