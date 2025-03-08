# 10. Regular Expression Matching
Difficulty: Hard

## QUESTION

You are given an input string `s` consisting of lowercase english letters, and a pattern `p` consisting of lowercase english letters, as well as `'.'`, and `'*'` characters.

Return `true` if the pattern matches the entire input string, otherwise return `false`.

- `'.'` Matches any single character
- `'*'` Matches zero or more of the preceding element.

### EXAMPLE

```
Input: s = "aa", p = ".b"
Output: false
```

```
Input: s = "nnn", p = "n*"
Output: true
```

```
Input: s = "xyz", p = ".*z"
Output: true
```
## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        m, n = len(s), len(p)

        def dfs(i, j):
            # Base case: if we reach the end of pattern, check if s is also fully matched
            if j == n:
                return i == m
            
            # Check if the current characters match (direct match or wildcard '.')
            match = i < m and (s[i] == p[j] or p[j] == ".")

            # Handle '*' wildcard (it looks at the next character in the pattern)
            if (j + 1) < n and p[j + 1] == "*":
                return (dfs(i, j + 2) or         # Skip '*' (treat "x*" as empty)
                        (match and dfs(i + 1, j)))  # Use '*' (consume character in s)

            # If characters match, move to the next pair of characters
            if match:
                return dfs(i + 1, j + 1)

            return False  # No match found
        
        return dfs(0, 0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        # Initialize a 1D DP array where dp[j] tracks whether s[i:] matches p[j:]
        dp = [False] * (len(p) + 1)
        dp[len(p)] = True  # Base case: empty string matches empty pattern
        
        # Iterate from the end of s to the beginning (bottom-up DP)
        for i in range(len(s), -1, -1):
            dp1 = dp[len(p)]  # Store the rightmost value before overwriting
            dp[len(p)] = (i == len(s))  # True if we reached the end of s
            
            # Iterate over pattern p from right to left
            for j in range(len(p) - 1, -1, -1):
                match = i < len(s) and (s[i] == p[j] or p[j] == ".")
                res = False
                
                # Handle '*' wildcard (if p[j+1] == '*')
                if (j + 1) < len(p) and p[j + 1] == "*":
                    res = dp[j + 2]  # Skip 'x*' (treat it as empty)
                    if match:
                        res |= dp[j]  # Use '*' (consume character in s)
                elif match:
                    res = dp1  # Move both pointers forward
                
                dp[j], dp1 = res, dp[j]  # Update dp[j] while storing the previous value for the next iteration

        return dp[0]  # Final result: does s match p?
```
