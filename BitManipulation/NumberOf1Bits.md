# 191. Number of 1 Bits
Difficulty: Easy

## QUESTION

You are given an unsigned integer `n`. Return the number of `1` bits in its binary representation.

You may assume `n` is a non-negative integer which fits within 32-bits.

### EXAMPLE

```
Input: n = 00000000000000000000000000010111
Output: 4
```

```
Input: n = 01111111111111111111111111111101
Output: 30
```

## SOLUTION


### APPROACH: BIT MASK

```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        res = 0  # Initialize count of set bits (1s)
        
        while n:
            n &= n - 1  # Flip the rightmost set bit to 0
            res += 1  # Increment the count of set bits
        
        return res  # Return the total number of 1s in the binary representation
```

### APPROACH: BUILT-IN FUNCTION

```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        return bin(n).count('1')  # Convert to binary and count '1's
```
