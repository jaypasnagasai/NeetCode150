# 000. Permutation in String
Difficulty: Medium

## QUESTION

You are given two strings `s1` and `s2`.

Return `true` if `s2` contains a permutation of `s1`, or `false` otherwise. That means if a permutation of `s1` exists as a substring of `s2`, then return `true`.

Both strings only contain lowercase letters.

### EXAMPLE

```
Input: s1 = "abc", s2 = "lecabee"
Output: true
```

```
Input: s1 = "abc", s2 = "lecaabee"
Output: false
```

## SOLUTION


### APPROACH: BRUTE FORCE

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

### APPROACH: HASH TABLE

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        # Create a frequency dictionary for s1
        count1 = {}
        for c in s1:
            count1[c] = 1 + count1.get(c, 0)
        
        need = len(count1)  # Number of unique characters in s1
        
        # Iterate through s2 to check for substrings matching s1's character count
        for i in range(len(s2)):
            count2, cur = {}, 0  # Frequency dictionary for current substring & matched unique count
            
            for j in range(i, len(s2)):
                count2[s2[j]] = 1 + count2.get(s2[j], 0)
                
                # If the current character count exceeds s1's count, break
                if count1.get(s2[j], 0) < count2[s2[j]]:
                    break
                
                # If character's count in both dictionaries match, increase 'cur'
                if count1.get(s2[j], 0) == count2[s2[j]]:
                    cur += 1
                
                # If all unique character counts match, return True (valid permutation found)
                if cur == need:
                    return True
        
        return False  # Return False if no valid permutation match is found
```

### APPROACH: SLIDING WINDOW

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        # If s1 is longer than s2, no valid permutation can exist
        if len(s1) > len(s2):
            return False
        
        # Frequency count arrays for s1 and the initial window in s2
        s1Count, s2Count = [0] * 26, [0] * 26

        # Initialize frequency counts for s1 and the first window in s2
        for i in range(len(s1)):
            s1Count[ord(s1[i]) - ord('a')] += 1
            s2Count[ord(s2[i]) - ord('a')] += 1
        
        # Count initial character matches
        matches = 0
        for i in range(26):
            matches += (1 if s1Count[i] == s2Count[i] else 0)
        
        # Sliding window approach
        l = 0
        for r in range(len(s1), len(s2)):  # Expand window by moving right pointer
            if matches == 26:  # If all character frequencies match, return True
                return True
            
            # Add new character from s2 into the window
            index = ord(s2[r]) - ord('a')
            s2Count[index] += 1
            if s1Count[index] == s2Count[index]:  # If it now matches s1
                matches += 1
            elif s1Count[index] + 1 == s2Count[index]:  # If it just became unmatched
                matches -= 1

            # Remove the leftmost character from the window
            index = ord(s2[l]) - ord('a')
            s2Count[index] -= 1
            if s1Count[index] == s2Count[index]:  # If it now matches s1
                matches += 1
            elif s1Count[index] - 1 == s2Count[index]:  # If it just became unmatched
                matches -= 1
            
            l += 1  # Move left pointer to maintain window size

        return matches == 26  # Return True if final window matches s1's frequency
```
