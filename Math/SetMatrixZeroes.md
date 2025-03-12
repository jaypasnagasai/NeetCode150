# 73. Set Matrix Zeroes
Difficulty: Medium

## QUESTION

Given an `m x n` matrix of integers `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must update the matrix in-place.

Follow up: Could you solve it using `O(1)` space?

### EXAMPLE

```
Input: matrix = [
  [0,1],
  [1,0]
]

Output: [
  [0,0],
  [0,0]
]
```

```
Input: matrix = [
  [1,2,3],
  [4,0,5],
  [6,7,8]
]

Output: [
  [1,0,3],
  [0,0,0],
  [6,0,8]
]
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
from typing import List

class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Modifies the given matrix in-place such that if an element is 0, its entire row and column are set to 0.
        """
        
        # Get the number of rows and columns in the matrix
        ROWS, COLS = len(matrix), len(matrix[0])
        
        # Create a copy of the matrix to store changes separately
        mark = [[matrix[r][c] for c in range(COLS)] for r in range(ROWS)]

        # Iterate through the matrix to find zeroes
        for r in range(ROWS):
            for c in range(COLS):
                if matrix[r][c] == 0:
                    # Set the entire row to zero in the copied matrix
                    for col in range(COLS):
                        mark[r][col] = 0
                    # Set the entire column to zero in the copied matrix
                    for row in range(ROWS):
                        mark[row][c] = 0

        # Copy the modified values back to the original matrix
        for r in range(ROWS):
            for c in range(COLS):
                matrix[r][c] = mark[r][c]
```

### APPROACH: ITERATION

```python
from typing import List

class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Modifies the given matrix in-place such that if an element is 0, 
        its entire row and column are set to 0 without using extra space.
        """

        # Get the dimensions of the matrix
        ROWS, COLS = len(matrix), len(matrix[0])
        
        # Flag to check if the first row needs to be set to zero
        rowZero = False  

        # First pass: Mark rows and columns that need to be zeroed
        for r in range(ROWS):
            for c in range(COLS):
                if matrix[r][c] == 0:
                    # Mark the column for zeroing using the first row
                    matrix[0][c] = 0

                    if r > 0:
                        # Mark the row for zeroing using the first column
                        matrix[r][0] = 0
                    else:
                        # If a zero is found in the first row, track it separately
                        rowZero = True  

        # Second pass: Use marks in the first row and column to set matrix elements to zero
        for r in range(1, ROWS):  # Start from row 1 (skip first row)
            for c in range(1, COLS):  # Start from column 1 (skip first column)
                if matrix[0][c] == 0 or matrix[r][0] == 0:
                    matrix[r][c] = 0  # Zero out the cell

        # Handle the first column separately if needed
        if matrix[0][0] == 0:
            for r in range(ROWS):
                matrix[r][0] = 0

        # Handle the first row separately if needed
        if rowZero:
            for c in range(COLS):
                matrix[0][c] = 0
```
