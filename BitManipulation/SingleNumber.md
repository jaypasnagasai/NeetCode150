# 136. Single Number
Difficulty: Easy

## QUESTION

You are given a non-empty array of integers `nums`. Every integer appears twice except for one.

Return the integer that appears only once.

You must implement a solution with O(n) runtime complexity and use only O(1) extra space.

### EXAMPLE

```
Input: nums = [3,2,3]
Output: 2
```

```
Input: nums = [7,6,6,7,8]
Output: 8
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
from typing import List

class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        # Iterate through each element in nums
        for i in range(len(nums)):
            flag = True  # Assume nums[i] is the unique element
            
            # Compare nums[i] with all other elements
            for j in range(len(nums)):
                if i != j and nums[i] == nums[j]:  # If a duplicate is found
                    flag = False
                    break  # No need to check further, move to the next i
            
            # If no duplicate was found, return the unique element
            if flag:
                return nums[i]
```

### APPROACH: SORTING

```python
from typing import List

class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        # Sort the array so that duplicate numbers appear consecutively
        nums.sort()
        
        i = 0
        while i < len(nums) - 1:
            # If the current number has a duplicate next to it, skip both
            if nums[i] == nums[i + 1]:
                i += 2  # Move to the next pair
            else:
                return nums[i]  # The first non-duplicate number is the answer
        
        return nums[i]  # The last remaining element is the unique number
```

### APPROACH: BIT MANIPULATION

```python
from typing import List

class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        res = 0  # Initialize result
        
        # XOR all numbers in the array
        for num in nums:
            res = num ^ res  # XOR cancels out duplicate numbers
        
        return res  # The remaining number is the unique element
```
