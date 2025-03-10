# 190. Reverse Bits
Difficulty: Easy

## QUESTION

Given a 32-bit unsigned integer `n`, reverse the bits of the binary representation of `n` and return the result.

### EXAMPLE

```
Input: n = 00000000000000000000000000010101
Output:    2818572288 (10101000000000000000000000000000)
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def reverseBits(self, n: int) -> int:
        binary = ""  # String to store the binary representation

        # Step 1: Convert number to 32-bit binary representation (reversed)
        for i in range(32):
            if n & (1 << i):  # Check if the i-th bit is set
                binary += "1"
            else:
                binary += "0"

        res = 0  # Store the reversed integer

        # Step 2: Convert the reversed binary string back to an integer
        for i, bit in enumerate(binary[::-1]):  # Iterate in reverse order
            if bit == "1":
                res |= (1 << i)  # Set the i-th bit in res

        return res  # Return the final reversed integer
```

### APPROACH: BIT MANIPULATION

```python
class Solution:
    def reverseBits(self, n: int) -> int:
        res = n  # Store the original number
        
        # Step 1: Swap the left and right 16-bit halves
        res = (res >> 16) | (res << 16) & 0xFFFFFFFF

        # Step 2: Swap 8-bit chunks within each half
        res = ((res & 0xff00ff00) >> 8) | ((res & 0x00ff00ff) << 8)

        # Step 3: Swap 4-bit chunks within each byte
        res = ((res & 0xf0f0f0f0) >> 4) | ((res & 0x0f0f0f0f) << 4)

        # Step 4: Swap 2-bit chunks
        res = ((res & 0xcccccccc) >> 2) | ((res & 0x33333333) << 2)

        # Step 5: Swap adjacent bits (bitwise reversal)
        res = ((res & 0xaaaaaaaa) >> 1) | ((res & 0x55555555) << 1)

        # Ensure the result is within 32-bit unsigned integer range
        return res & 0xFFFFFFFF
```
