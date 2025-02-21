# 417. Pacific Atlantic Water Flow
Difficulty: Medium

## QUESTION

You are given a rectangular island `heights` where `heights[r][c]` represents the height above sea level of the cell at coordinate `(r, c)`.

The islands borders the Pacific Ocean from the top and left sides, and borders the Atlantic Ocean from the bottom and right sides.

Water can flow in four directions (up, down, left, or right) from a cell to a neighboring cell with height equal or lower. Water can also flow into the ocean from cells adjacent to the ocean.

Find all cells where water can flow from that cell to both the Pacific and Atlantic oceans. Return it as a 2D list where each element is a list `[r, c]` representing the row and column of the cell. You may return the answer in any order.

### EXAMPLE

```
Input: heights = [
  [4,2,7,3,4],
  [7,4,6,4,7],
  [6,3,5,3,6]
]
Output: [[0,2],[0,4],[1,0],[1,1],[1,2],[1,3],[1,4],[2,0]]
```

```
Input: heights = [[1],[1]]
Output: [[0,0],[0,1]]
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        # Get the number of rows and columns in the heights grid
        ROWS, COLS = len(heights), len(heights[0])

        # Define directions for moving up, down, left, and right
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        # Flags to track if the current cell can reach Pacific and Atlantic oceans
        pacific = atlantic = False

        # Depth-First Search function
        def dfs(r, c, prevVal):
            nonlocal pacific, atlantic
            # Check if we've reached the Pacific Ocean (top or left edge)
            if r < 0 or c < 0:
                pacific = True
                return
            # Check if we've reached the Atlantic Ocean (bottom or right edge)
            if r >= ROWS or c >= COLS:
                atlantic = True
                return
            # Stop if the next cell is higher than the previous cell (invalid flow)
            if heights[r][c] > prevVal:
                return

            # Mark the current cell as visited by setting it to infinity
            tmp = heights[r][c]
            heights[r][c] = float('inf')

            # Explore all four directions
            for dx, dy in directions:
                dfs(r + dx, c + dy, tmp)
                # Stop early if both oceans can be reached
                if pacific and atlantic:
                    break

            # Restore the original height after DFS
            heights[r][c] = tmp

        # List to store cells that can reach both oceans
        res = []

        # Iterate over every cell in the grid
        for r in range(ROWS):
            for c in range(COLS):
                # Reset ocean reachability flags for each cell
                pacific = False
                atlantic = False
                # Start DFS from the current cell
                dfs(r, c, float('inf'))
                # If both oceans can be reached, add the cell to the result
                if pacific and atlantic:
                    res.append([r, c])

        # Return the list of coordinates that can reach both oceans
        return res
```

### APPROACH: DEPTH FIRST SEARCH

```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        # Get the number of rows and columns in the heights grid
        ROWS, COLS = len(heights), len(heights[0])

        # Sets to store cells reachable by Pacific and Atlantic oceans
        pac, atl = set(), set()

        # Depth-First Search function
        def dfs(r, c, visit, prevHeight):
            # Return if cell is already visited, out of bounds, or lower than previous height
            if ((r, c) in visit or 
                r < 0 or c < 0 or 
                r == ROWS or c == COLS or 
                heights[r][c] < prevHeight
            ):
                return
            # Mark current cell as visited
            visit.add((r, c))

            # Explore all four directions
            dfs(r + 1, c, visit, heights[r][c])  # Down
            dfs(r - 1, c, visit, heights[r][c])  # Up
            dfs(r, c + 1, visit, heights[r][c])  # Right
            dfs(r, c - 1, visit, heights[r][c])  # Left

        # Perform DFS from Pacific Ocean borders (top row and left column)
        for c in range(COLS):
            dfs(0, c, pac, heights[0][c])          # Top row
            dfs(ROWS - 1, c, atl, heights[ROWS - 1][c])  # Bottom row

        # Perform DFS from Atlantic Ocean borders (bottom row and right column)
        for r in range(ROWS):
            dfs(r, 0, pac, heights[r][0])          # Left column
            dfs(r, COLS - 1, atl, heights[r][COLS - 1])  # Right column

        # Collect cells that can reach both oceans
        res = []
        for r in range(ROWS):
            for c in range(COLS):
                if (r, c) in pac and (r, c) in atl:
                    res.append([r, c])

        # Return result containing coordinates that can flow to both oceans
        return res
```

### APPROACH: BREADTH FIRST SEARCH

```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        # Get the number of rows and columns in the heights grid
        ROWS, COLS = len(heights), len(heights[0])

        # Define directions for moving up, down, left, and right
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        # Create 2D grids to track cells reachable by Pacific and Atlantic oceans
        pac = [[False] * COLS for _ in range(ROWS)]
        atl = [[False] * COLS for _ in range(ROWS)]

        # Breadth-First Search function
        def bfs(source, ocean):
            # Initialize queue with starting cells
            q = deque(source)
            while q:
                r, c = q.popleft()
                # Mark current cell as reachable by the ocean
                ocean[r][c] = True

                # Explore all four directions
                for dr, dc in directions:
                    nr, nc = r + dr, c + dc
                    # Continue BFS if next cell is within bounds, unvisited, and height is valid
                    if (0 <= nr < ROWS and 0 <= nc < COLS and 
                        not ocean[nr][nc] and 
                        heights[nr][nc] >= heights[r][c]
                    ):
                        q.append((nr, nc))

        # Initialize starting points for Pacific and Atlantic oceans
        pacific = []
        atlantic = []

        # Add top and bottom rows to Pacific and Atlantic respectively
        for c in range(COLS):
            pacific.append((0, c))          # Top row for Pacific
            atlantic.append((ROWS - 1, c))  # Bottom row for Atlantic

        # Add left and right columns to Pacific and Atlantic respectively
        for r in range(ROWS):
            pacific.append((r, 0))          # Left column for Pacific
            atlantic.append((r, COLS - 1))  # Right column for Atlantic

        # Perform BFS from Pacific and Atlantic sources
        bfs(pacific, pac)
        bfs(atlantic, atl)

        # Collect cells reachable by both oceans
        res = []
        for r in range(ROWS):
            for c in range(COLS):
                if pac[r][c] and atl[r][c]:
                    res.append([r, c])

        # Return result containing coordinates that can flow to both oceans
        return res
```
