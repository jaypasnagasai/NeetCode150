# 1143. Longest Common Subsequence
Difficulty: Medium

## QUESTION

Given two strings `text1` and `text2`, return the length of the longest common subsequence between the two strings if one exists, otherwise return `0`.

A subsequence is a sequence that can be derived from the given sequence by deleting some or no elements without changing the relative order of the remaining characters.

For example, `"cat"` is a subsequence of `"crabt"`.

A common subsequence of two strings is a subsequence that exists in both strings.

### EXAMPLE

```
Input: text1 = "cat", text2 = "crabt" 
Output: 3 
```

```
Input: text1 = "abcd", text2 = "abcd"
Output: 4
```

```
Input: text1 = "abcd", text2 = "efgh"
Output: 0
```
## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        
        def dfs(i, j):
            # Base case: if we reach the end of either string, return 0
            if i == len(text1) or j == len(text2):
                return 0
            
            # If characters match, increment count and move diagonally
            if text1[i] == text2[j]:
                return 1 + dfs(i + 1, j + 1)
            
            # If characters don't match, explore both options:
            # 1. Move in text1 (i+1, j)
            # 2. Move in text2 (i, j+1)
            # Take the maximum of both choices
            return max(dfs(i + 1, j), dfs(i, j + 1))
        
        # Start the DFS from the beginning of both strings
        return dfs(0, 0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        # Ensure text1 is the longer string to optimize space complexity
        if len(text1) < len(text2):
            text1, text2 = text2, text1

        # Create a 1D DP array initialized to 0 with size of the smaller string + 1
        dp = [0] * (len(text2) + 1)

        # Iterate through text1 in reverse order (bottom-up DP)
        for i in range(len(text1) - 1, -1, -1):
            prev = 0  # Temporary variable to store previous diagonal value
            for j in range(len(text2) - 1, -1, -1):
                temp = dp[j]  # Store current value before modifying
                
                # If characters match, take the diagonal value and add 1
                if text1[i] == text2[j]:
                    dp[j] = 1 + prev
                else:
                    # Otherwise, take the max between moving right or down
                    dp[j] = max(dp[j], dp[j + 1])
                
                prev = temp  # Update prev for next iteration (previous diagonal)

        # The result is stored in dp[0] after processing both strings
        return dp[0]
```
