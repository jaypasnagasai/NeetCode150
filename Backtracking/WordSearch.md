# 79. Word Search
Difficulty: Medium

## QUESTION

Given a 2-D grid of characters `board` and a string `word`, return `true` if the word is present in the grid, otherwise return `false`.

For the word to be present it must be possible to form it with a path in the board with horizontally or vertically neighboring cells. The same cell may not be used more than once in a word.

### EXAMPLE

```
Input: 
board = [
  ["A","B","C","D"],
  ["S","A","A","T"],
  ["A","C","A","E"]
],
word = "CAT"

Output: true
```

```
Input: 
board = [
  ["A","B","C","D"],
  ["S","A","A","T"],
  ["A","C","A","E"]
],
word = "BAT"

Output: false
```

## SOLUTION


### APPROACH: BACKTRACKING

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        """
        Determines if a given word exists in the 2D board using DFS.

        :param board: 2D list of characters representing the board.
        :param word: The target word to search for.
        :return: True if the word exists in the board, False otherwise.
        """
        ROWS, COLS = len(board), len(board[0])  # Get board dimensions

        def dfs(r, c, i):
            """
            Depth-First Search (DFS) to explore possible word formations.

            :param r: Current row index.
            :param c: Current column index.
            :param i: Index of the current character in the word.
            :return: True if the word can be found, False otherwise.
            """
            # If we have matched the entire word, return True
            if i == len(word):
                return True

            # Boundary and validity checks:
            # - Out of bounds
            # - Character does not match
            # - Already visited (marked as '#')
            if (r < 0 or c < 0 or r >= ROWS or c >= COLS or
                word[i] != board[r][c] or board[r][c] == '#'):
                return False

            # Mark the cell as visited
            board[r][c] = '#'

            # Explore all four possible directions
            res = (dfs(r + 1, c, i + 1) or  # Down
                   dfs(r - 1, c, i + 1) or  # Up
                   dfs(r, c + 1, i + 1) or  # Right
                   dfs(r, c - 1, i + 1))    # Left

            # Restore the cell after the search
            board[r][c] = word[i]

            return res

        # Start DFS from every cell in the board
        for r in range(ROWS):
            for c in range(COLS):
                if dfs(r, c, 0):  # If a valid path is found, return True
                    return True

        return False  # Return False if no match is found
```
