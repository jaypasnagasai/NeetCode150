# 43. Multiply Strings
Difficulty: Medium

## QUESTION

You are given two strings `num1` and `num2` that represent non-negative integers.

Return the product of `num1` and `num2` in the form of a string.

Assume that neither `num1` nor `num2` contain any leading zero, unless they are the number `0` itself.

Note: You can not use any built-in library to convert the inputs directly into integers.

### EXAMPLE

```
Input: num1 = "3", num2 = "4"
Output: "12"
```

```
Input: num1 = "111", num2 = "222"
Output: "24642"
```

## SOLUTION


### APPROACH: MULTIPLICATION & ADDITION

```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        """
        Multiplies two large numbers represented as strings without using built-in integer multiplication.
        Uses digit-wise multiplication and addition similar to manual long multiplication.
        """

        # If either number is "0", the product is "0"
        if num1 == "0" or num2 == "0":
            return "0"

        # Ensure num1 is the longer number for efficiency
        if len(num1) < len(num2):
            return self.multiply(num2, num1)  # Swap if necessary

        res, zero = "", 0  # Result string and zero-padding counter

        # Iterate through digits of num2 from rightmost to leftmost
        for i in range(len(num2) - 1, -1, -1):
            cur = self.mul(num1, num2[i], zero)  # Multiply num1 by single digit num2[i]
            res = self.add(res, cur)  # Add the partial result to the final sum
            zero += 1  # Increase zero padding for next iteration
        
        return res

    def mul(self, s: str, d: str, zero: int) -> str:
        """
        Multiplies a number `s` (as a string) with a single digit `d` (as a string).
        Uses manual multiplication with carry handling and appends required zeroes.
        """

        i, carry = len(s) - 1, 0
        d = int(d)  # Convert single digit to integer
        cur = []

        # Multiply each digit of `s` with `d` from right to left
        while i >= 0 or carry:
            n = int(s[i]) if i >= 0 else 0  # Extract digit from `s`
            prod = n * d + carry  # Multiply and add carry
            cur.append(str(prod % 10))  # Store last digit of product
            carry = prod // 10  # Update carry
            i -= 1  # Move to the next digit
        
        # Reverse and append required zero padding for place value
        return ''.join(cur[::-1]) + '0' * zero

    def add(self, num1: str, num2: str) -> str:
        """
        Adds two large numbers represented as strings.
        Uses manual addition with carry handling.
        """

        i, j, carry = len(num1) - 1, len(num2) - 1, 0
        res = []

        # Iterate from right to left, adding digits
        while i >= 0 or j >= 0 or carry:
            n1 = int(num1[i]) if i >= 0 else 0  # Get digit from num1
            n2 = int(num2[j]) if j >= 0 else 0  # Get digit from num2
            total = n1 + n2 + carry  # Compute sum
            res.append(str(total % 10))  # Store last digit of sum
            carry = total // 10  # Update carry
            i -= 1
            j -= 1
        
        return ''.join(res[::-1])  # Reverse result and return
```

### APPROACH: MULTIPLICATION

```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        # If either number is "0", the product is "0"
        if "0" in [num1, num2]:
            return "0"

        # Initialize result array to store intermediate sums
        res = [0] * (len(num1) + len(num2))

        # Reverse both numbers to align least significant digits
        num1, num2 = num1[::-1], num2[::-1]

        # Perform digit-wise multiplication
        for i1 in range(len(num1)):
            for i2 in range(len(num2)):
                # Multiply corresponding digits
                digit = int(num1[i1]) * int(num2[i2])
                
                # Add to the corresponding position in the result array
                res[i1 + i2] += digit

                # Carry over to the next position
                res[i1 + i2 + 1] += res[i1 + i2] // 10

                # Store the last digit at the current position
                res[i1 + i2] = res[i1 + i2] % 10

        # Reverse the result array back to the correct order
        res, beg = res[::-1], 0

        # Remove leading zeros
        while beg < len(res) and res[beg] == 0:
            beg += 1

        # Convert result to string
        res = map(str, res[beg:])
        return "".join(res)
```
