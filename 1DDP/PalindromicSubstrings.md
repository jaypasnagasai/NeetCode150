# 647. Palindromic Substrings
Difficulty: Medium

## QUESTION

Given a string `s`, return the number of substrings within `s` that are palindromes.

A palindrome is a string that reads the same forward and backward.

### EXAMPLE

```
Input: s = "abc"
Output: 3
```

```
Input: s = "aaa"
Output: 6
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        # Initialize the result counter to track palindromic substrings
        res = 0

        # Iterate through all possible substrings using two pointers
        for i in range(len(s)):
            for j in range(i, len(s)):
                l, r = i, j  # Set left and right pointers for the current substring

                # Check if the substring is a palindrome
                while l < r and s[l] == s[r]:
                    l += 1
                    r -= 1

                # If the substring is a palindrome, increment the result counter
                res += (l >= r)

        # Return the total count of palindromic substrings
        return res
```

### APPROACH: TWO POINTER

```python
class Solution:
    
    def countSubstrings(self, s: str) -> int:
        # Initialize the result counter to count palindromic substrings
        res = 0

        # Iterate through each character in the string
        for i in range(len(s)):
            # Count odd-length palindromic substrings centered at index i
            res += self.countPali(s, i, i)
            # Count even-length palindromic substrings centered between i and i+1
            res += self.countPali(s, i, i + 1)

        # Return the total count of palindromic substrings
        return res

    # Helper function to count palindromes centered at l and r
    def countPali(self, s, l, r):
        res = 0
        # Expand around the center while the substring remains a palindrome
        while l >= 0 and r < len(s) and s[l] == s[r]:
            res += 1  # Count the current palindrome
            l -= 1    # Move left pointer outward
            r += 1    # Move right pointer outward
        return res
```

### APPROACH: MANACHER'S ALGORITHM

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        # Manacher's Algorithm to count palindromic substrings
        def manacher(s):
            # Preprocess the string by adding '#' between characters
            # This allows uniform handling of even and odd length palindromes
            t = '#' + '#'.join(s) + '#'
            n = len(t)

            # Array to store the palindrome radius at each center
            p = [0] * n

            # Variables to track the current center and right boundary
            l, r = 0, 0

            # Iterate through the transformed string
            for i in range(n):
                # If within the current palindrome, use mirrored value
                if i < r:
                    p[i] = min(r - i, p[l + (r - i)])

                # Expand around the current center i
                while (i + p[i] + 1 < n and i - p[i] - 1 >= 0 and
                       t[i + p[i] + 1] == t[i - p[i] - 1]):
                    p[i] += 1

                # Update the center and right boundary if the palindrome expands further
                if i + p[i] > r:
                    l, r = i - p[i], i + p[i]

            return p

        # Apply Manacher's Algorithm to get palindrome lengths
        p = manacher(s)

        # Count total palindromic substrings
        # Each p[i] represents the radius of the palindrome, (p[i] + 1) // 2 gives valid counts
        res = 0
        for i in p:
            res += (i + 1) // 2

        return res
```
