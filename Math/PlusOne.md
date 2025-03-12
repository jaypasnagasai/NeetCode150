# 66. Plus One
Difficulty: Easy

## QUESTION

You are given an integer array `digits`, where each `digits[i]` is the `ith` digit of a large integer. It is ordered from most significant to least significant digit, and it will not contain any leading zero.

Return the digits of the given integer after incrementing it by one.

### EXAMPLE

```
Input: digits = [1,2,3,4]
Output: [1,2,3,5]
```

```
Input: digits = [9,9,9]
Output: [1,0,0,0]
```

## SOLUTION


### APPROACH: RECURSION

```python
from typing import List

class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        """
        Increments the integer represented by the list of digits by one.
        Handles carry propagation recursively.
        """
        
        # If the list is empty (edge case), return [1] (unlikely scenario)
        if not digits:
            return [1]

        # If the last digit is less than 9, simply increment and return
        if digits[-1] < 9:
            digits[-1] += 1
            return digits
        else:
            # If the last digit is 9, it becomes 0 and we recursively increment the rest
            return self.plusOne(digits[:-1]) + [0]
```

### APPROACH: ITERATION

```python
from typing import List

class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        """
        Increments the integer represented by the list of digits by one.
        Handles carry propagation efficiently using an iterative approach.
        """

        n = len(digits)  # Get the length of the digits list

        # Iterate from the last digit to the first
        for i in range(n - 1, -1, -1):
            if digits[i] < 9:
                # If the digit is not 9, simply increment it and return
                digits[i] += 1
                return digits
            # If the digit is 9, set it to 0 (carry over)
            digits[i] = 0

        # If all digits were 9 (e.g., [9,9,9] → [0,0,0]), add a leading 1
        return [1] + digits
```
