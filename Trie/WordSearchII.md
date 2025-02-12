# 212. Word Search II
Difficulty: Hard

## QUESTION

Given a 2-D grid of characters `board` and a list of strings `words`, return all words that are present in the grid.

For a word to be present it must be possible to form the word with a path in the board with horizontally or vertically neighboring cells. The same cell may not be used more than once in a word.

### EXAMPLE

```
Input:
board = [
  ["a","b","c","d"],
  ["s","a","a","t"],
  ["a","c","k","e"],
  ["a","c","d","n"]
],
words = ["bat","cat","back","backend","stack"]

Output: ["cat","back","backend"]
```

```
Input:
board = [
  ["x","o"],
  ["x","o"]
],
words = ["xoxo"]

Output: []
```

## SOLUTION


### APPROACH: BACKTRACKING

```python
class TrieNode:
    def __init__(self):
        """
        Initializes a TrieNode.
        - `children`: An array of size 26 to store child nodes for each letter (a-z).
        - `idx`: Stores the index of the word in the words list if this node marks the end of a word.
        - `refs`: Counts references to this node, used for pruning.
        """
        self.children = [None] * 26  # Array to store child nodes
        self.idx = -1  # Index of word in the list if this is the end of a word
        self.refs = 0  # Reference count for pruning optimization

    def addWord(self, word, i):
        """
        Adds a word to the trie.

        :param word: The word to be added.
        :param i: The index of the word in the words list.
        """
        cur = self
        cur.refs += 1  # Increment reference count
        for c in word:
            index = ord(c) - ord('a')  # Convert character to index (0-25)
            if not cur.children[index]:  # Create a new TrieNode if it doesn't exist
                cur.children[index] = TrieNode()
            cur = cur.children[index]  # Move to the next node
            cur.refs += 1  # Increment reference count
        cur.idx = i  # Store the word's index at the last node


class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        """
        Finds all words from the list that can be formed in the board.

        :param board: 2D character board.
        :param words: List of words to search for.
        :return: List of words found in the board.
        """
        root = TrieNode()  # Create a trie root node
        for i in range(len(words)):
            root.addWord(words[i], i)  # Add words to the trie

        ROWS, COLS = len(board), len(board[0])  # Get board dimensions
        res = []  # List to store found words

        def getIndex(c):
            """
            Converts a character to a trie index (0-25).
            """
            index = ord(c) - ord('a')
            return index

        def dfs(r, c, node):
            """
            Performs DFS to explore possible words in the board.

            :param r: Current row index.
            :param c: Current column index.
            :param node: Current trie node.
            """
            # Base case: Out of bounds, visited cell, or no matching trie node
            if (r < 0 or c < 0 or r >= ROWS or 
                c >= COLS or board[r][c] == '*' or 
                not node.children[getIndex(board[r][c])]):
                return

            tmp = board[r][c]  # Store current character
            board[r][c] = '*'  # Mark cell as visited
            prev = node  # Store previous node
            node = node.children[getIndex(tmp)]  # Move to next trie node
            
            if node.idx != -1:  # If this node marks the end of a word
                res.append(words[node.idx])  # Add the word to the result list
                node.idx = -1  # Mark word as found
                node.refs -= 1  # Decrement reference count
                
                # Prune the trie if no more references exist
                if not node.refs:
                    prev.children[getIndex(tmp)] = None
                    node = None
                    board[r][c] = tmp  # Restore board cell
                    return

            # Explore all possible directions
            dfs(r + 1, c, node)  # Down
            dfs(r - 1, c, node)  # Up
            dfs(r, c + 1, node)  # Right
            dfs(r, c - 1, node)  # Left

            board[r][c] = tmp  # Restore board cell

        # Start DFS from each cell in the board
        for r in range(ROWS):
            for c in range(COLS):
                dfs(r, c, root)

        return res  # Return list of found words
```
