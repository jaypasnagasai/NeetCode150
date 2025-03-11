# 48. Rotate Image
Difficulty: Medium

## QUESTION

Given a square `n x n` matrix of integers `matrix`, rotate it by 90 degrees clockwise.

You must rotate the matrix in-place. Do not allocate another 2D matrix and do the rotation.

### EXAMPLE

```
Input: matrix = [
  [1,2],
  [3,4]
]

Output: [
  [3,1],
  [4,2]
]
```

```
Input: matrix = [
  [1,2,3],
  [4,5,6],
  [7,8,9]
]

Output: [
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Rotates the given n x n 2D matrix 90 degrees clockwise in-place.
        """
        n = len(matrix)
        # Create an auxiliary matrix to store the rotated values
        rotated = [[0] * n for _ in range(n)]
        
        # Populate the rotated matrix by shifting elements to their new positions
        for i in range(n):
            for j in range(n):
                rotated[j][n - 1 - i] = matrix[i][j]
        
        # Copy the rotated matrix back to the original matrix
        for i in range(n):
            for j in range(n):
                matrix[i][j] = rotated[i][j]
```

### APPROACH: ROTATE BY FOUR CELLS

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Rotates the given n x n 2D matrix 90 degrees clockwise in-place.
        """
        l, r = 0, len(matrix) - 1  # Define the left and right boundary indices

        # Process layers from outer to inner
        while l < r:
            for i in range(r - l):
                top, bottom = l, r  # Define the top and bottom boundary indices

                # Save the top-left element
                topLeft = matrix[top][l + i]

                # Move bottom-left to top-left
                matrix[top][l + i] = matrix[bottom - i][l]

                # Move bottom-right to bottom-left
                matrix[bottom - i][l] = matrix[bottom][r - i]

                # Move top-right to bottom-right
                matrix[bottom][r - i] = matrix[top + i][r]

                # Move top-left (saved value) to top-right
                matrix[top + i][r] = topLeft

            # Move inward to process the next layer
            r -= 1
            l += 1
```

### APPROACH: REVERSE AND TRANSPOSE

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Rotates the given n x n 2D matrix 90 degrees clockwise in-place.
        """

        # Reverse the matrix vertically to simulate a 90-degree rotation
        matrix.reverse()

        # Transpose the matrix by swapping elements across the diagonal
        for i in range(len(matrix)):
            for j in range(i + 1, len(matrix)):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```
