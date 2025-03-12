# 2013. Detect Squares
Difficulty: Medium

## QUESTION

You are given a stream of points consisting of x-y coordinates on a 2-D plane. Points can be added and queried as follows:

- Add - new points can be added to the stream into a data structure. Duplicate points are allowed and should be treated as separate points.
- Query - Given a single query point, count the number of ways to choose three additional points from the data structure such that the three points and the query point form a square. The square must have all sides parallel to the x-axis and y-axis, i.e. no diagonal squares are allowed. Recall that a square must have four equal sides.

Implement the `CountSquares` class:

- `CountSquares()` Initializes the object.
- `void add(int[] point)` Adds a new point `point = [x, y]`.
- `int count(int[] point)` Counts the number of ways to form valid squares with point `point = [x, y]` as described above.

### EXAMPLE

```
Input: 
["CountSquares", "add", [[1, 1]], "add", [[2, 2]], "add", [[1, 2]], "count", [[2, 1]], "count", [[3, 3]], "add", [[2, 2]], "count", [[2, 1]]]
       
Output:
[null, null, null, null, 1, 0, null, 2]

Explanation:
CountSquares countSquares = new CountSquares();
countSquares.add([1, 1]);
countSquares.add([2, 2]);
countSquares.add([1, 2]);

countSquares.count([2, 1]);   // return 1.
countSquares.count([3, 3]);   // return 0.
countSquares.add([2, 2]);     // Duplicate points are allowed.
countSquares.count([2, 1]);   // return 2. 
```

## SOLUTION


### APPROACH: HASH MAP

```python
from collections import defaultdict
from typing import List

class CountSquares:
    def __init__(self):
        # Dictionary to store the count of points at each (x, y) coordinate
        self.ptsCount = defaultdict(lambda: defaultdict(int))

    def add(self, point: List[int]) -> None:
        """
        Adds a point to the data structure.
        Increments the count of occurrences for the given (x, y) point.
        """
        self.ptsCount[point[0]][point[1]] += 1

    def count(self, point: List[int]) -> int:
        """
        Counts the number of squares that can be formed using the given point
        as one of the corners.
        """
        res = 0
        x1, y1 = point  # The given point

        # Iterate over all y-coordinates that exist for x1
        for y2 in self.ptsCount[x1]:
            side = y2 - y1  # Compute the side length of the potential square
            
            if side == 0:
                continue  # Skip if the side length is 0 (same point)

            # Compute the potential x-coordinates for completing the square
            x3, x4 = x1 + side, x1 - side

            # Count squares where the third and fourth points are at (x3, y1) and (x3, y2)
            res += (self.ptsCount[x1][y2] * self.ptsCount[x3][y1] *
                    self.ptsCount[x3][y2])

            # Count squares where the third and fourth points are at (x4, y1) and (x4, y2)
            res += (self.ptsCount[x1][y2] * self.ptsCount[x4][y1] *
                    self.ptsCount[x4][y2])

        return res
```
