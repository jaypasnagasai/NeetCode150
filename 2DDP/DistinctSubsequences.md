# 115. Distinct Subsequences
Difficulty: Hard

## QUESTION

You are given two strings `s` and `t`, both consisting of english letters.

Return the number of distinct subsequences of `s` which are equal to `t`.

### EXAMPLE

```
Input: s = "caaat", t = "cat"
Output: 3
```

```
Input: s = "xxyxy", t = "xy"
Output: 5
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        # If t is longer than s, it's impossible to form t as a subsequence
        if len(t) > len(s):
            return 0
            
        def dfs(i, j):
            # If we reach the end of t, we found a valid subsequence
            if j == len(t):
                return 1
            # If we reach the end of s without matching all of t, return 0
            if i == len(s):
                return 0
            
            # Case 1: Skip s[i] and continue searching for t[j]
            res = dfs(i + 1, j)
            
            # Case 2: If s[i] matches t[j], try matching the next character in t
            if s[i] == t[j]:
                res += dfs(i + 1, j + 1)
            
            return res
        
        return dfs(0, 0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        m, n = len(s), len(t)
        
        # Initialize a 1D DP array where dp[j] represents the number of ways
        # to form t[j:] as a subsequence of s[i:].
        dp = [0] * (n + 1)
        
        # Base case: An empty t can always be formed once from any suffix of s
        dp[n] = 1  

        # Iterate backwards over s
        for i in range(m - 1, -1, -1):
            prev = 1  # This stores dp[j] for the next iteration (right neighbor)
            
            # Iterate backwards over t
            for j in range(n - 1, -1, -1):
                res = dp[j]  # Store the current value before modifying

                # If characters match, add the number of ways to form t[j+1:] from s[i+1:]
                if s[i] == t[j]:
                    res += prev  # prev holds the value of dp[j+1] before update

                prev = dp[j]  # Save the current dp[j] for the next iteration
                dp[j] = res  # Update dp[j] with the computed result
                
        return dp[0]  # dp[0] stores the number of ways to form t from s
```
