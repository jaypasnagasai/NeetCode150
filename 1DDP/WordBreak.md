# 139. Word Break
Difficulty: Medium

## QUESTION

Given a string s and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of dictionary words.

You are allowed to reuse words in the dictionary an unlimited number of times. You may assume all dictionary words are unique.

### EXAMPLE

```
Input: s = "neetcode", wordDict = ["neet","code"]
Output: true
```

```
Input: s = "applepenapple", wordDict = ["apple","pen","ape"]
Output: true
```

```
Input: s = "catsincars", wordDict = ["cats","cat","sin","in","car"]
Output: false
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        # Depth-First Search (DFS) function to check if the string can be segmented
        def dfs(i):
            # If the end of the string is reached, return True
            if i == len(s):
                return True

            # Iterate through all words in the dictionary
            for w in wordDict:
                # Check if the word matches the substring starting at index i
                if ((i + len(w)) <= len(s) and s[i : i + len(w)] == w):
                    # Recursively check the remaining substring
                    if dfs(i + len(w)):
                        return True  # If valid segmentation is found, return True

            # If no valid segmentation is found, return False
            return False

        # Start DFS from index 0
        return dfs(0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class TrieNode:
    def __init__(self):
        # Initialize a Trie node with children dictionary and word flag
        self.children = {}
        self.is_word = False

class Trie:
    def __init__(self):
        # Initialize the Trie with a root node
        self.root = TrieNode()

    def insert(self, word):
        # Insert a word into the Trie
        node = self.root
        for char in word:
            # Create a new TrieNode if the character is not in children
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        # Mark the end of the word
        node.is_word = True

    def search(self, s, i, j):
        # Search for a word in the Trie from index i to j in string s
        node = self.root
        for idx in range(i, j + 1):
            # If character is not in the current node's children, return False
            if s[idx] not in node.children:
                return False
            node = node.children[s[idx]]
        # Return True if the path ends at a valid word
        return node.is_word

class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        # Initialize Trie and insert all words from wordDict
        trie = Trie()
        for word in wordDict:
            trie.insert(word)

        # dp[i] represents whether s[i:] can be segmented into words from wordDict
        dp = [False] * (len(s) + 1)
        dp[len(s)] = True  # Base case: empty string is a valid segmentation

        # Find the maximum word length to optimize search
        max_word_len = 0
        for w in wordDict:
            max_word_len = max(max_word_len, len(w))

        # Traverse the string from end to start
        for i in range(len(s) - 1, -1, -1):
            # Check substrings of length up to max_word_len
            for j in range(i, min(len(s), i + max_word_len)):
                # If the substring s[i:j+1] exists in Trie
                if trie.search(s, i, j):
                    # Set dp[i] to True if dp[j+1] is also True
                    dp[i] = dp[j + 1]
                    # If dp[i] is True, break early to avoid unnecessary checks
                    if dp[i]:
                        break

        # Return whether the entire string can be segmented
        return dp[0]
```

