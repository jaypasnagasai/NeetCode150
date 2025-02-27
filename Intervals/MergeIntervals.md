# 56. Merge Intervals
Difficulty: Medium

## QUESTION

Given an array of `intervals` where `intervals[i] = [start_i, end_i]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

You may return the answer in any order.

Note: Intervals are non-overlapping if they have no common point. For example, `[1, 2]` and `[3, 4]` are non-overlapping, but `[1, 2]` and `[2, 3]` are overlapping.

### EXAMPLE

```
Input: intervals = [[1,3],[1,5],[6,7]]
Output: [[1,5],[6,7]]
```

```
Input: intervals = [[1,2],[2,3]]
Output: [[1,3]]
```

## SOLUTION


### APPROACH: SORTING

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        # Sort intervals by their start times
        intervals.sort(key=lambda pair: pair[0])
        
        # Initialize the output list with the first interval
        output = [intervals[0]]

        # Iterate through the sorted intervals
        for start, end in intervals:
            lastEnd = output[-1][1]  # Get the end of the last interval in output

            # If the current interval overlaps with the last interval, merge them
            if start <= lastEnd:
                output[-1][1] = max(lastEnd, end)  # Extend the last interval's end
            else:
                output.append([start, end])  # No overlap, add the new interval

        return output
```

### APPROACH: SWEEP LINE ALGORITHM

```python
from collections import defaultdict

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        # Dictionary to store the count of interval starts and ends
        mp = defaultdict(int)

        # Populate the map with frequency changes
        for start, end in intervals:
            mp[start] += 1  # Increment at start of interval
            mp[end] -= 1  # Decrement at end of interval

        # List to store merged intervals
        res = []
        interval = []
        have = 0  # Active interval count

        # Process sorted keys of the map
        for i in sorted(mp):
            if not interval:  # Start of a new interval
                interval.append(i)
            
            have += mp[i]  # Update active interval count

            if have == 0:  # End of a merged interval
                interval.append(i)
                res.append(interval)  # Store the completed interval
                interval = []  # Reset for next interval

        return res
```

### APPROACH: GREEDY

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        # Find the maximum starting value in the intervals
        max_val = max(interval[0] for interval in intervals)
        
        # Create an array to track the furthest end time for each start point
        mp = [0] * (max_val + 1)
        
        # Populate the array with the maximum end+1 values for each start point
        for start, end in intervals:
            mp[start] = max(end + 1, mp[start])

        res = []
        have = -1  # Track the latest end of the current merged interval
        interval_start = -1  # Track the start of the current interval

        # Iterate over the map to merge intervals
        for i in range(len(mp)):
            if mp[i] != 0:  # If there's an interval starting at index i
                if interval_start == -1:
                    interval_start = i  # Mark the start of a new interval
                have = max(mp[i] - 1, have)  # Extend the end of the interval
            
            if have == i:  # If the interval has ended
                res.append([interval_start, have])
                have = -1
                interval_start = -1

        # Handle the case where the last interval extends beyond the loop
        if interval_start != -1:
            res.append([interval_start, have])

        return res
```
