# 74. Search A 2D Matrix
Difficulty: Medium

## QUESTION

You are given an `m x n` 2-D integer array `matrix` and an integer `target`.

- Each row in `matrix` is sorted in non-decreasing order.
- The first integer of every row is greater than the last integer of the previous row.

Return `true` if `target` exists within `matrix` or `false` otherwise.

Can you write a solution that runs in `O(log(m * n))` time?

### EXAMPLE

```
Input: matrix = [[1,2,4,8],[10,11,12,13],[14,20,30,40]], target = 10
Output: true
```

```
Input: matrix = [[1,2,4,8],[10,11,12,13],[14,20,30,40]], target = 15
Output: false
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'searchMatrix' to check if a target exists in a given 2D matrix.
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        # Iterate through each row in the matrix.
        for r in range(len(matrix)):
            # Iterate through each column in the current row.
            for c in range(len(matrix[0])):
                # If the current element matches the target, return True.
                if matrix[r][c] == target:
                    return True
        # If the target is not found, return False.
        return False
```

### APPROACH: STAIRCASE SEARCH

```python
class Solution:
    # Define a method 'searchMatrix' to check if a target exists in a given 2D matrix.
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        # Get the number of rows and columns in the matrix.
        m, n = len(matrix), len(matrix[0])
        
        # Start from the top-right corner of the matrix.
        r, c = 0, n - 1

        # Traverse the matrix while staying within bounds.
        while r < m and c >= 0:
            # If the current element is greater than the target, move left.
            if matrix[r][c] > target:
                c -= 1
            # If the current element is smaller than the target, move down.
            elif matrix[r][c] < target:
                r += 1
            # If the current element matches the target, return True.
            else:
                return True
        
        # If the target is not found, return False.
        return False
```

### APPROACH: BINARY SEARCH

```python
class Solution:
    # Define a method 'searchMatrix' to check if a target exists in a sorted 2D matrix.
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        # Get the number of rows and columns in the matrix.
        ROWS, COLS = len(matrix), len(matrix[0])

        # Perform binary search to find the correct row where the target could be present.
        top, bot = 0, ROWS - 1
        while top <= bot:
            row = (top + bot) // 2  # Find the middle row.
            if target > matrix[row][-1]:  # If target is greater than the last element of the row, move down.
                top = row + 1
            elif target < matrix[row][0]:  # If target is smaller than the first element, move up.
                bot = row - 1
            else:
                break  # If the target is within the row range, stop searching.

        # If no valid row was found, return False.
        if not (top <= bot):
            return False
        
        # The row where the target might exist.
        row = (top + bot) // 2

        # Perform binary search within the found row.
        l, r = 0, COLS - 1
        while l <= r:
            m = (l + r) // 2  # Find the middle column.
            if target > matrix[row][m]:  # If target is greater, move right.
                l = m + 1
            elif target < matrix[row][m]:  # If target is smaller, move left.
                r = m - 1
            else:
                return True  # Found the target.

        # If not found in the row, return False.
        return False
```
