# 703. Kth Largest Element in a Stream
Difficulty: Easy

## QUESTION

You are given an 2-D array `points` where `points[i] = [xi, yi]` represents the coordinates of a point on an X-Y axis plane. You are also given an integer `k`.

Return the `k` closest points to the origin `(0, 0)`.

The distance between two points is defined as the Euclidean distance `(sqrt((x1 - x2)^2 + (y1 - y2)^2))`.

You may return the answer in any order.

### EXAMPLE

```
Input: points = [[0,2],[2,2]], k = 1
Output: [[0,2]]
```

```
Input: points = [[0,2],[2,0],[2,2]], k = 2
Output: [[0,2],[2,0]]
```

## SOLUTION


### APPROACH: SORTING

```python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        """
        Finds the k closest points to the origin (0, 0) using the Euclidean distance.

        :param points: List of [x, y] coordinates.
        :param k: Number of closest points to return.
        :return: List of k closest points.
        """
        # Sort the points based on their squared Euclidean distance from the origin
        points.sort(key=lambda p: p[0]**2 + p[1]**2)
        
        # Return the first k points
        return points[:k]
```

### APPROACH: MIN HEAP

```python
import heapq

class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        """
        Finds the k closest points to the origin (0,0) using a min-heap.

        :param points: List of [x, y] coordinates.
        :param k: Number of closest points to return.
        :return: List of k closest points.
        """
        minHeap = []  # Min-heap to store (distance, x, y) tuples

        # Calculate squared distance for each point and store it in the heap
        for x, y in points:
            dist = (x ** 2) + (y ** 2)  # Squared Euclidean distance
            minHeap.append([dist, x, y])

        heapq.heapify(minHeap)  # Convert list into a heap
        
        res = []  # List to store k closest points
        while k > 0:
            dist, x, y = heapq.heappop(minHeap)  # Extract the closest point
            res.append([x, y])
            k -= 1
            
        return res  # Return the k closest points
```

### APPROACH: MAX HEAP

```python
import heapq

class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        """
        Finds the k closest points to the origin (0,0) using a max-heap.

        :param points: List of [x, y] coordinates.
        :param k: Number of closest points to return.
        :return: List of k closest points.
        """
        maxHeap = []  # Max-heap to store the k closest points

        for x, y in points:
            dist = -(x ** 2 + y ** 2)  # Use negative distance to simulate a max-heap
            heapq.heappush(maxHeap, [dist, x, y])

            # Maintain only k elements in the heap
            if len(maxHeap) > k:
                heapq.heappop(maxHeap)
        
        res = []  # List to store k closest points
        while maxHeap:
            dist, x, y = heapq.heappop(maxHeap)  # Extract the k closest points
            res.append([x, y])

        return res  # Return the k closest points
```

### APPROACH: QUICK SELECT

```python
class Solution:
    def kClosest(self, points, k):
        """
        Finds the k closest points to the origin (0,0) using Quickselect algorithm.

        :param points: List of [x, y] coordinates.
        :param k: Number of closest points to return.
        :return: List of k closest points.
        """
        euclidean = lambda x: x[0] ** 2 + x[1] ** 2  # Squared Euclidean distance function

        def partition(l, r):
            """
            Partition function for Quickselect.

            :param l: Left boundary index.
            :param r: Right boundary index.
            :return: The index of the pivot after partitioning.
            """
            pivotIdx = r  # Choose the rightmost element as pivot
            pivotDist = euclidean(points[pivotIdx])  # Get pivot distance
            i = l  # Pointer for smaller elements
            
            for j in range(l, r):
                if euclidean(points[j]) <= pivotDist:  # Compare distance with pivot
                    points[i], points[j] = points[j], points[i]  # Swap elements
                    i += 1
            
            points[i], points[r] = points[r], points[i]  # Place pivot in correct position
            return i  # Return pivot index

        L, R = 0, len(points) - 1
        pivot = len(points)

        # Quickselect: Repeatedly partition until the k closest points are in the first k indices
        while pivot != k:
            pivot = partition(L, R)
            if pivot < k:
                L = pivot + 1  # Search in the right half
            else:
                R = pivot - 1  # Search in the left half

        return points[:k]  # Return the k closest points
```
