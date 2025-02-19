# 133. Clone Graph
Difficulty: Medium

## QUESTION

Given a node in a connected undirected graph, return a deep copy of the graph.

Each node in the graph contains an integer value and a list of its neighbors.

```python
class Node {
    public int val;
    public List<Node> neighbors;
}
```
The graph is shown in the test cases as an adjacency list. An adjacency list is a mapping of nodes to lists, used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

For simplicity, nodes values are numbered from 1 to `n`, where `n` is the total number of nodes in the graph. The index of each node within the adjacency list is the same as the node's value (1-indexed).

The input node will always be the first node in the graph and have `1` as the value.


### EXAMPLE

```
Input: adjList = [[2],[1,3],[2]]
Output: [[2],[1,3],[2]]
```

```
Input: adjList = [[]]
Output: [[]]
```

```
Input: adjList = []
Output: []
```
## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        # Dictionary to map original nodes to their clones
        oldToNew = {}

        def dfs(node):
            # If node is already cloned, return the clone
            if node in oldToNew:
                return oldToNew[node]

            # Create a copy of the current node
            copy = Node(node.val)
            # Store the copy in the mapping
            oldToNew[node] = copy

            # Recursively clone all neighbors
            for nei in node.neighbors:
                copy.neighbors.append(dfs(nei))
            
            return copy

        # If the graph is empty, return None
        return dfs(node) if node else None
```

### APPROACH: BREADTH FIRST SEARCH

```python
from collections import deque
from typing import Optional

"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        # If the input graph is empty, return None
        if not node:
            return None

        # Dictionary to map original nodes to their cloned nodes
        oldToNew = {}
        # Create a copy of the starting node
        oldToNew[node] = Node(node.val)
        # Initialize queue for BFS traversal
        q = deque([node])

        # BFS traversal to clone the graph
        while q:
            cur = q.popleft()
            # Iterate over all neighbors of the current node
            for nei in cur.neighbors:
                # If the neighbor hasn't been cloned yet
                if nei not in oldToNew:
                    # Clone the neighbor node
                    oldToNew[nei] = Node(nei.val)
                    # Add the neighbor to the queue for further traversal
                    q.append(nei)
                # Link the cloned neighbor to the cloned current node
                oldToNew[cur].neighbors.append(oldToNew[nei])

        # Return the cloned graph's starting node
        return oldToNew[node]
```
