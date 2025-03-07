# 97. Interleaving String
Difficulty: Medium

## QUESTION

You are given three strings `s1`, `s2`, and `s3`. Return `true` if `s3` is formed by interleaving `s1` and `s2` together or `false` otherwise.

Interleaving two strings `s` and `t` is done by dividing `s` and `t` into `n` and `m` substrings respectively, where the following conditions are met

- `|n - m| <= 1`, i.e. the difference between the number of substrings of `s` and `t` is at most `1`.
- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- Interleaving `s` and `t` is `s1 + t1 + s2 + t2 + ...` or `t1 + s1 + t2 + s2 + ...`

You may assume that `s1`, `s2` and `s3` consist of lowercase English letters.

### EXAMPLE

```
Input: s1 = "aaaa", s2 = "bbbb", s3 = "aabbbbaa"
Output: true
```

```
Input: s1 = "", s2 = "", s3 = ""
Output: true
```

```
Input: s1 = "abc", s2 = "xyz", s3 = "abxzcy"
Output: false
```
## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        
        def dfs(i, j, k):
            # Base case: if we reach the end of s3, check if both s1 and s2 are fully used
            if k == len(s3):
                return (i == len(s1)) and (j == len(s2))
            
            # If the current character in s1 matches s3, try moving forward in s1
            if i < len(s1) and s1[i] == s3[k]:
                if dfs(i + 1, j, k + 1):
                    return True
            
            # If the current character in s2 matches s3, try moving forward in s2
            if j < len(s2) and s2[j] == s3[k]:
                if dfs(i, j + 1, k + 1):
                    return True
            
            # If neither s1[i] nor s2[j] match s3[k], return False
            return False
        
        # Edge case: If lengths don't match, return False immediately
        if len(s1) + len(s2) != len(s3):
            return False
        
        return dfs(0, 0, 0)  # Start DFS from the beginning of all strings
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        m, n = len(s1), len(s2)
        
        # If the lengths don't match, return False immediately
        if m + n != len(s3):
            return False

        # Ensure that s1 is the shorter string (optimization for memory usage)
        if n < m:
            s1, s2 = s2, s1
            m, n = n, m
        
        # Initialize a 1D DP array with False values
        dp = [False for _ in range(n + 1)]
        dp[n] = True  # Base case: empty strings match

        # Iterate from bottom-right to top-left in the DP table (reverse order)
        for i in range(m, -1, -1):
            nextDp = True  # Stores the previous dp[j] value for the next row
            for j in range(n - 1, -1, -1):
                res = False  # Store the result for dp[j]
                
                # If s1[i] matches s3[i+j] and dp[j] (from previous iteration) is True
                if i < m and s1[i] == s3[i + j] and dp[j]:
                    res = True
                
                # If s2[j] matches s3[i+j] and nextDp (right neighbor) is True
                if j < n and s2[j] == s3[i + j] and nextDp:
                    res = True
                
                dp[j] = res  # Update dp[j] with the result
                nextDp = dp[j]  # Update nextDp for the next iteration

        # The answer is stored in dp[0] after processing both strings
        return dp[0]
```
