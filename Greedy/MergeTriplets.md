# 1899. Merge Triplets to Form Target Triplet
Difficulty: Medium

## QUESTION

You are given a 2D array of integers `triplets`, where `triplets[i] = [ai, bi, ci]` represents the `ith` triplet. You are also given an array of integers `target = [x, y, z]` which is the triplet we want to obtain.

To obtain `target`, you may apply the following operation on `triplets` zero or more times:

Choose two different triplets `triplets[i]` and `triplets[j]` and update `triplets[j]` to become `[max(ai, aj), max(bi, bj), max(ci, cj)]`.
* E.g. if `triplets[i] = [1, 3, 1]` and `triplets[j] = [2, 1, 2]`, `triplets[j]` will be updated to `[max(1, 2), max(3, 1), max(1, 2)] = [2, 3, 2]`.

Return `true` if it is possible to obtain `target` as an element of `triplets`, or `false` otherwise.

### EXAMPLE

```
Input: triplets = [[1,2,3],[7,1,1]], target = [7,2,3]
Output: true
```

```
Input: triplets = [[2,5,6],[1,4,4],[5,7,5]], target = [5,4,6]
Output: false
```

## SOLUTION


### APPROACH: GREEDY

```python
from typing import List

class Solution:
    def mergeTriplets(self, triplets: List[List[int]], target: List[int]) -> bool:
        # Flags to check if we can achieve each target value from triplets
        x = y = z = False  

        # Iterate through the given triplets
        for t in triplets:
            # Check if the triplet is a valid candidate (does not exceed any target value)
            if t[0] > target[0] or t[1] > target[1] or t[2] > target[2]:
                continue

            # Update flags if the triplet contributes to forming the target triplet
            x |= (t[0] == target[0])
            y |= (t[1] == target[1])
            z |= (t[2] == target[2])

            # If all three values are achieved, return True
            if x and y and z:
                return True

        # If we couldn't achieve all target values, return False
        return False
```

