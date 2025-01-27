# 242. Valid Anagram
Difficulty: Easy

## QUESTION

Given two strings `s` and `t`, return `true` if the two strings are anagrams of each other, otherwise return `false`.

An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.

### EXAMPLE

```
Input: s = "racecar", t = "carrace"
Output: true
```

```
Input: s = "jar", t = "jam"
Output: false
```

## SOLUTION


### APPROACH: SORTING

```python
class Solution:
    # Define a method 'isAnagram' to check if two strings are anagrams of each other.
    def isAnagram(self, s: str, t: str) -> bool:
        # First, check if the lengths of the strings are different.
        # If the lengths are different, they cannot be anagrams.
        if len(s) != len(t):
            return False
            
        # Sort both strings and compare them.
        # If the sorted strings are identical, they are anagrams.
        return sorted(s) == sorted(t)
```

### APPROACH: HASH TABLE

```python
class Solution:
    # Define a method 'isAnagram' to check if two strings are anagrams of each other.
    def isAnagram(self, s: str, t: str) -> bool:
        # First, check if the lengths of the strings are different.
        # If the lengths are different, they cannot be anagrams.
        if len(s) != len(t):
            return False

        # Initialize two dictionaries to count the frequency of characters in both strings.
        countS, countT = {}, {}

        # Iterate through the characters of both strings simultaneously.
        for i in range(len(s)):
            # Increment the count of the current character in the dictionary for 's'.
            countS[s[i]] = 1 + countS.get(s[i], 0)
            # Increment the count of the current character in the dictionary for 't'.
            countT[t[i]] = 1 + countT.get(t[i], 0)

        # Compare the frequency dictionaries. If they are equal, the strings are anagrams.
        return countS == countT
```

