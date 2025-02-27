# 920. Meeting Rooms
Difficulty: Easy

## QUESTION

Given an array of meeting time interval objects consisting of start and end times `[[start_1,end_1],[start_2,end_2],...]` `(start_i < end_i)`, determine if a person could add all meetings to their schedule without any conflicts.

### EXAMPLE

```
Input: intervals = [(0,30),(5,10),(15,20)]
Output: false
```

```
Input: intervals = [(5,8),(9,15)]
Output: true
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
"""
Definition of Interval:
class Interval(object):
    def __init__(self, start, end):
        self.start = start
        self.end = end
"""

class Solution:
    def canAttendMeetings(self, intervals: List[Interval]) -> bool:
        # Get the number of intervals
        n = len(intervals)
        
        # Brute-force approach: Check for overlapping intervals
        for i in range(n):
            A = intervals[i]
            for j in range(i + 1, n):
                B = intervals[j]
                # Check if two intervals overlap
                if min(A.end, B.end) > max(A.start, B.start):
                    return False  # Overlapping intervals found, return False
        
        return True  # No overlapping intervals, return True
```

### APPROACH: SORTING

```python
"""
Definition of Interval:
class Interval(object):
    def __init__(self, start, end):
        self.start = start
        self.end = end
"""

class Solution:
    def canAttendMeetings(self, intervals: List[Interval]) -> bool:
        # Sort intervals based on their start time
        intervals.sort(key=lambda i: i.start)

        # Check for overlapping intervals
        for i in range(1, len(intervals)):
            i1 = intervals[i - 1]
            i2 = intervals[i]

            # If the previous meeting's end time overlaps with the next meeting's start time
            if i1.end > i2.start:
                return False  # Overlapping intervals found, return False

        return True  # No overlapping intervals, return True
```

