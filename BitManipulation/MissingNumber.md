# 268. Missing Number
Difficulty: Easy

## QUESTION

Given an array `nums` containing `n` integers in the range `[0, n]` without any duplicates, return the single number in the range that is missing from `nums`.

Follow-up: Could you implement a solution using only `O(1)` extra space complexity and `O(n)` runtime complexity?

### EXAMPLE

```
Input: nums = [1,2,3]
Output: 0
```

```
Input: nums = [0,2]
Output: 1
```

## SOLUTION


### APPROACH: SORTING

```python
from typing import List

class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        
        # Sort the array to bring numbers in order
        nums.sort()
        
        # Iterate through the sorted array
        for i in range(n):
            if nums[i] != i:  # If the number at index i is incorrect, return i
                return i
        
        # If all numbers are in place, the missing number is n
        return n
```

### APPROACH: HASH SET 

```python
from typing import List

class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        # Convert the list into a set for fast lookups
        num_set = set(nums)
        n = len(nums)

        # Iterate from 0 to n and find the missing number
        for i in range(n + 1):  
            if i not in num_set:  # If i is not in the set, it's the missing number
                return i
```

### APPROACH: BITWISE XOR

```python
from typing import List

class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        xorr = n  # Start with n since one number from [0, n] is missing

        # XOR all indices and numbers in nums
        for i in range(n):
            xorr ^= i ^ nums[i]  # XOR index and value

        return xorr  # The remaining value is the missing number
```

### APPROACH: MATH

```python
from typing import List

class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        res = len(nums)  # Start with n (as one number from [0, n] is missing)

        # Iterate through the array and adjust res based on index and value differences
        for i in range(len(nums)):
            res += i - nums[i]  # Add the index and subtract the number at that index

        return res  # The result is the missing number
```
