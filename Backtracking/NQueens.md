# 51. N-Queens
Difficulty: Hard

## QUESTION

The n-queens puzzle is the problem of placing `n` queens on an `n x n` chessboard so that no two queens can attack each other.

A queen in a chessboard can attack horizontally, vertically, and diagonally.

Given an integer ` n`, return all distinct solutions to the n-queens puzzle.

Each solution contains a unique board layout where the queen pieces are placed. `'Q'` indicates a queen and `'.'` indicates an empty space.

You may return the answer in any order.

### EXAMPLE

```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
```

```
Input: n = 1
Output: [["Q"]]
```

## SOLUTION


### APPROACH: BACKTRACKING

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        # Store all valid solutions
        res = []
        # Initialize the board
        board = [["."] * n for _ in range(n)]

        def backtrack(r):
            # If queens are placed in all rows, add board configuration
            if r == n:
                temp = ["".join(row) for row in board]
                res.append(temp)
                return
            # Try placing a queen in each column of the current row
            for c in range(n):
                if self.isSafe(r, c, board):
                    board[r][c] = "Q"
                    backtrack(r + 1)
                    board[r][c] = "."

        # Start the recursion from the first row
        backtrack(0)
        return res

    def isSafe(self, r: int, c: int, board):
        # Check if a queen exists in the same column above
        row = r - 1
        while row >= 0:
            if board[row][c] == "Q":
                return False
            row -= 1

        # Check the upper-left diagonal
        row, col = r - 1, c - 1
        while row >= 0 and col >= 0:
            if board[row][col] == "Q":
                return False
            row -= 1
            col -= 1

        # Check the upper-right diagonal
        row, col = r - 1, c + 1
        while row >= 0 and col < len(board):
            if board[row][col] == "Q":
                return False
            row -= 1
            col += 1

        return True
```

