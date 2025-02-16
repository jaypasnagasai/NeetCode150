# 973. K Closest Points to Origin
Difficulty: Medium

## QUESTION

Design a class to find the `kth` largest integer in a stream of values, including duplicates. E.g. the `2nd` largest from [1, 2, 3, 3] is 3. The stream is not necessarily sorted.

Implement the following methods:
- `constructor(int k, int[] nums)` Initializes the object given an integer `k` and the stream of integers `nums`.
- `int add(int val)` Adds the integer `val` to the stream and returns the `kth` largest integer in the stream.

### EXAMPLE

```
Input:
["KthLargest", [3, [1, 2, 3, 3]], "add", [3], "add", [5], "add", [6], "add", [7], "add", [8]]

Output:
[null, 3, 3, 3, 5, 6]

Explanation:
KthLargest kthLargest = new KthLargest(3, [1, 2, 3, 3]);
kthLargest.add(3);   // return 3
kthLargest.add(5);   // return 3
kthLargest.add(6);   // return 3
kthLargest.add(7);   // return 5
kthLargest.add(8);   // return 6
```

## SOLUTION


### APPROACH: SORTING

```python
import heapq

class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        """
        Initializes the KthLargest object with an integer k and a list of numbers.

        :param k: The kth largest element to track.
        :param nums: Initial list of numbers.
        """
        self.k = k  # Store k
        self.min_heap = nums  # Store the numbers in a heap

        heapq.heapify(self.min_heap)  # Convert nums into a min-heap
        while len(self.min_heap) > k:  # Maintain only k largest elements
            heapq.heappop(self.min_heap)

    def add(self, val: int) -> int:
        """
        Adds a new value to the stream and returns the kth largest element.

        :param val: The value to be added.
        :return: The kth largest element after insertion.
        """
        heapq.heappush(self.min_heap, val)  # Add the new value
        if len(self.min_heap) > self.k:  # Maintain only k largest elements
            heapq.heappop(self.min_heap)

        return self.min_heap[0]  # The kth largest element is at the root of the min-heap
```

### APPROACH: MIN HEAP

```python
import heapq

class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        """
        Initializes the KthLargest object with an integer k and a list of numbers.

        :param k: The kth largest element to track.
        :param nums: Initial list of numbers.
        """
        self.minHeap, self.k = nums, k  # Store the heap and k value
        heapq.heapify(self.minHeap)  # Convert nums into a min-heap

        # Maintain only k largest elements in the min-heap
        while len(self.minHeap) > k:
            heapq.heappop(self.minHeap)

    def add(self, val: int) -> int:
        """
        Adds a new value to the stream and returns the kth largest element.

        :param val: The value to be added.
        :return: The kth largest element after insertion.
        """
        heapq.heappush(self.minHeap, val)  # Add the new value to the heap
        if len(self.minHeap) > self.k:  # Ensure heap contains only k elements
            heapq.heappop(self.minHeap)  # Remove the smallest element

        return self.minHeap[0]  # The kth largest element is at the root of the min-heap
```

