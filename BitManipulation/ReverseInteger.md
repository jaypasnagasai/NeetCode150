# 7. Reverse Integer
Difficulty: Medium

## QUESTION

You are given a signed 32-bit integer `x`.

Return `x` after reversing each of its digits. After reversing, if `x` goes outside the signed 32-bit integer range `[-2^31, 2^31 - 1]`, then return `0` instead.

Solve the problem without using integers that are outside the signed 32-bit integer range.

### EXAMPLE

```
Input: x = 1234
Output: 4321
```

```
Input: x = -1234
Output: -4321
```

```
Input: x = 1234236467
Output: 0
```
## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def reverse(self, x: int) -> int:
        # Store the original value to check if it was negative
        org = x
        # Convert x to its absolute value
        x = abs(x)
        # Reverse the digits by converting to string, reversing, and converting back to integer
        res = int(str(x)[::-1])
        # If the original number was negative, make the result negative
        if org < 0:
            res *= -1
        # Check if the result is within the 32-bit signed integer range
        if res < -(1 << 31) or res > (1 << 31) - 1:
            return 0  # Return 0 if the reversed number overflows
        return res  # Return the reversed integer
```

### APPROACH: RECURSION

```python
class Solution:
    def reverse(self, x: int) -> int:
        # Recursive function to reverse the digits
        def rec(n: int, rev: int) -> int:
            # Base case: when the number becomes 0, return the reversed result
            if n == 0:
                return rev
            
            # Append the last digit of n to rev
            rev = rev * 10 + n % 10
            # Recursive call with the remaining digits
            return rec(n // 10, rev)
        
        # Determine the sign of the number
        sign = -1 if x < 0 else 1
        # Convert x to its absolute value
        x = abs(x)        
        # Get the reversed number using recursion
        reversed_num = rec(x, 0)
        # Restore the original sign
        reversed_num *= sign        
        # Check if the result is within the 32-bit signed integer range
        if reversed_num < -(1 << 31) or reversed_num > (1 << 31) - 1:
            return 0  # Return 0 if overflow occurs
            
        return reversed_num  # Return the reversed integer
```

### APPROACH: ITERATION

```python
class Solution:
    def reverse(self, x: int) -> int:
        # Define the minimum and maximum 32-bit signed integer limits
        MIN = -2147483648  # -2^31
        MAX = 2147483647   #  2^31 - 1

        res = 0  # Initialize the reversed number
        while x:
            # Extract the last digit using modulo (handling negative numbers correctly)
            digit = int(math.fmod(x, 10))
            # Remove the last digit from x by performing integer division
            x = int(x / 10)

            # Check for overflow before updating res
            if res > MAX // 10 or (res == MAX // 10 and digit > MAX % 10):
                return 0  # Return 0 if overflow occurs
            if res < MIN // 10 or (res == MIN // 10 and digit < MIN % 10):
                return 0  # Return 0 if underflow occurs
            
            # Append the extracted digit to res
            res = (res * 10) + digit

        return res  # Return the reversed integer
```
