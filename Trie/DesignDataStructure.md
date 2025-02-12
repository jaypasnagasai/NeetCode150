# 211. Design Add and Search Words Data Structure
Difficulty: Hard

## QUESTION

Design a data structure that supports adding new words and searching for existing words.

Implement the `WordDictionary` class:

- `void addWord(word)` Adds `word` to the data structure.
- `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.

### EXAMPLE

```
Input:
["WordDictionary", "addWord", "day", "addWord", "bay", "addWord", "may", "search", "say", "search", "day", "search", ".ay", "search", "b.."]

Output:
[null, null, null, null, false, true, true, true]

Explanation:
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("day");
wordDictionary.addWord("bay");
wordDictionary.addWord("may");
wordDictionary.search("say"); // return false
wordDictionary.search("day"); // return true
wordDictionary.search(".ay"); // return true
wordDictionary.search("b.."); // return true
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class WordDictionary:

    def __init__(self):
        """
        Initializes the WordDictionary.
        - `store`: A list to store words added to the dictionary.
        """
        self.store = []  # List to store words

    def addWord(self, word: str) -> None:
        """
        Adds a word to the dictionary.

        :param word: The word to be added.
        """
        self.store.append(word)  # Append the word to the list

    def search(self, word: str) -> bool:
        """
        Searches for a word in the dictionary.
        Supports '.' wildcard matching any character.

        :param word: The word to search for (with optional wildcard characters).
        :return: True if a matching word exists, False otherwise.
        """
        for w in self.store:
            if len(w) != len(word):  # Skip words of different lengths
                continue
            
            i = 0  # Pointer to track character comparison
            while i < len(w):
                # If characters match or the character in `word` is '.', continue checking
                if w[i] == word[i] or word[i] == '.':
                    i += 1
                else:
                    break  # Stop checking this word if there's a mismatch
            
            if i == len(w):  # If the entire word matches, return True
                return True
        
        return False  # No matching word found
```

### APPROACH: DEPTH FIRST SEARCH

```python
class TrieNode:
    def __init__(self):
        """
        Initializes a TrieNode.
        - `children`: A dictionary to store child nodes for each character.
        - `word`: A boolean flag indicating whether this node marks the end of a word.
        """
        self.children = {}  # Dictionary to store child nodes dynamically
        self.word = False  # Marks whether this node represents the end of a word


class WordDictionary:
    def __init__(self):
        """
        Initializes the WordDictionary with a root TrieNode.
        """
        self.root = TrieNode()  # Root node of the trie

    def addWord(self, word: str) -> None:
        """
        Adds a word to the dictionary.

        :param word: The word to be added.
        """
        cur = self.root  # Start from the root node
        for c in word:
            if c not in cur.children:
                cur.children[c] = TrieNode()  # Create a new TrieNode if character doesn't exist
            cur = cur.children[c]  # Move to the next node
        cur.word = True  # Mark the last node as the end of a word

    def search(self, word: str) -> bool:
        """
        Searches for a word in the dictionary.
        Supports '.' wildcard matching any character.

        :param word: The word to search for (with optional wildcard characters).
        :return: True if a matching word exists, False otherwise.
        """

        def dfs(j, root):
            """
            Depth-First Search (DFS) helper function for searching the word.

            :param j: Current index in the word being checked.
            :param root: Current TrieNode being explored.
            :return: True if a match is found, False otherwise.
            """
            cur = root  # Start from the given TrieNode

            for i in range(j, len(word)):
                c = word[i]

                if c == ".":  # If the character is a wildcard, try all possible children
                    for child in cur.children.values():
                        if dfs(i + 1, child):  # Recursively check each child
                            return True
                    return False  # No match found among children

                else:  # Regular character match
                    if c not in cur.children:  # If character is missing, return False
                        return False
                    cur = cur.children[c]  # Move to the next node

            return cur.word  # Return True if this is the end of a valid word

        return dfs(0, self.root)  # Start DFS from the root node
```
