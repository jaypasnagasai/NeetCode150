# 5. Longest Palindromic Substring
Difficulty: Medium

## QUESTION

Given a string `s`, return the longest substring of `s` that is a palindrome.

A palindrome is a string that reads the same forward and backward.

If there are multiple palindromic substrings that have the same length, return any one of them.

### EXAMPLE

```
Input: s = "ababd"
Output: "bab"
```

```
Input: s = "abbc"
Output: "bb"
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # Initialize variables to store the longest palindrome and its length
        res, resLen = "", 0

        # Iterate over all possible substrings
        for i in range(len(s)):
            for j in range(i, len(s)):
                l, r = i, j  # Set left and right pointers

                # Check if the substring from l to r is a palindrome
                while l < r and s[l] == s[r]:
                    l += 1
                    r -= 1

                # If a palindrome is found and its length is greater than the current result
                if l >= r and resLen < (j - i + 1):
                    res = s[i : j + 1]  # Update the result with the new palindrome
                    resLen = j - i + 1  # Update the length of the longest palindrome

        # Return the longest palindromic substring found
        return res
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # Initialize variables to store the starting index and length of the longest palindrome
        resIdx, resLen = 0, 0
        n = len(s)

        # Create a DP table where dp[i][j] is True if s[i:j+1] is a palindrome
        dp = [[False] * n for _ in range(n)]

        # Iterate from the end to the start for the left pointer
        for i in range(n - 1, -1, -1):
            # Iterate from the current left pointer to the end for the right pointer
            for j in range(i, n):
                # Check if characters at i and j match and if the substring is a palindrome
                if s[i] == s[j] and (j - i <= 2 or dp[i + 1][j - 1]):
                    dp[i][j] = True  # Mark current substring as a palindrome

                    # Update result if the current palindrome is longer
                    if resLen < (j - i + 1):
                        resIdx = i  # Update starting index
                        resLen = j - i + 1  # Update length

        # Return the longest palindromic substring
        return s[resIdx : resIdx + resLen]
```

### APPROACH: TWO POINTER

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # Initialize variables to store the starting index and length of the longest palindrome
        resIdx = 0
        resLen = 0

        # Iterate through each character in the string as a potential center
        for i in range(len(s)):
            # Check for odd-length palindromes centered at index i
            l, r = i, i
            while l >= 0 and r < len(s) and s[l] == s[r]:
                # Update result if the current palindrome is longer
                if (r - l + 1) > resLen:
                    resIdx = l  # Update starting index
                    resLen = r - l + 1  # Update palindrome length
                l -= 1  # Expand left
                r += 1  # Expand right

            # Check for even-length palindromes centered between i and i+1
            l, r = i, i + 1
            while l >= 0 and r < len(s) and s[l] == s[r]:
                # Update result if the current palindrome is longer
                if (r - l + 1) > resLen:
                    resIdx = l  # Update starting index
                    resLen = r - l + 1  # Update palindrome length
                l -= 1  # Expand left
                r += 1  # Expand right

        # Return the longest palindromic substring
        return s[resIdx : resIdx + resLen]
```

### APPROACH: MANACHER's ALGORITHM

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # Manacher's Algorithm to find the longest palindromic substring
        def manacher(s):
            # Preprocess the string by inserting '#' between characters
            # to handle even-length palindromes uniformly
            t = '#' + '#'.join(s) + '#'
            n = len(t)
            
            # Array to store the palindrome radius at each center
            p = [0] * n
            
            # Variables to track the current center and right boundary
            l, r = 0, 0

            # Iterate through the transformed string
            for i in range(n):
                # Use previously calculated palindrome length if within boundary
                if i < r:
                    p[i] = min(r - i, p[l + (r - i)])

                # Expand around center i to find maximum palindrome length
                while (i + p[i] + 1 < n and i - p[i] - 1 >= 0 and 
                       t[i + p[i] + 1] == t[i - p[i] - 1]):
                    p[i] += 1

                # Update center and right boundary if the palindrome expands beyond r
                if i + p[i] > r:
                    l, r = i - p[i], i + p[i]

            return p

        # Apply Manacher's Algorithm
        p = manacher(s)

        # Find the maximum palindrome length and its center index
        resLen, center_idx = max((v, i) for i, v in enumerate(p))

        # Calculate the starting index in the original string
        resIdx = (center_idx - resLen) // 2

        # Extract and return the longest palindromic substring
        return s[resIdx : resIdx + resLen]
```
