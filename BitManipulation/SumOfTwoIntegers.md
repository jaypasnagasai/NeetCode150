# 371. Sum of Two Integers
Difficulty: Medium

## QUESTION

Given two integers `a` and `b`, return the sum of the two integers without using the `+` and `-` operators.

### EXAMPLE

```
Input: a = 1, b = 1
Output: 2
```

```
Input: a = 4, b = 7
Output: 11
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        return a + b
```

### APPROACH: BIT MANIPULATION

```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        # Define a mask to handle 32-bit integer overflow
        mask = 0xFFFFFFFF
        # Define the maximum positive integer in 32-bit representation
        max_int = 0x7FFFFFFF

        # Iterate until there is no carry left
        while b != 0:
            # Compute the carry by ANDing a and b, then shifting left by 1
            carry = (a & b) << 1
            # Compute sum without carry using XOR and apply mask to maintain 32-bit representation
            a = (a ^ b) & mask
            # Update b to carry and apply mask to maintain 32-bit representation
            b = carry & mask

        # If the result is within the positive integer range, return as is
        # Otherwise, convert to negative using two's complement
        return a if a <= max_int else ~(a ^ mask)
```
