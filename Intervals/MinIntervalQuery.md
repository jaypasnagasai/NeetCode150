# 1851. Minimum Interval to Include Each Query
Difficulty: Hard

## QUESTION

You are given a 2D integer array `intervals`, where `intervals[i] = [left_i, right_i]` represents the `ith` interval starting at `left_i` and ending at `right_i` (inclusive).

You are also given an integer array of query points `queries`. The result of `query[j]` is the length of the shortest interval `i` such that `left_i <= queries[j] <= right_i`. If no such interval exists, the result of this query is `-1`.

Return an array `output` where `output[j]` is the result of `query[j]`.

Note: The length of an interval is calculated as `right_i - left_i + 1`.

### EXAMPLE

```
Input: intervals = [[1,3],[2,3],[3,7],[6,6]], queries = [2,3,1,7,6,8]
Output: [2,2,3,5,1,-1]
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def minInterval(self, intervals: List[List[int]], queries: List[int]) -> List[int]:
        n = len(intervals)
        res = []

        # Iterate through each query
        for q in queries:
            cur = -1  # Initialize the minimum interval size as -1 (no valid interval found)

            # Check all intervals to find the smallest one that contains the query
            for l, r in intervals:
                if l <= q <= r:  # Check if the query falls within the interval
                    if cur == -1 or (r - l + 1) < cur:  # Update if a smaller interval is found
                        cur = r - l + 1
            
            # Store the smallest interval length that contains the query
            res.append(cur)

        return res
```

### APPROACH: MIN HEAP

```python
import heapq

class Solution:
    def minInterval(self, intervals: List[List[int]], queries: List[int]) -> List[int]:
        # Sort intervals by their start time
        intervals.sort()
        # Min heap to keep track of valid intervals sorted by size
        minHeap = []
        # Dictionary to store results for each query
        res = {}
        i = 0  # Pointer for intervals

        # Process queries in sorted order
        for q in sorted(queries):
            # Add all intervals that start before or at the query point
            while i < len(intervals) and intervals[i][0] <= q:
                l, r = intervals[i]
                heapq.heappush(minHeap, (r - l + 1, r))  # Store interval size and right boundary
                i += 1

            # Remove intervals from the heap that end before the query point
            while minHeap and minHeap[0][1] < q:
                heapq.heappop(minHeap)

            # Store the smallest valid interval size for the query
            res[q] = minHeap[0][0] if minHeap else -1

        # Return results in the original order of queries
        return [res[q] for q in queries]
```

### APPROACH: LAZY PROPOGATION

```python
class SegmentTree:
    def __init__(self, N):
        self.n = N
        self.tree = [float('inf')] * (4 * N)
        self.lazy = [float('inf')] * (4 * N)

    def propagate(self, treeidx, lo, hi):
        """ Propagate lazy updates to child nodes. """
        if self.lazy[treeidx] != float('inf'):
            self.tree[treeidx] = min(self.tree[treeidx], self.lazy[treeidx])
            if lo != hi:
                self.lazy[2 * treeidx + 1] = min(self.lazy[2 * treeidx + 1], self.lazy[treeidx])
                self.lazy[2 * treeidx + 2] = min(self.lazy[2 * treeidx + 2], self.lazy[treeidx])
            self.lazy[treeidx] = float('inf')

    def update(self, treeidx, lo, hi, left, right, val):
        """ Update a range with the given value. """
        self.propagate(treeidx, lo, hi)
        if lo > right or hi < left:
            return
        if lo >= left and hi <= right:
            self.lazy[treeidx] = min(self.lazy[treeidx], val)
            self.propagate(treeidx, lo, hi)
            return
        mid = (lo + hi) // 2
        self.update(2 * treeidx + 1, lo, mid, left, right, val)
        self.update(2 * treeidx + 2, mid + 1, hi, left, right, val)
        self.tree[treeidx] = min(self.tree[2 * treeidx + 1], self.tree[2 * treeidx + 2])

    def query(self, treeidx, lo, hi, idx):
        """ Query the minimum value at a specific index. """
        self.propagate(treeidx, lo, hi)
        if lo == hi:
            return self.tree[treeidx]
        mid = (lo + hi) // 2
        if idx <= mid:
            return self.query(2 * treeidx + 1, lo, mid, idx)
        else:
            return self.query(2 * treeidx + 2, mid + 1, hi, idx)

    def update_range(self, left, right, val):
        """ Public method to update a range. """
        self.update(0, 0, self.n - 1, left, right, val)

    def query_point(self, idx):
        """ Public method to query a point. """
        return self.query(0, 0, self.n - 1, idx)

class Solution:
    def minInterval(self, intervals: List[List[int]], queries: List[int]) -> List[int]:
        # Collect all unique points (start, end, queries)
        points = []
        for interval in intervals:
            points.append(interval[0])
            points.append(interval[1])
        for q in queries:
            points.append(q)

        # Coordinate compression
        points = sorted(set(points))
        compress = {points[i]: i for i in range(len(points))}

        # Initialize Segment Tree
        segTree = SegmentTree(len(points))

        # Update the segment tree with interval lengths
        for interval in intervals:
            start = compress[interval[0]]
            end = compress[interval[1]]
            length = interval[1] - interval[0] + 1
            segTree.update_range(start, end, length)

        # Answer queries using the segment tree
        ans = []
        for q in queries:
            idx = compress[q]
            res = segTree.query_point(idx)
            ans.append(res if res != float('inf') else -1)
        
        return ans
```
