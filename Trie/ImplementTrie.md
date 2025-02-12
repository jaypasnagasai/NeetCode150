# 208. Implement Trie (Prefix Tree)
Difficulty: Medium

## QUESTION

A prefix tree (also known as a trie) is a tree data structure used to efficiently store and retrieve keys in a set of strings. Some applications of this data structure include auto-complete and spell checker systems.

Implement the PrefixTree class:

- `PrefixTree()` Initializes the prefix tree object.
- `void insert(String word)` Inserts the string `word` into the prefix tree.
- `boolean search(String word)` Returns `true` if the string `word` is in the prefix tree (i.e., was inserted before), and `false` otherwise.
- `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

### EXAMPLE

```
Input: 
["Trie", "insert", "dog", "search", "dog", "search", "do", "startsWith", "do", "insert", "do", "search", "do"]

Output:
[null, null, true, false, true, null, true]

Explanation:
PrefixTree prefixTree = new PrefixTree();
prefixTree.insert("dog");
prefixTree.search("dog");    // return true
prefixTree.search("do");     // return false
prefixTree.startsWith("do"); // return true
prefixTree.insert("do");
prefixTree.search("do");     // return true
```

## SOLUTION


### APPROACH: PREFIX TREE [ARRAY]

```python
class TrieNode:
    def __init__(self):
        """
        Initializes a Trie node with:
        - `children`: An array of 26 elements (for each lowercase letter 'a' to 'z'),
                      initialized to None.
        - `endOfWord`: A boolean flag indicating whether this node marks the end of a word.
        """
        self.children = [None] * 26  # Stores child nodes for each alphabet letter
        self.endOfWord = False  # Marks whether this node represents the end of a word


class PrefixTree:
    def __init__(self):
        """
        Initializes a Trie (Prefix Tree) with an empty root node.
        """
        self.root = TrieNode()  # The root node of the trie

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.

        :param word: The word to insert
        """
        cur = self.root  # Start from the root node
        for c in word:
            i = ord(c) - ord("a")  # Convert character to index (0-25)
            
            # If the corresponding child node does not exist, create it
            if cur.children[i] is None:
                cur.children[i] = TrieNode()
            
            # Move to the next node
            cur = cur.children[i]
        
        # Mark the last node as the end of the word
        cur.endOfWord = True

    def search(self, word: str) -> bool:
        """
        Searches for a word in the trie.

        :param word: The word to search for
        :return: True if the word exists in the trie, False otherwise
        """
        cur = self.root  # Start from the root node
        for c in word:
            i = ord(c) - ord("a")  # Convert character to index (0-25)
            
            # If the corresponding child node does not exist, the word is not in the trie
            if cur.children[i] is None:
                return False
            
            # Move to the next node
            cur = cur.children[i]
        
        # Return True only if this node marks the end of a complete word
        return cur.endOfWord

    def startsWith(self, prefix: str) -> bool:
        """
        Checks if there is any word in the trie that starts with the given prefix.

        :param prefix: The prefix to check
        :return: True if any word in the trie starts with the prefix, False otherwise
        """
        cur = self.root  # Start from the root node
        for c in prefix:
            i = ord(c) - ord("a")  # Convert character to index (0-25)
            
            # If the corresponding child node does not exist, no word has this prefix
            if cur.children[i] is None:
                return False
            
            # Move to the next node
            cur = cur.children[i]
        
        # If we've traversed the prefix successfully, return True
        return True
```

### APPROACH: PREFIX TREE [HASH MAP]

```python
class TrieNode:
    def __init__(self):
        """
        Initializes a TrieNode with:
        - `children`: A dictionary to store child nodes for each character.
        - `endOfWord`: A boolean flag indicating whether this node marks the end of a word.
        """
        self.children = {}  # Dictionary to store child nodes dynamically
        self.endOfWord = False  # Marks whether this node represents the end of a word


class PrefixTree:
    def __init__(self):
        """
        Initializes a Trie (Prefix Tree) with an empty root node.
        """
        self.root = TrieNode()  # Root node of the trie

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.

        :param word: The word to insert
        """
        cur = self.root  # Start from the root node
        for c in word:
            # If the character does not exist in the current node's children, create a new node
            if c not in cur.children:
                cur.children[c] = TrieNode()
            
            # Move to the next node
            cur = cur.children[c]
        
        # Mark the last node as the end of the word
        cur.endOfWord = True

    def search(self, word: str) -> bool:
        """
        Searches for a word in the trie.

        :param word: The word to search for
        :return: True if the word exists in the trie, False otherwise
        """
        cur = self.root  # Start from the root node
        for c in word:
            # If the character does not exist in the current node's children, the word is not in the trie
            if c not in cur.children:
                return False
            
            # Move to the next node
            cur = cur.children[c]
        
        # Return True only if this node marks the end of a complete word
        return cur.endOfWord

    def startsWith(self, prefix: str) -> bool:
        """
        Checks if there is any word in the trie that starts with the given prefix.

        :param prefix: The prefix to check
        :return: True if any word in the trie starts with the prefix, False otherwise
        """
        cur = self.root  # Start from the root node
        for c in prefix:
            # If the character does not exist in the current node's children, no word has this prefix
            if c not in cur.children:
                return False
            
            # Move to the next node
            cur = cur.children[c]
        
        # If we've traversed the prefix successfully, return True
        return True
```
