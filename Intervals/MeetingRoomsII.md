# 253. Meeting Rooms II
Difficulty: Medium

## QUESTION

Given an array of meeting time interval objects consisting of start and end times `[[start_1,end_1],[start_2,end_2],...] (start_i < end_i)`, find the minimum number of days required to schedule all meetings without any conflicts.

### EXAMPLE

```
Input: intervals = [(0,40),(5,10),(15,20)]
Output: 2
```

```
Input: intervals = [(4,9)]
Output: 1
```

## SOLUTION


### APPROACH: MIN HEAP

```python
import heapq

"""
Definition of Interval:
class Interval(object):
    def __init__(self, start, end):
        self.start = start
        self.end = end
"""

class Solution:
    def minMeetingRooms(self, intervals: List[Interval]) -> int:
        # Sort intervals based on start time
        intervals.sort(key=lambda x: x.start)

        # Min heap to track the end times of meetings
        min_heap = []

        for interval in intervals:
            # If the earliest meeting in heap has ended before the current starts, remove it
            if min_heap and min_heap[0] <= interval.start:
                heapq.heappop(min_heap)

            # Add current meeting's end time to the heap
            heapq.heappush(min_heap, interval.end)

        # The size of the heap represents the minimum number of rooms required
        return len(min_heap)
```

### APPROACH: TWO POINTERS

```python
"""
Definition of Interval:
class Interval(object):
    def __init__(self, start, end):
        self.start = start
        self.end = end
"""

class Solution:
    def minMeetingRooms(self, intervals: List[Interval]) -> int:
        # Extract and sort start and end times separately
        start = sorted(i.start for i in intervals)
        end = sorted(i.end for i in intervals)
        
        # Variables to track the number of rooms needed
        res = count = 0
        s = e = 0  # Pointers for start and end arrays

        # Process meetings in order of start times
        while s < len(intervals):
            if start[s] < end[e]:  # A new meeting starts before the earliest one ends
                count += 1
                s += 1
            else:  # A meeting ends, freeing up a room
                count -= 1
                e += 1
            
            # Update the maximum number of rooms needed
            res = max(res, count)
        
        return res  # Return the maximum number of rooms required
```

### APPROACH: GREEDY

```python
"""
Definition of Interval:
class Interval(object):
    def __init__(self, start, end):
        self.start = start
        self.end = end
"""

class Solution:
    def minMeetingRooms(self, intervals: List[Interval]) -> int:
        # Create a list of events (start times as +1 and end times as -1)
        time = []
        for i in intervals:
            time.append((i.start, 1))  # Meeting starts, increase room count
            time.append((i.end, -1))   # Meeting ends, decrease room count
        
        # Sort by time, if times are the same, sort by type (end before start)
        time.sort(key=lambda x: (x[0], x[1]))

        # Variables to track max rooms needed
        res = count = 0

        # Iterate over sorted times, updating active meetings
        for t in time:
            count += t[1]  # Increase or decrease active meetings
            res = max(res, count)  # Update max rooms needed

        return res  # Return the maximum number of rooms needed at any point
```
