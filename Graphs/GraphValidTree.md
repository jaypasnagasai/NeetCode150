# 261. Graph Valid Tree
Difficulty: Medium

## QUESTION

Given `n` nodes labeled from `0` to `n - 1` and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

### EXAMPLE

```
Input:
n = 5
edges = [[0, 1], [0, 2], [0, 3], [1, 4]]

Output:
true
```

```
Input:
n = 5
edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]]

Output:
false
```

## SOLUTION


### APPROACH: CYCLE DETECTION (DFS)

```python
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        # A valid tree should have exactly n - 1 edges
        if len(edges) > (n - 1):
            return False
        
        # Build adjacency list for the graph
        adj = [[] for _ in range(n)]
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)

        # Set to track visited nodes during DFS
        visit = set()

        # Depth-First Search function to detect cycles and visit nodes
        def dfs(node, par):
            # If node is already visited, a cycle exists
            if node in visit:
                return False
            
            # Mark current node as visited
            visit.add(node)

            # Visit all neighbors
            for nei in adj[node]:
                # Skip the parent node to avoid backtracking
                if nei == par:
                    continue
                # If any neighbor causes a cycle, return False
                if not dfs(nei, node):
                    return False

            # No cycle detected from this node
            return True

        # Start DFS from node 0
        # A valid tree should be fully connected and acyclic
        return dfs(0, -1) and len(visit) == n
```

### APPROACH: BREADTH FIRST SEARCH

```python
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        # A valid tree must have exactly n - 1 edges
        if len(edges) > n - 1:
            return False
        
        # Build adjacency list for the graph
        adj = [[] for _ in range(n)]
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)

        # Set to track visited nodes
        visit = set()

        # Initialize queue for BFS with (node, parent) pairs
        q = deque([(0, -1)])  # Start from node 0 with no parent
        visit.add(0)  # Mark starting node as visited

        # Perform BFS
        while q:
            node, parent = q.popleft()

            # Iterate over neighbors of the current node
            for nei in adj[node]:
                # Skip the parent to prevent immediate backtracking
                if nei == parent:
                    continue
                
                # If neighbor is already visited, a cycle is detected
                if nei in visit:
                    return False
                
                # Mark neighbor as visited and add to the queue
                visit.add(nei)
                q.append((nei, node))

        # Valid tree if all nodes are connected (visited)
        return len(visit) == n
```

### APPROACH: DISJOINT SET UNION

```python
class DSU:
    def __init__(self, n):
        # Initialize the number of components (each node is its own component initially)
        self.comps = n
        
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
        # Find the parents (roots) of both nodes
        pu = self.find(u)
        pv = self.find(v)

        # If both nodes share the same parent, a cycle exists
        if pu == pv:
            return False

        # Decrease component count after successful union
        self.comps -= 1

        # Union by size: attach smaller tree under larger one
        if self.Size[pu] < self.Size[pv]:
            pu, pv = pv, pu  # Swap to ensure pu has the larger size
        
        # Merge the two components
        self.Size[pu] += self.Size[pv]
        self.Parent[pv] = pu
        return True

    def components(self):
        # Return the number of remaining connected components
        return self.comps

class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        # A valid tree must have exactly n - 1 edges
        if len(edges) > n - 1:
            return False
        
        # Initialize Disjoint Set Union (DSU)
        dsu = DSU(n)

        # Process each edge
        for u, v in edges:
            # If union returns False, a cycle is detected
            if not dsu.union(u, v):
                return False

        # A valid tree must have exactly one connected component
        return dsu.components() == 1
```
