# 130. Surrounded Regions
Difficulty: Medium

## QUESTION

You are given a 2-D matrix `board` containing `'X'` and `'O'` characters.

If a continous, four-directionally connected group of `'O'`s is surrounded by `'X'`s, it is considered to be surrounded.

Change all surrounded regions of `'O'`s to `'X'`s and do so in-place by modifying the input board.

### EXAMPLE

```
Input: board = [
  ["X","X","X","X"],
  ["X","O","O","X"],
  ["X","O","O","X"],
  ["X","X","X","O"]
]

Output: [
  ["X","X","X","X"],
  ["X","X","X","X"],
  ["X","X","X","X"],
  ["X","X","X","O"]
]
```

## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        # Get the number of rows and columns in the board
        ROWS, COLS = len(board), len(board[0])

        # Depth-First Search function to mark connected 'O's
        def capture(r, c):
            # Return if out of bounds or current cell is not 'O'
            if (r < 0 or c < 0 or r == ROWS or 
                c == COLS or board[r][c] != "O"
            ):
                return
            # Temporarily mark the current 'O' as 'T'
            board[r][c] = "T"
            # Recursively explore all four directions
            capture(r + 1, c)  # Down
            capture(r - 1, c)  # Up
            capture(r, c + 1)  # Right
            capture(r, c - 1)  # Left

        # Start DFS from 'O's on the left and right borders
        for r in range(ROWS):
            if board[r][0] == "O":
                capture(r, 0)  # Left border
            if board[r][COLS - 1] == "O":
                capture(r, COLS - 1)  # Right border
        
        # Start DFS from 'O's on the top and bottom borders
        for c in range(COLS):
            if board[0][c] == "O":
                capture(0, c)  # Top border
            if board[ROWS - 1][c] == "O":
                capture(ROWS - 1, c)  # Bottom border

        # Traverse the board to finalize changes
        for r in range(ROWS):
            for c in range(COLS):
                # Convert surrounded 'O's to 'X'
                if board[r][c] == "O":
                    board[r][c] = "X"
                # Restore temporary 'T's back to 'O'
                elif board[r][c] == "T":
                    board[r][c] = "O"
```

### APPROACH: BREADTH FIRST SEARCH

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        # Get the number of rows and columns in the board
        ROWS, COLS = len(board), len(board[0])

        # Define directions for moving up, down, left, and right
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        # Breadth-First Search function to mark border-connected 'O's
        def capture():
            # Initialize queue for BFS
            q = deque()
            
            # Add all 'O's on the border to the queue
            for r in range(ROWS):
                for c in range(COLS):
                    if ((r == 0 or r == ROWS - 1 or 
                         c == 0 or c == COLS - 1) and 
                        board[r][c] == "O"):
                        q.append((r, c))

            # Perform BFS to mark all 'O's connected to borders
            while q:
                r, c = q.popleft()
                # Mark current 'O' as 'T' to indicate it's connected to a border
                if board[r][c] == "O":
                    board[r][c] = "T"
                    # Explore all four directions
                    for dr, dc in directions:
                        nr, nc = r + dr, c + dc
                        # Add valid neighboring 'O's to the queue
                        if 0 <= nr < ROWS and 0 <= nc < COLS:
                            q.append((nr, nc))
        
        # Start capturing process
        capture()

        # Final pass to update the board
        for r in range(ROWS):
            for c in range(COLS):
                # Convert surrounded 'O's to 'X'
                if board[r][c] == "O":
                    board[r][c] = "X"
                # Restore temporary 'T's back to 'O'
                elif board[r][c] == "T":
                    board[r][c] = "O"
```

### APPROACH: DISJOINT SET UNION

```python
class DSU:
    def __init__(self, n):
        # Initialize parent list where each node is its own parent
        self.Parent = list(range(n + 1))
        # Initialize size list to keep track of component sizes
        self.Size = [1] * (n + 1)

    def find(self, node):
        # Path compression: make the found root the direct parent
        if self.Parent[node] != node:
            self.Parent[node] = self.find(self.Parent[node])
        return self.Parent[node]

    def union(self, u, v):
        # Find the parents of both nodes
        pu = self.find(u)
        pv = self.find(v)

        # If both nodes have the same parent, they're already connected
        if pu == pv:
            return False

        # Union by size: attach the smaller tree under the larger one
        if self.Size[pu] >= self.Size[pv]:
            self.Size[pu] += self.Size[pv]
            self.Parent[pv] = pu
        else:
            self.Size[pv] += self.Size[pu]
            self.Parent[pu] = pv
        return True

    def connected(self, u, v):
        # Check if two nodes are connected by comparing their roots
        return self.find(u) == self.find(v)

class Solution:
    def solve(self, board: List[List[str]]) -> None:
        # Get number of rows and columns in the board
        ROWS, COLS = len(board), len(board[0])

        # Define directions for moving up, down, left, and right
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        # Initialize DSU with extra node for the border
        dsu = DSU(ROWS * COLS + 1)

        # Iterate through each cell in the board
        for r in range(ROWS):
            for c in range(COLS):
                # Skip cells that are not 'O'
                if board[r][c] != "O":
                    continue

                # If the cell is on the border, union it with the extra node
                if (r == 0 or c == 0 or 
                    r == (ROWS - 1) or c == (COLS - 1)):
                    dsu.union(ROWS * COLS, r * COLS + c)
                else:
                    # Union current cell with its adjacent 'O' cells
                    for dx, dy in directions:
                        nr, nc = r + dx, c + dy
                        if board[nr][nc] == "O":
                            dsu.union(r * COLS + c, nr * COLS + nc)

        # Final pass to update the board
        for r in range(ROWS):
            for c in range(COLS):
                # Convert 'O' to 'X' if it's not connected to the border
                if not dsu.connected(ROWS * COLS, r * COLS + c):
                    board[r][c] = "X"
```
