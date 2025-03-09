# 338. Counting Bits
Difficulty: Easy

## QUESTION

Given an integer `n`, count the number of `1`'s in the binary representation of every number in the range `[0, n]`.

Return an array `output` where `output[i]` is the number of `1`'s in the binary representation of `i`.

### EXAMPLE

```
Input: n = 4
Output: [0,1,1,2,1]
```

## SOLUTION


### APPROACH: BIT MANIPULATION

```python
from typing import List

class Solution:
    def countBits(self, n: int) -> List[int]:
        # Initialize a DP array where dp[i] stores the number of 1s in the binary representation of i
        dp = [0] * (n + 1)
        
        # Iterate through all numbers from 0 to n
        for i in range(n + 1):
            # The number of 1s in i is equal to:
            # - The number of 1s in i >> 1 (i divided by 2, removing the least significant bit)
            # - Plus 1 if the least significant bit of i is set (i & 1)
            dp[i] = dp[i >> 1] + (i & 1)
        
        return dp  # Return the list of Hamming weights for numbers 0 to n
```
