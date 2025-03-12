# 202. Happy Number
Difficulty: Easy

## QUESTION

A non-cyclical number is an integer defined by the following algorithm:

- Given a positive integer, replace it with the sum of the squares of its digits.
- Repeat the above step until the number equals `1`, or it loops infinitely in a cycle which does not include `1`.
- If it stops at 1, then the number is a non-cyclical number.

Given a positive integer `n`, return `true` if it is a non-cyclical number, otherwise return `false`.

### EXAMPLE

```
Input: n = 100
Output: true
```

```
Input: n = 101
Output: false
```

## SOLUTION


### APPROACH: HASH SET

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        """
        Determines if a number is a 'happy number'.
        A happy number is a number where repeatedly replacing it 
        with the sum of the squares of its digits eventually leads to 1.
        If a cycle is detected, the number is not happy.
        """

        # Set to track visited numbers to detect cycles
        visit = set()

        # Continue iterating until a cycle is detected or n becomes 1
        while n not in visit:
            visit.add(n)  # Mark the number as visited
            n = self.sumOfSquares(n)  # Replace n with the sum of squares of its digits
            if n == 1:
                return True  # If we reach 1, the number is happy
        
        return False  # If a cycle occurs, the number is not happy

    def sumOfSquares(self, n: int) -> int:
        """
        Computes the sum of the squares of the digits of n.
        """
        output = 0  # Initialize sum

        while n:
            digit = n % 10  # Extract last digit
            digit = digit ** 2  # Square the digit
            output += digit  # Add squared digit to the sum
            n = n // 10  # Remove last digit
        
        return output  # Return the computed sum
```

### APPROACH: FAST AND SLOW POINTERS 

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        """
        Determines if a number is a 'happy number' using Floyd's Cycle Detection Algorithm.
        A happy number is a number that, when replacing it with the sum of the squares of its digits, 
        eventually reaches 1. If a cycle is detected, the number is not happy.
        """

        # Initialize slow and fast pointers for cycle detection
        slow, fast = n, self.sumOfSquares(n)
        
        # Power and lam variables are used for Brent's cycle detection optimization
        power = 1  # Power of 2 step size for the slow pointer
        lam = 1    # Distance between slow and fast pointer

        while slow != fast:  # Detect cycle
            if power == lam:  # If the number of moves reaches the power of 2
                slow = fast  # Reset slow pointer to current fast pointer position
                power *= 2  # Double the power step
                lam = 0  # Reset lam to start counting again
            
            fast = self.sumOfSquares(fast)  # Move fast pointer one step ahead
            lam += 1  # Increase step count
        
        # If the cycle ends at 1, it's a happy number
        return fast == 1

    def sumOfSquares(self, n: int) -> int:
        """
        Computes the sum of the squares of the digits of n.
        """
        output = 0  # Initialize sum

        while n:
            digit = n % 10  # Extract last digit
            digit = digit ** 2  # Square the digit
            output += digit  # Add squared digit to the sum
            n = n // 10  # Remove last digit
        
        return output  # Return computed sum
```
