# 70. Climbing Stairs
Difficulty: Easy

## QUESTION

You are given an integer `n` representing the number of steps to reach the top of a staircase. You can climb with either `1` or `2` steps at a time.

Return the number of distinct ways to climb to the top of the staircase.

### EXAMPLE

```
Input: n = 2
Output: 2
```

```
Input: n = 3
Output: 3
```

## SOLUTION


### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        # Initialize variables to represent the number of ways to reach the last two steps
        one, two = 1, 1  # one -> ways to reach (i-1)th step, two -> ways to reach (i-2)th step

        # Iterate from 0 to n-2 since we already initialized for the first step
        for i in range(n - 1):
            temp = one           # Store current value of 'one'
            one = one + two      # Update 'one' to be the sum of the last two steps
            two = temp           # Move 'two' to the previous 'one'

        # 'one' now holds the total number of ways to climb 'n' steps
        return one
```

### APPROACH: MATRIX EXPONENTIATION

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        # Base case: If there is only 1 step, there's only 1 way to climb it
        if n == 1:
            return 1

        # Matrix multiplication function for 2x2 matrices
        def matrix_mult(A, B):
            return [[A[0][0] * B[0][0] + A[0][1] * B[1][0], 
                     A[0][0] * B[0][1] + A[0][1] * B[1][1]],
                    [A[1][0] * B[0][0] + A[1][1] * B[1][0], 
                     A[1][0] * B[0][1] + A[1][1] * B[1][1]]]

        # Function to raise a 2x2 matrix to the power 'p' using fast exponentiation
        def matrix_pow(M, p):
            # Initialize result as identity matrix
            result = [[1, 0], [0, 1]]  
            base = M

            # Perform exponentiation by squaring
            while p:
                if p % 2 == 1:
                    result = matrix_mult(result, base)
                base = matrix_mult(base, base)
                p //= 2

            return result

        # Fibonacci sequence matrix representation
        M = [[1, 1], 
             [1, 0]]

        # Raise matrix M to the power 'n' to compute the nth Fibonacci number
        result = matrix_pow(M, n)

        # The top-left element of the resulting matrix holds the desired Fibonacci number
        return result[0][0]
```

### APPROACH: MATH

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        # Calculate square root of 5 for Binet's formula
        sqrt5 = math.sqrt(5)
        
        # Calculate the golden ratio (phi) and its conjugate (psi)
        phi = (1 + sqrt5) / 2  # Golden ratio
        psi = (1 - sqrt5) / 2  # Conjugate of the golden ratio

        # Increment n by 1 as Binet's formula starts from F(1)
        n += 1

        # Apply Binet's formula for Fibonacci numbers
        result = (phi**n - psi**n) / sqrt5

        # Round the result to get an integer number of ways
        return round(result)
```
