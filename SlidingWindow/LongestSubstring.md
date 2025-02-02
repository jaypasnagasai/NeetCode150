# 3. Longest Substring Without Repeating Characters
Difficulty: Medium

## QUESTION

Given a string `s`, find the length of the longest substring without duplicate characters.

A substring is a contiguous sequence of characters within a string.

### EXAMPLE

```
Input: s = "zxyzxyz"
Output: 3
```

```
Input: s = "xxxx"
Output: 1
```


## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        # Initialize the maximum length of substring
        res = 0

        # Iterate through each character as the starting point of the substring
        for i in range(len(s)):
            charSet = set()  # Set to store unique characters in the current substring
            
            # Expand the substring from index 'i' onwards
            for j in range(i, len(s)):
                if s[j] in charSet:  # If a repeating character is found, stop expanding
                    break
                
                charSet.add(s[j])  # Add character to the set if not seen before
            
            # Update the result with the maximum length found so far
            res = max(res, len(charSet))
        
        return res  # Return the length of the longest unique substring
```

### APPROACH: SLIDING WINDOW

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        mp = {}  # Dictionary to store the last seen index of each character
        l = 0  # Left pointer for the sliding window
        res = 0  # Stores the length of the longest substring without repeating characters
        
        for r in range(len(s)):  # Right pointer moves through the string
            if s[r] in mp:  # If the character is already seen
                # Move left pointer past the last occurrence of s[r]
                l = max(mp[s[r]] + 1, l)
            
            # Update the last seen index of the current character
            mp[s[r]] = r
            
            # Update the maximum length found so far
            res = max(res, r - l + 1)
        
        return res  # Return the length of the longest unique substring
```

