# 210. Course Schedule II
Difficulty: Medium

## QUESTION

You are given an array `prerequisites` where `prerequisites[i] = [a, b]` indicates that you must take course `b` first if you want to take course `a`.

- For example, the pair `[0, 1]`, indicates that to take course 0 you have to first take course `1`.

There are a total of `numCourses` courses you are required to take, labeled from `0` to numCourses `- 1`.

Return a valid ordering of courses you can take to finish all courses. If there are many valid answers, return any of them. If it's not possible to finish all courses, return an empty array.

### EXAMPLE

```
Input: numCourses = 3, prerequisites = [[1,0]]
Output: [0,1,2]
```

```
Input: numCourses = 3, prerequisites = [[0,1],[1,2],[2,0]]
Output: []
```

## SOLUTION


### APPROACH: CYCYLE DETECTION (DFS)

```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        # Create a mapping of each course to its list of prerequisites
        prereq = {c: [] for c in range(numCourses)}
        for crs, pre in prerequisites:
            prereq[crs].append(pre)

        # List to store the final course order
        output = []
        
        # Set to track visited courses
        visit = set()
        
        # Set to track courses in the current DFS path (for cycle detection)
        cycle = set()

        # Depth-First Search function to build course order
        def dfs(crs):
            # Cycle detected, invalid course order
            if crs in cycle:
                return False
            
            # Course already processed
            if crs in visit:
                return True

            # Mark course as part of current path
            cycle.add(crs)

            # Recursively process all prerequisites
            for pre in prereq[crs]:
                if dfs(pre) == False:
                    return False  # Cycle detected in prerequisites

            # Remove course from current path
            cycle.remove(crs)

            # Mark course as visited
            visit.add(crs)

            # Add course to output (postorder)
            output.append(crs)
            return True

        # Run DFS for all courses
        for c in range(numCourses):
            if dfs(c) == False:
                return []  # Return empty list if cycle detected

        # Return the valid course order
        return output
```

### APPROACH: TOPOLOGICAL SORT (DFS)

```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        # Initialize adjacency list for graph representation
        adj = [[] for _ in range(numCourses)]
        
        # Initialize indegree list to count prerequisites for each course
        indegree = [0] * numCourses
        
        # Build graph and compute indegrees
        for nxt, pre in prerequisites:
            indegree[nxt] += 1  # Count prerequisite for course 'nxt'
            adj[pre].append(nxt)  # Add 'nxt' as a neighbor of 'pre'

        # List to store the final course order
        output = []

        # Depth-First Search function to process courses
        def dfs(node):
            # Add current course to the output list
            output.append(node)

            # Decrease indegree of the current node (mark as visited)
            indegree[node] -= 1

            # Iterate through neighbors (courses that depend on current course)
            for nei in adj[node]:
                indegree[nei] -= 1  # Decrease indegree for dependent course
                # If indegree becomes zero, it can now be taken
                if indegree[nei] == 0:
                    dfs(nei)

        # Start DFS for courses with no prerequisites
        for i in range(numCourses):
            if indegree[i] == 0:
                dfs(i)

        # If all courses are added to the output, return it; otherwise, return empty list
        return output if len(output) == numCourses else []
```
