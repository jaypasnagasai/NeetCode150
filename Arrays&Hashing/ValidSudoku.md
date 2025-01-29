# 000. Valid Sudoku
Difficulty: Medium

## QUESTION

You are given a a `9 x 9` Sudoku board `board`. A Sudoku board is valid if the following rules are followed:

- Each row must contain the digits `1-9` without duplicates.
- Each column must contain the digits `1-9` without duplicates.
- Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without duplicates.

Return `true` if the Sudoku board is valid, otherwise return `false`.

Note: A board does not need to be full or be solvable to be valid.

### EXAMPLE

```
Input: board = 
[["1","2",".",".","3",".",".",".","."],
 ["4",".",".","5",".",".",".",".","."],
 [".","9","8",".",".",".",".",".","3"],
 ["5",".",".",".","6",".",".",".","4"],
 [".",".",".","8",".","3",".",".","5"],
 ["7",".",".",".","2",".",".",".","6"],
 [".",".",".",".",".",".","2",".","."],
 [".",".",".","4","1","9",".",".","8"],
 [".",".",".",".","8",".",".","7","9"]]

Output: true
```

```
Input: board = 
[["1","2",".",".","3",".",".",".","."],
 ["4",".",".","5",".",".",".",".","."],
 [".","9","1",".",".",".",".",".","3"],
 ["5",".",".",".","6",".",".",".","4"],
 [".",".",".","8",".","3",".",".","5"],
 ["7",".",".",".","2",".",".",".","6"],
 [".",".",".",".",".",".","2",".","."],
 [".",".",".","4","1","9",".",".","8"],
 [".",".",".",".","8",".",".","7","9"]]

Output: false

```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'isValidSudoku' to check if a given 9x9 Sudoku board is valid.
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # Check rows for duplicates.
        for row in range(9):
            seen = set()  # Set to track seen numbers in the current row.
            for i in range(9):
                if board[row][i] == ".":  # Ignore empty cells.
                    continue
                if board[row][i] in seen:  # If duplicate found, return False.
                    return False
                seen.add(board[row][i])  # Add the number to the set.

        # Check columns for duplicates.
        for col in range(9):
            seen = set()  # Set to track seen numbers in the current column.
            for i in range(9):
                if board[i][col] == ".":  # Ignore empty cells.
                    continue
                if board[i][col] in seen:  # If duplicate found, return False.
                    return False
                seen.add(board[i][col])  # Add the number to the set.

        # Check 3x3 sub-boxes for duplicates.
        for square in range(9):
            seen = set()  # Set to track seen numbers in the current 3x3 box.
            for i in range(3):  # Iterate over rows within the 3x3 box.
                for j in range(3):  # Iterate over columns within the 3x3 box.
                    row = (square // 3) * 3 + i  # Compute row index.
                    col = (square % 3) * 3 + j  # Compute column index.
                    if board[row][col] == ".":  # Ignore empty cells.
                        continue
                    if board[row][col] in seen:  # If duplicate found, return False.
                        return False
                    seen.add(board[row][col])  # Add the number to the set.

        # If all checks pass, the board is valid.
        return True
```

### APPROACH: HASH SET

```python
class Solution:
    # Define a method 'isValidSudoku' to check if a given 9x9 Sudoku board is valid.
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # Use defaultdict with sets to track numbers in rows, columns, and 3x3 squares.
        cols = defaultdict(set)  # Key: column index, Value: set of numbers seen in that column.
        rows = defaultdict(set)  # Key: row index, Value: set of numbers seen in that row.
        squares = defaultdict(set)  # Key: (row // 3, col // 3) tuple, Value: set of numbers in that 3x3 sub-box.

        # Iterate over every cell in the 9x9 board.
        for r in range(9):
            for c in range(9):
                # Skip empty cells.
                if board[r][c] == ".":
                    continue

                # Check if the current number already exists in the corresponding row, column, or sub-box.
                if (board[r][c] in rows[r] or  # Duplicate in row
                    board[r][c] in cols[c] or  # Duplicate in column
                    board[r][c] in squares[(r // 3, c // 3)]):  # Duplicate in 3x3 square
                    return False  # Sudoku board is invalid.

                # Add the current number to the corresponding row, column, and sub-box.
                cols[c].add(board[r][c])
                rows[r].add(board[r][c])
                squares[(r // 3, c // 3)].add(board[r][c])

        # If no duplicates are found, return True (valid board).
        return True
```

### APPROACH: BIT MASK

```python
class Solution:
    # Define a method 'isValidSudoku' to check if a given 9x9 Sudoku board is valid.
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # Use integer bit masks (9-bit) to track numbers in rows, columns, and 3x3 squares.
        rows = [0] * 9  # Tracks numbers seen in each row.
        cols = [0] * 9  # Tracks numbers seen in each column.
        squares = [0] * 9  # Tracks numbers seen in each 3x3 sub-box.

        # Iterate over every cell in the 9x9 board.
        for r in range(9):
            for c in range(9):
                # Skip empty cells.
                if board[r][c] == ".":
                    continue
                
                # Convert character to integer and adjust to 0-based index.
                val = int(board[r][c]) - 1

                # Check if the number is already present in the row, column, or 3x3 square.
                if (1 << val) & rows[r]:  # Check if the bit is already set in the row.
                    return False
                if (1 << val) & cols[c]:  # Check if the bit is already set in the column.
                    return False
                if (1 << val) & squares[(r // 3) * 3 + (c // 3)]:  # Check in 3x3 square.
                    return False
                
                # Set the bit for the current number in the respective row, column, and 3x3 square.
                rows[r] |= (1 << val)
                cols[c] |= (1 << val)
                squares[(r // 3) * 3 + (c // 3)] |= (1 << val)

        # If no duplicates are found, the board is valid.
        return True
```
