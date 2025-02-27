# 57. Insert Interval
Difficulty: Medium

## QUESTION

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [start_i, end_i]` represents the start and the end time of the `ith` interval. `intervals` is initially sorted in ascending order by `start_i`.

You are given another interval `newInterval = [start, end]`.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `start_i` and also `intervals` still does not have any overlapping intervals. You may merge the overlapping intervals if needed.

Return `intervals` after adding `newInterval`.

Note: Intervals are non-overlapping if they have no common point. For example, [1,2] and [3,4] are non-overlapping, but [1,2] and [2,3] are overlapping.

### EXAMPLE

```
Input: intervals = [[1,3],[4,6]], newInterval = [2,5]
Output: [[1,6]]
```

```
Input: intervals = [[1,2],[3,5],[9,10]], newInterval = [6,7]
Output: [[1,2],[3,5],[6,7],[9,10]]
```

## SOLUTION


### APPROACH: LINEAR SEARCH

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        n = len(intervals)
        i = 0
        res = []

        # Add all intervals that end before the new interval starts
        while i < n and intervals[i][1] < newInterval[0]:
            res.append(intervals[i])
            i += 1

        # Merge overlapping intervals with the new interval
        while i < n and newInterval[1] >= intervals[i][0]:
            newInterval[0] = min(newInterval[0], intervals[i][0])  # Expand start
            newInterval[1] = max(newInterval[1], intervals[i][1])  # Expand end
            i += 1
        
        # Insert the merged new interval
        res.append(newInterval)

        # Add remaining intervals after the merged interval
        while i < n:
            res.append(intervals[i])
            i += 1

        return res
```

### APPROACH: BINARY SEARCH

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        # If there are no existing intervals, return the new interval
        if not intervals:
            return [newInterval]

        n = len(intervals)
        target = newInterval[0]
        left, right = 0, n - 1

        # Binary search to find the correct insertion index for newInterval
        while left <= right:
            mid = (left + right) // 2
            if intervals[mid][0] < target:
                left = mid + 1
            else:
                right = mid - 1

        # Insert the new interval at the found position
        intervals.insert(left, newInterval)

        # Merge overlapping intervals
        res = []
        for interval in intervals:
            if not res or res[-1][1] < interval[0]:  # No overlap, add new interval
                res.append(interval)
            else:  # Merge overlapping intervals
                res[-1][1] = max(res[-1][1], interval[1])

        return res
```

### APPROACH: GREEDY

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        res = []

        for i in range(len(intervals)):
            # If new interval ends before the current interval starts, insert and return the rest
            if newInterval[1] < intervals[i][0]:
                res.append(newInterval)
                return res + intervals[i:]

            # If new interval starts after the current interval ends, add current interval
            elif newInterval[0] > intervals[i][1]:
                res.append(intervals[i])

            # Merge overlapping intervals
            else:
                newInterval = [
                    min(newInterval[0], intervals[i][0]),  # Take the smaller start
                    max(newInterval[1], intervals[i][1]),  # Take the larger end
                ]

        # Add the final merged interval
        res.append(newInterval)
        return res
```
