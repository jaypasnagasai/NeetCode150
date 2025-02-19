# 286. Walls And Gates
Difficulty: Medium

## QUESTION

You are given a m×n 2D `grid` initialized with these three possible values:

1. `-1` - A water cell that can not be traversed.
2. `0` - A treasure chest.
3. `INF` - A land cell that can be traversed. We use the integer `2^31 - 1 = 2147483647` to represent `INF`.

Fill each land cell with the distance to its nearest treasure chest. If a land cell cannot reach a treasure chest than the value should remain `INF`.

Assume the grid can only be traversed up, down, left, or right.

Modify the `grid` in-place.

### EXAMPLE

```
Input: [
  [2147483647,-1,0,2147483647],
  [2147483647,2147483647,2147483647,-1],
  [2147483647,-1,2147483647,-1],
  [0,-1,2147483647,2147483647]
]

Output: [
  [3,-1,0,1],
  [2,2,1,-1],
  [1,-1,2,-1],
  [0,-1,3,4]
]
```

```
Input: [
  [0,-1],
  [2147483647,2147483647]
]

Output: [
  [0,-1],
  [1,2]
]
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
from typing import List

class Solution:
    def islandsAndTreasure(self, grid: List[List[int]]) -> None:
        # Get grid dimensions
        ROWS, COLS = len(grid), len(grid[0])
        # Define possible movement directions (up, down, left, right)
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        # Define INF as the placeholder for empty cells
        INF = 2147483647
        # Create a visited matrix to avoid revisiting cells during DFS
        visit = [[False for _ in range(COLS)] for _ in range(ROWS)]

        # Depth-First Search function to find the shortest distance to a treasure
        def dfs(r, c):
            # Base cases: out of bounds, wall (-1), or already visited cell
            if (r < 0 or c < 0 or r >= ROWS or
                c >= COLS or grid[r][c] == -1 or
                visit[r][c]):
                return INF

            # If the current cell is a treasure (0), return distance 0
            if grid[r][c] == 0:
                return 0

            # Mark the current cell as visited
            visit[r][c] = True
            # Initialize the result with INF
            res = INF

            # Explore all four directions
            for dx, dy in directions:
                res = min(res, 1 + dfs(r + dx, c + dy))

            # Backtrack: unmark the current cell as visited
            visit[r][c] = False
            return res

        # Iterate over all cells in the grid
        for r in range(ROWS):
            for c in range(COLS):
                # If the cell is an empty room (INF), perform DFS to find the shortest path
                if grid[r][c] == INF:
                    grid[r][c] = dfs(r, c)
```

### APPROACH: BREADTH FIRST SEARCH

```python
from collections import deque
from typing import List

class Solution:
    def islandsAndTreasure(self, grid: List[List[int]]) -> None:
        # Get grid dimensions
        ROWS, COLS = len(grid), len(grid[0])
        # Define possible movement directions (up, down, left, right)
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        # Define INF as the placeholder for empty cells
        INF = 2147483647

        # Breadth-First Search function to find the shortest distance to a treasure
        def bfs(r, c):
            # Initialize queue with starting cell
            q = deque([(r, c)])
            # Track visited cells to avoid revisits
            visit = [[False] * COLS for _ in range(ROWS)]
            visit[r][c] = True
            # Initialize steps to track distance from the starting cell
            steps = 0

            # Perform BFS
            while q:
                for _ in range(len(q)):
                    row, col = q.popleft()
                    # If we reach a treasure (0), return the distance
                    if grid[row][col] == 0:
                        return steps
                    # Explore neighboring cells
                    for dr, dc in directions:
                        nr, nc = row + dr, col + dc
                        # Check boundaries, unvisited cells, and avoid walls (-1)
                        if (0 <= nr < ROWS and 0 <= nc < COLS and 
                            not visit[nr][nc] and grid[nr][nc] != -1):
                            visit[nr][nc] = True
                            q.append((nr, nc))
                # Increment steps after exploring one level
                steps += 1

            # Return INF if no path to treasure exists
            return INF

        # Iterate over all cells in the grid
        for r in range(ROWS):
            for c in range(COLS):
                # If the cell is an empty room (INF), perform BFS to find the shortest path
                if grid[r][c] == INF:
                    grid[r][c] = bfs(r, c)
```

### APPROACH: MULTI SOURCE BFS

```python
from collections import deque
from typing import List

class Solution:
    def islandsAndTreasure(self, grid: List[List[int]]) -> None:
        # Get grid dimensions
        ROWS, COLS = len(grid), len(grid[0])
        # Set to track visited cells
        visit = set()
        # Queue for BFS traversal
        q = deque()

        # Function to add valid cells to the queue
        def addCell(r, c):
            # Check boundaries, avoid walls (-1) and visited cells
            if (min(r, c) < 0 or r == ROWS or c == COLS or
                (r, c) in visit or grid[r][c] == -1):
                return
            # Mark the cell as visited and add to queue
            visit.add((r, c))
            q.append([r, c])

        # Add all treasure cells (cells with value 0) to the queue as BFS starting points
        for r in range(ROWS):
            for c in range(COLS):
                if grid[r][c] == 0:
                    q.append([r, c])
                    visit.add((r, c))

        # Initialize distance from treasures
        dist = 0
        # Perform BFS to fill distances
        while q:
            for i in range(len(q)):
                r, c = q.popleft()
                # Update grid with the current distance
                grid[r][c] = dist
                # Explore neighbors
                addCell(r + 1, c)
                addCell(r - 1, c)
                addCell(r, c + 1)
                addCell(r, c - 1)
            # Increment distance after each BFS level
            dist += 1
```
