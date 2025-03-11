# 54. Spiral Matrix
Difficulty: Medium

## QUESTION

Given an `m x n` matrix of integers `matrix`, return a list of all elements within the matrix in spiral order.

### EXAMPLE

```
Input: matrix = [[1,2],[3,4]]
Output: [1,2,4,3]
```

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
```

```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```
## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        """
        Returns the elements of the given 2D matrix in spiral order.
        """
        m, n = len(matrix), len(matrix[0])  # Get matrix dimensions
        res = []  # List to store the spiral order elements

        # Recursive function to traverse the matrix in the given direction
        def dfs(row, col, r, c, dr, dc):
            if row == 0 or col == 0:
                return  # Base case: stop when no more elements remain
            
            # Traverse in the given direction and collect elements
            for i in range(col):
                r += dr
                c += dc
                res.append(matrix[r][c])

            # Recursive call to traverse the next layer by swapping row/col
            dfs(col, row - 1, r, c, dc, -dr)

        # Start traversal by moving to the right
        dfs(m, n, 0, -1, 0, 1)
        return res  # Return the collected elements in spiral order
```

### APPROACH: ITERATION

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        """
        Returns the elements of the given 2D matrix in spiral order.
        """
        res = []  # List to store the spiral order elements

        # Define movement directions: right, down, left, up
        directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        
        # Steps to take in each direction before switching
        steps = [len(matrix[0]), len(matrix) - 1]

        # Initialize row, column, and direction index
        r, c, d = 0, -1, 0

        # Continue until there are steps left in the current direction
        while steps[d & 1]:
            for i in range(steps[d & 1]):  # Traverse in the current direction
                r += directions[d][0]
                c += directions[d][1]
                res.append(matrix[r][c])
            
            # Reduce steps for the current direction after completing a pass
            steps[d & 1] -= 1
            d += 1  # Move to the next direction
            d %= 4  # Ensure direction stays within range [0,3]

        return res  # Return the collected elements in spiral order
```
