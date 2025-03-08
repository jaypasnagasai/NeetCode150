# 72. Edit Distance
Difficulty: Medium

## QUESTION

You are given two strings `word1` and `word2`, each consisting of lowercase English letters.

You are allowed to perform three operations on `word1` an unlimited number of times:

- Insert a character at any position
- Delete a character at any position
- Replace a character at any position

Return the minimum number of operations to make `word1` equal `word2`.

### EXAMPLE

```
Input: word1 = "monkeys", word2 = "money"
Output: 2
```

```
Input: word1 = "neatcdee", word2 = "neetcode"
Output: 3
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)

        def dfs(i, j):
            # If word1 is exhausted, return the remaining length of word2 (insertions needed)
            if i == m:
                return n - j
            # If word2 is exhausted, return the remaining length of word1 (deletions needed)
            if j == n:
                return m - i
            # If characters match, move both pointers forward
            if word1[i] == word2[j]:
                return dfs(i + 1, j + 1)
            
            # Compute the minimum of three operations:
            # 1. Deletion from word1 (move i forward)
            # 2. Insertion into word1 (move j forward)
            # 3. Replacement (move both i and j forward)
            res = min(dfs(i + 1, j), dfs(i, j + 1), dfs(i + 1, j + 1))

            return res + 1  # Add 1 to account for the chosen operation
        
        return dfs(0, 0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)
        
        # Ensure word1 is the longer string (for memory optimization)
        if m < n:
            m, n = n, m
            word1, word2 = word2, word1
        
        # Initialize DP array where dp[j] represents the edit distance for word1[i:] and word2[j:]
        dp = [n - i for i in range(n + 1)]  # Base case: converting word2[j:] to empty string

        # Iterate from bottom to top (reverse order)
        for i in range(m - 1, -1, -1):
            nextDp = dp[n]  # Store dp[j+1] for next iteration
            dp[n] = m - i  # Base case: converting word1[i:] to empty string
            
            for j in range(n - 1, -1, -1):
                temp = dp[j]  # Store current dp[j] before modifying
                
                if word1[i] == word2[j]:
                    dp[j] = nextDp  # If characters match, take previous diagonal value
                else:
                    dp[j] = 1 + min(dp[j], dp[j + 1], nextDp)  
                    # Choices:
                    # 1. Deletion (dp[j]) → Remove character from word1
                    # 2. Insertion (dp[j + 1]) → Insert character into word1
                    # 3. Replacement (nextDp) → Replace character in word1
                
                nextDp = temp  # Update nextDp for next iteration
        
        return dp[0]  # Minimum edit distance between word1 and word2
```
