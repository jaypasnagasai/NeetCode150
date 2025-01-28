# 49. Group Anagrams
Difficulty: Medium

## QUESTION

Given an array of strings `strs`, group all anagrams together into sublists. You may return the output in any order.

An anagram is a string that contains the exact same characters as another string, but the order of the characters can be different.

### EXAMPLE

```
Input: strs = ["act","pots","tops","cat","stop","hat"]
Output: [["hat"],["act", "cat"],["stop", "pots", "tops"]]
```

```
Input: strs = ["x"]
Output: [["x"]]
```

```
Input: strs = [""]
Output: [[""]]
```

## SOLUTION


### APPROACH: SORTING

```python
class Solution:
    # Define a method 'groupAnagrams' to group strings that are anagrams of each other.
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # Initialize a defaultdict with lists to store grouped anagrams.
        res = defaultdict(list)

        # Iterate through each string in the input list.
        for s in strs:
            # Sort the characters of the string to create a "signature" for anagrams.
            sortedS = ''.join(sorted(s))
            # Use the sorted string as a key and append the original string to the corresponding group.
            res[sortedS].append(s)
        
        # Return the values of the defaultdict as a list of grouped anagrams.
        return list(res.values())
```

### APPROACH: HASH TABLE

```python
class Solution:
    # Define a method 'groupAnagrams' to group strings that are anagrams of each other.
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # Initialize a defaultdict with lists to store grouped anagrams.
        res = defaultdict(list)

        # Iterate through each string in the input list.
        for s in strs:
            # Create a frequency count array of size 26 for each character in the string.
            count = [0] * 26
            # Count the frequency of each character in the string.
            for c in s:
                # Update the count for the character 'c'.
                count[ord(c) - ord('a')] += 1
            # Use the frequency count tuple as the key and append the original string to the group.
            res[tuple(count)].append(s)
        
        # Return the values of the defaultdict as a list of grouped anagrams.
        return list(res.values())
```

