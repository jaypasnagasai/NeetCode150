# 332. Reconstruct Itinerary
Difficulty: Hard

## QUESTION

You are given a list of flight tickets `tickets` where `tickets[i] = [from_i, to_i]` represent the source airport and the destination airport.

Each `from_i` and `to_i` consists of three uppercase English letters.

Reconstruct the itinerary in order and return it.

All of the tickets belong to someone who originally departed from `"JFK"`. Your objective is to reconstruct the flight path that this person took, assuming each ticket was used exactly once.

If there are multiple valid flight paths, return the lexicographically smallest one.

For example, the itinerary `["JFK", "SEA"]` has a smaller lexical order than `["JFK", "SFO"]`.
You may assume all the tickets form at least one valid flight path.

### EXAMPLE

```
Input: tickets = [["BUF","HOU"],["HOU","SEA"],["JFK","BUF"]]
Output: ["JFK","BUF","HOU","SEA"]
```

```
Input: tickets = [["HOU","JFK"],["SEA","JFK"],["JFK","SEA"],["JFK","HOU"]]
Output: ["JFK","HOU","JFK","SEA","JFK"]
```

## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
from typing import List

class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        # Create adjacency list to store destinations for each source
        adj = {src: [] for src, dst in tickets}
        
        # Sort tickets to ensure lexical order traversal
        tickets.sort()
        
        # Populate adjacency list
        for src, dst in tickets:
            adj[src].append(dst)

        res = ["JFK"]  # Result path starts from "JFK"

        def dfs(src):
            if len(res) == len(tickets) + 1:  # If all tickets are used, return True
                return True
            if src not in adj:  # If no outgoing flights, return False
                return False

            temp = list(adj[src])  # Make a copy of current destinations
            for i, v in enumerate(temp):  # Try each possible destination
                adj[src].pop(i)  # Remove the destination
                res.append(v)  # Add to the itinerary
                
                if dfs(v):  # Recursively explore further
                    return True
                
                adj[src].insert(i, v)  # Backtrack: restore the removed destination
                res.pop()  # Backtrack: remove the last added airport
                
            return False  # If no valid itinerary found, return False
            
        dfs("JFK")  # Start DFS from "JFK"
        return res  # Return the constructed itinerary
```

### APPROACH: HIERHOLZER'S ALGORITHM

```python
from typing import List
from collections import defaultdict

class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        # Create an adjacency list where each source points to a list of destinations
        adj = defaultdict(list)

        # Sort tickets in reverse lexical order and populate adjacency list
        # Sorting in reverse ensures that we process the smallest lexical destination last
        for src, dst in sorted(tickets, reverse=True):
            adj[src].append(dst)
        
        stack = ["JFK"]  # Stack for DFS traversal, starting from "JFK"
        res = []  # List to store the final itinerary

        while stack:
            curr = stack[-1]  # Peek at the current airport
            if not adj[curr]:  # If there are no more destinations from current airport
                res.append(stack.pop())  # Add airport to itinerary
            else:
                stack.append(adj[curr].pop())  # Visit the next destination

        return res[::-1]  # Reverse the result to get the correct itinerary order
```
