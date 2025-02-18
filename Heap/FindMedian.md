# 295. Find Median from Data Stream
Difficulty: Hard

## QUESTION

The median is the middle value in a sorted list of integers. For lists of even length, there is no middle value, so the median is the mean of the two middle values.

For example:

- For `arr = [1,2,3]`, the median is `2`.
- For `arr = [1,2]`, the median is `(1 + 2) / 2 = 1.5`

Implement the MedianFinder class:
- `MedianFinder()` initializes the `MedianFinder` object.
- `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
- `double findMedian()` returns the median of all elements so far.

### EXAMPLE

```
Input:
["MedianFinder", "addNum", "1", "findMedian", "addNum", "3" "findMedian", "addNum", "2", "findMedian"]

Output:
[null, null, 1.0, null, 2.0, null, 2.0]

Explanation:
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.findMedian(); // return 1.0
medianFinder.addNum(3);    // arr = [1, 3]
medianFinder.findMedian(); // return 2.0
medianFinder.addNum(2);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

## SOLUTION


### APPROACH: SORTING

```python
class MedianFinder:
    def __init__(self):
        # Store all numbers
        self.data = []

    def addNum(self, num: int) -> None:
        # Insert the number into the list
        self.data.append(num)

    def findMedian(self) -> float:
        # Sort the list to access middle elements
        self.data.sort()
        n = len(self.data)
        # If the list has an odd length, return the middle element
        # Otherwise return the average of the two middle elements
        return (self.data[n // 2] if (n & 1) 
                else (self.data[n // 2] + self.data[n // 2 - 1]) / 2)
```

### APPROACH: HEAP

```python
import heapq

class MedianFinder:
    def __init__(self):
        # Use two heaps: a max heap (small) and a min heap (large)
        # The size of both heaps differs by at most 1
        self.small, self.large = [], []

    def addNum(self, num: int) -> None:
        # Decide which heap to push the new number into
        if self.large and num > self.large[0]:
            heapq.heappush(self.large, num)
        else:
            heapq.heappush(self.small, -num)
        
        # Balance the heaps if one is bigger by more than 1 element
        if len(self.small) > len(self.large) + 1:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        if len(self.large) > len(self.small) + 1:
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -val)

    def findMedian(self) -> float:
        # If one heap has more elements, its top is the median
        if len(self.small) > len(self.large):
            return -self.small[0]
        elif len(self.large) > len(self.small):
            return self.large[0]
        # Otherwise, the median is the average of both tops
        return (-self.small[0] + self.large[0]) / 2.0
```

