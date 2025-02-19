# 994. Rotting Oranges
Difficulty: Medium

## QUESTION

You are given a 2-D matrix `grid`. Each cell can have one of three possible values:

- `0` representing an empty cell
- `1` representing a fresh fruit\
- `2` representing a rotten fruit

Every minute, if a fresh fruit is horizontally or vertically adjacent to a rotten fruit, then the fresh fruit also becomes rotten.

Return the minimum number of minutes that must elapse until there are zero fresh fruits remaining. If this state is impossible within the grid, return `-1`.

### EXAMPLE

```
Input: grid = [[1,1,0],[0,1,1],[0,1,2]]
Output: 4
```

```
Input: grid = [[1,0,1],[0,2,0],[1,0,1]]
Output: -1
```

## SOLUTION


### APPROACH: BREADTH FIRST SEARCH

```python
import collections
from typing import List

class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        # Queue for BFS traversal
        q = collections.deque()
        # Count of fresh oranges
        fresh = 0
        # Time taken to rot all oranges
        time = 0

        # Initialize queue with positions of rotten oranges and count fresh ones
        for r in range(len(grid)):
            for c in range(len(grid[0])):
                if grid[r][c] == 1:
                    fresh += 1  # Count fresh oranges
                if grid[r][c] == 2:
                    q.append((r, c))  # Add rotten oranges to queue

        # Directions for adjacent cells (right, left, down, up)
        directions = [[0, 1], [0, -1], [1, 0], [-1, 0]]

        # BFS traversal until all reachable fresh oranges are rotten
        while fresh > 0 and q:
            length = len(q)
            for i in range(length):
                r, c = q.popleft()

                # Explore all adjacent cells
                for dr, dc in directions:
                    row, col = r + dr, c + dc
                    # Check if the neighbor is a fresh orange
                    if (row in range(len(grid))
                        and col in range(len(grid[0]))
                        and grid[row][col] == 1):
                        # Mark it as rotten
                        grid[row][col] = 2
                        # Add to queue for the next round
                        q.append((row, col))
                        # Decrease fresh orange count
                        fresh -= 1
            # Increment time after processing each level
            time += 1

        # If all fresh oranges are rotten, return time, else return -1
        return time if fresh == 0 else -1
```

### APPROACH: BREADTH FIRST SEARCH (NO QUEUE)

```python
from typing import List

class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        # Get grid dimensions
        ROWS, COLS = len(grid), len(grid[0])
        # Count of fresh oranges
        fresh = 0
        # Time taken to rot all oranges
        time = 0

        # Count initial number of fresh oranges
        for r in range(ROWS):
            for c in range(COLS):
                if grid[r][c] == 1:
                    fresh += 1

        # Define directions for adjacent cells (right, left, down, up)
        directions = [[0, 1], [0, -1], [1, 0], [-1, 0]]

        # Loop until all fresh oranges are rotten
        while fresh > 0:
            # Flag to track if any fresh orange was rotted in this iteration
            flag = False

            # Traverse the grid to rot adjacent fresh oranges
            for r in range(ROWS):
                for c in range(COLS):
                    if grid[r][c] == 2:
                        for dr, dc in directions:
                            row, col = r + dr, c + dc
                            # If the adjacent cell has a fresh orange
                            if (row in range(ROWS) and 
                                col in range(COLS) and 
                                grid[row][col] == 1):
                                # Temporarily mark it as rotting (3)
                                grid[row][col] = 3
                                fresh -= 1
                                flag = True

            # If no fresh orange was rotted, return -1 (not all can rot)
            if not flag:
                return -1

            # Convert all temporarily marked oranges to fully rotten
            for r in range(ROWS):
                for c in range(COLS):
                    if grid[r][c] == 3:
                        grid[r][c] = 2

            # Increment time after each round of rotting
            time += 1

        # Return total time taken to rot all oranges
        return time
```
