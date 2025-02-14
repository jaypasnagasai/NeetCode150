# 131. Palindrome Partitioning
Difficulty: Medium

## QUESTION

Given a string `s`, split `s` into substrings where every substring is a palindrome. Return all possible lists of palindromic substrings.

You may return the solution in any order.

### EXAMPLE

```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

```
Input: s = "a"
Output: [["a"]]
```

## SOLUTION


### APPROACH: BACKTRACKING

```python
class Solution:
    
    def partition(self, s: str) -> List[List[str]]:
        # Length of the string
        n = len(s)
        
        # dp[i][j] will be True if substring s[i:j+1] is a palindrome
        dp = [[False] * n for _ in range(n)]
        
        # Fill the dp table for all possible substring lengths
        for l in range(1, n + 1):
            for i in range(n - l + 1):
                dp[i][i + l - 1] = (
                    s[i] == s[i + l - 1]
                    and (i + 1 > (i + l - 2) or dp[i + 1][i + l - 2])
                )
        
        # res will store all possible palindrome partitioning
        res = []
        # part is a temporary list to hold the current partition
        part = []
        
        # Depth-first search to create partitions
        def dfs(i):
            # If we've reached the end of the string, add a copy of current partition
            if i >= len(s):
                res.append(part.copy())
                return
            # Explore all possible partitions starting from index i
            for j in range(i, len(s)):
                if dp[i][j]:
                    part.append(s[i:j+1])
                    dfs(j + 1)
                    part.pop()
        
        # Initiate the recursive process from index 0
        dfs(0)
        
        # Return all partitions that split the string into palindromes
        return res
```
