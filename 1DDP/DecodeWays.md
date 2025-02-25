# 91. Decode Ways
Difficulty: Medium

## QUESTION

A string consisting of uppercase english characters can be encoded to a number using the following mapping:
```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```
To decode a message, digits must be grouped and then mapped back into letters using the reverse of the mapping above. There may be multiple ways to decode a message. For example, `"1012"` can be mapped into:

- `"JAB"` with the grouping `(10 1 2)`
- `"JL"` with the grouping `(10 12)`

The grouping `(1 01 2)` is invalid because `01` cannot be mapped into a letter since it contains a leading zero.

Given a string `s` containing only digits, return the number of ways to decode it. You can assume that the answer fits in a 32-bit integer.

### EXAMPLE

```
Input: s = "12"
Output: 2
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
```

```
Input: s = "01"
Output: 0
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        # Depth-First Search (DFS) function to count decoding ways
        def dfs(i):
            # If reached the end of the string, it's a valid decoding
            if i == len(s):
                return 1

            # If the current character is '0', it cannot be decoded
            if s[i] == '0':
                return 0

            # Count decoding by taking one digit
            res = dfs(i + 1)

            # Check if two digits can be taken (valid 10-26)
            if i < len(s) - 1:
                if (s[i] == '1' or 
                   (s[i] == '2' and s[i + 1] < '7')):
                    res += dfs(i + 2)

            return res

        # Start DFS from the beginning of the string
        return dfs(0)
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        # Initialize variables for dynamic programming
        dp = dp2 = 0  # dp: current value, dp2: value two steps ahead
        dp1 = 1       # dp1: value one step ahead, initialized to 1 for base case

        # Iterate through the string from right to left
        for i in range(len(s) - 1, -1, -1):
            # If current character is '0', no valid decoding from here
            if s[i] == "0":
                dp = 0
            else:
                # One-digit decoding is valid, take value from dp1
                dp = dp1

            # Check if two-digit decoding is possible (valid 10-26)
            if i + 1 < len(s) and (s[i] == "1" or
               (s[i] == "2" and s[i + 1] in "0123456")):
                dp += dp2  # Add ways from dp2 for the two-digit decoding

            # Shift dp variables for the next iteration
            dp, dp1, dp2 = 0, dp, dp1

        # dp1 holds the final count of decoding ways
        return dp1
```
