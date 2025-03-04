# 269. Alien Dictionary
Difficulty: Hard

## QUESTION

There is a foreign language which uses the latin alphabet, but the order among letters is not "a", "b", "c" ... "z" as in English.

You receive a list of non-empty strings `words` from the dictionary, where the words are sorted lexicographically based on the rules of this new language.

Derive the order of letters in this language. If the order is invalid, return an empty string. If there are multiple valid order of letters, return any of them.

A string `a` is lexicographically smaller than a string `b` if either of the following is true:

- The first letter where they differ is smaller in `a` than in `b`.
- There is no index `i` such that `a[i] != b[i]` and `a.length < b.length`.

### EXAMPLE

```
Input: ["z","o"]
Output: "zo"
```

```
Input: ["hrn","hrf","er","enn","rfnn"]
Output: "hernf"
```

## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
from typing import List

class Solution:
    def foreignDictionary(self, words: List[str]) -> str:
        # Create an adjacency list for the directed graph of character precedence
        adj = {c: set() for w in words for c in w}

        # Build the graph based on the order of characters in words
        for i in range(len(words) - 1):
            w1, w2 = words[i], words[i + 1]
            minLen = min(len(w1), len(w2))

            # If w1 is longer than w2 but has the same prefix, it's an invalid order
            if len(w1) > len(w2) and w1[:minLen] == w2[:minLen]:
                return ""

            # Compare characters to determine ordering
            for j in range(minLen):
                if w1[j] != w2[j]:  # First differing character defines precedence
                    adj[w1[j]].add(w2[j])
                    break

        visited = {}  # Dictionary to track visited nodes (True = processing, False = processed)
        res = []  # List to store topologically sorted order of characters

        def dfs(char):
            if char in visited:
                return visited[char]  # If processing, cycle detected (invalid order)

            visited[char] = True  # Mark character as being processed

            # Visit all adjacent characters (dependencies)
            for neighChar in adj[char]:
                if dfs(neighChar):  # Cycle detected
                    return True

            visited[char] = False  # Mark character as fully processed
            res.append(char)  # Append character to result in post-order

        # Perform DFS on each character
        for char in adj:
            if dfs(char):
                return ""  # Return empty string if cycle detected

        res.reverse()  # Reverse result to get correct topological order
        return "".join(res)  # Return the foreign dictionary order as a string
```

### APPROACH: KAHN'S ALGORITHM

```python
from collections import deque

class Solution:
    def foreignDictionary(self, words):
        # Create an adjacency list for character precedence
        adj = {c: set() for w in words for c in w}
        # Initialize in-degree count for each character
        indegree = {c: 0 for c in adj}
        
        # Build the graph and compute in-degrees
        for i in range(len(words) - 1):
            w1, w2 = words[i], words[i + 1]
            minLen = min(len(w1), len(w2))
            
            # If w1 is longer than w2 but has the same prefix, it's an invalid order
            if len(w1) > len(w2) and w1[:minLen] == w2[:minLen]:
                return ""
            
            # Determine ordering from the first differing character
            for j in range(minLen):
                if w1[j] != w2[j]:
                    if w2[j] not in adj[w1[j]]:  # Avoid duplicate edges
                        adj[w1[j]].add(w2[j])
                        indegree[w2[j]] += 1  # Increase in-degree for the dependent character
                    break  # Stop after the first differing character

        # Initialize a queue with characters having zero in-degree (starting points)
        q = deque([c for c in indegree if indegree[c] == 0])
        res = []

        # Perform topological sorting (Kahn's Algorithm)
        while q:
            char = q.popleft()
            res.append(char)  # Append character to the result order
            for neighbor in adj[char]:  # Reduce in-degree of connected nodes
                indegree[neighbor] -= 1
                if indegree[neighbor] == 0:  # Add to queue if no remaining dependencies
                    q.append(neighbor)

        # If not all characters are included, there's a cycle (invalid ordering)
        if len(res) != len(indegree):
            return ""

        return "".join(res)  # Return the constructed dictionary order
```

