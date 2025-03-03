# 763. Partition Labels
Difficulty: Medium

## QUESTION

You are given a string `s` consisting of lowercase english letters.

We want to split the string into as many substrings as possible, while ensuring that each letter appears in at most one substring.

Return a list of integers representing the size of these substrings in the order they appear in the string.

### EXAMPLE

```
Input: s = "xyxxyzbzbbisl"
Output: [5, 5, 1, 1, 1]
```

```
Input: s = "abcabc"
Output: [6]
```

## SOLUTION


### APPROACH: TWO POINTERS (GREEDY)

```python
from typing import List

class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        # Dictionary to store the last occurrence index of each character
        lastIndex = {}
        for i, c in enumerate(s):
            lastIndex[c] = i  # Update with the latest occurrence
        
        res = []  # Result list to store partition sizes
        size = end = 0  # Track current partition size and end index

        # Iterate through the string to determine partitions
        for i, c in enumerate(s):
            size += 1  # Increment partition size
            end = max(end, lastIndex[c])  # Update the farthest end of the current partition

            # If the current index reaches the partition end, finalize the partition
            if i == end:
                res.append(size)  # Append partition size to result
                size = 0  # Reset partition size for the next segment
        
        return res  # Return the list of partition sizes
```
