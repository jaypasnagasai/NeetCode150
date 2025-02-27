# 435. Non-overlapping Intervals
Difficulty: Medium

## QUESTION

Given an array of intervals `intervals` where `intervals[i] = [start_i, end_i]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

Note: Intervals are non-overlapping even if they have a common point. For example, `[1, 3]` and `[2, 4]` are overlapping, but `[1, 2]` and `[2, 3]` are non-overlapping.

### EXAMPLE

```
Input: intervals = [[1,2],[2,4],[1,4]]
Output: 1
```

```
Input: intervals = [[1,2],[2,4]]
Output: 0
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        # Sort the intervals based on their start times
        intervals.sort()

        # Recursive DFS function to find the maximum number of non-overlapping intervals
        def dfs(i, prev):
            # Base case: if all intervals are processed, return 0
            if i == len(intervals):
                return 0

            # Case 1: Skip the current interval
            res = dfs(i + 1, prev)

            # Case 2: Include the current interval if it doesn't overlap with the previous one
            if prev == -1 or intervals[prev][1] <= intervals[i][0]:
                res = max(res, 1 + dfs(i + 1, i))

            return res

        # The minimum number of intervals to remove = total intervals - max non-overlapping intervals
        return len(intervals) - dfs(0, -1)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        # Sort intervals based on their end times
        intervals.sort(key=lambda x: x[1])
        n = len(intervals)
        
        # DP array to store the maximum number of non-overlapping intervals
        dp = [0] * n
        dp[0] = 1  # At least one interval can always be taken

        # Binary search function to find the rightmost interval that doesn't overlap
        def bs(r, target):
            l = 0
            while l < r:
                m = (l + r) >> 1
                if intervals[m][1] <= target:
                    l = m + 1
                else:
                    r = m
            return l

        # Iterate through the intervals and update the dp array
        for i in range(1, n):
            idx = bs(i, intervals[i][0])  # Find the last non-overlapping interval
            if idx == 0:
                dp[i] = dp[i - 1]  # If no valid previous interval, use the previous dp value
            else:
                dp[i] = max(dp[i - 1], 1 + dp[idx - 1])  # Take max of skipping or including the interval
        
        # The minimum number of removals = total intervals - max non-overlapping intervals
        return n - dp[n - 1]
```

### APPROACH: GREEDY

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        # Sort intervals based on their end times to minimize removals
        intervals.sort(key=lambda pair: pair[1])

        # Initialize previous interval's end time and removal counter
        prevEnd = intervals[0][1]
        res = 0

        # Iterate through intervals starting from the second one
        for i in range(1, len(intervals)):
            # If the current interval overlaps with the previous one
            if prevEnd > intervals[i][0]:
                res += 1  # Increment removal count
            else:
                prevEnd = intervals[i][1]  # Update previous end time if no overlap

        # Return the minimum number of intervals to remove
        return res
```
