# 000. Products of Array Except Self
Difficulty: Medium

## QUESTION

Given an integer array `nums`, return an array `output` where `output[i]` is the product of all the elements of `nums` except `nums[i]`.

Each product is guaranteed to fit in a 32-bit integer.

Follow-up: Could you solve it in O(n) time without using the division operation?

### EXAMPLE

```
Input: nums = [1,2,4,6]
Output: [48,24,12,8]
```

```
Input: nums = [-1,0,1,2,3]
Output: [0,-6,0,0,0]
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'productExceptSelf' that returns an array where each element 
    # is the product of all elements in the input array except the one at the same index.
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        # Get the length of the input list.
        n = len(nums)
        # Initialize a result list with zeros, same size as the input list.
        res = [0] * n

        # Iterate through each index of the result list.
        for i in range(n):
            # Initialize a variable to store the product.
            prod = 1
            # Iterate through the entire list again to compute the product.
            for j in range(n):
                # Skip multiplying the element at the current index i.
                if i == j:
                    continue    
                prod *= nums[j]
            
            # Store the computed product in the result list at index i.
            res[i] = prod
        
        # Return the result list.
        return res
```

### APPROACH: DIVISION

```python
class Solution:
    # Define a method 'productExceptSelf' that returns an array where each element 
    # is the product of all elements in the input array except the one at the same index.
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        # Initialize product variable and zero count.
        prod, zero_cnt = 1, 0

        # First pass: Calculate the total product of non-zero numbers and count zeros.
        for num in nums:
            if num:
                prod *= num  # Multiply all non-zero numbers.
            else:
                zero_cnt += 1  # Count the number of zeros.

        # If more than one zero is present, the result is all zeros.
        if zero_cnt > 1: 
            return [0] * len(nums)

        # Initialize result array with zeros.
        res = [0] * len(nums)

        # Second pass: Compute the result based on zero count.
        for i, c in enumerate(nums):
            if zero_cnt:  
                # If there is one zero, all elements except that index should be zero.
                res[i] = 0 if c else prod
            else:  
                # If there are no zeros, divide total product by the current element.
                res[i] = prod // c

        # Return the result list.
        return res
```

### APPROACH: PREFIX AND SUFFIX

```python
class Solution:
    # Define a method 'productExceptSelf' that returns an array where each element 
    # is the product of all elements in the input array except the one at the same index.
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        # Initialize product variable and zero count.
        prod, zero_cnt = 1, 0

        # First pass: Calculate the total product of non-zero numbers and count zeros.
        for num in nums:
            if num:
                prod *= num  # Multiply all non-zero numbers.
            else:
                zero_cnt += 1  # Count the number of zeros.

        # If more than one zero is present, the result is all zeros.
        if zero_cnt > 1: 
            return [0] * len(nums)

        # Initialize result array with zeros.
        res = [0] * len(nums)

        # Second pass: Compute the result based on zero count.
        for i, c in enumerate(nums):
            if zero_cnt:  
                # If there is one zero, all elements except that index should be zero.
                res[i] = 0 if c else prod
            else:  
                # If there are no zeros, divide total product by the current element.
                res[i] = prod // c

        # Return the result list.
        return res
```
