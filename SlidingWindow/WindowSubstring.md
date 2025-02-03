# 000. Minimum Window Substring
Difficulty: HARD

## QUESTION

Given two strings `s` and `t`, return the shortest substring of `s` such that every character in `t`, including duplicates, is present in the substring. If such a substring does not exist, return an empty string `""`.

You may assume that the correct output is always unique.

### EXAMPLE

```
Input: s = "OUZODYXAZV", t = "XYZ"
Output: "YXAZ"
```

```
Input: s = "xyz", t = "xyz"
Output: "xyz"
```

```
Input: s = "x", t = "xy"
Output: ""
```
## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        # Edge case: if t is empty, return an empty string
        if t == "":
            return ""

        # Create a dictionary to store the frequency of characters in t
        countT = {}
        for c in t:
            countT[c] = 1 + countT.get(c, 0)

        # Initialize result variables: window range and minimum window length
        res, resLen = [-1, -1], float("infinity")

        # Iterate over the string s
        for i in range(len(s)):
            countS = {}  # Dictionary to store the frequency of characters in the current window

            # Expand the window from i to the end of s
            for j in range(i, len(s)):
                countS[s[j]] = 1 + countS.get(s[j], 0)  # Update character frequency

                # Check if the current window contains all characters of t
                flag = True
                for c in countT:
                    if countT[c] > countS.get(c, 0):  # If any required character count is insufficient
                        flag = False
                        break
                
                # If a valid window is found and is smaller than the previous result, update the result
                if flag and (j - i + 1) < resLen:
                    resLen = j - i + 1
                    res = [i, j]

        # Extract and return the minimum window substring, or return an empty string if no valid window was found
        l, r = res
        return s[l : r + 1] if resLen != float("infinity") else ""
```


### APPROACH: SLIDING WINDOW

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        # Edge case: if t is empty, return an empty string
        if t == "":
            return ""

        # Dictionaries to store the frequency of characters in t and the current window in s
        countT, window = {}, {}

        # Populate countT with the frequency of characters in t
        for c in t:
            countT[c] = 1 + countT.get(c, 0)

        have, need = 0, len(countT)  # 'have' tracks matched characters, 'need' is the number of unique characters in t
        res, resLen = [-1, -1], float("infinity")  # Variables to store the best window range and its length
        l = 0  # Left pointer for the sliding window

        # Iterate through s using the right pointer 'r'
        for r in range(len(s)):
            c = s[r]  # Current character in s
            window[c] = 1 + window.get(c, 0)  # Add it to the window dictionary

            # If the current character is in t and its frequency matches, increment 'have'
            if c in countT and window[c] == countT[c]:
                have += 1

            # When we have all characters from t in the current window
            while have == need:
                # Update the result if the current window is smaller than the previous smallest
                if (r - l + 1) < resLen:
                    res = [l, r]
                    resLen = r - l + 1

                # Shrink the window from the left
                window[s[l]] -= 1
                if s[l] in countT and window[s[l]] < countT[s[l]]:
                    have -= 1  # If a required character count goes below needed, decrement 'have'
                
                l += 1  # Move the left pointer to try and find a smaller valid window

        # Extract and return the minimum window substring, or return an empty string if no valid window was found
        l, r = res
        return s[l : r + 1] if resLen != float("infinity") else ""
```
