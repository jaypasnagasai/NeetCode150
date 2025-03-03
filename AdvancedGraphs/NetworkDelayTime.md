# 743. Network Delay Time
Difficulty: Medium

## QUESTION

You are given a network of `n` directed nodes, labeled from `1` to `n`. You are also given `times`, a list of directed edges where `times[i] = (ui, vi, ti)`.

- `ui` is the source node (an integer from `1` to `n`)
- `vi` is the target node (an integer from `1` to `n`)
- `ti` is the time it takes for a signal to travel from the source to the target node (an integer greater than or equal to `0`).

You are also given an integer `k`, representing the node that we will send a signal from.

Return the minimum time it takes for all of the `n` nodes to receive the signal. If it is impossible for all the nodes to receive the signal, return `-1` instead.

### EXAMPLE

```
Input: times = [[1,2,1],[2,3,1],[1,4,4],[3,4,1]], n = 4, k = 1
Output: 3
```

```
Input: times = [[1,2,1],[2,3,1]], n = 3, k = 2
Output: -1
```

## SOLUTION


### APPROACH: DIJKSTRA'S ALGORITHM

```python
import collections
import heapq
from typing import List

class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        # Create adjacency list representation of the graph
        edges = collections.defaultdict(list)
        for u, v, w in times:
            edges[u].append((v, w))  # Store neighbors and edge weights

        # Min-heap to store (time, node) with initial node k and time 0
        minHeap = [(0, k)]
        visit = set()  # Set to track visited nodes
        t = 0  # Variable to store the maximum time to reach all nodes

        while minHeap:
            w1, n1 = heapq.heappop(minHeap)  # Get the node with the smallest time
            if n1 in visit:  # Skip if the node is already visited
                continue
            visit.add(n1)  # Mark node as visited
            t = w1  # Update the time

            # Explore neighbors of the current node
            for n2, w2 in edges[n1]:
                if n2 not in visit:  # Only process unvisited nodes
                    heapq.heappush(minHeap, (w1 + w2, n2))  # Push updated time and node
        
        # If all nodes are visited, return the maximum time; otherwise, return -1
        return t if len(visit) == n else -1
```
