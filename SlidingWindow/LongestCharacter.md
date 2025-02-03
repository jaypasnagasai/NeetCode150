# 424. Longest Repeating Character Replacement
Difficulty: Medium

## QUESTION

You are given a string `s` consisting of only uppercase english characters and an integer `k`. You can choose up to `k` characters of the string and replace them with any other uppercase English character.

After performing at most `k` replacements, return the length of the longest substring which contains only one distinct character.

### EXAMPLE

```
Input: s = "XYYX", k = 2
Output: 4
```

```
Input: s = "AAABABB", k = 1
Output: 5
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        res = 0  # Stores the length of the longest valid substring
        
        # Iterate through each character as a starting point
        for i in range(len(s)):
            count, maxf = {}, 0  # Dictionary to store frequency of characters, max frequency tracker
            
            # Expand the substring from index 'i' onwards
            for j in range(i, len(s)):
                count[s[j]] = 1 + count.get(s[j], 0)  # Update character frequency
                maxf = max(maxf, count[s[j]])  # Track the max frequency of any character in the window
                
                # If the number of replacements needed is within the allowed limit (k)
                if (j - i + 1) - maxf <= k:
                    res = max(res, j - i + 1)  # Update the max length found
        
        return res  # Return the length of the longest substring that can be transformed
```


### APPROACH: SLIDING WINDOW

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        count = {}  # Dictionary to store character frequencies
        res = 0  # Stores the length of the longest valid substring
        
        l = 0  # Left pointer for the sliding window
        maxf = 0  # Track the highest frequency of any single character in the current window
        
        # Iterate over the string using the right pointer
        for r in range(len(s)):
            count[s[r]] = 1 + count.get(s[r], 0)  # Update frequency of current character
            maxf = max(maxf, count[s[r]])  # Update max frequency seen in the window
            
            # If replacements needed exceed k, shrink the window from the left
            while (r - l + 1) - maxf > k:
                count[s[l]] -= 1  # Reduce count of the leftmost character
                l += 1  # Move left pointer forward
            
            # Update the result with the maximum window size found
            res = max(res, r - l + 1)
        
        return res  # Return the longest substring length
```
