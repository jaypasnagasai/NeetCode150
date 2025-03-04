# 1584. Min Cost to Connect All Points
Difficulty: Medium

## QUESTION

You are given a 2-D integer array `points`, where `points[i] = [xi, yi]`. Each `points[i]` represents a distinct point on a 2-D plane.

The cost of connecting two points `[xi, yi]` and `[xj, yj]` is the manhattan distance between the two points, i.e. `|xi - xj| + |yi - yj|`.

Return the minimum cost to connect all points together, such that there exists exactly one path between each pair of points.

### EXAMPLE

```
Input: points = [[0,0],[2,2],[3,3],[2,4],[4,2]]
Output: 10
```

## SOLUTION


### APPROACH: KRUSKAL'S ALGORITHM

```python
from typing import List

class DSU:
    def __init__(self, n):
        # Initialize parent array where each node is its own parent
        self.Parent = list(range(n + 1))
        # Initialize size array to track the size of each set
        self.Size = [1] * (n + 1)

    def find(self, node):
        # Path compression: recursively find the root of the set
        if self.Parent[node] != node:
            self.Parent[node] = self.find(self.Parent[node])
        return self.Parent[node]

    def union(self, u, v):
        # Find the roots of both nodes
        pu = self.find(u)
        pv = self.find(v)
        
        # If they are already in the same set, return False
        if pu == pv:
            return False
        
        # Union by size: attach the smaller tree under the larger tree
        if self.Size[pu] < self.Size[pv]:
            pu, pv = pv, pu  # Swap to ensure pu is always the larger root
        self.Size[pu] += self.Size[pv]  # Merge the sets
        self.Parent[pv] = pu  # Update the parent of the smaller set
        return True

class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        n = len(points)
        dsu = DSU(n)  # Create a Disjoint Set Union (DSU) for Kruskal's algorithm
        edges = []

        # Generate all possible edges with their Manhattan distances
        for i in range(n):
            x1, y1 = points[i]
            for j in range(i + 1, n):
                x2, y2 = points[j]
                dist = abs(x1 - x2) + abs(y1 - y2)  # Manhattan distance
                edges.append((dist, i, j))  # Store (distance, point1, point2)
        
        # Sort edges by distance (Kruskal's algorithm)
        edges.sort()
        res = 0  # Variable to store the total cost of connecting all points
        
        # Process edges in sorted order, adding them to the MST if they connect new components
        for dist, u, v in edges:
            if dsu.union(u, v):  # If the edge connects two different components
                res += dist  # Add the cost of this edge to the result
        
        return res  # Return the minimum cost to connect all points
```

### APPROACH: PRIM'S ALGORITHM

```python
from typing import List

class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        n, node = len(points), 0  # Number of points and starting node
        dist = [100000000] * n  # Initialize distances to a large number
        visit = [False] * n  # Track visited nodes
        edges, res = 0, 0  # Edge count and total cost

        while edges < n - 1:  # Run while there are still edges to be added
            visit[node] = True  # Mark the current node as visited
            nextNode = -1  # Variable to track the next node to visit
            
            for i in range(n):  # Iterate through all nodes
                if visit[i]:  # Skip already visited nodes
                    continue
                # Calculate the Manhattan distance between the current node and node i
                curDist = (abs(points[i][0] - points[node][0]) + 
                           abs(points[i][1] - points[node][1]))
                dist[i] = min(dist[i], curDist)  # Update the minimum distance
                
                # Select the next node with the smallest distance
                if nextNode == -1 or dist[i] < dist[nextNode]:
                    nextNode = i
                    
            res += dist[nextNode]  # Add the distance to the result
            node = nextNode  # Move to the next node
            edges += 1  # Increase the edge count

        return res  # Return the minimum cost to connect all points
```


