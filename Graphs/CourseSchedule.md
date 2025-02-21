# 207. Course Schedule
Difficulty: Medium

## QUESTION

You are given an array `prerequisites` where `prerequisites[i] = [a, b]` indicates that you must take course `b` first if you want to take course `a`.

The pair `[0, 1]`, indicates that must take course `1` before taking course `0`.

There are a total of `numCourses` courses you are required to take, labeled from `0` to `numCourses - 1`.

Return `true` if it is possible to finish all courses, otherwise return `false`.

### EXAMPLE

```
Input: numCourses = 2, prerequisites = [[0,1]]
Output: true
```

```
Input: numCourses = 2, prerequisites = [[0,1],[1,0]]
Output: false
```

## SOLUTION


### APPROACH: CYCLE DETECTION (DFS)

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # Map each course to its prerequisites
        preMap = {i: [] for i in range(numCourses)}
        for crs, pre in prerequisites:
            preMap[crs].append(pre)

        # Set to track courses currently in the DFS path
        visiting = set()

        # Depth-First Search function to detect cycles
        def dfs(crs):
            # If course is already in the current path, a cycle is detected
            if crs in visiting:
                return False

            # If no prerequisites left, course can be completed
            if preMap[crs] == []:
                return True

            # Mark course as visiting
            visiting.add(crs)

            # Recursively visit prerequisites
            for pre in preMap[crs]:
                if not dfs(pre):
                    return False

            # Remove course from visiting after traversal
            visiting.remove(crs)

            # Clear prerequisites for course (memoization)
            preMap[crs] = []

            return True

        # Perform DFS for each course
        for c in range(numCourses):
            if not dfs(c):
                return False  # Cycle detected, cannot finish all courses

        # No cycles detected, all courses can be completed
        return True
```

### APPROACH: TOPOLOGICAL SORT (KAHN'S ALGORITHM)

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # Initialize indegree array to count prerequisites for each course
        indegree = [0] * numCourses
        
        # Create adjacency list to store course dependencies
        adj = [[] for _ in range(numCourses)]
        for src, dst in prerequisites:
            indegree[dst] += 1  # Increment indegree for destination course
            adj[src].append(dst)  # Add destination course to source's adjacency list

        # Initialize queue with courses having no prerequisites (indegree == 0)
        q = deque()
        for n in range(numCourses):
            if indegree[n] == 0:
                q.append(n)

        # Counter to track number of completed courses
        finish = 0

        # Perform BFS to process courses
        while q:
            node = q.popleft()  # Take course with no remaining prerequisites
            finish += 1  # Increment completed courses count

            # Reduce indegree for neighboring courses
            for nei in adj[node]:
                indegree[nei] -= 1  # Remove current course from prerequisites
                if indegree[nei] == 0:
                    q.append(nei)  # Add course to queue if it has no more prerequisites

        # If all courses are finished, return True; otherwise, False
        return finish == numCourses
```
