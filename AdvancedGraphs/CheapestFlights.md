# 787. Cheapest Flights Within K Stops
Difficulty: Medium

## QUESTION

There are `n` airports, labeled from `0` to `n - 1`, which are connected by some flights. You are given an array `flights` where `flights[i] = [from_i, to_i, price_i]` represents a one-way flight from airport `from_i` to airport `to_i` with cost `price_i`. You may assume there are no duplicate flights and no flights from an airport to itself.

You are also given three integers `src`, `dst`, and `k` where:

- `src` is the starting airport
- `dst` is the destination airport
- `src != dst`
- `k` is the maximum number of stops you can make (not including `src` and `dst`)

Return the cheapest price from `src` to `dst` with at most `k` stops, or return `-1` if it is impossible.

### EXAMPLE

```
Input: n = 4, flights = [[0,1,200],[1,2,100],[1,3,300],[2,3,100]], src = 0, dst = 3, k = 1
Output: 500
```

```
Input: n = 3, flights = [[1,0,100],[1,2,200],[0,2,100]], src = 1, dst = 2, k = 1
Output: 200
```

## SOLUTION


### APPROACH: DIJKSTRA'S ALGORITHM

```python
import heapq
from typing import List

class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        INF = float("inf")  # Define a large value for unreachable nodes
        adj = [[] for _ in range(n)]  # Adjacency list to store graph
        dist = [[INF] * (k + 5) for _ in range(n)]  # Distance array with extra buffer for stops
        
        # Build the adjacency list from flight data
        for u, v, cst in flights:
            adj[u].append([v, cst])  # Store destination and cost
        
        dist[src][0] = 0  # Starting node has a cost of 0
        minHeap = [(0, src, -1)]  # Min-heap to track (cost, current node, number of stops)
        
        while minHeap:
            cst, node, stops = heapq.heappop(minHeap)  # Extract the cheapest flight
            
            if dst == node:  # If we reached the destination, return the cost
                return cst
            
            if stops == k or dist[node][stops + 1] < cst:  # If stop limit exceeded or suboptimal path, skip
                continue
            
            # Explore neighboring flights
            for nei, w in adj[node]:
                nextCst = cst + w  # Calculate new cost
                nextStops = stops + 1  # Increment stop count
                
                # If a cheaper path with the same or fewer stops is found, update and push to heap
                if dist[nei][nextStops + 1] > nextCst:
                    dist[nei][nextStops + 1] = nextCst
                    heapq.heappush(minHeap, (nextCst, nei, nextStops))

        return -1  # If destination cannot be reached within k stops, return -1
```

### APPROACH: BELLMAN FORD ALGORITHM

```python
from typing import List

class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        # Initialize the price array with infinity, except for the source node
        prices = [float("inf")] * n
        prices[src] = 0  # Cost to reach the source is 0

        # Run the Bellman-Ford algorithm for at most k + 1 iterations
        for i in range(k + 1):
            tmpPrices = prices.copy()  # Copy the prices array to track updates

            # Iterate through all flights
            for s, d, p in flights:  # s = source, d = destination, p = price
                if prices[s] == float("inf"):  # If source is unreachable, skip
                    continue
                # Relax the edge if a cheaper price is found
                if prices[s] + p < tmpPrices[d]:
                    tmpPrices[d] = prices[s] + p
            
            # Update the prices array with the new computed values
            prices = tmpPrices
        
        # Return the cheapest price to reach destination or -1 if unreachable
        return -1 if prices[dst] == float("inf") else prices[dst]
```

### APPROACH: SHORTEST PART FASTER ALGORITHM

```python
from typing import List
from collections import deque

class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        # Initialize the price array with infinity, except for the source node
        prices = [float("inf")] * n
        prices[src] = 0  # Cost to reach the source is 0

        # Create an adjacency list representation of the graph
        adj = [[] for _ in range(n)]
        for u, v, cst in flights:
            adj[u].append([v, cst])  # Store destination and cost

        # Use a queue for BFS traversal (cost, current node, number of stops)
        q = deque([(0, src, 0)])

        while q:
            cst, node, stops = q.popleft()  # Dequeue the current node
            
            # If stops exceed the allowed limit, skip processing
            if stops > k:
                continue
            
            # Explore all possible destinations from the current node
            for nei, w in adj[node]:
                nextCost = cst + w  # Compute the new cost to reach the neighbor
                
                # If a cheaper path is found, update and enqueue
                if nextCost < prices[nei]:
                    prices[nei] = nextCost
                    q.append((nextCost, nei, stops + 1))

        # Return the cheapest price to reach the destination or -1 if unreachable
        return prices[dst] if prices[dst] != float("inf") else -1
```
