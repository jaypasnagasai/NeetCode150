# 684. Redundant Connection
Difficulty: Medium

## QUESTION

You are given a connected undirected graph with `n` nodes labeled from `1` to `n`. Initially, it contained no cycles and consisted of `n-1` edges.

We have now added one additional edge to the graph. The edge has two different vertices chosen from `1` to `n`, and was not an edge that previously existed in the graph.

The graph is represented as an array `edges` of length `n` where `edges[i] = [ai, bi]` represents an edge between nodes `ai` and `bi` in the graph.

Return an edge that can be removed so that the graph is still a connected non-cyclical graph. If there are multiple answers, return the edge that appears last in the input `edges`.

### EXAMPLE

```
Input: edges = [[1,2],[1,3],[3,4],[2,4]]
Output: [2,4]
```

```
Input: edges = [[1,2],[1,3],[1,4],[3,4],[4,5]]
Output: [3,4]
```

## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        # Number of nodes in the graph
        n = len(edges)

        # Initialize adjacency list for the graph
        adj = [[] for _ in range(n + 1)]

        # Build the graph from the edges
        for u, v in edges:
            adj[u].append(v)
            adj[v].append(u)

        # List to track visited nodes during DFS
        visit = [False] * (n + 1)

        # Set to store nodes involved in a detected cycle
        cycle = set()

        # Variable to mark the starting point of the cycle
        cycleStart = -1

        # Depth-First Search function to detect cycles
        def dfs(node, par):
            nonlocal cycleStart

            # If node is already visited, a cycle is detected
            if visit[node]:
                cycleStart = node  # Mark where the cycle starts
                return True

            # Mark current node as visited
            visit[node] = True

            # Explore neighbors
            for nei in adj[node]:
                if nei == par:
                    continue  # Skip the parent node
                if dfs(nei, node):
                    # If still in the cycle path, add node to cycle set
                    if cycleStart != -1:
                        cycle.add(node)
                    # Once the cycle is fully traversed, reset cycleStart
                    if node == cycleStart:
                        cycleStart = -1
                    return True  # Continue bubbling up the cycle detection

            return False  # No cycle found from this path

        # Start DFS from node 1
        dfs(1, -1)

        # Iterate through edges in reverse to find the redundant one
        for u, v in reversed(edges):
            # The redundant edge will be the one in the detected cycle
            if u in cycle and v in cycle:
                return [u, v]

        # Return empty list if no redundant edge is found (should not happen)
        return []
```

### APPROACH: DISJOINT SET UNION

```python
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        # Initialize parent list for Union-Find (each node is its own parent)
        par = [i for i in range(len(edges) + 1)]
        
        # Initialize rank list to keep track of tree sizes
        rank = [1] * (len(edges) + 1)

        # Find function with path compression
        def find(n):
            p = par[n]
            # Traverse up the tree to find the root
            while p != par[p]:
                # Path compression: point current node to its grandparent
                par[p] = par[par[p]]
                p = par[p]
            return p

        # Union function to merge two sets
        def union(n1, n2):
            # Find roots of both nodes
            p1, p2 = find(n1), find(n2)

            # If both nodes share the same root, a cycle is detected
            if p1 == p2:
                return False

            # Union by rank: attach smaller tree under larger one
            if rank[p1] > rank[p2]:
                par[p2] = p1
                rank[p1] += rank[p2]
            else:
                par[p1] = p2
                rank[p2] += rank[p1]
            return True

        # Process each edge to find the redundant connection
        for n1, n2 in edges:
            # If union returns False, the edge forms a cycle
            if not union(n1, n2):
                return [n1, n2]

        # Return empty list if no redundant edge is found (should not happen)
        return []
```
