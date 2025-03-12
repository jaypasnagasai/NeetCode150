# 50. Pow(x, n)
Difficulty: Medium

## QUESTION

`Pow(x, n)` is a mathematical function to calculate the value of `x` raised to the power of `n` (i.e., `x^n`).

Given a floating-point value `x` and an integer value `n`, implement the `myPow(x, n)` function, which calculates `x` raised to the power `n`.

You may not use any built-in library functions.

### EXAMPLE

```
Input: x = 2.00000, n = 5
Output: 32.00000
```

```
Input: x = 1.10000, n = 10
Output: 2.59374
```

```
Input: x = 2.00000, n = -3
Output: 0.12500
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        """
        Computes x raised to the power of n (x^n) using an iterative approach.
        """

        # Edge case: Any number raised to the power of 0 is 1
        if n == 0:
            return 1
        
        # Edge case: If x is 0, return 0 (0^n is 0 for n > 0)
        if x == 0:
            return 0  

        res = 1  # Initialize result variable

        # Compute x^|n| using simple multiplication
        for i in range(abs(n)):
            res *= x

        # If n is negative, return the reciprocal
        return res if n > 0 else 1 / res
```

### APPROACH: BINARY EXPONENTIATION

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        """
        Computes x^n using Exponentiation by Squaring with bitwise operations.
        This approach reduces the number of multiplications to O(log n) time complexity.
        """

        # Edge case: If x is 0, return 0 (0^n is 0 for n > 0)
        if x == 0:
            return 0  

        # Any number raised to the power of 0 is 1
        if n == 0:
            return 1  

        res = 1  # Initialize result variable
        power = abs(n)  # Work with the absolute value of n
        
        while power:
            # If the current power is odd (lowest bit is 1), multiply res by x
            if power & 1:
                res *= x  
            
            x *= x  # Square the base
            power >>= 1  # Right shift power (equivalent to floor division by 2)

        # If the original n was negative, return the reciprocal
        return res if n > 0 else 1 / res
```
