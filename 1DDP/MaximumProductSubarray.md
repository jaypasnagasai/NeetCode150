# 152. Maximum Product Subarray
Difficulty: Medium

## QUESTION

Given an integer array `nums`, find a subarray that has the largest product within the array and return it.

A subarray is a contiguous non-empty sequence of elements within an array.

You can assume the output will fit into a 32-bit integer.

### EXAMPLE

```
Input: nums = [1,2,-3,4]
Output: 4
```

```
Input: nums = [-2,-1]
Output: 2
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        # Initialize result with the first element
        res = nums[0]

        # Iterate through each starting index of the subarray
        for i in range(len(nums)):
            # Start with the current number
            cur = nums[i]
            # Update the result with the current number
            res = max(res, cur)
            
            # Multiply current number with subsequent numbers to find the product
            for j in range(i + 1, len(nums)):
                cur *= nums[j]  # Multiply with next element
                res = max(res, cur)  # Update the result if a larger product is found

        # Return the maximum product found
        return res
```

### APPROACH: PREFIX AND SUFFIX

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        # Initialize variables
        n = len(nums)  # Length of the input list
        res = nums[0]  # Start with the first element as the initial result
        prefix = suffix = 0  # Initialize prefix and suffix products to 0

        # Iterate through the list from both the start and end
        for i in range(n):
            # Calculate prefix product (left to right)
            prefix = nums[i] * (prefix or 1)  # If prefix is 0, reset to 1 before multiplying

            # Calculate suffix product (right to left)
            suffix = nums[n - 1 - i] * (suffix or 1)  # Same reset logic for suffix

            # Update the result with the maximum of current prefix, suffix, and result
            res = max(res, max(prefix, suffix))

        # Return the maximum product found
        return res
```
