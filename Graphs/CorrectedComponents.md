# 321. Number of Connected Components In An Undirected Graph
Difficulty: Medium

## QUESTION

There is an undirected graph with `n` nodes. There is also an `edges` array, where `edges[i] = [a, b]` means that there is an edge between node `a` and node `b` in the graph.

The nodes are numbered from `0` to `n - 1`.

Return the total number of connected components in that graph.

### EXAMPLE

```
Input:
n=3
edges=[[0,1], [0,2]]

Output:
1
```

```
Input:
n=6
edges=[[0,1], [1,2], [2,3], [4,5]]

Output:
2
```

## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        # Initialize adjacency list to represent the graph
        adj = [[] for _ in range(n)]
        
        # Initialize visited list to keep track of visited nodes
        visit = [False] * n

        # Build the adjacency list from the edges
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)

        # Depth-First Search function to explore connected nodes
        def dfs(node):
            for nei in adj[node]:
                if not visit[nei]:
                    visit[nei] = True  # Mark neighbor as visited
                    dfs(nei)  # Recursively visit all neighbors

        # Variable to count the number of connected components
        res = 0

        # Iterate over all nodes
        for node in range(n):
            # If node has not been visited, it's a new component
            if not visit[node]:
                visit[node] = True  # Mark current node as visited
                dfs(node)  # Explore the entire component
                res += 1  # Increment component count

        # Return total number of connected components
        return res
```

### APPROACH: BREADTH FIRST SEARCH

```python
class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        # Initialize adjacency list to represent the graph
        adj = [[] for _ in range(n)]
        
        # Initialize visited list to keep track of visited nodes
        visit = [False] * n

        # Build the adjacency list from the edges
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)

        # Breadth-First Search function to explore connected nodes
        def bfs(node):
            # Initialize queue for BFS and mark the starting node as visited
            q = deque([node])
            visit[node] = True

            # Process nodes in the queue
            while q:
                cur = q.popleft()  # Dequeue the current node
                for nei in adj[cur]:  # Explore neighbors
                    if not visit[nei]:  # If neighbor not visited
                        visit[nei] = True  # Mark as visited
                        q.append(nei)  # Enqueue neighbor

        # Variable to count the number of connected components
        res = 0

        # Iterate over all nodes
        for node in range(n):
            # If node has not been visited, it's a new component
            if not visit[node]:
                bfs(node)  # Perform BFS to visit entire component
                res += 1  # Increment component count

        # Return total number of connected components
        return res
```

### APPROACH: DISJOINT SET UNION

```python
class DSU:
    def __init__(self, n):
        # Initialize parent list where each node is its own parent
        self.parent = list(range(n))
        # Initialize rank list to keep track of component sizes
        self.rank = [1] * n

    def find(self, node):
        # Find the root of the node with path compression
        cur = node
        while cur != self.parent[cur]:
            # Path compression: point node directly to its grandparent
            self.parent[cur] = self.parent[self.parent[cur]]
            cur = self.parent[cur]
        return cur

    def union(self, u, v):
        # Find roots of both nodes
        pu = self.find(u)
        pv = self.find(v)

        # If both nodes share the same parent, they are already connected
        if pu == pv:
            return False

        # Union by rank: attach the smaller tree under the larger one
        if self.rank[pv] > self.rank[pu]:
            pu, pv = pv, pu  # Swap to ensure pu has the larger rank

        # Connect pv's root to pu's root
        self.parent[pv] = pu
        # Update rank of the new root
        self.rank[pu] += self.rank[pv]
        return True

class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        # Initialize Disjoint Set Union (DSU)
        dsu = DSU(n)
        
        # Start with each node as its own component
        res = n

        # Process each edge and reduce the component count on successful unions
        for u, v in edges:
            if dsu.union(u, v):
                res -= 1  # Reduce component count after merging two components

        # Return the total number of connected components
        return res
```
