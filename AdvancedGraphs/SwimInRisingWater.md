# 778. Swim in Rising Water
Difficulty: Hard

## QUESTION

You are given a square 2-D matrix of distinct integers `grid` where each integer `grid[i][j]` represents the elevation at position `(i, j)`.

Rain starts to fall at time = `0`, which causes the water level to rise. At time `t`, the water level across the entire grid is `t`.

You may swim either horizontally or vertically in the grid between two adjacent squares if the original elevation of both squares is less than or equal to the water level at time `t`.

Starting from the top left square `(0, 0)`, return the minimum amount of time it will take until it is possible to reach the bottom right square `(n - 1, n - 1)`.

### EXAMPLE

```
Input: grid = [[0,1],[2,3]]
Output: 3
```

```
Input: grid = [
  [0,1,2,10],
  [9,14,4,13],
  [12,3,8,15],
  [11,5,7,6]]
]
Output: 8
```

## SOLUTION


### APPROACH: DIJKSTRA'S ALGORITHM

```python
from typing import List
import heapq

class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        N = len(grid)  # Size of the grid
        visit = set()  # Set to track visited cells
        minH = [[grid[0][0], 0, 0]]  # Min-heap storing (max-height/time, row, col)
        directions = [[0, 1], [0, -1], [1, 0], [-1, 0]]  # Possible movement directions

        visit.add((0, 0))  # Mark the starting position as visited

        while minH:
            t, r, c = heapq.heappop(minH)  # Get the current cell with the lowest max-height
            if r == N - 1 and c == N - 1:  # If we reached the bottom-right corner, return the time
                return t
            
            # Explore all 4 possible directions
            for dr, dc in directions:
                neiR, neiC = r + dr, c + dc
                # Check if the new position is within bounds and not visited
                if (neiR < 0 or neiC < 0 or 
                    neiR == N or neiC == N or
                    (neiR, neiC) in visit):
                    continue
                
                visit.add((neiR, neiC))  # Mark the new position as visited
                # Push the new position into the heap with the max height encountered so far
                heapq.heappush(minH, [max(t, grid[neiR][neiC]), neiR, neiC])
```

### APPROACH: KRUSKAL'S ALGORITHM

```python
from typing import List

class DSU:
    def __init__(self, n):
        # Initialize each node as its own parent (disjoint set)
        self.Parent = list(range(n + 1))
        # Initialize size of each set to 1
        self.Size = [1] * (n + 1)

    def find(self, node):
        # Path compression: make nodes point directly to the root
        if self.Parent[node] != node:
            self.Parent[node] = self.find(self.Parent[node])
        return self.Parent[node]

    def union(self, u, v):
        # Find roots of both nodes
        pu = self.find(u)
        pv = self.find(v)
        
        # If already in the same set, return False
        if pu == pv:
            return False
        
        # Union by size: attach smaller tree under larger tree
        if self.Size[pu] < self.Size[pv]:
            pu, pv = pv, pu  # Ensure pu is the larger root
        self.Size[pu] += self.Size[pv]  # Merge the sets
        self.Parent[pv] = pu  # Update parent
        return True

    def connected(self, u, v):
        # Check if two nodes belong to the same set
        return self.find(u) == self.find(v)

class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        N = len(grid)
        dsu = DSU(N * N)  # Disjoint Set Union for connectivity tracking
        # Sort all grid positions by their elevation values
        positions = sorted((grid[r][c], r, c) for r in range(N) for c in range(N))
        directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # Possible movement directions
        
        # Process each position in increasing order of elevation
        for t, r, c in positions:
            # Try to connect the current cell with its valid neighbors
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                if 0 <= nr < N and 0 <= nc < N and grid[nr][nc] <= t:
                    dsu.union(r * N + c, nr * N + nc)
            
            # If top-left (0,0) is connected to bottom-right (N-1, N-1), return the time
            if dsu.connected(0, N * N - 1):
                return t
```
